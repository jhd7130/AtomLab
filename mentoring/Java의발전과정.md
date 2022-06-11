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
