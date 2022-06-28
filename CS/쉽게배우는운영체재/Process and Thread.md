## #1.들어가기에 앞서 Process와 Thread의 차이 
OS관리의 최소 단위는 Process이다. Process는 연산의 연속적인 흐름이다. 그 안의 흐름이 여러개가 생기게되면 Thread가 n개 생기는 것이다. 
- Process라는 작업이 있다면 Process는 기본적으로 1개의 Thread(연산의 흐름)를 가지고 있다. 
- OS는 Process에게 Virtual Memory(가상공간)을 할당(Attetch)시킨다. Process 속 Thread는 Process에 속해있기 때문에 사용가능한 공간 또한 VM으로 제한된다. 
- 각 Thread는 VM에서 영역을 가지게된다. Stack과 Thread Local Storage이다. 그리고 나머지 Process전체에서 공유할 수 있는 공간은 Heap이라고 한다.

## #2. Process의 OS할당과 상태  
### Program -> Process  
컴퓨터는 기본적으로 CPU, RAM, HDD같은 한정된 자원을 가지고 있다. 이 한정된 자원을 효율적으로 사용할 수 있게 해주는 응용 프로그램이 Operation System이다. 우리가 하나의 프로그램을 사용할 때는 설치를 한다. 설치의 과정은 HDD에 프로그램에 대한 정보를 저장하는 것이다. 프로그램을 가지고있지만 실행 전의 상태이다. 사용자가 프로그램을 실행시키면 OS는 프로그램을 HDD에서 RAM메모리로 인스턴스 화 시킨다. 이때 OS는 Process에게 Virture Memory(Ram과 Hdd의 논리적 조합)영역 할당과 PCB(process controll box)를 할당 시킨다. 이 PCB에는 PID, 프로세스 작동에 필요한 기계어와 같은 Process 관리에 필요한 정보들이 존재한다.  
  
### Process의 상태  
프로세스가 어떻게 할당되고 준비되는지 알아 보았다. 이제 준비된 후 Process가 어떻게 작동하는지 보자.  
Process는 상태라는 것을 가지고 이 상태는 전이(변화)된다. 어떤 상태가 있는지 알아보자.  
**1. 준비상태, 완료상태, 대기상태**  
<img width="700" alt="image" src="https://user-images.githubusercontent.com/78134917/174938802-e8472176-e970-47ce-b087-b14db081776d.png">  
blocking과 non-blocking의 차이는 이 요청 프로세스의 상태와 관련이 있다. 대기 상태로 상태 전이가 일어나는가 아닌가에서 어떤 IO인지 결정난다.  
**dispatch : 선택해서 꺼낸다.**   
- 자신이 속해있는 프로세스의 상태가 Ready(준비상태)인 Thread들은 Ready-Queue에 줄을 선다. 그럼 OS가 줄 서있는 맨 앞 쓰레드들을 하나씩 dispatch해서 CPU로 넘겨준다.   
  
**2. sleep상태, suspend 상태**   
두가지는 휴식상태과 보류상태를 만드는 역할을 하는 API이다.   
  
두 상태의 큰 차이는 개발자가 의도적으로 발생시켰는지에 대한 여부이다. sleep의 경우가 의도적 탈출이고 suspend의 경우는 OS와 같은 외부적 요인에서 탈출이 발생한다.  
  
sleep의 경우 조심해서 사용해야하는데 이유는 다음과 같다. sleep과 suspend를 사용할 경우 해당 프로세스는 준비상태 일때 대기했던 Ready queue에서 탈출하게 된다. 그리고 나중에 맨 뒤로 재진입을 한다. 이 시점이 문제가 된다. slepp(10)처럼 ms를 설정할 수 있는데 10ms만 대기하다 재진입하지 않는다는게 문제다. 휴식을 하는데 컴퓨터 마음대로 재진입 시기를 결정하기 때문에 위험하다. 개발자의 조작에 의해 결정되는 것이 아니기 때문이다. 예상할 수 없다면 사용하지 않는게 좋다.  

