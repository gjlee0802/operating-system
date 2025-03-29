# 면접 기출 예시

## 개념 질문

### 1. Demand paging이란 무엇인가? (P 23-24 First)
~~~
프로그램이 실제로 필요로 할 때(요구할 때)에만 해당 페이지를 메모리에 불러오는 방식
즉, 프로그램의 전체 페이지를 한꺼번에 메모리에 올리지 않고, 실제로 접근하는 페이지만 로딩하는 것
~~~

### 2. 요구 페이징(demand-paging)에서 페이지 폴트(page fault)는 어떤 상황에서 발생하는가? 페이지 폴트가 발생했을 때 운영체제가 수행하는 동작들을 설명하시오. (P 24-25 Second)
✅ Page Fault 발생 조건:  
~~~
프로세스가 접근하려는 페이지가 현재 메모리에 없는 경우 (즉, 디스크에 존재)
~~~

✅ Page Fault 발생 시 OS 동작:  
~~~
1. Page Fault Exception 처리 => OS로 제어권 넘김
2. 해당 Page가 디스크에 있는지 확인
3. Frame 확보 (필요시 Page 교체)
4. 디스크에서 메모리로 Page 로드
5. Page Table 업데이트
6. 중단된 명령어 재실행
~~~

### 3. TLB(Translation Lookaside Buffer)란 무엇인가? TLB의 역할과 동작 방식에 대해 명확히 설명하시오. (P 24-25 First)
✅ TLB란 무엇인가?  
~~~
가상 주소 => 물리 주소 변환을 빠르게 하기 위한 Cache 메모리
(MMU에 내장됨)
~~~

✅ 역할 및 기능:  
~~~
자주 참조되는 Page Table Entry를 캐싱하여
Page Table에 접근할 필요가 없게끔 하고,
Address Translation 속도를 향상시키는 역할

TLB hit가 발생하면 빠른 변환이 이루어짐
TLB miss가 발생하면 Page Table 접근하여 TLB에 항목 추가를 고려
~~~

### 4. TLB 슛다운(TLB shootdown)이란 무엇인가? 그것이 왜 (또는 언제) 필요한지 설명하시오. (P 24-25 First)
✅ TLB Shootdown이란?  
~~~
멀티코어 환경에서 주소 매핑 일관성을 유지하기 위한
TLB 무효화 작업 (각 코어의 TLB 항목 무효화)
~~~

✅ 언제 왜 필요한가?  
~~~
여러 코어(쓰레드)에서 공유되는 Page Table이 변경될 때,
TLB는 각 코어마다 따로 존재하고, 
Page Table이 변경되면 TLB에 캐시된 내용과 불일치 문제가 발생 가능함

따라서, 일관성을 유지하기 위해 필요함
~~~

### 5. Copy-on-Write는 무엇인가? 사용했을 때의 이점은 무엇인가?
✅ CoW란?
~~~
Write할때 Copy하자는 의미로써 
어떤 한 프로세스가 공유 페이지를 작성할때만 해당 공유페이지를 복사하는 방법
~~~
✅ 이점
~~~
fork 했을 때 바로 복사하는 것보다 쓰기가 발생할 때만 복사하여 오버헤드가 줄어듦
~~~

### 6. DMA (Direct Memory Access)가 어떻게 시스템의 동시성을 높이는가? 어떻게 하드웨어의 복잡성을 높이는가? (P 22-23 First)
✅ DMA가 시스템의 동시성(System Concurrency)을 향상시키는 이유  
~~~
DMA는 CPU의 개입 없이 메모리와 데이터를 주고받을 수 있게 하는 방식

DMA를 사용하면,
→ CPU는 I/O 작업을 DMA에게 맡기고
→ 자신은 다른 작업(계산 등)을 계속 수행할 수 있음
~~~

✅ DMA가 하드웨어 설계를 복잡하게 만드는 이유  
~~~
a. 버스 충돌 관리
CPU와 DMA가 동시에 메모리에 접근할 경우 버스 충돌이 일어나는 것을 방지하기 위한 제어 필요

