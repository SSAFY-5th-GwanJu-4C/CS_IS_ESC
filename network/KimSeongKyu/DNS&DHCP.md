# 1. DNS란?

- **D**omain **N**ame **S**ystem의 약자로, IP 주소를 Host Name으로, Host Name을 IP주소로 바꿔주는 CS Model의 Application Layer에서 사용하는 Protocol
- **UDP**의 53번 포트를 사용

    🙋‍♂️왜 TCP가 아닌 UDP?

    - 컴퓨터를 사용하는 모든 사람들이 사용하기 떄문에 Server가 TCP Connection을 감당할 수 없음 → UDP를 사용하여 서버에서 응답이 오지 않으면 재요청

---

# 2. DNS 구조

## 2.1) Overview

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/dns%EA%B5%AC%EC%A1%B0.png" width="500" height="300">


🙋‍♂️ 왜 Domain을 generic domain과 country domain으로 나눴지?

→ 처음에는 수요가 많을 줄 몰라서 generic domain만 만들어서 사용하다가 다른 나라에서도 수요가 생기면서 country domain이 생겨남

---

## 2.2) Root DNS Server

- DNS query가 오면 이에 대한 Response를 해주는 것이 아니라 해당 Query를 TLD Server로 전달만 해주는 Server

---

## 2.3) TLD(Top-Level_Domain) Server

- `.com, .org` 등 최상위 domain에 해당하는 Server로 각각의 domain마다 관리하는 회사가 다름

---

## 2.4) Authoritative DNS Server

- `yahoo, amazon` 등 organization에서 관리하는 domain Server

---

## 2.5) Local DNS Server

- 사용자의 DNS Query가 가장 먼저 접근하는 Server
- 공식적인 DNS Server구조에 포함되지 않음

---

## 2.6) 왜 구조를 나눠야 하지?

- **Single Point of Failure**: 하나의 DNS 서버를 사용할 경우, DNS에 문제가 생기면 인터넷 전체에 문제가 생김
- **Traffic Volume**: 하나의 DNS 서버가 모든 traffic을 전부 감당할 수 없음
- **Distant Centralized Database**: DNS 서버와 먼 host는 그만큼 지연시간이 길어짐
- **Maintanence**: 유지보수가 어려움, 유지보수를 위해 DNS 서버를 중지시킬 경우 인터넷 망 전체가 마비됨

---

# 3. Name Resolution

## 3.1) Recursive Query (Default)

- DNS 서버에서 Query에 해당하는 domain을 자신의 하위 Server에 물어보고 최종적으로 조회한  domain 정보를 상위 Server에 전달하는 방식

---

## 3.2) Iterative Query

- 자신의 하위 Server에 해당하는 domain 정보를 Local DNS Server에 전달하고 Local DNS Server가 하위 Server에 직접 요청하는 방식
- Local DNS Server에서 Name Resolution을 모두 처리해야 하기 때문에 부담이 큼

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/dns%20query.png" width="500" height="300">

---

# 4. DNS Caching

- 매번 Root, TLD, Authoritative DNS를 거치며 IP 주소를 조회하지 않기 위해 DNS Server에서 응답이 오면 host name과 IP 주소를 Mapping하여 Caching하여 사용하는 방식
- 일반적으로 TLD Server에서 Local DNS를 Caching하고 있음
- TTL(Time To Live) 시간동안만 Caching을 하고 시간이 지나면 Cache정보를 파기

    📢 host의 IP 주소가 바뀔 경우 TTL이 지나 새로 Caching을 하기 전까지 이를 알 수 없음

    → update/notify mechanism을 사용

---

# 5. DNS Resource Record

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/dns%20resource%20format.png" width="500" height="100">

## 5.1) Type A

- name: host name
- value: IP 주소

---

## 5.2) Type NS

- name: domain
- value: Authoritative DNS Server의 host name

---

## 5.3) Type CNAME

- name: 실제 host name의 alias name
- value: 실제 host name

ex) [www.ibm.com](http://www.ibm.com) → www.servereast.backup2.ibm.com

---

# 6. DHCP(Dynamic Host Configuration Protocol)

## 6.1) 사전 개념

### 6.1.1) Broadcast

- IP 통신에 사용되는 방식 중 하나로 Unicast, Broadcast, Multicast 방식이 있음
- **Unicast:** 두 IP간의 1대1 양방향 통신
- **Broadcast:** 같은 Subnet에 해당하는 모든 host에게 전송하는 1대 다 단방향 통신
    - **Router를 넘어갈 수 없음**
    - Subnet이란?
        - 같은 Router를 사용하여 동일한 네트워크 주소(Network Part)를 갖는 host Group
            - IPv4의 IP주소: Network Part + Host Part

                <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/IPv4%20class.png" width="500" height="300">
                
        ex) host의 IP 주소가 223.1.2.3, 223.1.2.4, 223.1.3.5, 223.1.3.6일 경우 `223.1.2.3, 223.1.2.4` 가 하나의 Subnet을 구성하고 `223.1.3.5, 223.1.3.6`이 하나의 Subnet을 구성

- **Multicast:** 같은 Multicast Group 내의 모든 host에게 전송하는 1대 다 단방향 통신
    - 같은 Multicast Group 내의 host들은 같은 Multicast 주소를 가짐
    - **Router를 넘어갈 수 있음**
    - Router가 Multicast를 지원하기 위해서는 부가적인 cost가 많아 일반적인 인터넷에서는 지원하지 않음 (인터넷 방송 스트리밍에 주로 사용)

---

## 6.2) DHCP란?

- 모든 IP 주소가 고정적이면 사용할 수 있는 IP주소가 한정적이기 때문에 host가 네트워크에 접속하면 동적으로 IP 주소를 할당하는 방식
- UDP를 사용하여 connectionless함
    - 67번 포트: DHCP Server
    - 68번 포트: Client

---

## 6.3) DHCP Operation

1. **DHCP Discover**: DHCP Server를 찾기 위해 host가 broadcast방식으로 보내는 메시지
    - DHCP Server는 여러개가 존재할 수 있음
    - broadcast방식을 사용하는 이유?
        - Server에 대한 IP 주소를 모르기 떄문
    - broadcast 방식은 Router를 넘어갈 수 없댔는데?
        - **DHCP에 한해서 Router를 넘어갈 수 있음**
2. **DHCP Offer**: DHCP Discover를 받은 DHCP Server가 사용 가능한 IP주소를 host에게 전달하는 메시지
3. **DHCP Request**: DHCP Offer가 여러개의 Server에서 올 수 있기 때문에 그 중 하나를 사용하겠다고 DHCP Server에 알리는 메시지
4. **DCHP Ack**: host가 IP 주소를 사용하는 것을 허락하는 메시지

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/DHCP.png" width="700" height="400">

---
