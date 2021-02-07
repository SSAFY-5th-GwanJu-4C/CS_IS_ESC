# 1. HTTP란?

- **H**yper **T**ext **T**ransfer **P**rotocol의 약자로, Application Layer의 Web에서 사용하는 프로토콜

---

# 2. HTTP전송 과정

1. Transport Layer에서 TCP를 통해 Sender와 Receiver간의 Connection을 맺음
2. **Application Layer에서 HTTP를 사용해 데이터 전송**
3. Transport Layer에서 Connection 종료

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/http%20%EC%A0%84%EC%86%A1%EA%B3%BC%EC%A0%95.png" width="300" height="300">

---

# 3. Non-Persistent vs Persistent

## 3.1) Non-Persistent HTTP

- HTTP/1.0 버전에서 사용되던 기술
- Object를 전송하는 동안 Connection을 유지하지 않고 전송할 때 마다 TCP Connection을 연결

    → Object를 전송할 때 마다 TCP Connection을 맺고 끊어야 해서 OS에 overhead가 큼

---

## 3.2) Persistent HTTP

- HTTP/1.1, 2.0버전에서 사용되는 기술
- Object를 전송하는 동안 Connection을 유지하고 모든 Object를 전송한 후 TCP Connection 종료
    - 장점: CPU&Memory 사용이 적음,
    - 단점: 모든 Object 전송 후 Client가 TCP Connection을 종료할 수 있기 때문에 Client가 Connection 종료를 요청하기 전까지 다른 Client가 Server를 사용할 수 없음

---

# 4. HTTP Message Format

## 4.1) HTTP Request

- Client가 Server에 전달하는 HTTP Format

---

### 4.1.1) Methods

- **GET**: Server에 데이터를 요청
- **POST**: Server에 데이터를 전송
- **PUT**: Server에 수정할 데이터의 모든 내용을 전송
- **PATCH**: Server에 수정할 데이터의 일부를 전송
- **DELETE**: Server에 삭제할 데이터를 전송

---

## 4.2_ HTTP Response

- Server가 Client에 전달하는 HTTP Format

---

### 4.2.2) Status Codes

- **1XX**: HTTP Request를 받았고, 계속해서 진행
- **2XX**: HTTP Request가 성공적으로 전달되었음
- **3XX**: HTTP Request를 완료하기 위해 Redirection이 필요함
- **4XX**: Client Error
- **5XX**: Server Error

---

# 5. Web Brower의 응답속도를 높이는 3가지 HTTP 기능

## 5.1) HTTP Cookie

- HTTP는 Client가 Server에 Request를 하더라도 이를 기억하고 있지 않아서 같은 HTTP Request가 오면 Server는 매번 같은 Response를 보냄

    → 사용자가 website를 사용하는 동안 website로부터 받은 **작은 데이터 조각**을 사용자의 Web Browser에 저장하고 나중에 동일한 HTTP Request가 오면 사용자의 Web Browser에 저장된 데이터를 사용

- 사용 예시: 인증, 장바구니, 추천 등

📢 비밀번호, 신용카드 번호 등의 민감한 정보는 암호화 하여 저장

---

### 5.1.1) Cookie Mechanism

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/cookie%20mechanism.png" width="500" height="300">

---

## 5.2) Conditional GET (Browser Cache)

- Cookie와 마찬가지로 Web Page의 렌더링을 할 때도 매번 같은 HTTP Request를 보내야 함

    → Cache: Web Page의 렌더링 요소들을 임시 저장하여 이를 사용

    🙋‍♂️ Cache 정보가 업데이트되면 어떻게 하죠?

    → Client가 Server에 HTTP Request를 할 때 Cache가 최신 버전인지를 물어보고 최신버전이라면 body에 아무 내용 없이 `304 Not Modified` Response를 보내고 Cache가 업데이트 되었으면 일반 HTTP Response를 보냄

---

## 5.2.1) Cookie vs Cache

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/cookie%20vs%20cache.png" width="500" height="300">

---

## 5.3) Proxy Server (Web Cache)

- 많은 Client가 사용하는 Server의 경우 Request가 많아 Response의 속도가 느림

    → 어떤 Client가 보낸 Request를 기존 Server가 Response하였을 때 다른 Client가 동일한 Request를 보낸다면 기존 Server를 거치지 않고 Proxy Server에서 Response해줌

    - Proxy Server가 Client입장에서는 Server이지만 기존 Server입장에서는 Client가 됨

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/proxy%20server.png" width="400" height="300">

---

# 6. HTTPS

- Hyper Text Transfer Protocol Secure의 약자로 **보안이 강화된** HTTP
- 일반 HTTP는 80포트를 사용하지만 HTTPS는 443포트를 사용

---

## 6.1) Why HTTPS?

- HTTP 메세지는 ASCII 코드로 작성되고 암호화가 되어있지 않기 때문에 보안에 취약하다는 문제점을 가짐

    📢 Application Layer → Transport Layer로 전달되는 과정에서 Header를 제외하면 사용자의 정보를 알 수 있음

    → Application Layer와 Transport Layer 사이에 Sub Layer(TLS/SSL)을 두어 암호화 진행

    - TLS - Transport Layer Security
    - SSL - Secure Sockets Layer

        (SSL은 과거의 통신 보안을 위한 암호 규약이며 표준화가 되면서 TLS라는 용어로 바뀜)

---
