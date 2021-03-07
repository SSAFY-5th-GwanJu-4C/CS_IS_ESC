# 1. Thread란?

- Process 내에서 실질적인 작업을 하는 곳
- process를 새로 생성하는 것 보다 thread를 생성하는 것이 더 가벼움

---

# 2. Multi-Thread

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/multi_thread.png" width="600" height="300">

---

## 2.1) Multi-Thread의 장점

- 자원 공유: thread는 하나의 process내의 자원을 서로 공유하기 때문에 process의 자원 공유 방식인 shared memory와 message passing보다 훨씬 자원 공유가 쉬움
- Economy: process 생성보다 간단하며, context switching보다 thread switching의 overhead가 적음

---

## 2.2) Concurrent vs Parallel

- Concurrent: 하나의 progress내에서 많은 것을 **다루는** 것

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/concurrent.png" width="600" height="100">
    
- Parallel: **동시에** 많은 일을 **하는** 것

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/parallel.png" width="600" height="200">
    
---

## 2.3) Parallelism의 종류

### 2.3.1) Data Parallelism

- 동일한 data를 여러개로 나누어 하나의 작업을 빠르게 처리하는 parallelism

---

### 2.3.2) Task Parallelism

- 여러개의 작업을 thread별로 나누어 각각의 thread가 별도의 작업을 처리함으로써 전체 작업을 빠르게 처리하는 parallelism

---

## 2.4) Amdahl's Law

- 실제 core의 개수가 늘어나더라도 속도 향상이 core 개수에 비례해서 증가하지 않는다는 법칙

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/amdhal.png" width="200" height="100">
    
    - S: serial portion, N: core의 개수
    - N이 증가하여 무한대에 수렴하면 속도 향상은 1/S가 되어 core의 개수가 증가하여도 serial portion에 의해 속도가 N과 1대 1로 비례하여 증가하지 못함

---

# 3. Multi-thread Models

- User thread: thread library에 의해 user level에서 관리되는 thread
- Kernel thread: OS의 kernel에 의해 관리되는 thread

---

## 3.1) Many-To-One

- 여러 개의 user thread가 하나의 kernel thread에 연결되어 있는 multi-thread
- 최근에는 많이 사용되지 않음
- kernel thread가 하나이기 때문에 실질적으로 parallel하게 동작하지 않음

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/many_to_one.png" width="400" height="300">
    
---

## 3.2) One-To-One

- 하나의 user thread가 하나의 kernel thread에 연결되어 있는 multi-thread
- user thread를 생성하면 kernel thread도 같이 생성되기 때문에 user thread를 생성하는데 overhead가 있음 → 생성할 수 있는 thread의 개수가 제한적임
- many-to-one model에 비해 concurrent함

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/one_to_one.png" width="400" height="200">
    
---

## 3.3) Many-To-Many

- 여러 개의 user thread가 여러 개의 kernel thread와 연결되어 있는 multi-thread
- 정해진 수의 kernel thread에 user thread를 할당하기 때문에 user thread의 생성에 제한이 없음

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/many_to_many.png" width="400" height="300">
    
---

## 3.4) Two-Level Model

- Many-To-Many Model과 비슷하지만 하나의 user thread가 별도의 kernel thread에 할당될 수 있도록 한 multi-thread

    <img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/two_level.png" width="400" height="300">
    
---

# 4. Threading Issues

## 4.1) Signal Handling

- **Signal**: UNIX 기반의 시스템에서 사용되는 용어로, 특정 event가 발생했을 때 process에게 이를 알리는 신호
- **Signal Handler**
    - Signal을 처리하기 위한 handler로, default handler와 user-defined handler로 나뉨
    - 모든 signal은 default handler를 가지며 user-defined handler를 override해서 사용할 수 있음

🙋‍♂️ process에 알리면 multi-thread인 경우에는 어떻게 처리하나요?

📢 작업을 수행해야 하는 thread에 signal을 전달하여 multi-thread 처리

---

## 4.2) Thread Cancellation

- Thread의 작업이 완료되기 이전에 thread를 종료하는 경우로, 종료하고자 하는 thread를 target thread라 칭함

---

### 4.2.1) Asynchronous Cancellation

- target thread를 즉시 종료

---

### 4.2.2) Deferred Cancellation

- target thread를 주기적으로 확인하여 종료해야 할 때 종료

---

## 4.3) TLS - Thread Local Storage

- Thread의 data를 관리하는 곳으로, 각각의 thread는 TLS의 data를 복사하여 사용
- Thread 생성 시 별도의 조작이 필요 없는 경우 유용함
- global data보다는 static data에 유사함

---
