## 들어가기에 앞서 Process와 Thread의 차이 
OS관리의 최소 단위는 Process이다. Process는 연산의 연속적인 흐름이다. 그 안의 흐름이 여러개가 생기게되면 Thread가 n개 생기는 것이다. 
- Process라는 작업이 있다면 Process는 기본적으로 1개의 Thread(연산의 흐름)를 가지고 있다. 
- OS는 Process에게 Virtual Memory(가상공간)을 할당(Attetch)시킨다. Process 속 Thread는 Process에 속해있기 때문에 사용가능한 공간 또한 VM으로 제한된다. 
- 각 Thread는 VM에서 영역을 가지게된다. Stack과 Thread Local Storage이다. 그리고 나머지 Process전체에서 공유할 수 있는 공간은 Heap이라고 한다.
