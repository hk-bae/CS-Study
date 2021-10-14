# CPU Scheduling
## 개요
* 스케줄링이란?

  * CPU를 잘 사용하기 위해 **프로세스**를 배정하는 알고리즘

  * 각각의 프로세스는 **자기 자신의 주소공간**을 갖게 된다.

    <img width="496" alt="그림1" src="https://user-images.githubusercontent.com/68215452/137302238-3751fbe2-2498-4f3a-82d8-d165eff5115d.png">

  * 프로세스의 개수만큼 프로세서가 존재한다면 스케줄링을 고민할 필요가 없다.

    <img width="230" alt="스크린샷 2021-10-14 오후 2 05 42" src="https://user-images.githubusercontent.com/68215452/137302298-39c8a0c4-cb10-4e15-bec4-f2eff26d33d9.png">

  * 하지만 일반적으로 프로세서의 수보다 실행해야하는 프로세스의 수가 더 많기 때문에 적절한 CPU 스케줄링이 필요하다.




### 스케줄링 척도
<img width="400" alt="그림3" src="https://user-images.githubusercontent.com/68215452/137302373-86f2defb-3ad0-42af-a401-f28342f2a58d.png">

* CPU Utilization : CPU 사용률
* Throughput : 처리량
  * 일정 시간 단위안에 완료되는 프로세스의 개수
* Response Time : 응답시간
  * 작업이 처음 실행되기 까지 걸린 시간
* Waiting Time : 대기시간
  * 대기 큐에서 프로세스가 기다리는 전체 시간
* Turnaround Time : 경과시간
  * 실행 시간과 대기 시간을 모두 합한 시간으로 작업이 완료될 때 까지 걸린 시간
  
### 스케줄링 목표

  * 공통
    * Starvation 없어야 한다.
         * 우선순위가 낮은 프로세스가 영원히 진행되지 못하는 상황
         * 어떤 프로세스를 실행하기 위해 필요한 리소스(CPU,lock)를 우선순위가 더 높은 다른 프로세스가 계속 차지하여 반영구적으로 진행되지 못하는 상황
    * Fairness : 각 프로세스에 CPU를 공정하게 점유시켜야 한다.
    * Balance : 시스템의 모든 부분을 균형있게 사용해야 한다.

  * 시스템의 특징마다 주요한 목표가 조금씩 다르다.	
    * `Batch System` : 처리량, CPU 사용률 
    * `Interactive System` : 빠른 응답시간, 적은 대기 시간


## CPU 스케줄링 종류

* 선점 (preemptive) : OS가 CPU의 사용권을 선점할 수 있는 경우, 강제 회수하는 경우
* 비선점(nonpreemptive) : 프로세스 종료 or I/O 등의 이벤트가 있을 때 까지 실행을 보장


### 비선점 스케줄링

* FCFS (First Come First Served)

  * **큐에 도착한 순서대로 CPU 할당**

  * 일단 CPU를 잡으면 CPU burst가 완료될 때 까지 CPU를 반환하지 않는다. 

  * 비선점 스케줄링 - 할당되었던 CPU가 반환될 때만 스케줄링이 이루어진다.

  * 모든 작업들이 평등하게 다뤄진다. -> No Starvation

  * 문제점

    * **Convey effect** : 소요시간이 긴 프로세스가 먼저 큐에 도달하여 효율성을 낮추는 현상

    * 실행시간이 긴 프로세스가 먼저 들어오면 평균 대기시간이 길어진다.

      <img width="570" alt="그림4" src="https://user-images.githubusercontent.com/68215452/137302537-1bdc0ad1-3c5a-48e1-92d0-449655427c1a.png">

      

* SJF (Shortest Job First)

  * 예상되는 CPU burst가 가장 작은 작업 우선 선택
  * 다른 프로세스가 먼저 도착했어도 CPU burst time이 짧은 프로세스에게 우선 할당
  * **최소 평균 대기 시간을 보장한다.**
    * SJF는 이론상 최적의 스케줄링 알고리즘
    * 하지만 CPU Burst 크기를 정확하게 예측할 수 없다.

  <img width="533" alt="그림5" src="https://user-images.githubusercontent.com/68215452/137302576-c2512396-079a-4a3d-8a2d-f11884b2a166.png">

  * 문제점 

    * starvation (기아 현상)

    * CPU burst time이 긴 프로세스가 거의 영원히 CPU를 할당받을 수 없는 상태

      

* HRN (Highest Response-ratio Next)

  * 우선순위를 계산하여 점유 불평등을 보완한 방법 (SJF의 단점 보완)
  * 우선순위 = (대기시간 + 실행시간) / 실행시간



### 선점 스케줄링

* SRTF(Shortest Remaining Time First)

  * SJF를 선점 방식으로 사용

  * 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다.

  * 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 뺏긴다.

    <img width="485" alt="그림6" src="https://user-images.githubusercontent.com/68215452/137302616-2f3273ee-77be-46d8-95f4-c57adcec4bd8.png">

  * 문제점

    * starvation
    * 새로운 프로세스가 도달할 때마다 스케줄링을 다시하기 때문에 CPU burst time을 정확히 알 수 없다.

  

