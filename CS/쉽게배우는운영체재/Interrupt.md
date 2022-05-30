## 1. Interrupt
### #1. 인터럽트의 종류
이전시간에 배운 interrupt를 조금 더 자세하게 다뤄보자.  
저번 시간에 알게된 interrupt는 여러 interrupt종류 중 하나인 i/o단계에서의 interruptd이다. 다른 종류의 interrupt도 존재한다.  
1. 전원이상 interrupt : 말그대로 전원이상 또는 정전  
2. 기계착오 인터럽트 : CPU의 기능적 오류
3. 외부신호 인터럽트 
   - 타이머에 의한 인터럽트
   - 키보드로 인터럽트 키를 누를 경우 (ex> Controll + Alt + Delete)
   - 외부장치로부터 인터럽트의 요청이 있는 경우 : I/O 인터럽트와 다름
4. 입출력 인터럽트 (I/O interrupt)
   - 데이터의 전송이 일어나거나 전송이 끝날 경우 
   - 입출력 데이터에 이상이 있는 경우

이것 말고도 내부적으로 발생하는 인터럽트가 있을 수 있다. 
잘못된 명령, 데이터가 사용될 경우 trap이 있을 수 있다. 
프로그램 check interrupt
- Division by Zero
- OverFlow/UnderFlow
- 기타 Exception

### #2. 인터럽트의 동작 순서
1. 인터럽트가 요청된다. 
2. 현재 실행중이던 모든 프로그램을 중단한다. 단, micro operation까지는 수행한다.
3. 현재 실행중이던 프로그램의 상태를 보존한다. (현재 프로그램의 상태를 process controll block에 저장거나 program counter에 저장한다.)  
4. 인터럽트 처리루틴 실행 : 인터럽트를 요청한 장치를 식별한다. 
5. 인터럽트 서비스 루틴 실행 : 원인을 파악(위에서 처럼 인터럽트에는 여러 원이이 존재한다.)하고 실질적인 작업을 수행한다. 
6. 상태를 복구한다. 인터럽트가 발생했을 때 저장해둔 PC를 복구한다. 
7. 꺼내온 프로그램 상태정보를 통해 프로그램 실행을 재개한다.  


### #3. I/O 인터럽트 발생에 대한 그림  
<img width="331" alt="image" src="https://user-images.githubusercontent.com/78134917/170910220-21db4eea-be87-4010-8779-b98ebffd11b9.png">

위처럼 기존에는 메인보드에 브릿지 칩셋이 있고 CPU와 RAM의 처리를 도와주었다. 하지만 요즘의 CPU는 RAM과 직접 연결될 수 있게 나온다.  
### #4. DirectX가 나오게된 계기와 DMA의 등장 
우리의 컴퓨터는 세개의 층으로 되어 있다.   
<img width="834" alt="image" src="https://user-images.githubusercontent.com/78134917/170910936-1e60103f-6a0d-483e-b3e0-fa07f596e877.png">  
고성능 엔진기술을 처리하기 위해서는 엄청나게 많은 연산이 반복해서 여러번 이루어져야한다. 하지만 왼쪽의 그림처럼 지나쳐가야하는 단계가 많다면 interrupt가 발생한다. 하지만 이 interrupt가 발생하면 성능이 
굉장히 많이 줄어든다. 고사양 게임을 부드럽게 실행시키기 위해서는 대책이 필요했고 API만 가지고 있으면 user여역부터 kernel 영역까지 한번에 처리가 가능한 DirectX가 등장했고 이 DirectX의 영향으로 
메모리에 직접 접근이 가능해졌다. 이를 통해 DMA(Direct Memory Access)라는 개념이 등장했다.



