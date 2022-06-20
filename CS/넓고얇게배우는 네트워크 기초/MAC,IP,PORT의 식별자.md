## #1. MAC,IP,PORT는 뭘 식별할까?  
<img width="635" alt="image" src="https://user-images.githubusercontent.com/78134917/170925000-6ff9b320-c2d5-49c7-a52b-9f20c842b589.png">


**MAC주소 : NIC의 식별자**
인터넷을 연결할 수 있는 NIC(network interface card)의 식별을 담당한다. 여기서 NIC를 LAN카드(유선/무선)를 의미한다.   
하드웨어 적인 부분을 이야기한다. 변경이 가능하다. 하드웨어 기기에 NIC가 두개이면 MAC주소도 두개이다.
  
**IP주소 : 컴퓨터 기기에 대한 식별자**  
HOST에 대한 식별자. 여기서 HOST는 인터넷에 연결되어 있는 컴퓨터 기기를 이야기한다. 이 IP주소는 기기 한대당 n개를 가질 수 있다.   
  
**Port번호 :  Process에 대한 식별자**   
Process에 대한 식별자를 이야기한다. 소프트웨어 개발자에게는 process 식별자, 네트워크 3,4계층 담당자에게는 service 식별자, 하드웨어 담당자들에게는 선을 꽂는 구멍에 대한 식별자 .   

개발자 입장에서의 port 번호는 위에서 이야기했다 싶이 process에 대한 식별자로 생각해야한다. 하지만 이 포트번호는 네트워크에서 많이 사용되는데 어떻게 사용될까? 우리가 흔히 네트워크 통신을 할때 kernel mode (TCP/IP)와 user mode를 연결해주기 위해 socket을 이용한다. 각 process마다 개별적인 socket이 생기고 해당 process의 port번호는 소캣에 바인딩된다.

## #2. Host, Switch, Network
### Host와 Switch
Host: Coumputer <=> Network(네트워크에 연결된 컴퓨터)를 Host라고 부른다. 하지만 네트워크를 구성하고 있는 것들도 컴퓨터이기 때문에 이러한 Host도 상황에 따라 부르는 경우가 다른다. 아래 두가지다.
- 네트워크 이용주체 : End-Point 이러한 host는 server가 될 수도 있고 peer가 될 수도 있고 client가 될 수도 있다. (client, server 중 아무역할도 하지 않으면 peer라고 함.)
- 네트워크를 구성하고있는 주체 : Switch  

 네트워크 자체인 Switch의 대표격이 Router이다.   
 
 switcher의 경우에는 osLayer영역 중 무엇을 가지고 switching하느냐에 따라 이름이 달라진다. 
 L2 switch : MAC 주소로 switching.   
 L3 switch : IP 주소로 switching.(대표적인 Router)
 L4 switch : port 번호로 switching.  
 만약 Http의 어떤 기준을 가지고 switch를 한다면 L7 switch가 된다.  
 
 L2는 단순한 전기 신호를 가지고 구분하기 때문에 비용이 적게든다. 앞의 내용을 통해 알 수 있듯이 레이어가 높아질 수록 연산이 복잡해 지기 때문에 비싸다. 


### Network
internet은 Router와 DNS의 거대한 집합체이고
  
## #3. IPv4 주소체계   
**IPv4** : 32bit(2^32)   
**IPv6** : 128bit(2^128)  

IP주소는 8bit씩 끊어서 표시 000.000.000.000 로 표현  
IPv4 = 네트워크 id + 호스트 id 이다.  
여기서 네트워크 id를 부르는 명칭을 '넷 마스크'라고 부른다. 그러면 만약 내 ip의 서브넷 마스크가 24비트의 길이를 가진다면 000.000.000.000/24 라고 표현할 수 있다. 이를 CIDR이라는 용어로 표현한다.


## #4. Switch와 네트워크  
네트워크(인터넷)을 도로라고 생각해보자. 네트워크는 라우터의 집합이라고 이야기했다. 우리는 도로(네트워크)를 지날때 교차로를 만난다. 이 교차로를 네트워크에서는 switch라고 부른다. 네트워크는 라우터의 집합이라고 했고 위에서 라우터는 switch의 한 종류로서 L3Switch라고 부를 수 있다. 그렇다면 교차로는 Router다. 우리가 도로를 타는 이유는 목적지까지 향하기 위해서다. 도로에 이정표가 없다면 우리는 목적지까지 효율적으로 찾아갈 수 없다. 이러한 이정표를 네트워크에서는 Routing Table이라고한다. 이제 다시 정리를 해보자.  
  
우리는 도로(네트워크)를 통해 목적지(목적Host)까지 가려고 한다. 도로를 달리며 교차로(Router,L3Switch)를 만나게되는데 이정표(Routing table)을 보고 경로 선택(Swtiching)을 한다. 

## #5. 네트워크 데이터의 단위
Socket(Application, user mode, process)단위에서 Stream 데이터 단위를 이용해서 데이터를 전송한다. 이 Stream이라는 데이터 단위는 전송할 File I/O에 사용될 길이가 긴 데이터다.  
하지만 이 긴 데이터를 네트워크 영역에서 한번에 보내는 것은 불가능하다. Socket레벨에서 전송한 긴 Stream 데이터를 TCP영역으로 보낼때 데이터를 잘게 분해한다. 이를 우리는 Segment화 시킨다고하여 Segmentation이라고 부른다. 이 Stream을 어떤 기준으로 Segmentation해서 Segement로 만들까? 정해진 기준이 있다. MSS(Maximum Segment Size)를 기준으로 Segementation한다. 이제 이렇게 분해된 Segement를 IP 영역으로 보낼때 포장한다고 해서 Packet이라는 단위를 사용한다. 이 packet도 한계가 있고 그 한계를 MTU(Maximum Transfer Unit)라고 부른다. 이 MTU의 최대 크기는 1500byte이고 segment 하나는 MTU보다 작다. 그리고 이렇게 Segement들의 포장인 Packet을 네트워크로 보내기 위해 Frame으로 변환 시킨다.


## #6. 인터페이스 선택의 핵심원리  
Chrome에서 인터넷을 통해 데이터를 보내보자. 데이터는 각각의 레이어에 맞는 데이터로 변환되어 넘어갈 것이고 결국에는 출구(인터페이스: NIC)를 통해 인터넷의 바다로 나아가게 된다.  
만약 이 출구(인터페이스)가 두개라면 어떻게 될까? 컴퓨터는 누굴 선택할까? 답은 메트릭 값에 있다. 이 메트릭 값은 비용이라고 보면되는데 이 비용이 낮은 것을 기준으로 출구를 찾아 나간다.  
  
  

