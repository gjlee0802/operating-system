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

## Scheduling 관련 심층 문제

### 1. Consider three processes P1, P2, and P3 with the following arrival times, execution (burst) times, and priorities (where 1 is the highest). Assuming no context switching overhead,explain each execution sequence with (a) priority scheduling and (b) round-robin scheduling. (P 23-24 First)

| Process   | Arrival Time  | Execution Time    | Priority  |
|-----------|---------------|-------------------|-----------|
| P1        | 0             | 8                 | 3         |
| P2        | 1             | 4                 | 1         |
| P3        | 4             | 2                 | 2         |