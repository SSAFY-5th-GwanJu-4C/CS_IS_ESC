## 1. 배경 지식

### 1.1) Main Memory 구조

CPU의 접근 가능한 저장소는 **main memory**와 **register**가 있다. main memory와 관련된 기본적인 register는 process가 할당받은 main memory의 시작 주소인 **base register**와 process가 할당받은 main memory의 크기인 **limit register**가 있다.(이후에 그 외에도 다양한 register가 사용된다.) 아래의 그림은 main memory의 구조와 process가 main memory에 주소를 할당하는 과정을 나타낸다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/main%20memory%20%EA%B5%AC%EC%A1%B0%20%EB%B0%8F%20process%EC%9D%98%20memory%20%EC%A3%BC%EC%86%8C%20%ED%95%A0%EB%8B%B9%20%EB%A1%9C%EC%A7%81.png" width="1000" height="400">

---

### 1.2) Memory 주소 할당 시기

- **Compile Time:** memory의 어디에 위치해야 할 지 아는 **absolute code**에 대한 memory 할당이 이루어진다. 단, code의 memory 위치가 변경될 경우 compile을 다시 해야 한다.
- **Load Time:** Compile Time단계에서 메모리를 할당받지 못한 **relocatable code**에 대해 memory 할당이 발생한다.
- **Execution Time:** process가 다른 memory segment로 memory를 할당받을 경우 발생한다.

---

## 2. Swapping

process를 memory에 할당할 때, 모든 process를 memory에 계속해서 쌓는다면 결국에는 memory가 부족해질 것이다. 때문에 process를 memory와 disk간에 주고받으며 memory의 사용 가능한 공간을 동적으로 관리하는 방식을 **swapping**이라고 한다. 하지만 실행중인 process를 변경하여 context switch가 발생할 때마다 memory에 쓰고 지우는 IO Interrupt가 발생하고, process가 IO Interrupt동안 멈추는 문제가 발생한다. 이러한 문제점으로 인해 standard swapping은 잘 사용되지 않고, 사용 가능한 main memory가 매우 적을 때 process를 swapping하는 방식으로 주로 사용된다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/swapping.png" width="400" height="300">

---

## 3. Contiguous Allocation

main memory는 os가 사용하는 **low memory**와 사용자의 process가 사용하는 **high memory**로 구분된다. 그 중에서 high memory를 관리하기 위해 process를 memory에 할당하는 것을 **contiguous allocation**이라 한다. process가 할당받은 memory 공간을 하나의 **partition**이라 하며, partition을 제외한 나머지 사용 가능한 memory 공간을 **hole**이라 한다.contiguous allcation은 앞서 언급한 process의 memory 할당 방식에 relocation register를 추가하여 process가 할당받을 memory의 위치를 동적으로 관리할 수 있으며, process가 종료된 후 생성된 hole은 인접한 hole과 결합하여 하나의 hole을 이룬다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/contiguous%20allcation.png" width="1000" height="400">

---

### 3.1) Contiguous Allocation Problem

- **First Fit:** process가 할당받을 수 있는 **첫번째** hole에 proess를 할당한다.
- **Best Fit:** process가 할당받을 수 있는 **가장 작은** hole에 process를 할당한다.
- **Worst Fit:** process가 할당받을 수 있는 **가장 큰** hole에 process를 할당한다.

📢 Best Fit와 Worst Fit은 모든 hole을 전부 탐색해야 하며, storage 관리와 성능 상 Worst Fit보단 First 혹은 Best Fit이 더 낫다.

---

### 3.2) Fragmentation

main memory의 사용 가능한 공간인 fragmentation은 크게 **External Fragmentation**과 **Internal Fragmentation**으로 구분된다.

- **External Fragmentation:** 전체 memory를 기준으로 사용 가능한 memory 공간, 즉 **hole**을 뜻한다.
- **Internal Fragmentation:** 어떤 process가 할당받은 memory의 공간보다 해당 process의 크기가 작아 발생하는 **partition 내의 사용 가능한 memory 공간**을 뜻한다.

📢 External Fragmentation을 해결하기 위한 기법은 모든 fragmentation을 contiguous하게 하나로 합치는 **compaction**과 non-contiguous하게 관리하는 **segmentation**과 **paging**이 있다.

---

## 4. Segmentation

