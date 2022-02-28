# 3주차 과제

## Permanant Generation, MetaSpace
Permanat Generation은 8버전 이후로는 존재하지 않고 MetaSpace로 대체되었다.    

그럼 Permanant Generation은 어떤 영역이었을까? 우리가 논리적으로 5가지로 나눴던 Runtime Data Area 중 Method Area의 물리적인 저장 영역이다. 이 Permanant Gen영역은 Heap영역에 존재했다.  

이 영역에는 아래 5가지 정보를 가지고 있었다. 
1. Class, Method의 메타정보
2. Static의 Object
3. 상수화된 Object, 상수 Pool
4. Class와 관련된 배열 객체 정보
5. JVM의 내부적 객체들과 최적화 컴파일러(JIT)의 최적화 정보
  
이러한 정보들은 8버전이후 Heap과 MetaSpace 영역으로 나눠졌다.

기능적이유가 가장 중요하기에 어떠한 기술적 문제가 발생했는지 부터 살펴보자.  
개발자의 무분별한 Static Object 생성은 잦은 OOM(Out of Memory)를 발생 시켰다. 개발자의 실수로 발생된다기 보다는 Perm Gen영역의 메모리가 제한적이기 때문에 GC의 관리가 들어간다 하여도
OOM이 발생하였다.    

이러한 이유로 수정이 필요 없는 메타정보들은 MetaSpace 영역으로 보냈고 GC의 관리가 필요한 Static Variable(Static 변수)와 Static Object, Constant Pool information(String pool 포함)
들을 Heap영역에 저장되게 했다.   

