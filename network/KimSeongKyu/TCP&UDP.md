# 1. TCP(Transmission Control Protocol)

## 1.1) 특징

- **Connection-oriented&reliable** - Receiver가 Data를 전달받으면 ACK를 보냄 (Positive ACK)
- in-order delivery
- Congestion Control
- Flow Control
- Connection Setup Delay

---

## 1.2) Sementation & Reassemble

- Segmentation - Sender는 Data를 보낼 때 MSS(Maximum Segment Size)에 맞춰 Data를 나눠서 보냄
- Reassemble - Receiver는 Data를 수신할 때 순서대로 Data를 합침

---

## 1.3) Sequence Number & Acknowledge Number

- Sequence Number - 현재 보낼 Segment의 Sequence에 대한 번호
- Acknowledge Number - 다음에 받아야 할 Segment의 Sequence Number를 상대방에게 알리기 위한 번호

---

## 1.4) RTT(Round Trip Time)

- Segment를 보내고 ACK를 받을 때 까지 걸리는 시간
- Network 환경에 따라 변함

---

## 1.5) Flow Control

- Receiver는 자신이 처리할 수 없을 때 전달받은 Segment를 위해 Buffer를 사용하여 처리
- Buffer의 크기는 유한하고 "Buffer에서 Segment를 처리하는 속도 < Sender에서 Segment를 보내는 속도"이기 때문에 overflow가 발생할 수 있음

    → Sender는 Receiver의 Buffer 상태를 알 수 없기 때문에 Receiver는 자신의 Buffer 상태(Window Size)를 Sender에게 알려줌

---

# 2. UDP(User Datagram Protocol)

## 2.1) 특징

- **Connectionless&Fast** - Sender와 Receiver간의 handshaking이 없음 (Data를 전송하기 전에 connection을 생성하지 않아도 되서 빠름)
- TCP에 비해 Header가 작아 더 많은 Data를 보낼 수 있음
- **Unreliable** - Sender에서 Data를 보내기만 하고 Receiver가 받았는지에 대해서는 신경쓰지 않음
- **Out-Of-Order** - Application Layer에서 추가적으로 re-order해야 함
- Network 사이에 traffic이 적음