**segmetation**은 하나의 process를 여러개의 segment(method, object, local variable, global variable, ...)로 구분하여 main memory에서 관리하는 방식이다. segmentation을 위한 virtual address는 <segment number, offset>의 tuple로 이루어져 있으며, 이를 이용해 segmentation table에 접근한다. segmentation table은 physical address의 시작 주소인 **base**값과 할당된 주소 길이인 **limit**을 갖는 2차원 배열로 이루어져 있다. 아래의 그림은 Segmentation이 hardware 상에서 동작하는 방식과, 이를 통해 memory에 할당되는 모습을 나타낸다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/Segmentation%EC%9D%98%20hardware%20%EC%83%81%EC%97%90%EC%84%9C%EC%9D%98%20%EB%8F%99%EC%9E%91%20%EB%B0%A9%EC%8B%9D%20%EB%B0%8F%20Memory%20Allocation.png" width="1000" height="400">

---

## 5. Paging

paging을 이해하기 위해서는 **page**와 **frame**이라는 용어를 알아야 한다. 이 둘은 모두 memory 조각을 의미하지만 logical memory냐 physical memory이냐에 따라 구분된다.

- **page:** **logical memory** 조각
- **frame:** **physical memory** 조각

Paging은 하나의 process를 page와 frame라는 개념을 이용해 main memory에서 관리하는 방식이다. Paging은 Segmentation처럼 하나의 process를 여러개의 부분으로 나누어 관리하지만, 이를 관리하는 **table의 구조와 로직이 다르다는 차이점**을 가진다.

- **Segmentation Table:** **직접적인 physical address**를 나타내는 base와 segment의 크기인 limit으로 이루어져있다.
- **Page Table:** **간접적인 physical address**를 나타내는 **frame number**로만 이루어져있다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/Paging%EC%9D%98%20hardware%20%EC%83%81%EC%97%90%EC%84%9C%EC%9D%98%20%EB%8F%99%EC%9E%91%20%EB%B0%A9%EC%8B%9D%20%EB%B0%8F%20Memory%20Allocation.png" width="1000" height="400">

📢 Paging을 사용하면 하나의 data/instruction에 접근하기 위해 2번의 memory 접근 (page Table 접근 + frame에 해당하는 실제 data/instruction 접근)이 필요하다는 문제점가 발생한다. 이를 hardware level에서 빠르게 해결하기 위해 hardware cache인 **TLB(Translation Look-aside Buffer)**를 사용한다.

---

### 5.1) TLB - Translation Look-aside Buffer

TLB는 key값인 **page number**와 value값인 **frame number**로 구성되어 있다. TLB는 hardware level에 있기 때문에 속도가 빠르다는 장점이 있으며, cache 구조이기 때문에 크기가 작다. (32~1024 entries)

만약 page number를 이용해 TLB를 탐색했을 때, 해당 page number가 TLB에 존재한다면 **TLB hit**이 발생해 value값인 frame number를 반환하지만, page number가 TLB에 존재하지 않아 **TLB miss**가 발생한다면 page number에 해당하는 frame number를 TLB에 저장한다.

📢 몇몇 TLB는 **ASIDs**(Address Space IDentifiers)를 가지고 있는데, 이는 각각의 process의 주소 공간을 위한 ID값을 나타낸다. ASIDs는 multiple process에 사용되며 만약 ASIDs가 없다면 context switch가 발생할 때 TLB flush도 함께 발생하게 된다. 아래의 그림은 TLB를 적용한 paging의 로직이다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/paging%20with%20TLB.png" width="400" height="300">

---

### 5.2) Memory Protection

paging에서 memory protection을 위해 각각의 frame은 자신이 접근 제한이 read-only인지, read-write인지를 나타내는 **protectecion bit**를 가진다. 여기에 bit를 추가하여 execute-only 등의 접근 제한을 추가할 수 있다.또한 page table의 각각의 entry에 **valid-invalid bit**를 추가하여 해당 entry가 현재 process의 logical address에 해당하는 page인지를 나타낸다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/memory%20proection%20in%20paging.png" width="400" height="300">

---

### 5.3) Shared Pages

여러 process에서 공유하는 code의 경우 각각의 process가 자신의 page table에 있는 page를 개별적으로 physical address에 할당하지 않고 하나의 physical address만 할당받아 이를 공유한다.

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/os/KimSeongKyu/image/shared%20pages%20in%20paging.png" width="400" height="300">

---

### 5.4) Structure Of Page Table

page size 혹은 logical address space가 커지게 되면 그에 따라 Page Table의 크기는 기하급수적으로 커지게 되어 memory의 공간이 부족하게 된다. 이러한 문제를 해결하기 위해 다음과 같은 Paging 기법이 존재한다.

- **Hierarchical Page Table:** page table의 page number 부분을 둘(two-level page table) 혹은 셋(three level page table)로 나누어 page의 level을 구분하는 방식이다.
- **Hashed Page Table:** page Table에 hash function을 이용해 접근하는 방식이다.
- **Inverted Page Table:** Page Table을 logical address를 기준으로 생성하는 것이 아닌 physical address를 기준으로 생성하여 단 하나의 page table만을 생성하는 방식이다.

---
