# 1. Load Balancer란?

## 1.1) Server의 성능 향상

### 1.1.1) Scale-Up

- Server의 하드웨어 성능 자체를 향상시키는 방법
- 하드웨어의 성능을 끌어올리는데는 많은 비용이 듬

---

### 1.1.2) Scale-Out

- Server를 여러 대 두어 Traffic을 분산시키는 방법

    🙋‍♂️ 여러 대의 Server를 두었지만 특정 Server로 Traffic이 몰리게 되면 어떻게 하지?

    **→ 이러한 문제를 해결해 주기 위한 것이 Load Balancer**

    📢 Load Balancer란 Traffic을 여러개의 Server로 분산하는 과정에서 균등하게 분산되도록 Traffic을 관리해주는 기술

---

# 2. Load Balancer의 종류

- OSI-7 Layer를 기준으로 어떤 Layer에서 Load Balancing을 하느냐에 따라 다음과 같이 나뉨

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/Load%20Balancer.png" width="800" height="200">

- 상위 계층으로 갈수록 섬세한 분산이 가능하지만 가격이 비싸짐
- 하위 계층으로 갈수록 단순한 분산이 가능하지만 가격이 저렴함

---

# 3. Load Balancer의 기능

## 3.1) NAT(Network Address Translation)

- Private IP 주소와 Public IP 주소를 서로간에 변경해주는 기술

    🙋‍♂️ 왜 Private IP 주소를 Public IP 주소로 바꿔서 사용해야 하지? Private IP 주소를 그냥 사용하면 안되는 건가?

    📢 Private IP 주소는 Router가 주소를 할당하기 때문에 **여러 Router 내에서 같은 Private IP 주소를 가질 수 있으며, 외부 접근이 불가능** 하기 때문에 Public IP 주소로 바꿔서 사용해야 함

- SNAT(Source NAT): Private IP → Public IP
- DNAT(Destination NAT): Public IP → Private IP

---

### 3.1.1) NAT in Load Balancer

- 접속을 시도하는 Client의 TCP/IP Header를 수정하여 Server로 고르게 분산하여 Packet을 전달한 뒤, Server에서 Load Balancer로 돌아오는 Packet을 다시 수정하여 Request를 요청한 Client로 Traffic을 되돌려 줌
- 보통 L2 Network에서 사용하여 VLAN 범주 이내에서 사용하나, IP의 Header를 수정하여 Load Balancing을 하므로 GNAT(Gloval NAT)이라 하여 VLAN 밖으로 Packet을 전송할수도 있음
- 일반적으로 Server에 어떠한 세팅을 하지 않고도 이용이 가능 하다는 장점으로 인해 **가장 많이 사용하는 Load Balancing 방식**
- 단점
    - TCP/IP Connection Session을 Load Balancer가 계속 유지해야 하고, Server에서 응답한 Traffic이 Load Balancer로 다시 온 뒤 DNAT 이후 Client로 돌려 보내야 하는 점으로 인하여, Load Balancer에 부하가 가중됨
    - Data Center를 넘나 들어 Load Balancing할 경우 Traffic 비용이 이중으로 발생

---

## 3.2) IP Tunneling

- 가상의 Tunnel을 구축한 상호 간에 Data를 Encapsulation하여 전달하고 Decapsulation하여 수신하는 통신 기술

---

### 3.2.1) IP Tunneling in Load Balancer

- Client의 Request Packet이 Load Balancer로 전달 되면 Load Balancer에서 Packet의 목적지 주소와 Port를 검사하여 가상 서버 정책과 일치하면 Scheduling에 따라 Server로 전달

    → 이때 Packet을 일반적인 Stream 방식으로 전달하는 것이 아닌 Encapsulation하여 전달

    → Server에서는 Packet을 Decapsulation하고 요청을 처리한 다음 Server의 Routing Table에 따라 **사용자에게 직접 결과를 돌려주는 방식**

- 장점
    - VLAN 밖으로의 패킷 전달 가능(다른 국가간 Load Balancin 가능)
    - NAT 방식의 문제인 전송된 Packet이 DNAT을 위해 Load Balancer로 회귀하여 Load Balancer의 부하를 가중 시키고, 데이타 센터로 회귀함으로써 발생하는 Traffic의 이중 과금이 발생하지 않음
- 단점
    - Server가 IP Tunneling 전송 규약을 지원해야 하기에 지원하지 못하는 OS에서 문제발생(Server가 WINDOW 운영 체제인 경우는 지원 하지 않음)
    - 다른 국가의 Data Center를 Load Balancing을 할 경우, 대부분 통신 사업자들이 타 회사 IP에 망 관리 상 배타적이기 때문에 망 관리 혹은 Data Center 차원에서 보안 정책과 Traffic 관리 정책 상 Load Balander의 IP가 자신들의 IP가 아닌 경우 **Packet이 DROP 되는 경우가 대부분**
- Cloud Service에서 L7 Switch가 다른 방식에 비해 퍼포먼스가 상대적으로 떨어지는 경우, 상대적으로 L7에 비하여 부하에 강하여 Data Center 내에서 이 방식으로 Load Balancing을 많이 이용

---

## 3.3) DSR(Direct Server Return)

- Server에서 Client로 Traffic을 전달할 때 목적지 주소를 Switch의 IP 주소가 아닌 Client의 IP 주소로 설정하여 Traffic을 Network Switch를 거치지 않고 바로 Client로 전달하는 기술

---

### 3.3.1) L2DSR vs L3DSR

- L2DSR은 L2 Layer의 Header인 MAC 주소를 변경하여 Client에게 직접적으로 Traffic을 전달하고 L3DSR은 L3 Layer의 Header인 IP 주소를 변경하여 Client에게 직접적으로 Traffic을 전달

    → L2DSR은 동일한 MAC 주소 변경을 위해 동일한 Broadcasting domain 내에 있어야 하기 때문에 물리적인 한계점을 가짐

    →  L3DSR은 IP 주소 변경을 통해 Client로 전달되기 때문에 물리적인 한계 극복

- L3DSR은 IP Header에 따라 IP Tunneling기반과 TOS(DSCP)기반으로 구분(카카오에서 DSCP기반의 L3SDR 방식 사용)

---

# 4. Load Balancing Algorithm

## 4.1) Round Robin

- Client의 요청을 모든 Server에 순서대로 Load Balancing
- 대상 Server의 성능이 동일하고 처리 시간이 짧은 Application의 경우 적합함
- 단순하지만 Server의 부하 등을 고려하지 않기 때문에 효과적이지 못함

---

## 4.2) Weighted Round Robin

- Server마다 가중치를 두어 Client의 요청을 Round Robin 방식으로 처리하는 Load Balancing
- 부하가 많아질 경우 특정 Server에 요청이 편향되는 문제가 발생

---

## 4.3) IP Hash

- Client의 IP 주소를 특정 Server에 Mapping하여 Load Balancing
- 사용자가 항상 동일한 Server로 연결되는 것이 보장됨
- Server의 성능 및 상황에 따라 사용자마다 응답 시간이 달라질 수 있음

---

## 4.4) Least Connection

- 가장 접속이 적은 Server로 Load Balancing
- 비슷한 성능의 Server로 구성될 경우 요청이 많은 경우에도 효과적으로 분산하여 가장 빠른 Server에서 처리할 수 있음

---

## 4.5) Least Response Time

- Server가 요청에 응답하는 시간이 가장 짧은 Server로 Load Balancing

---

References
[https://idchowto.com/?p=18988](https://idchowto.com/?p=18988)
[http://wiki.hash.kr/index.php/로드밸런싱](http://wiki.hash.kr/index.php/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1)