b. 메모리 일관성 문제
DMA가 메모리를 직접 수정하면 CPU 캐시와 메모리 사이에 일관성 오류가 발생하는 것을 방지하기 위한 제어 필요

c. 인터럽트 및 우선순위 처리
DMA 완료 시 인터럽트를 통해 CPU에게 알리기 위한 인터럽트 제어 필요
~~~

## 💪 Address Translation 관련 심층 문제
### 1. Consider a logical address space of eight pages of 1024 words each, mapped onto a physical memory of 32 frames. How many bits are there in the logical address and the physical address? (P 24-25 Second)
~~~
각각 1024 바이트 (2^10) 크기의 Page
Virtual Memory의 8개 Page를
Physical memory의 32개 Frame에 매핑

Virtual Address: 13 bits
Page offset으로 10 bits 사용
Virtual Page Number로 3 bits 사용

Physical Address: 15 bits
Frame offset으로 10 bits 사용
Physical Frame Number로 5 bits 사용
~~~

-----

### 2. A certain computer provides its users with a virtual-memory space of 2^32 bytes. The computer has 2^18 bytes of physical memory. The virtual memory is implemented by paging, and the page size is 4096 bytes (where 4096 = 2^12). A user process generates the virtual address 11123456 (that is, 0001 0001 0001 0010 0011 0100 0101 0110 in binary format). Explain how the system computes the corresponding physical location. (P 23-24 Second)

1️⃣ Step 1. Virtual Address, Physical Address 필드 해석  
~~~
Page 하나의 크기는 2^12 bytes 이므로, 12 bits는 Page Offset으로 사용된다.
따라서, 주어진 Virtual Address의 456 ("0100 0101 0110") 부분이 Page Offset이다.
그리고 나머지 11123 ("0001 0001 0001 0010 0011") 부분이 Virtual Page Number이다.

Physical Memory의 크기는 2^18 bytes이므로, Physical Address는 총 18 bits로 이루어지며,
이 중, 12 bits는 Frame Offset로 사용되고,
나머지 6 bits는 Physical Frame Number로 사용된다.
~~~

2️⃣ Step 2. Page Table: Virtual Page Number → Physical Frame Number  
~~~
이 부분은 시스템에 저장된 Page Table에 따라 달라짐
→ 즉, 실제 VPN 0x11123이 어떤 Physical Frame Number(PFN)에 매핑되는지는 Page Table이 필요함
~~~

3️⃣ Step 3. PFN을 찾은 이후, 변환된 Physical Address  
~~~
Page Table에서 VPN에 해당하는 PFN이 0x15("00010101")라고 가정한다면,

PFN 상위 6 bits와 Offset 12 bits를 합친 것이 Physical Address가 됨
"0001 01" + "0100 0101 0110" → "000101010001010110"
~~~

-----

### 3. For a processor with 64-bit virtual addresses, a single-level page table, a 34-bit physical address space, and 64KB pages, show a diagram depicting how a virtual address is translated into a physical address. Make sure to label each field and path with a name and the number of bits, and include the TLB and page table in your diagram (Assume no page faults). (P 22-23 First)
1️⃣ Step 1. Virtual Address, Physical Address 필드 해석  
~~~
Virtual Address 해석:
Page 하나의 크기가 64KB = 2^16 bytes이므로, 
    Page Offset은 16 bits
Virtual Address(64 bits)에서 Page Offset(16 bits)를 제외한
    Virtual Page Number는 48 bits

Physical Address 해석:
Offset 크기는 VA의 Page Offset의 크기와 같으므로,
    Frame Offset은 16bits
Physical Address(34 bits)에서 Frame Offset(16 bits)를 제외한
    Physical Frame Number는 18 bits
~~~
2️⃣ Step 2. TLB (hit일 경우): Virtual Page Number → Physical Frame Number  
~~~
TLB를 탐색하여 대상 VPN(48 bits)이 등록되어 있으면,
48비트 VPN을 받아 18 bits PFN을 반환함
반환된 PFN과 Page Offset을 합쳐서 최종 Physical Address가 만들어짐

이제 완성된 Physical Address로 물리 메모리의 데이터에 접근 가능
~~~

