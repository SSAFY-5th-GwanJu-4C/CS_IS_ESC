# 1. Protocol

## 1.1) 정의

- 두 End System간에 정보를 원활하게 주고 받기 위해 정의한 규칙
    - **What, How, When** it is communicated

---

## 1.2) Key Elements

### 1.2.1) Syntax

- Data format
- Encoding/Decoding information
- Signal Level - 사용에 따라 high voltage를 1로 표현할지 0으로 표현할 지 규격을 맞춰야 함

---

### 1.2.2) Semantics

- Response - Protocol 내의 data가 원하는 대로 response해야 함
- Control Information - 제어하는 data를 통해 부가적인 동작이 이루어 지게 함
- Error Handling

---

### 1.2.3) Timing

- Speed Matching
- Sequence

---

# 2. Layer

## 2.1) Protocol & Interface

- Protocol - 같은 Layer끼리 통신을 하기 위한 수단
    - 각 Layer마다 서로 다른 Protocol을 사용
- Interface - 다른 Layer끼리 통신을 하기 위한 수단
    - Layer간의 독립성을 위해 바로 옆 Layer를 건너 뛰고 다른 Layer와 통신하지 않음

---

## 2.2) Layer Architecture의 장점

- **독립성**이 보장되기 때문에 Protocol을 디자인할 때 다른 layer를 신경쓰지 않아도 됨
- 한 layer를 수정하더라도 다른 layer에 영향을 주지 않음

---

## 2.3) Layer Architecture 사용 조건

- 두 entity가 같은 layer에서 통신해야 함
- 두 entity가 같은 protocol을 사용해야 함
- 상/하위 layer간에 동일한 interface를 사용해야 함

---

## 2.4) PDU & SDU

- SDU (Service Data Unit) - 이전 Layer에서 전달받은 Data
- PDU (Protocol Data Unit) - 다음 Layer에 전달하고자 하는 Data
    - Application Layer - Data
    - Transport Layer - Segment
    - Netowkr(IP) Layer - Datagram(Packet)
    - Datalink Layer - Frame
- 각각의 Layer는 이전 Layer에서 전달받은 SDU에 자신의 Header를 추가한 PDU를 다음 Layer에 전달함

---

# 3. Application Layer

## 3.1) 역할

- 사용자가 네트워크에 접근할 수 있는 최초의 Interface를 제공하는 Layer
- 추상적인 개념을 data로 변환해 줌
- 사용자가 네트워크에 대해 알지 못해도 접근할 수 있도록 해줌

---

## 3.2) Process / API / Socket

### 3.2.1) Process

- 사용자가 사용하고 있는 Application Program

---

### 3.2.2) API

- Process와 Kernel(OS)간의 Application Programming Interface
- Process가 Kernel에 직접 접근하면 Kernel 모든 Process를 직접 관리하기 때문에 일관성이 떨어짐

---

### 3.2.3) Socket

- Process와 Network Protocol간의 Network Programming Interface
- API와 마찬가지로 Kernel의 일관성을 위해 Socket을 통해 접근

---

## 3.3) Client-Server vs Peer-To-Peer

### 3.3.1) Server

- Clinet가 언제 접근할 지 모르기 때문에 항상 켜져 있어야 함
- 고정적 IP Address
- 많은 Client가 하나의 Server에 접근하면 부하가 걸리기 때문에 data center를 구축
- 모든 data는 Server에서 관리함 - **정보의 비대칭성**

---

### 3.3.2) Client

- 다양한 IP Address - 아무때나, 어느 곳에서나 Server에 접근할 수 있어야 함
- Client끼리 직접적인 통신이 이루어질 수 없음 (Server를 거쳐야 함)

---

### 3.3.3) Peer-To-Peer

- 모든 Peer가 server도 될 수 있고 client도 될 수 있음
- 항상 켜져있을 필요가 없음
- 다양한 IP Address
- peer 간의 직접적인 통신이 가능(data를 request할 때는 client, data를 response할 때는 server)
- 관리가 어려움

---

# 4. Transport Layer

## 4.1) 역할

- 두 End Point 사이에서 실행되는 Application Program간의 Logical Communication을 담당
- Send side - Application Layer에서 전달받은 message에 Segment를 더해 Network Layer에 전달
- Receive side - Network Layer에서 전달받은 segment에서 Header를 제거한 message를 Application Layer에 전달

---

