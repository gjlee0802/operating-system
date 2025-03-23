# 면접 기출 예시

## 개념 질문

### 1. 요구 페이징(demand-paging)에서 페이지 폴트(page fault)는 어떤 상황에서 발생하는가? 페이지 폴트가 발생했을 때 운영체제가 수행하는 동작들을 설명하시오. (P 24-25 Second)
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

### 2. TLB(Translation Lookaside Buffer)란 무엇인가? TLB의 역할과 동작 방식에 대해 명확히 설명하시오. (P 24-25 First)
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

### 3. TLB 슛다운(TLB shootdown)이란 무엇인가? 그것이 왜 (또는 언제) 필요한지 설명하시오. (P 24-25 First)
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

## 심층 문제

### 1. Consider a logical address space of eight pages of 1024 words each, mapped onto a physical memory of 32 frames. How many bits are there in the logical address and the physical address? (P 24-25 Second)


-----

### 2. A certain computer provides its users with a virtual-memory space of 2^32 bytes. The computer has 2^18 bytes of physical memory. The virtual memory is implemented by paging, and the page size is 4096 bytes (where 4096 = 212). A user process generates the virtual address 11123456 (that is, 0001 0001 0001 0010 0011 0100 0101 0110 in binary format). Explain how the system computes the corresponding physical location. (P 23-24 Second)


-----

### 3. Consider a virtual memory system with paging. Assume there is no cache. A memory access is done by a page table lookup followed by an access to the target physical memory address. Now the system provides hardware support with translation look-aside buffers (TLBs) to accelerate the page table lookup time upon a TLB hit. Assume that it takes 20 nanoseconds to search the TLBs, and 100 nanoseconds to access from the main memory. (P 22-23 Second)


-----

### 4. For a processor with 64-bit virtual addresses, a single-level page table, a 34-bit physical address space, and 64KB pages, show a diagram depicting how a virtual address is translated into a physical address. Make sure to label each field and path with a name and the number of bits, and include the TLB and page table in your diagram (Assume no page faults). (P 22-23 First)