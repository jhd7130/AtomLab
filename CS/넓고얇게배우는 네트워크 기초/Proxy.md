## #1. (Application)Proxy
제목을 보면 L7 User Mode 계층에서 이루어지는 작업이라는 것을 알 수 있다. 이 Porxy는 구조라는 개념으로 이해하면 좋을 것 같다. 내 PC에서 다른 서버를 proxy로 등록하면 내 모든 요청은 해당 Proxy를 거쳐서 나가게된다. 요청이 한 다리를 거쳐서 나가게되니 응답도 똑같이 들어오게된다. 이러한 구조를 proxy 구조라고 이야기하고 다양한 곳에서 이 Proxy구조를 활용하고 있다. Proxy는 보통 User mode application Proxy라고 부른다. User mode 레벨에서 동작하니 다루는 데이터는 Socket을 통한 Stream 데이터를 다룬다. 따라서 들어오는 요청을 위한 Socket과 요청을 다시 반환하기 위해 사용할 Socket 두가지가 있다.  
  
한가지 외워야 할 것.  
- Usermode Application Proxy ==> Socket통신을 통한 Stream 데이터를 다룸  
- Inline과 Out of path의 경우 ==> Packet데이터를 다룸
  
## #2. Proxy 활용
1. 우회  
<img width="1682" alt="스크린샷 2022-06-25 오전 11 10 18" src="https://user-images.githubusercontent.com/78134917/175755367-2c8dca96-ab63-4cae-8c36-72d1ad121791.png">  
  
2. 분석  
<img width="1663" alt="스크린샷 2022-06-25 오전 11 10 33" src="https://user-images.githubusercontent.com/78134917/175755412-a53a041d-4540-4454-82ef-ea75610cabaa.png">

  
3. 감시와 보호  
  
<img width="1674" alt="스크린샷 2022-06-25 오전 11 10 41" src="https://user-images.githubusercontent.com/78134917/175755416-f9fca1e9-8f55-48ac-a762-245e07d86027.png">

   
4. Reverse Proxy  
   
<img width="1682" alt="스크린샷 2022-06-25 오전 11 10 50" src="https://user-images.githubusercontent.com/78134917/175755420-4dc9770e-f200-40dd-9c90-465130d2c9d9.png"> 
