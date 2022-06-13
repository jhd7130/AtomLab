## #1. 8버전 

**PermGen 영역의 제거**

기존의 jvm메모리 구조에서 PermGeneration 영역이 Os의 Native meta memory 영역으로 이동했다. 기존의 메타정보들은 Native 메모리 영역으로 이동한 시켰다. 

**Lamda Expression 및  Method Reference 도입**

반환 타입이나 메서드 명이 필요 없는 익명 함수로 사용 가능한 람다식, 이런 것을 통해 메서드를 인자 값으로 넘기는 것도 가능해 졌다. 

**Collections & Streams** 

Stream Interface는 Collection 객체를 다룰 수 있는 새로운 방법을 제공했다. 중간에 필터링을 한다던지 정렬을 한다던지 중간작업을 빌딩형식으로 작성이 가능해졌다. 

**Interface Default Method 도입**

인터페이스에서는 public abstract methods만 사용 가능했지만 static과 default 메서드도 사용가능해졌다. 

**Optional 객체**

예상치 못한 nullpointException 예외를 위한 if문 없이도  막을 수 있는 Wrapper객체이다. 

[**java.util.Date](http://java.util.Date) 클래스를 대체할 Date 관련 객체들** 

불변객체가 아니기 때문에 쓰레드 세이프하지 않아서 불변객체인 Date관련 객체들이 나옴.



## #2. 9버전

**모듈 시스템의 등장**   
  
JDK에서 필요한 부분만 골라서 사용하고, 클래스 경로를 쉽게 유추할 수 있으며, 플랫폼을 진화시킬 수 있는 강력한 캡슐화를 제공할 새로운 건축 구조가 필요했다.  
따라서 자바에서 제공하는 단위로써 모듈이 추가되었고 이는 패키지보다 큰 단위를 의미한다.  
의존하는 다른 모듈(requires)과 다른 모듈에서 접근할 수 있는 항목(exports)을 포함한 모듈에 대한 설정이 들어있는 module-info.java 파일을 생성함, 이로써 필요한 모듈만 구동하여 크기와 성능 최적화가 가능  
  
**private metode interface 지원**  
  
private method / private static method라는 새로운 기능을 제공  
  
**다이아몬드 연산자**   
  
익명 클래스에 대한 Diamond Operator를 허용한다.  

**프로세스API**   
  
현재 프로세스, 종료된 프로세스, 하위 프로세스의 정보를 확인할 수 있는 API제공  
  
**Collection들을 위한 팩토리 메서드 제공**   
  
.of를 제공한다. 다만 이 메서드로 작성된 컬렉션은 불변 클래스이다.    
  
**Reactive Streams (from Flow API)**    
  
publish-subscribe 프레임워크를 제공하는 Flow API에 Reactive Stream이 추가 되었다.  
  
**Stream의 개선**   
  
 iterate(), takeWhile()/dropWhile(), ofNullable()  과 같은 새로운 추가 메소드들을 통해 비동기 프로그래밍에서의 유용한 개선사항 제공    
 
 
## #3. 10 버전  

**지역 변수 타입 추론 기능 추가 var 제공 (변수 초기화 시 Diamond operation을 통한 Generic 명시**    
  
**G1GC개선**    
  
**unmodifable collection를 만들어주는 copyOf 메서드와 Stream API에서 collect()에서 사용하는 Collectors.toUnmodifiableList() 메서드가 추가  
  
**Optional에서 orElseThrow 메서드가 추가**  
  
## #4. 11 버전  
**HTTP Client API 정식 통합**   
  
**Optional에서도 추론타입 var 사용가능**   
  
String API 메서드 추가 ****isBlank, lines, strip, stripLeading, stripTrailing, repeat 메서드가 추가되었습니다.    

Collection to Array 추가 toArray(String []::new), toArray(Integer []::new) 방식으로 사용 가능합니다.  

 
