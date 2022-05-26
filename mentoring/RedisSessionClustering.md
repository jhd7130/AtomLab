# 1. Redis를 Session을 저장소로 사용하기.  
('Session저장소'가 'Redis'다 라는 말보다 깔끔한 것 같다.)
### 개요
로그인 API를 개발하고 있다. 이 로그인 API가 대용량 트래픽이 들어오는 서비스에 사용되었을 때 그 많은 트래픽을 감당할 수 있을까? 라는 의문이 생겼다. 그럼 어떻게 해야할까?라는 생각부터 파생되어 나갔다.
우선 CPU부하를 줄이기위해 서버 scale-out을 사용하는 곳이 많을 것이었고 그럼 로그인 api의 refreshToken을 저장할  
## #1. Session 저장소란?  
Session  
>세션은 HTTP가 지원하지 않는 요청자 정보를 잠시 저장할 수 있는 특별한 저장 공간이기도 하면서, 클라이언트가 서버에 실제는 연결이 되어 있지 않지만 마치 연결이 되어 있는 것처럼 만들어 주는 논리적인 연결 을 의미합니다.  
>https://thecodinglog.github.io/web/2020/08/11/what-is-session.html -- 세션과 쿠키에 대해 정말 잘 쓰여져 있다. 꼭 참고해서 보길 바란다.  
Session의 경우 요청이 들어오면 클라이언트 마다 저장공간을 부여하고 저장공간의 식별자인 Session Id를 반환한다. 여기서 말하는 저장공간은 DB(Header,Detail을 가진 테이블)가 될 수도 있고 
Map<String,Object>처럼 자료구조가 될 수도 있다. 뭘 사용해도 된다는 말이다.


## #2. Redis란?  

## #3. in-memory 방식의 Session storage(Redis)를 선택한 이유

## #4. Spring-Boot에서 Redis를 Session 저장소로 사용하기(준비부터 상태값 확인까지)