2️⃣ Step 3. TLB (miss일 경우): Page Table 접근  
~~~
TLB를 탐색하여 대상 VPN이 등록되지 않았다면,
Page Table에 접근하여 Physical Frame Number을 얻고자 함
~~~

✅ 다이어그램  
![page_table_and_tlb](../image_files/page_table_and_tlb.png)

-----

### 4. Assume a simple paging system with 232 bytes of physical memory, 248 bytes of logical address space and pages that are 220 bytes in size. Further assume that each page table entry contains 4 bits indicating protection and validity of the entry. (Cambridge 21 Second)

정리하면,
* Physical Memory 크기
    * 2^32 바이트 (4GB)
* Virtual Address Space 크기
    * 2^48 바이트 (256TB)
* Page 크기
    * 2^20 바이트 (1MB)
* PTE당 추가 정보
    * 4 비트

#### 4.1. How many bits are used for the frame number and how many for the frame offset?
~~~
Page 하나의 크기는 2^20 바이트 이므로, Page Offset는 20 비트
Frame Offset은 Page Offset과 동일하게 20 비트 사용

Physical Address를 위해 32 비트 사용
Frame Number는 32 비트에서 20 비트(Frame Offset 필드)를 제외한 12 비트
~~~

#### 4.2. What is the total size of the page table in number of bits?

1️⃣ Step 1. Page의 개수 구하기  
~~~
Virtual Address Space의 크기를 Page 하나의 크기로 나누면, Page 개수를 구할 수 있음

Page 개수 = 2^48 / 2^20 = 2^28 개
~~~

2️⃣ Step 2. Page Table Entry 크기 구하기  
~~~
PTE는 Frame Number와 4 비트로 구성되었으므로,

PTE 크기 = Frame Number 크기 + 4 bits
    = 12 + 4 = 16 bits
~~~

3️⃣ Step 3. Page Table 크기 구하기  
~~~
🎯 Page Table 크기 = Page 개수 x PTE 크기
    = 2^28 x 2^4  = 2^32 bits (512MB)
~~~

#### 4.3. Assume that the working set of a typical process is fixed throughout the process lifetime and consists of 20 pages. How many entries would you suggest for the Translation Lookaside Buffer (TLB) for this system? What would its total size be in number of bits? Explain your answer.

1️⃣ Step 1. TLB에 몇 개의 엔트리를 넣을 것인가?  
~~~
working set이 20 페이지로 구성되므로,
TLB에서 20개의 페이지를 수용할 수 있다면, Miss가 발생하지 않게 최적화 가능함

🎯 따라서, TLB에 20개의 엔트리를 넣을 것임
~~~

2️⃣ Step 2. TLB 크기 구하기  
~~~
TLB 엔트리 하나의 크기 = Frame Number 크기 + Page Number 크기 + 추가 비트
PFN 크기: 12 bits
VPN 크기: 28 bits
추가 비트 크기: 4 bits

TLB Entry 크기 = 12 + 28 + 4 = 44 bits

TLB 크기 = TLB Entry 크기 x 20 = 44 x 20 = 880 bits

🎯 따라서, TLB 크기는 880 bits
~~~

#### 4.4. Further assume that TLB search time is 20ns, TLB hit ratio is 80% and memory access time is 100ns. How many page table levels would you need to achieve an effective access time of 160ns, and why?
~~~
TLB hit ratio: 0.8
TLB miss ratio: 0.2