* Priority Scheduling

  * 정적/동적으로 우선순위를 부여하여 우선순위가 높은 순서대로 처리

  * 문제점

    * **Starvation**

      * 우선순위가 낮은 작업이 영원히 실행되지 못함
      * **Aging** : 대기시간에 비례하여 일정 시간이 지나면 우선순위를 한 단계씩 높여주는 방법

    * **Priority Inversion Problem**

      * 낮은 우선순위 작업이 높은 우선순위 작업에서 필요로 하는 리소스를 가지고 있는 문제

      * 또 다른 낮은 우선순위의 작업이 높은 우선순위의 작업을 간접적으로 선점한다.

        <img width="740" alt="그림7" src="https://user-images.githubusercontent.com/68215452/137302627-c137d8f7-424b-4c74-830b-f03f57e3bb6b.png">

      * **PIP (Priority Inheritance Protocol)** : 높은 우선순위 작업의 우선순위를 물려주는 방법.

        

* Round Robin

  * FCFS에 의해 프로세스들이 보내지면 각 프로세스는 동일한 시간의 Time Qunatum만큼 CPU를 할당 받음

    * Time Quantum or Time Slice : 실행의 최소 단위 시간

    * 할당시간이 짧으면 잦은 Context Switching으로 오버헤드 증가

    * 할당시간이 크면 평균 응답시간이 길어진다. FIFO 방식에 수렴

    * 일반적으로 10 -100ms로 설정

      

* Multilevel-Queue (다단계 큐)

  <img width="646" alt="그림8" src="https://user-images.githubusercontent.com/68215452/137302645-16382d48-b9f0-4e48-b2a7-4f871874dafe.png">

  * 작업들을 여러 종류의 그룹으로 나누어 여러 개의 큐를 이용하는 기법

    * Foreground Queue, Background Queue, ...

  * 각각의 큐들에서 서로 다른 스케줄링 기법을 사용할 수 있다.

  * 큐들간의 스케줄링을 추가로 고려해줘야 한다.

    * 고정 우선 순위 스케줄링
      * starvation 가능성
    * Time Slice
      * 각 큐들마다 시간할당량을 부여

  * 문제점

    * 프로세스의 한번 특정 큐에 들어가면  다른 큐로 이동되거나 변경되는 것이 불가능하다.
    * 작업들의 종류를 파악하기 어렵다.

      

* Multilevel-Feedback-Queue (다단계 피드백 큐)

  <img width="279" alt="스크린샷 2021-10-14 오후 7 44 17" src="https://user-images.githubusercontent.com/68215452/137302779-16347e47-e019-484f-a765-5762592827a3.png">

  * 우선순위 레벨에 따라 여러개의 큐를 사용한다.

  * 각각의 큐들에서 서로 다른 스케줄링 기법을 사용할 수 있다.

    

  * 작업을 보다 높은/낮은 우선순위의 큐로 격상/격하시키는 시기 결정 알고리즘

    * Round Robin/FCFS를 이용한다.
    * 모든 프로세스들이은 우선 제일 위에 있는 큐(가장 높은 우선순위)로 이동한다. 
    * 제일 위에 있는 큐 RR으로 스케줄링을 한다.
    * 자신의 time quantum을 다 채우지 못한 프로세스는 그대로 두고 time qunatum을 다 채운 프로세스는 그 밑의 레벨에 있는 큐로 이동한다. 밑에 있는 큐는 타임 퀀텀의 크기를 첫 번째 큐의 두 배로 상정한다.
    * 마지막 큐는 대게 FCFS로 처리한다.

  
  * CPU burst와 중요도의 상관관계를 이용한 알고리즘이다.
    * 보통 사용자와 interactive하지 않는 백그라운드의 프로세스는 CPU burst가 매우 크다는 특징을 이용
    * CPU burst가 큰 프로세스는 우선순위가 낮은 프로세스로 판단
    * 사용자와 상호작용할 가능성이 높은 프로세스, 입출력을 필요로 하는 프로세스는 CPU처리와 I/O 작업이 번갈아 일어나므로 CPU burst가 작다.
    * 그리고 CPU burst가 작은 작업들은 우선순위가 높다.

  
  * 다단계 큐에서 자신의 Time Quantum을 다 채운 프로세스는 밑으로 내려가고 자신의 Time Quantum을 다 채우지 못한 프로세스는 원래 큐 그대로
    * Time Quantum을 다 채운 프로세스는 CPU burst 프로세스로 판단한다.
  * 짧은 작업에 유리, 입출력 위주(Interrupt가 잦은) 작업에 우선권을 준다.
  * 처리 시간이 짧은 프로세스를 먼저 처리하기 때문에 Turnaround 평균 시간을 줄여줌


## 참고자료
* 아주대학교 O.S 강의자료
* https://jhnyang.tistory.com/156
* https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#cpu-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%9F%AC
* https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/CPU%20Scheduling.md


