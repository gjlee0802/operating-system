# 자주 등장한 문제
## Chapter 3. Process
### Q: Process와 Thread의 주요 차이를 설명하시오. 
~~~
1. 생성/전환 비용:
프로세스는 생성/전환 비용이 큼
쓰레드는 생성/전환 비용이 작음

2. 메모리 영역의 독립성:
프로세스는 자신만의 독립된 메모리 공간을 가져 완전히 분리되어 있음
쓰레드는 Code, Data, Heap 영역을 공유하며, Stack은 쓰레드마다 개별적으로 따로 존재함
~~~

## Chapter 7. Deadlocks
### Q: Deadlock가 발생하기 위한 4가지 필요 조건들을 제시하시오. 각 조건을 정확히 설명하시오.
~~~
1. 상호 배제 (Mutual Exclusion) 조건:
    자원은 한 번에 하나의 프로세스만 사용할 수 있어야 함

2. 점유와 대기 (Hold and Wait) 조건:
    프로세스가 어떤 자원을 점유한 상태에서 다른 자원을 요청하며 대기하여야 함

3. 비선점(Non Preemptation) 조건
    할당된 자원을 강제로 뺏을 수 없으며,
    프로세스가 자원을 반납해야 함

4. 순환 대기 (Circular Wait) 조건
    서로의 자원을 기다리며 원형으로 대기하는 상황이어야 함
~~~

# 등장하지 않았지만 중요한 개념

## Chapter 5. Process Synchronization

### Q: 프로세스의 컨텍스트(context)란 무엇이며, 컨텍스트 스위칭(context switching)에서 수행되는 주요 단계들은 무엇인가?
~~~
~~~

### Q: Dispatcher란 무엇인가?
~~~

~~~

### Q: Critical Section이란 무엇인가?
~~~

~~~

## Chapter 7. Deadlocks
Deadlock Avoidance - 뱅커 알고리즘 문제  

## Chapter 8. Memory Management Strategies
### Q: Segmentation, Paging의 차이점과 장단점을 설명하시오.
📌 Segmentation 개념  
~~~
프로그램을 논리적인 단위(Segment)로 나누어 메모리를 관리하는 방식
예: 코드 영역, 데이터 영역, 스택 등
~~~

📌 Paging 개념  
~~~
가상 주소 공간과 물리 주소 공간을 고정 크기 페이지(Page)로 나누어 관리하는 방식
~~~

🎯 Segmentation과 Paging의 차이점  
~~~
Segmentation은 가변 크기의 Segmentation 단위로 나누지만, Paging은 고정 크기의 Page 단위로 나눔
~~~

🎯 **Segmentation**의 **장단점**  
~~~
✅ 장점:
    1. 내부 단편화가 없음
    2. Dynamic relocation이 가능함 (실행 중에도 Segment의 위치 이동 가능)

❌ 단점:
    1. 외부 단편화가 있음
    2. Segment의 모든 메모리를 사용하지 않을 수 있음 (e.g., Heap)
~~~

🎯 **Paging**의 **장단점**  
~~~
✅ 장점:
    1. 내부 단편화가 없음
    2. 할당과 해제가 빠름
	3. Swapping이 쉬움

❌ 단점 
    1. 내부 단편화: Page Size가 커질수록 내부 단편화가 심해짐
    2. Page Table을 위한 추가적인 메모리 공간이 필요함
    3. Page를 찾기 위해 추가적인 메모리 접근이 필요함
~~~

-----

### Q: 외부 단편화, 내부 단편화란 무엇인지 설명하시오.

🎯 외부 단편화 (External Fragmentation)  
~~~
작고 흩어진 빈 공간(조각)들이 많이 생겨서,
총 유효한 여유 공간은 충분하지만,
연속된 큰 공간이 없어서 큰 프로세스를 수용할 수 없는 현상
~~~

🎯 내부 단편화 (Internal Fragmentation)  
~~~
프로세스에 할당된 메모리 블록(조각) 내에서
실제 사용하지 않고 낭비되는 공간이 내부에 존재하는 현상
~~~

-----

### Q: 페이지 크기가 클수록 Page Table 크기와 내부 단편화 문제는 어떻게 바뀌는지 설명하시오.

* 페이지 크기가 클수록
    * Page Table 크기는 커지고,
    * 내부 단편화 문제는 심해짐