160 ns = TLB hit ratio x (20 + Memory Access Time) + TLB Miss ratio x (20 + Memory Access Time x # of Levels + Memory Access Time)

160 ns = 0.8 x 120 + 0.2 x (120 + 100 x '# of Levels')
    = 96 + 24 + 20 x '# of levels'
    = 120 + 20 x '# of levels'

# of levels = 2

🎯 따라서, 2단계의 Page Table Levels가 필요함
~~~

## 💪 Performance 관련 심층 문제

### 1. Consider a virtual memory system with paging. Assume there is no cache. A memory access is done by a page table lookup followed by an access to the target physical memory address. Now the system provides hardware support with translation look-aside buffers (TLBs) to accelerate the page table lookup time upon a TLB hit. Assume that it takes 20 nanoseconds to search the TLBs, and 100 nanoseconds to access from the main memory. (P 22-23 Second)

### 1.1. With 80% hit ratio of TLBs, calculate the average memory access time.  

AMAT = `TLB hit ratio x (TLB search time + Memory access time)` + `TLB miss ratio x (TLB search time + 2 x Memory access time)`
~~~
AMAT = 0.8 x (20 + 100) + 0.2 x (20 + 200) = 0.8 x 120 + 0.2 x 220 = 96 + 44 = 140 ns
~~~
### 1.2. With 98% hit ratio of TLBs, calculate the average memory access time.  
~~~
AMAT = 0.98 x (20 + 100) + 0.02 x (20 + 200) = 0.98 x 120 + 0.02 x 220 = 122 ns
~~~

## 💪 Page Replacement 관련 심층 문제

### 1. Assume that there are five frames, and all frames are initially empty. For each of the following page replacement algorithms, how many page faults would occur? Consider the following page reference string: (P 22-23 Second)
~~~
1,2,3,4,2,1,5,6,2,1,2,3,7,6,3,6
~~~

#### 1.1. LRU replacement algorithm
| 순서  | 사용 Page | 교체 대상 | Frame 상태    | Page Fault 발생 여부 |
|-------|-----------|-----------|-------------|---------------------|
| 1     | 1         |           | 1           | O                   |
| 2     | 2         |           | 1,2         | O                   |
| 3     | 3         |           | 1,2,3       | O                   |
| 4     | 4         |           | 1,2,3,4     | O                   |
| 5     | 2         |           | 1,2,3,4     | X                   |
| 6     | 1         |           | 1,2,3,4     | X                   |
| 7     | 5         |           | 1,2,3,4,5   | X                   |
| 8     | 6         | 3         | 1,2,6,4,5   | O                   |
| 9     | 2         |           | 1,2,6,4,5   | X                   |
| 10    | 1         |           | 1,2,6,4,5   | X                   |
| 11    | 2         |           | 1,2,6,4,5   | X                   |
| 12    | 3         | 4         | 1,2,6,3,5   | O                   |
| 13    | 7         | 5         | 1,2,6,3,7   | O                   |
| 14    | 6         |           | 1,2,6,3,7   | X                   |
| 15    | 3         |           | 1,2,6,3,7   | X                   |
| 16    | 6         |           | 1,2,6,3,7   | X                   |

🎯 Page Fault `7번` 발생  

#### 1.2. Optimal replacement algorithm

| 순서  | 사용 Page | 교체 대상 | Frame 상태    | Page Fault 발생 여부 |
|-------|-----------|-----------|-------------|---------------------|
| 1     | 1         |           | 1           | O                   |
| 2     | 2         |           | 1,2         | O                   |
| 3     | 3         |           | 1,2,3       | O                   |
| 4     | 4         |           | 1,2,3,4     | O                   |
| 5     | 2         |           | 1,2,3,4     | X                   |
| 6     | 1         |           | 1,2,3,4     | X                   |
| 7     | 5         |           | 1,2,3,4,5   | X                   |
| 8     | 6         | 4         | 1,2,3,6,5   | O                   |
| 9     | 2         |           | 1,2,3,6,5   | X                   |
| 10    | 1         |           | 1,2,3,6,5   | X                   |
| 11    | 2         |           | 1,2,3,6,5   | X                   |
| 12    | 3         |           | 1,2,3,6,5   | X                   |
| 13    | 7         | 1         | 7,2,3,6,5   | O                   |
| 14    | 6         |           | 7,2,3,6,5   | X                   |
| 15    | 3         |           | 7,2,3,6,5   | X                   |
| 16    | 6         |           | 7,2,3,6,5   | X                   |

🎯 Page Fault `6번` 발생  

-----

### 2. How many page faults would occur for the following replacement algorithms, assuming one, four, or seven frames? Remember all frames are initially empty, so your first unique pages will all cost one fault each. Consider the following page reference string: (P 22-23 First)
~~~
1,2,3,4,2,1,5,6,2,1,2,3,7,6,3,2
~~~

#### 2.1. LRU replacement

**Assuming four frames:**  
| 순서  | 사용 Page | 교체 대상 | Frame 상태    | Page Fault 발생 여부 |
|-------|-----------|-----------|-------------|---------------------|
| 1     | 1         |           | 1           | O                   |
| 2     | 2         |           | 1,2         | O                   |
| 3     | 3         |           | 1,2,3       | O                   |
| 4     | 4         |           | 1,2,3,4     | O                   |
| 5     | 2         |           | 1,2,3,4     | X                   |
| 6     | 1         |           | 1,2,3,4     | X                   |
| 7     | 5         | 3         | 1,2,5,4     | O                   |
| 8     | 6         | 4         | 1,2,5,6     | O                   |
| 9     | 2         |           | 1,2,5,6     | X                   |
| 10    | 1         |           | 1,2,5,6     | X                   |
| 11    | 2         |           | 1,2,5,6     | X                   |
| 12    | 3         | 5         | 1,2,3,6     | O                   |
| 13    | 7         | 6         | 1,2,3,7     | O                   |
| 14    | 6         | 1         | 6,2,3,7     | O                   |
| 15    | 3         |           | 6,2,3,7     | X                   |
| 16    | 2         |           | 6,2,3,7     | X                   |

🎯 Page Fault `9번` 발생  

#### 2.2. FIFO replacement

**Assuming four frames:**  
| 순서  | 사용 Page | 교체 대상 | Frame 상태    | Page Fault 발생 여부 |
|-------|-----------|-----------|-------------|---------------------|
| 1     | 1         |           | 1           | O                   |
| 2     | 2         |           | 1,2         | O                   |
| 3     | 3         |           | 1,2,3       | O                   |
| 4     | 4         |           | 1,2,3,4     | O                   |
| 5     | 2         |           | 1,2,3,4     | X                   |
| 6     | 1         |           | 1,2,3,4     | X                   |
| 7     | 5         | 1         | 5,2,3,4     | O                   |
| 8     | 6         | 2         | 5,6,3,4     | O                   |
| 9     | 2         | 3         | 5,6,2,4     | O                   |
| 10    | 1         | 4         | 5,6,2,1     | O                   |
| 11    | 2         |           | 5,6,2,1     | X                   |
| 12    | 3         | 5         | 3,6,2,1     | X                   |
| 13    | 7         | 6         | 3,7,2,1     | O                   |
| 14    | 6         | 2         | 3,7,6,1     | O                   |
| 15    | 3         |           | 3,7,6,1     | X                   |
| 16    | 2         | 1         | 3,7,6,2     | O                   |

🎯 Page Fault `11번` 발생  

#### 2.3. Optimal replacement

**Assuming four frames:**  
| 순서  | 사용 Page | 교체 대상 | Frame 상태    | Page Fault 발생 여부 |
|-------|-----------|-----------|-------------|---------------------|
| 1     | 1         |           | 1           | O                   |
| 2     | 2         |           | 1,2         | O                   |
| 3     | 3         |           | 1,2,3       | O                   |
| 4     | 4         |           | 1,2,3,4     | O                   |
| 5     | 2         |           | 1,2,3,4     | X                   |
| 6     | 1         |           | 1,2,3,4     | X                   |
| 7     | 5         | 4         | 1,2,3,5     | O                   |
| 8     | 6         | 5         | 1,2,3,6     | O                   |
| 9     | 2         |           | 1,2,3,6     | X                   |
| 10    | 1         |           | 1,2,3,6     | X                   |
| 11    | 2         |           | 1,2,3,6     | X                   |
| 12    | 3         |           | 1,2,3,6     | X                   |
| 13    | 7         | 1         | 7,2,3,6     | O                   |
| 14    | 6         |           | 7,2,3,6     | X                   |
| 15    | 3         |           | 7,2,3,6     | X                   |
| 16    | 2         |           | 7,2,3,6     | X                   |

🎯 Page Fault `7번` 발생  
