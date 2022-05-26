# 1. Redis를 Session을 저장소로 사용하기.  
('Session저장소'가 'Redis'다 라는 말보다 깔끔한 것 같다.)
### 개요
로그인 API를 개발하던 중 서버가 scale-out되었을 때 로그인의 지속여부를 어떻게 확인해야하가 대한 고민에서 session clustering방식을 알게되었고 적용을 위해 공부한다.  
우선 Http 프로토콜을 이용한 Rest API통신의 경우에는 stateless, 즉 무상태성을 유지해야한다.(이전 요청과 다음 요청이 어떠한 관계도 있어선 안된다.) 따라서 서버에 어떠한 클라이언트의 정보도 저장되지 않기 때문에 지속적으로 빠른 서비스가 가능하다.
하지만 여기서 문제가 생긴다. 이렇게되면 사용자가 서비스에 로그인한 뒤 다른 페이지로 넘어갈 때마다 계속해서 로그인 해야한다.(서버에서 클라이언트 정보가 없기때문에) 그럼 서버가 클라이언트를 식별할 수 있는 방법은 없을까?  
  
요청시 필요한 정보를 모두 보내주거나 내가 누군지만 알려주고 서버에 저장된 정보를 이용하는 것(디비에 저장하면 이런 식으로 사용가능)이럴 때 Session을 사용합니다.(사용안하면 사용자가 로그인을 계속해야하는 불편함을 해결하지 못함, 아무리 HTTP규약이 중요하다고해도)

