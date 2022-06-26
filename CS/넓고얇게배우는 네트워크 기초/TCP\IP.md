## #1. TCP 송수신 원리  
상황 : 클라이언트가 웹서버에 파일을 하나 요청했고 서버는 해당 요청에 대한 파일(1.4MB 크기의 파일)을 클라이언트에게 송신하는 과정  
**서버의 상황**   

![IMG_0086](https://user-images.githubusercontent.com/78134917/175818021-75489b6e-6abc-48c9-ad15-3601c2984d9a.jpg)  

클라이언트에서 파일을 요청하면 서버는 파일을 송신해야한다. 위의 그림은 그 과정을 보여준다.  
위의 그림에서 자세히 보아야하는 것은 Buffer#1에 file이 어떻게 HDD로 부터 reading되어 쌓이는가 와 Socket(TCP를 추상화한 파일)에 wirte된 것을 TCP에서 Buffer#2에 Read하여 복사하는 과정이다.   
  
많은 데이터를 읽어올 경우 디스크로부터 64kb씩 읽어오게되고 application에 Buffer 메모리에 적재한다. 64kb가 적재되면 Socket에 이 데이터를 써넣고 이 긴 데이터를 TCP에서 Read해 간뒤 Buffer IO를 통해 똑같이 복사한다. 복사한 데이터는 IP로 넘어가기 전 Segmentation(분해)과정을 거쳐서 하나의 Segment단위(하나의 데이터가 쪼개짐으로 각 Segment마다 번호가 붙는다.)로 header가 씌워지면서 IP의 데이터 단위인 packet으로 포장된다. 포장된 Packet은 인터넷이라는 넓은 바다로 가기위해 배에 실린다. 요청한 클라이언트에게 향하게되고 다음 그림과 같은 과정을 거쳐 클라이언트에게 전달된다.  
  
![IMG_403878A58800-1](https://user-images.githubusercontent.com/78134917/175822803-6ada9db1-8525-40f1-a518-4883163b2aa6.jpeg)
  
  자 이제 그림을 보면 어느정도 알 수 있다. 가장 중요한 부분은 수신하는 곳에서 생길 수 있는 window size 부족으로 인해 웹 서버에서 추가적인 packet을 발송하지 않게되는 장애에 대한 이야기이다.  
  그림에서와 같이 어느 정도 segment가 쌓이면 클라이언트 서버 TCP 레이어에서는 ACK#3을 window size와 함께 보낸다. 이 사이즈를 mss와 체크 후 충분한 여유가 있을 때 추가적인 정보를 보낼 수 있다. 하지만 이 size가 부족하다면 송신 서버는 더 이상 segment를 보내지 않고 기다린다.  
이러한 이유로 클라이언트가 속도 측면의 클레임을 낼때는 수신 받는 서버의 TCP Buffer에 쌓이는 Segment들을 Process가 빠르게 읽어서 Read 후 Recieve 하는지 체크해야 한다. 결국 송신 받아 TCP Buffer에 쌓이는 속도보다 Process가 TCP Buffer를 읽어 자신의 Buffer에 쌓는 속도가 더 빨라야 한다.  