## 4.2) Multiplexing & Demultiplexing

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd36b3b1-9afa-4892-84fa-11b354eebbab/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd36b3b1-9afa-4892-84fa-11b354eebbab/Untitled.png)

### 4.2.1) Multiplexing

- Source End System에서 발생
- Application Layer에서 Socket을 통해 전달받은 data에 포트 번호를 부여하여 Network Layer에 전달하는 과정

---

### 4.2.2) Demultiplexing

- Destination End System에서 발생
- Network Layer로부터 전달 받은 segment의 포트 번호를 확인하여 해당 포트번호를 사용하는 Application으로 data를 전달하는 과정

---

### 4.2.3) Port Number

- 각각의 Process가 Socket을 생성하면 Socket마다 서로 다른 Source와 Destination 포트 번호가 부여됨
- TCP와 UDP 모두 32bits (Source - 16bits, Destination - 16bits)가 할당됨
- Source와 Destination의 포트 번호는 역순
- IANA(Internet Assigned Number Authority)에서 정의한 규격에 따라 포트 번호 부여
    - 0~1023: Well known ports - IANA가 부여
    - 1024~49151: Public use ports - IANA가 부여
    - 49152~65535: Private use ports - Kernel에서 random하게 부여

---

# 5. Network(IP) Layer

## 5.1) 역할

- Data를 잘게 자른 packet을 Network를 경유하여 Source End System에서 Destination End System으로 전달

---

## 5.2) Routing

- Router에 도착한 Packet을 Destination에 전달하기 위한 경로를 찾기 위한 과정
- Routing Protocol이 동작하여 주변의 Router에서 정보를 수집하고, 이를 기반으로 Routing/Forwarding Table을 만듬
- Routing 뿐만 아니라 Network 장비를 위한 관리도 필요

---

## 5.3) Forwarding

- Packet이 Router에 도착했을 때 Destination IP Address를 추출하여 Routing을 통해 생성된 Forward Table에 기반하여 Output Port로 Packet을 전달하는 과정

---

## 5.4) Router Architecture

### 5.4.1) Control Plane

- Software 기능을 담당 (Routing, Management)
    - Routing Processor: Routing과 관련된 동작을 관리하는 역할

---

### 5.4.2) Data Plane

- Hardware 기능을 담당 (Forwarding)
    - Input Port: 다른 Router에서 들어오는 Packet을 받아들이는 Port
    - Output Port: Forwarding을 통해 다른 Router로 Packet을 보내는 Port
    - Switching Fabric: Input Port로 들어온 Packet을 Output Port로 교환해주는 역할

---

# 6. Datalink Layer

## 6.1) 용어

- Nodes - Hosts & Routers
- Links - Node간에 제공되는 경로 (wired/wireless)

---

## 6.2) 역할

- Physical Layer로 연결되어 있는 두 Node간의 frame 전달
- 경로 상의 linke마다 각각의 Datalink Layer가 만들어짐
- Noise 등으로 인해 발생한 Error Detection & Correction
- Flow Control: Receive Buffer에서 Overflow가 발생하지 않게 Node간의 Flow 조절

---

## 6.3) Error Detection & Correction

- Error Detection의 경우 무선 네트워크에 적합하지 않음

    → 무선에서는 데이터를 보낼 때 손실이 자주 발생하는데, Error Detection을 통해 Error를 발견하여 재전송을 할 경우에도 데이터 손실이 발생할 수 있음

---

### 6.3.1) Parity Check - Error Detection

- Single Bit Parity: Even/Odd Parity Check에 맞추어 frame 내 1의 개수를 홀수 혹은 짝수로 맞춤

    → Error의 개수가 짝수일 경우 발견할 수 없음

    → 전화와 같이 Error가 잘 발생하지 않는 곳에 사용

- Two Dimentional Parity: Single Bit Parity를 2차원으로 적용하여 Even/Odd Parity Check를 하면 각 bit의 Error를 찾을 수 있음

---

### 6.3.2) CRC(Cyclic Redundancy Check) - Error Detection

- 오늘날 Computer Network에 많이 사용되는 Error Detection
- 기존의 data에 CRC 혹은 FCS(Frame Check Seuquence) bits를 추가
- D와 G가 주어지고 XOR연산을 사용하여 R 계산

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1cfb045f-9e89-4db3-b236-b9cbb9cb1f24/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1cfb045f-9e89-4db3-b236-b9cbb9cb1f24/Untitled.png)

---

### 6.3.3) Error Correction

- Error Detection을 통해 Error가 발생하면 이를 수정
