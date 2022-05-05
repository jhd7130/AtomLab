# REST API
  
  
REST API
HTTP 프로토콜을 이용한 데이터자원 통신 아키텍처를 이야기한다.

가장 중요한 두개의 원칙 
1. **URI는 정보의 자원을 표현해야한다.(명사)**
2. **자원에 가해지는 행위는 HTTP Method로 표시되어야한다. (HTTP Method : PUT, GET, POST, Delete**  

  
  
추가적으로 하나의 요청은 하나의 자원을 반환해야하며 모든 통신은 무상태(stateless)성을 유지해야한다.(Http의 특징과 같다.)


# Builder Pattern
객체 생성시 생성자의 인자값이 많이 필요할 경우 Builder pattern을 사용하는 것이 유리하다.
그 이외에도 Setter보다는 builder 패턴을 사용하는 이유는 아래와 같다.

1. 필요한 데이터만 설정할 수 있음
2. 유연성을 확보할 수 있음
3. 가독성을 높일 수 있음
4. 변경 가능성을 최소화할 수 있음

기존에는 데이터 객체를 만들때 필수값을 위한 생성자, 선택 값을 위한 생성자를 차례로 생성하는 **점층적 생성자** 패턴을 사용했지만 생성자가 어떠 변수의 데이터에 매핑되는지 체크하는 것이 어려웠고 변수 개수에 맞게 
매핑하는 것이 불가능했다.  
위와 같은 단점을 보완하기 위해 setter를 이용한 bean 패턴을 사용해 가독성은 올라갔지만 한번의 메서드 호출이 아닌 여러번의 메서드 호출이 필요했다. 또한 가장 중요한 문제는 getter, setter로 인해 데이터의 일관성이
일시적으로 깨지게되며 immutable 객체(불변의 객체)를 생성이 불가능했다. 이러한 문제를 해결하기 위해 Builder Pattern이 생겨났다.

작성 법
``` java
public class Member {
    private int id;
    private String email; // 필수
    private String name;  // 필수
    private String phone; // 선택
    private String pw;    // 필수

        static class Builder{
            private int id;
            private String email; // 필수
            private String name;  // 필수
            private String phone; // 선택
            private String pw;    // 필수

            public Builder(String email, String name, String pw){
                this.email = email;
                this.name = name;
                this.pw = pw;
            }

            public Builder phone(String phone){
                this.phone = phone;
                        return this;
            }

            public Member build(){
                return new Member(this);
            }
        }

        public Member(Builder builder){
            this.email = builder.email;
            this.name = builder.name;
            this.phone = builder.phone;
            this.pw = builder.pw;
        }
}
```


생각보다 코드의 양이 많지만 이렇게 사용할 경우 데이터의 일관성을 보장하는 immutable한 데이터 객체의 생성이 가능하다.
이러한 데이터 이동에 있어서 데이터 일관성이 지켜지게 되지 않으면 협업을 하는 과정에서 문제가 발생하게된다. 중간에 값이 계속 바뀌게된다면 동료들은 해당 데이터 객체로 이동해 오는 데이터를 신뢰할 수 없게 되며
데이터객체의 이동경로를 다 체크해야하는 일이 발생하게된다.  
따라서 데이터 객체의 이동에서는 최대한 변경이 없어야하기 때문에 변경자체를 막는 것이 맞다. 

# RestController vs Controller
기존의 controller 자체는 view를 반환하기 위해 만들어졌기 때문에 Data 자체를 반환하려면 @ResponseBody를 추가해주는 번거로움이 존재했다.  
이러한 번거로움을 해결하고 반환객체의 자유도를 주기 위해 @RestController가 만들어졌고 이 컨트롤러는 자체로 @ResponseBody를 가지고 있으며 기존의 controller의 viewresolver 대신
Spring은 클라이언트의 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해 적합한 HttpMessageConverter를 선택하여 이를 처리한다.  
  
# @Component vs @Bean 차이(feat.@Configuration)
### @Configuration 과 @Component가 같다?
@Component, @Configuration, @Controller, @RestController, @Service, @Repository 어노테이션은 서버 실행시 Component-Scan을 통해 Bean이 등록된다.
위 목록에서 @Conponent를 제외한 나머지 Annotaion들은 모두 Component를 가지고 있다. 따라서 둘은 다르지 않다.


### 그래서 언제 쓰는데!?
**1. @Component**   
 - 개발자가 직접 작성한 클래스를 bean 등록하고자 할 경우 사용  
   
  
 
**2. @Configuration + @Bean**
 - 외부라이브러 또는 내장 클래스를 bean으로 등록하고자 할 경우 사용. 
 - 1개 이상의 @Bean을 제공하는 클래스의 경우 반드시 @Configuraton을 명시  

# Interceptor
filter(Tomcat Server)는 interceptor(Spring Container)보다 더 바깥쪽에 존재한다. 그래서 filter에서 interceptor를 컨트롤할 수 있다. 먼저 Interceptor를 알아보자.  
인터셉터는 뭐다? 
인터셉터는 따로 추가를 해줘야한다??(CONFIGURATION?으로 인터셉터 명시 필요?)  
인터셉터는 