## #1. Session이란?  
**Session**  
>세션은 HTTP가 지원하지 않는 요청자 정보를 잠시 저장할 수 있는 특별한 저장 공간이기도 하면서, 클라이언트가 서버에 실제는 연결이 되어 있지 않지만 마치 연결이 되어 있는 것처럼 만들어 주는 논리적인 연결 을 의미합니다.(Rest API 방식의 HTTP 통신은 stateless(무상태)를 기본으로한다.  
>https://thecodinglog.github.io/web/2020/08/11/what-is-session.html -- 세션과 쿠키에 대해 정말 잘 쓰여져 있다. 꼭 참고해서 보길 바란다.     


Session의 경우 요청이 들어오면 클라이언트 마다 저장공간을 부여하고 저장공간의 식별자인 Session Id를 반환한다.(이부분이 잘 이해가 안되었었다.) 여기서 말하는 저장공간은 DB(Header,Detail을 가진 테이블)가 될 수도 있고 Map<String,Object>처럼 자료구조가 될 수도 있다. 뭘 사용해도 된다는 말이다.  
  
**Sticky Session**  
트래픽(요청)이 많아지면 서버의 CPU부하를 줄이기 위해서 scale-out을 많이 사용한다. 이 때 load-balancing기술을 사용해 여러개의 서버에 요청을 골고루 분배해준다. 이렇게 되면  클라이언트A가 두번의 요청을 했을때 한번은 서버A로 두번째는 서버B로 지정될 수 있다. 이처럼 요청별로 지정되는 서버가 달라진다. 서버에서 클라이언트 별로 최초요청에 session을 생성하고 session ID를 지정해준다고 했다. 그럼 다음 예를 보자. 클라이언트A가 첫번째 로그인을 요청했을 때 서버A로 갔고 session 저장소가 생성되고 로그인여부가 저장되었다. 두번째 요청이 서버B로 가게되었는데 서버B에는 클라이언트A를 위한 Session 저장소가 없다. 그럼 클라이언트A의 사용자는 다시 로그인을 해야한다. 이렇다고 했을 때 만약 서버가 100대라면? 사용자는 100번의 로그인을 해야할 지도 모른다. 이러한 것들을 방지하기 위해 load-balancer에서 요청별로 최초 연결된 서버 정보를 가지고 있어서 클라이언트별로 요청을 지정된 서버로 할당시켜준다. 이런 방식을 Sticky Session이라고 한다.  하지만 단점이 존재한다.  
하나의 서버로 요청이 몰리면? 로드밸런서가 고장나면? 서버가 고장나면 Session정보는 사라진다.  
이러한 단점들을 보안하기 위해 Session-clustering을 사용한다. (세션을 한 곳에 모아서 관리한다.)  
Session-clustering에는 두가지 방식이 존재한다. Session을 위한 DB를 만들어 Session 저장소를 만들거나, in-memory DB를 Session 저장소로 만드는 것이다. 여기서 중요한 것은 요청마다 SessionId가 부여된다. clustering방식의 Session 관리는 해당 SessionId를 가진 Session저장소들을 관리하는 거다. 클라이언트A가 어떠한 요청을하면 Session이 생기고 SessionId가부여된다. 그럼 그 SessionId를 식별자로 Session이 Session저장소에 저장된다. 클라이언트 B의 요청은 또다른 SessionId로 저장소에 저장된다. 

## #2. Redis란?
in-memory방식의 DataBase를 제공해주는 NoSQL이다. 장점은 disk를 사용하는 DBMS보다 빠르다. 서버에서 서버로 이동하는 IO가 없으니 당연히 빠르다. 이건 장점이긴하지만 Redis의 장점 중 하나 일뿐이다. 그리고 인메모리 디비는 다 빠른다. Redis가 다른 인메모리 디비보다 좋은 점 중 가장 큰 부분은 다양한 자료구조를 지원한다는 것이다. 
<img width="1074" alt="스크린샷 2022-05-26 오후 4 30 24" src="https://user-images.githubusercontent.com/78134917/170440117-cbfa4e7a-56da-4119-a33a-be94b7558ed6.png">  
https://devlog-wjdrbs96.tistory.com/374

 
## #3. DB가 아닌 in-memory 방식의 Session storage(Redis)를 선택한 이유
로그인 확인은 interceptor또는 AOP를 통해 지속적으로 들어오는데 이를 모두 DB서버를 왔다 갔다하면서 처리하면 IO부하가 많아지기도하고 로그인을 위해 저장되는 Session의 크기도 크지 않기 때문에 in-memory방식으로 관리하기로 했다. 

## #4. Spring-Boot에서 Redis를 Session 저장소로 사용하기(준비부터 상태값 확인까지)
1. 로컬또는 서버에 Redis설치하기
2. 레디스를 위한 Spring-boot 설정
```gradle
#Redis build.gradle implementation추가
implementation 'org.springframework.boot:spring-boot-starter-data-redis'
implementation 'org.springframework.session:spring-session-data-redis'
```  
```yml
# application.yml에 아래 값들 추가
spring:
  session:
      store-type: redis  
  redis:
      flush-mode: on_save
      namespace: spring:session
      host: localhost
      password:
      port: 6379
```  
```java
// Configuration 추가 하기
package com.flab.fire_inform.global.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.connection.RedisPassword;
import org.springframework.data.redis.connection.RedisStandaloneConfiguration;
import org.springframework.data.redis.connection.lettuce.LettuceConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.repository.configuration.EnableRedisRepositories;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import org.springframework.session.data.redis.config.annotation.web.http.EnableRedisHttpSession;

@Configuration
@EnableRedisRepositories
@EnableRedisHttpSession(maxInactiveIntervalInSeconds = 60) /* 세션 만료 시간 : 60초 */
public class RedisConfiguration {

    @Value( "${spring.redis.host}")
    public String host;

    @Value("${spring.redis.port}")
    public int port;

    @Value("${spring.redis.password}")
    public String password;

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setEnableTransactionSupport(true);

        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());

        redisTemplate.setConnectionFactory(redisConnectionFactory);
        return redisTemplate;
    }

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        RedisStandaloneConfiguration configuration = new RedisStandaloneConfiguration();
        configuration.setHostName(host);
        configuration.setPassword(password.isEmpty() ? RedisPassword.none() : RedisPassword.of(password));
        configuration.setPort(port);
        return new LettuceConnectionFactory(configuration);
    }
}

```

