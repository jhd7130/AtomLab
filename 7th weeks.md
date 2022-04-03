# 7주차 멘토링  
  
#1. Java Exception

![checked](https://user-images.githubusercontent.com/78134917/161390404-31001486-3307-4c09-8dfa-d07f8ad3302c.jpeg). 

  
전역 에러 처리
https://bcp0109.tistory.com/303?category=981824. 

모든 전역 에러 처리를 하는 이유는 여러가지이다.  
3 tier의 어플리케이션을 구현한다고 했을 때 controller, repository, busuiness logic 중 어느 곳에서든 에러는 발생할 수 있다. 하지만 모든 에러를 예상하고 처리하는 것은 불가능하다. 그리고 만약
모든 에러를 대처하기 위해 모든 method에 try-catch를 감싸게 된다면 코드는 아주 못생겨진다. 그럼 왜 custom한 exception을 생성하여 제공하는 것일까? framwork에서는 기본적으로 error에 대한 처리를
해주고 HttpStatus의 경우에 어떤 오류의 발생인지 명시되어 있어 사용자에게 알릴 수 있다. 하지만 모든 에러를 framwork에 맡기지 않는 이유는 HttpStatus가 제공하는 에러 메세지는 굉장히 불친절하기 때문이다.
