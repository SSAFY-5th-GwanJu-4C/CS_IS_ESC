# 1. Process란?

- Program: 디스크에 저장된 실행 가능한 파일 (정적인 개념)
- Process: 실행되어 Memory에 올라간 Program (동적인 개념)

---

# 2. Process 상태

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/process%EC%83%81%ED%83%9C.png" width="600" height="300">

---

# 3. Process Scheduler

- Short-term scheduler (CPU scheduler) - 다음에 실행할 process를 선택하고 CPU에 할당하는 scheduler
    - ms주기로 호출되기 때문에 빠름
- Long-term scheduler (job scheduler) - ready queue에 올릴 process를 선택하는 scheduler
    - Multi-programming의 정도를 조절하기 위한 scheduler
    - 몇 초, 분단위로 불규칙적으로 호출되기 때문에 비교적 느림
- Mid-term scheduler - Multi-programming의 정도를 감소시키기 위한 scheduler
    - Process를 memory에서 삭제하고 disk에 저장한 후, 나중에 disk에서 가져와 process를 계속 실행

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/mid_term_scheduler.png" width="600" height="200">
    
---

# 4. Context Switch

- 실행중인 process를 다른 process로 변경하기 위해 현재 process의 상태를 저장하고, 다른 process의 상태를 불러오는 과정
- process의 context는 PCB에 저장
- Context switch를 하는 동안에 시스템은 아무것도 할 수 없기 때문에 overhead가 굉장히 큼
    - OS와 PCB가 복잡할수록 context switch가 오래 걸림
    - hardware의 영향을 받음
        - CPU의 register가 많은 hardware일수록 한번에 많은 context를 불러올 수 있기 때문에 context switch 속도가 빠름

---

# 5. Process Operations

## 5.1) Process Creation

- 부모 process가 자식 process를 생성하는 것으로, pid(process-id)를 통해 관리

---

### 5.1.1) Resource Sharing Option

- 부모와 자식 간에 모든 자원을 공유
- 자식이 부모의 일부 자원을 공유
- 부모와 자식 간의 자원 공유 x

---

### 5.1.2) Excution Option

- 부모와 자식이 concurrent하게 동작
- 자식이 종료될 때 까지 부모가 기다림

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/process_operations.png" width="600" height="200">

---

## 5.2) Process Termination

- 작업을 마친 process는 `exit()`을 통해 OS에 종료를 요청
- status data를 return함 (부모 process에서 `wait()`의 parameter로 사용)
- `abort()`를 통해 부모 process에서 강제로 자식 process를 종료시킬 수 있음
    - 🙋‍♂️ 왜 자식 process에서 종료를 하지 않고 부모 process에서 강제 종료 시키나요?
    - 📢 더 이상 자식 process가 할 일이 없는 경우 / 자식이 할당받은 자원을 초과한 경우 / 부모 process를 종료하기 위해 자식 process를 강제 종료해야 하는 경우

---

### 5.2.1) Zombie & Orphan Process

- 부모 process는 자식 process가 종료되면 `wait()`를 통해 자식의 자원을 회수해야 함
    - 자식 process가 종료되었는데 부모 process에서 `wait()`을 호출하지 않아 자원을 반납하지 못하는 자식 process를 **zombie process**라고 함
- 부모 process가 종료되기 이전에 자식 process가 종료되어야 함
    - 부모 process가 종료되기 이전에 `wait()`을 호출하지 않아 자식 process가 끝나기를 기다리지 않은 경우 자식 **process를 orphan process**라고 함

---

# 6) IPC - InterProcess Communication

- Process는 다른 process와 자원 공유 등 영향을 주지 않고 동작하는 independent process와 서로 영향을 주며 동작하는 coorperating process로 나뉨
- coorperating process가 서로간의 자원을 공유하는 것을 IPC라 하며, **shared** **memory** 과 **message passing** 기법이 있음

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/ipc.png" width="500" height="300">
---

## 6.1) Direct Message Passing

- 두 process간에 직접적으로 message를 주고 받는 기법

    ex) A process가 B process에 message를 전달하는 경우

    A process에서 `send(B, message)`를 통해 process B로 message를 전달

    B process에서  `receive(A, message)`를 통해 process A로부터 message를 받음

- 두 process간에 하나의 link만 생성되며, 단방향 통신이 이루어짐

---

## 6.2) Indirect Message Passing

- 두 process간에 mailbox를 통해 간접적으로 message를 주고 받는 기법
- mailbox는 여러개가 존재할 수 있으며 각각의 id를 가지고 있음

    ex) A process와 B process가 M mailbox를 통해 message를 주고 받는 경우

    `send(M, message)`, `receive(M, message)`를 통해 서로 message를 주고 받음

- mailbox를 기준으로 link가 생성되기 때문에 여러 process가 동일한 link를 사용할 수 있음
- 각각의 process는 여러개의 link를 가질 수 있으며 단방향, 양방향 통신이 모두 가능

---

## 6.3) Synchronization

### 6.3.1) Blocking - synchronous

- **Blocking send:** receiver가 message를 전달받기 전까지 sender를 block시킴
- **Blocking receive:** message가 전달되기 전까지 receiver를 block시킴
- **Rendezvous:** sender와 receiver가 모두 blocking인 경우

---

### 6.3.2) Non-Blocking - asynchronous

- **Non-Blcking send:** sender는 receiver의 message 수신 여부와 관계 없이 message 전송하기만 함
- **Non-Blocking receive:** sender의 message 전송 여부와 관계없이 message를 수신하기 때문에 빈 message를 전달받을 수 있음

---
