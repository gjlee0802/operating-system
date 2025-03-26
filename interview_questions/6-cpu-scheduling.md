# 면접 기출 예시

## 개념 질문

### 1. Preemptive 스케줄링과 Nonpreemptive 스케줄링의 차이를 설명하시오. (P 24-25 Second)

✅ **Preemptive Scheduling** (선점형 스케줄링):  
* **OS가 언제든지 실행 중인 프로세스를 중단시키고**, 다른 프로세스를 실행시킬 수 있는 방식
* **장점**: 응답성이 높음 (즉시 반응)
* **단점**: 복잡함
* 알고리즘: 
    * Round Robin, 
    * STCF(Shortest Time-to-Completion First), 
    * PSJF(Preemptive Shortest Job First) ...

✅ **Nonpreemptive Scheduling** (비선점형 스케줄링):  
* 실행 중인 **프로세스가 자발적으로 CPU를 포기**할 때까지 OS는 다른 프로세스를 실행하지 않음
* **장점**: 응답성이 낮음
* **단점**: 단순함
* 알고리즘: 
    * FIFO(aka. FCFS), 
    * SJF(Shortest Job First) ...

### 2. Priority inversion 문제는 무엇인가? 간단한 해결책을 제시하시오. (P 24-25 Second)
✅ **Priority Inversion (우선순위 반전) 문제**란?
~~~
낮은 우선순위의 프로세스가 자원을 점유하고 있어서,
높은 우선순위의 프로세스가 대기하게 되는 현상

즉, 높은 우선순위 프로세스가 실행되지 못하고 '거꾸로' 기다리는 상황
~~~

✅ 해결책: **우선순위 상속** (Priority Inheritance Protocol)
~~~
자원을 점유한 낮은 우선순위 프로세스가 
잠시 동안 높은 우선순위를 상속 받아서 실행되도록 함
~~~

### 3. Convoy effect(호위 효과)는 무엇인가?
~~~
🎯 모든 다른 프로세스들이 하나의 긴 프로세스가 CPU를 양도하기를 기다리는 현상
~~~

### 4. 우선순위 스케줄링 알고리즘(e.g., SJF)의 주요 문제인 Indefinite Blocking 혹은 Starvation 문제가 무엇인지 설명하고, 해결 방법을 제시하시오.
✅ Indefinite Blocking / Starvation이란?  
~~~
본질적으로 낮은 우선순위를 가진 프로세스가 계속 실행되지 못하는 상황.
~~~

✅ 해결 방법: Aging  
~~~
낮은 우선순위 프로세스가 오랫동안 기다릴수록 우선순위를 점점 높여주는 방법

시간이 지날수록 우선순위를 자동으로 증가시켜서
결국엔 높은 우선순위 프로세스들 사이에 끼어들 수 있음
~~~

## 💪 Scheduling 관련 심층 문제

### 1. Consider three processes P1, P2, and P3 with the following arrival times, execution (burst) times, and priorities (where 1 is the highest). Assuming no context switching overhead,explain each execution sequence with (a) priority scheduling and (b) round-robin scheduling. (P 23-24 First)

| Process   | Arrival Time  | Execution Time    | Priority  |
|-----------|---------------|-------------------|-----------|
| P1        | 0             | 8                 | 3         |
| P2        | 1             | 4                 | 1         |
| P3        | 4             | 2                 | 2         |

### 1.1. Priority Scheduling
**비선점형(Nonpreemptive)일 경우**:  

| 시간 | 실행 프로세스 | 도착 / 변화                |
|------|----------------|-------------------------|
| 0    | P1             | P1 도착                 |
| 1    | P1             | P2 도착                 |
| 2    | P1             |                         |
| 3    | P1             |                         |
| 4    | P1             | P3 도착                 |
| 5    | P1             |                         |
| 6    | P1             |                         |
| 7    | P1             |                         |
| 8    | P2             | P1 종료                 |
| 9    | P2             |                         |
| 10   | P2             |                         |
| 11   | P2             |                         |
| 12   | P3             | P2 종료                 |
| 13   | P3             |                         |
| 14   | (끝)           | P3 종료                 |


### 1.2. Round-robin Scheduling
**Time Quantum = 1**인 Roud-Robin 스케줄링:  
* **새로 도착한 프로세스**는 즉시 Ready Queue 맨 뒤에 추가됨
* **현재 실행 중인 프로세스**는 Ready Queue에 포함되지 않음

| 시간 | 실행 프로세스 | 도착 / 변화         | Ready Queue 상태         |
|------|----------------|----------------------|---------------------------|
| 0    | P1             | P1 도착              | []                        |
| 1    | P2             | P2 도착, P1 대기     | [P1]                      |
| 2    | P1             |                      | [P2]                      |
| 3    | P2             |                      | [P1]                      |
| 4    | P1             | P3 도착              | [P3, P2]                  |
| 5    | P3             |                      | [P2, P1]                  |
| 6    | P2             |                      | [P1, P3]                  |
| 7    | P1             |                      | [P3, P2]                  |
| 8    | P3             |                      | [P2, P1]                  |
| 9    | P2             | P3 종료              | [P1]                      |
| 10   | P1             | P2 종료              | []                        |
| 11   | P1             |                      | []                        |
| 12   | P1             |                      | []                        |
| 13   | P1             |                      | []                        |
| 14   | (끝)           | P1 종료               | []                        |