![image](https://user-images.githubusercontent.com/78134917/155848432-4cc9b76d-dc25-4807-b774-d6390da835f7.png)


그럼 이 MetaSpace는 어떤 메모리 영역에 존재할까?  
OS선(Native Method로 관리한다.)에서 관리하는 Native Memory(off-heap)영역에 존재한다. 이걸 OS가 관리하는 Native Memory영역으로 넘긴 이유는 메모리가 부족하면 Os가 알아서 메모리 사이즈를 
늘려주기 때문이다. 따라서 개발자는 필요에따라 사이즈를 제한해주기만 하면된다.  (이게 좀 애매하다. OS 영역으로 넘겼는데 JVM이 동적으로 관리를 해준다고 이야기가 나온다. 출처 : https://brunch.co.kr/@heracul/1
이건 OS딴에서 자동으로 메모리를 늘려주는게 맞을까? 또한 Full Gc 발생시 MetaSpace에 대한 Mark and Sweep도 발생할까?)  

여기서 주의 깊게 봐야하는 것은 Static Object와 Constant pool information들이 Heap 영역으로 이동했다는 것이다.  

## Byte Stream  
우선 Stream이란 ? 바이트로 이루어진 끊기지 않고 연속적인 데이터를 말한다. (대표적으로 사진파일들을 예로 들 수 있다.) 

이와 같은 Byte Stream을 다루기 위해 자바에서는 대표적으로 두가지 InputStream과 OutputStream을 추상클래스가 존재한다.  
우리가 이 두가지 추상클래스를 보기 전에 I/O(input과output)을 볼 필요가 있다.  

I/O는 어떤 프로그램에 있는 내용을 파일에 읽거나 저장할 일이 있을 때, 다른 서버나 디바이스로 보낼일이 있을때 사용한다.  
JVM을 기준으로 읽을 때는 INPUT를 사용하고 전송할 때는 OUTPUT을 사용한다.(JVM의 기준에서 이렇게 사용한다.   
JAVA에서는 이러한 I/O를 처리하기 위해서 java.io 패키지를 제공, 이 패키지에서는 바이트 기반의 데이터를 처리하기 위해서 여러 종류의 Stream 클래스를 제공한다.  
읽는 작업은 inputstream 쓰는 작업을 outputsteam, 여기서 스트림이란 끊기지 않고 연속적인 데이터를 말한다.  
CHAR 문자열로 되어 있는 파일에서는 READER, WRITER 추상 클래스로 처리한다.   

## NIO(New I/O)
기존의 io의 속도를 향상시키기 위해  NIO라는 새로운 패키지가 탄생했다.(OS의 메모리 접근 문제와 System call(input,outputstream 실행시 매번 실행되는 비용발생)문제로 속도가 느리다.)  이 NIO라는 패키지 내부에 있는 클래스들은 스트림(input,output 각각의 스트림필요)을 사용하지 않고 Channel(하나의 채널로 read,write가능)을 사용하고 1byte씩 읽고 쓴느게 아니라 Buffer를 사용하여 쌓아두고 필요시 한번에 처리한다.  
Chennel은 물건을 중간에서 처리하는 도매상이라고 보면되고 Buffer는 도매상에서 물건을 사고 소비자에게 물건을 파는 소매상으로 생각하면 좋다.  
무언가 만들어서 buffer에 담아서 channel에 넘기면 그 buffer를 처리해준다. 따라서 자주사용하게될 buffer를 이해하는 것이 좋다.  
buffer에는 limit(), position(), capacity() 메서드가 존재하는데 이 세가지의 관계를 이해하는게 중요하다. 



## Serializtion(직렬화 : object ----> 전송가능한 byteStream으로 변환, 역직렬화는 반대로 된다.)
#### Serializable
Serializable은 인터페이스이다. 이 인터페이스를 구현하면 해당 클래스가 파일을 읽거나 쓸 수 있도록 만들어주고, 다른 서버로 해당 클래스를 보내거나 받을수 있도록 만들어준다.  
이 인터페이스를 구현한 후에는 식별자와 같은 serialVersionUID라는 값을 지정해주는 것이 좋다.  
저정방법은  
> static final long serialVersionUID = 1L;이다.  
이 UID는 식별자의 역할을 한다. 이 식별자는 직렬화된 객체를 역직렬화할때 필요하다.


## Generics  

Generics : 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 **컴파일 시의 타입체크(Runtime error 보다 컴파일시 에러가 발생하는게 예방차원에서 더 좋다.)** 를 해주는 기능이다(기존에도 존재했지만 한계가 존재했다.). 이 지네릭스는 다음 2가지의 장점을 제공한다.  
> 1. 타입의 안정성을 제공한다.  
> 2. 타입체크와 형변환을 생략할 수 있기때문에 코드가 간결해진다.(이미 지정되어 있는 타입을 컴파일할때 알고 있기 때문에 타입체크가 강해졌고 이로 인해 타입체크가 강화되었다.)  
> 3. 객체의 사용의 자유도가 높아진다. 
실행시 에러(Runtime Error)의 발생하는 ClassCastException을 컴파일시 에러(Compile Error)로 만들기 위해 만들어 실행시 에러발생으로 프로그램이 죽는걸 방지하기 위해 만들었다.  
T를 타입변수라고 하며 어떤 글자가 들어와도 상관없다. 실제 사용할때는 실제 사용할 타입을 타입변수 대신 지정해줘야하며 객체생성시 참조타입과 생성자의 타입이 같아야하한다.  
  
```java
import java.util.ArrayList;
  
class Tv{}
class Audio{}
  
public class GenericTest{
  public static void main(String[] args){
       ArrayList list = new ArrayList(); // 과거 
      ArrayList<Tv> list = new ArrayList<Tv>();  //현재 : Tv타입의 객체만 저장가능, 참조변수에 대입된 타입과 생성자에 대입된 타입이 동일해야한다.
      list.add(new Tv());
      list.add(new Audio()); // 에러 발생
      Tv t = (Tv)list.get(0); // 과거의 코드는 꺼내올때 Object 객체로 나오기 때문에 형변환 필요
      Tv t = list.get(0);     // 현재의 코드 이미 타입을 알고있기 때문에 형변환이 필요없다.
    }
}
```  

시그니쳐에서는 원시 타입은 다형성이 인정되지만 매개변수화된 타입은 다형성이 인정되지 않는다(와일드카드는 예외로써 사용이 가능하다.). 나머지는 다형성이 모두 인정된다.
  

## Optional 이란?  
존재하지 않는 값을 표시하는 null은 값이 존재하지 않는다는 것을 명시해주기때문에 인식적인 문제에서는 좋은 해결책이었으나.  결국 현대에는 null point exception을 발생시킨다.  
이 null과 관련된 문제는 크게 두가지가 있다.   
 
1. 런타임에 NPE(NullPointerException)라는 예외를 발생시킬 수 있다.
2. NPE 방어를 위해서 들어간 null 체크 로직 때문에 코드 가독성과 유지 보수성이 떨어진다.  
그냥 두자니 곳곳에 숨어서 일으켜 장애를 유발하고, 조치를 하자니 코드를 엉망으로 만드는게 null이다.  

자바는 null에 대해 신경을 덜 쓰기 위해 함수형프로그램에서 착안하여 Optional이라는 클래스가 만들었다. 이 클래스는 우리가 기본타입(primitive type)을 객체로 만들때 사용하는 Integer 클래스와 같은 Wrapper 클래스이다. null이 발생할 것 같은 객체를 이 Optional클래스로 감싸서 사용하게되면 NPE의 발생을 최소화할 수 있다.  

이 optional은 serializable이 구현되어 있지 않음으로 직렬화가 불가능하다. 이 말은 optional 클래스로 wrapping한 객체를 사용하는 클래스는 직렬화가 불가능하다는 말이다.

