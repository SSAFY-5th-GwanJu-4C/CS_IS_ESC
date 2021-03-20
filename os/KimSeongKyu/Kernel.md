# 1. Kernel의 탄생 배경

초기 컴퓨터는 하드웨어의 추상화 및 운영체제의 도움 없이 단순히 컴퓨터의 컴퓨팅 능력만으로도 프로그램 실행이 가능했기 때문에 Kernel이 꼭 필요한 것은 아니었다. 하지만 이러한 구조는 **자원을 관리할 수 없다는 문제점**을 발생시킨다. 새로운 프로그램을 실행하기 위해서는 컴퓨터를 재부팅하여 메모리에 있던 내용을 전부 지우고 새로운 프로그램을 실행해야 하는 것이다. 이러한 문제점을 해결하기 위해 **컴퓨터의 자원을 관리하고자** 탄생한 것이 바로, **Kernel**이다.

---

# 2. Kernel의 종류

- **Monolitic Kernel:** 운영체제 내에서 발생하는 시스템과 관련된 모든 작업을 Kernel이 처리한다. 속도가 빠르고 디자인이 간단하다는 장점이 있지만 사소한 문제 하나가 Kernel 전체에 문제를 주기 때문에 안정성에 큰 위험부담을 가지고 있다. Linux에서는 Monolitic Kernel을 사용하고 있다.
- **Micro Kernel:** Kernel에서 하던 일들을 사용자 level로 올려 더욱 간소화된 Kernel 방식이다.
- **Hybrid Kernel:** Monolitic Kernel에 Micro Kernel을 결합한 Kernel 방식이다.

---

# 3. Kernel의 기능

## 3.1) Memory 관리

Memory 관리는 Kernel이 탄생한 가장 큰 이유라고 할 수 있다. 앞서 언급했듯이 여러 개의 Process을 실행하기 위해서는 Memory를 썼다 지웠다 하는 I/O Interrupt가 발생해야 한다. 이러한 과정을 Kernel이 담당하여 Memory를 유연하게 사용할 수 있도록 해준다.

---

## 3.2) Process 관리

Memory 관리와 마찬가지로 Process 관리 또한 Kernel이 탄생한 가장 큰 이유 중 하나이다. 여러 개의 Process를 실행하다 보면 각각의 Process에 대해 CPU에서 우선순위를 두고 처리해야 한다. 이 우선순위를 결정하는 것을 CPU Scheduling이라고 하며, **CPU Scheduling 또한 Kernel에서 동작한다.**

또한 모든 Process는 한 번에 Memory위에 올라가지 않기 때문에 **Process의 생성과 종료도 관리**해주어야 하는데, 이를 Kernel에서 담당한다. 만약 Process가 스스로 생성되거나 종료된다면, 생성 당시에는 Memory의 어느 위치에 올라가야 할지도 모를뿐더러 종료 시에 다른 Process에게 자신의 종료를 알리는 것은 다른 Process를 살펴볼 수 있다는 것을 의미하기 때문에 보안상의 문제도 발생할 수 있다.

---

## 3.3) Device Driver

우리가 사용하는 컴퓨터는 단순히 컴퓨터만이 아닌 다양한 하드웨어를 같이 사용한다. 마우스, 스피커, 키보드, 프린터 등 이 모든 것들이 **개별적인** 하드웨어이며 컴퓨터에서 이를 사용하기 위해서는 각각의 사용방법을 알아야 한다. 하지만 세상에는 정말 다양한 종류의 하드웨어가 존재하고, 하나의 하드웨어 내에서도 수없이 많은 종류로 나뉜다. 때문에 하나의 컴퓨터가 이 수많은 하드웨어의 사용법을 전부 다 구현하는 것은 사실상 불가능하다. 이를 해결하기 위한 **컴퓨터와 하드웨어 간의 Interface를 Device Driver**라고 하며, **Device Driver를 인식하는 주체가 바로 Kernel**이다. 때문에 Kernel이 인식하지 못하는 Device Driver의 경우 사용자가 별도로 설치하여 추가해줘야 한다.

---

## 3.4) File Utility System

File Utility System은 파일의 CRUD를 담당하는 것이라고 생각하면 된다. 우리가 생각하기에 파일의 CRUD는 사용자가 하는 것이라고 생각할 수 있지만, 사용자는 Kernel에서 제공하는 기능을 Interface를 통해 사용하는 것일 뿐 실질적으로 파일과 관련된 CRUD 작업은 Kernel이 관리하게 된다.

---

## 3.5) System Call API

System Call API를 설명하기에 앞서 우선 **User Mode**와 **Kernel Mode**에 대해 알아야 한다. 운영체제는 환경에 따라 User Mode와 Kernel Mode로 동작하게 되는데 말 그대로 **사용자 관련 처리를 하는 환경을 User Mode**라 하고, **Kernel(시스템) 관련 처리를 하는 환경을 Kernel Mode**라고 한다.

🙋‍♂️ 이렇게 두 가지 환경을 분리하여 동작하는 이유는 무엇일까?

📢 사용자가 직접적으로 시스템에 접근할 수 없도록 보안 강화를 위해서이다.

단순히 이렇게 두 가지 환경으로 분리되어 있다면, 사용자는 시스템에 전혀 접근할 수 없게 된다. 그렇다면 위에서 언급했던 파일 관련 CRUD 작업을 할 수 없어야 하는데 우리는 실제로 파일 관련 CRUD 작업을 할 수 있다. 이것을 가능하게 해주는 것이 **System Call API**이다. 즉, 사**용자와 Kernel 간의 Interface를 두어서 시스템의 일부 기능(System Call)을 제공**하는 것이다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/system%20call%20api.png" width="700" height="200">

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/system%20calls.png" width="400" height="400">

---
