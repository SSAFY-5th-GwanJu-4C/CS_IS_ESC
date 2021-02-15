# TCP / UDP
><br>
>
> 전송계층에서 사용되는 프로토콜인 TCP와 UDP에 대해 알아보자.
><br>


#### Reference
- [[10분 테코톡] 르윈의 TCP UDP](https://youtu.be/ikDVGYp5dhg)
- [후니의 쉽게 쓴 CISCO 네트워크](http://www.yes24.com/Product/Goods/89520426)
- [hidaehyunlee velog, TCP와 UDP 차이를 자세히 알아보자](https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4)


### Transport Layer
- TCP와 UDP는 Transport Layer에서 사용되는 프로토콜이다.
- Transport Layer, 전송 계층은 IP에 의해 전달되는 패킷의 오류를 검사하고 재전송 요구 등의 제어를 담당하는 계층이다.


### 만약 Transport Layer(전송 계층)이 없다면?
- 데이터의 순차 전송이 원활히 진행되지 않는다
    - 송신자가 전송한 데이터 : | 3 | 2 | 1 |
    - 수신자가 받은 데이터  : | 2 | 3 | 1 |
- Flow(흐름 문제)
    - 원인: 송수신자 간의 데이터 처리 속도 차이
    - 수신자의 컴퓨터가 처리할 수 있는 데이터량을 초과
- Congestion(혼잡문제)
    - 원인 : 네트워크의 데이터 처리 속도(ex.라우터)
    - 네트워크가 혼잡할 경우

- 따라서, 전송계층이 없다면 결과적으로 데이터의 손실이 발생한다.

|송신자|수신자|
|------|------|
|`Hello Nice to meet you`|`Hell_______to___you`|


이처럼 데이터의 손실 발생은 중요한 문제이므로 사람들은 고민을 했다.
그래서 나온 개념이 바로 `TCP`이다.


### TCP(Transmission Control Protocol)
- 신뢰성있는 데이터 통신을 가능하게 해주는 프로토콜
- 특징 : Connection 연결(3 way-handshake) - 양방향 통신
- 데이터의 순차 전송을 보장
- Flow Control(흐름 제어)
- Congestion Control(혼잡 제어)
- Error Detection(오류 감지)

### 세그먼트 (Segment) - TCP 프로토콜의 PDU
> __세그먼트란?__
> IP 프로토콜의 패킷처럼 프로토콜 안에서 데이터가 처리되고 움직이는 단위

### Header 처리 과정
1. 어플리케이션 단에서 내려온 데이터를 TCP 프로토콜에서 내부적으로 자른다.
2. 이 자른 데이터들에 TCP Header라는걸 추가한다.
3. 이 데이터에 TCP Header가 더해진 단위를 세그먼트라고 한다.
4. 이것을 가지고 프로토콜 안에서 처리하고 이동한다.

### TCP Header
- SYN은 커넥션을 연결을 제어하는 flag bit
- FIN은 커넥션을 연결을 햇을 때 끊는 flag bit
- ACK는 데이터를 받는 사람이 다시 전송할때 쓰는 flag bit


### TCP Connection & Disconnection
<p align="center"><img src="./img/TCP_UDP/TCP_connection_disconnection.png" width = "800px"></p><br>

### TCP Connection (3-way handshake)
1. 먼저 open()을 실행한 클라이언트가 SYN을 보내고 SYN_SENT 상태로 대기한다.
2. 서버는 SYN_RCVD 상태로 바꾸고 SYN과 응답 ACK를 보낸다.
3. SYN과 응답 ACK을 받은 클라이언트는 ESTABLISHED 상태로 변경하고 서버에게 응답 ACK를 보낸다.
4. 응답 ACK를 받은 서버는 ESTABLISHED 상태로 변경한다.

### TCP Disconnection (4-way handshake)
1. 먼저 close()를 실행한 클라이언트가 FIN을 보내고 `FIN_WAIT1` 상태로 대기한다.
2. 서버는 `CLOSE_WAIT`으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어 있는 어플리케이션에게 close()를 요청한다.
3. ACK를 받은 클라이언트는 상태를 `FIN_WAIT2`로 변경한다.
4. close() 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트에 보내 `LAST_ACK` 상태로 바꾼다.
5. FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 `TIME_WAIT`으로 상태를 바꾼다. `TIME_WAIT`에서 일정 시간이 지나면 `CLOSED`된다. ACK를 받은 서버도 포트를 `CLOSED`로 닫는다.

>__주의__
>- 반드시 서버만 CLOSE_WAIT 상태를 갖는 것은 아니다.
>- 서버가 먼저 종료하겠다고 FIN을 보낼 수 있고, 이런 경우 서버가 FIN_WAIT1 상태가 된다.
>- 누가 먼저 close를 요청하느냐에 따라 상태가 달라질 수 있다.


### TCP의 문제점
- 매번 Connection을 연결해서 시간 손실 발생(3-way handshake / 4-way handshake)
- 패킷을 조금만 손실해도 재전송해야한다.

위와 같은 문제점으로, TCP 빠르게 데이터를 전송해야하는 서비스에서 사용하기 부적절하다. 이로 인해 고안된 프로토콜이 `UDP`이다.

### UDP(User Datagram Protocol)
- TCP보다 신뢰성이 떨어지지만 전송 속도가 일반적으로 빠른 프로토콜
(순차 전송x, 흐름제어x, 혼잡 제어 x)
- Connectionless(3 way-handshake X)
- Error Detection
- 비교적 데이터의 신뢰성이 중요하지 않을 때 사용(ex. 영상 스트리밍)

### Header 처리 과정
- data에 UDP Header를 붙여 User Datagram으로 동작
- TCP의 세그먼트와의 차이점은 data를 쪼개지 않음.
- 직접 어플리케이션 단에서 쪼개야함.

### UDP Header
- 포트번호
- 에러 검출을 위한 checksum(16bit)

### UDP 데이터 전송방법
- Connection이 없으니 확인 안하고 무조건 보낸다.
- 서버는 UDP관련 소켓을 열어두고 무조건 받는다.
- 오류 검출이 없다보니 데이터가 잘 안올 수 있다.

### 표로 비교한 TCP와 UDP
|TCP|UDP|
|---|---|
|연결지향형 프로토콜| 비 연결지향형 프로토콜|
|바이트 스트림을 통한 연결|메세지 스트림을 통한 연결|
|혼잡제어와 흐름제어 지원|혼잡제어와 흐름제어 지원X|
|순서보장|순서보장X|
|상대적으로 느림|상대적으로 빠름
|신뢰성있는 데이터 전송|데이터 전송 보장X|
|Segment 패킷| Datagram 패킷|
|HTTP, Email, File 전송에서 사용| DNS, Broadcasting 등 도메인, 실시간 동영상 서비스에서 사용|


### 결론
- 신뢰성이 요구되는 애플리케이션에서는 `TCP`를 사용하고 간단한 데이터를 빠른 속도로 전송하고자 하는 애플리케이션에서는 `UDP`를 사용한다.
- 원하는 서비스를 구현하고자 할 때,TCP와 UDP 헤더 분석을 통해 성능 향상을 이루도록 한다.