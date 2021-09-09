# PCB와 Context Switching

## 1. PCB(Process Control Block)

### 1-1 PCB 개요

![스크린샷 2021-09-08 오후 11.42.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5af5438d-9d0a-4cee-a6ce-da70d3fb0313/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-08_%EC%98%A4%ED%9B%84_11.42.21.png)

- PCB는 **특정 프로세스에 대한 중요한 정보**를 저장하고 있는 운영체제의 자료구조!
- 운영체제는 프로세스를 관리하기 위해 **프로세스의 생성과 동시에 고유한 PCB를 생성**
  - PCB는 Linked List 방식으로 관리된다.
  - PCB List Head에 PCB들이 생성될 때 마다 붙게 되는 방식
  - 주소 값으로 연결이 이루어져 있기 때문에 **삽입, 삭제가 용이**
  - **즉 프로세스가 생성되면 해당 PCB가 생성되고 프로세스가 완료된 PCB는 제거된다.**
- PCB가 저장되는 위치
  - PCB가 프로세스의 중요한 정보를 포함되고 있기 때문에, 일반 사용자가 접근하지 못하도록 보호된 메모리 영역 안에 남는다.
  - 일부 운영 체제에서 PCB는 커널 스택의 처음에 위치한다.
    - 이 메모리 영역은 편리하면서도 보호를 받는 위치이기 때문이다.

### 1-2 PCB가 왜 필요할까?

- 프로세스는 CPU를 할당받아 작업을 처리하다가도 프로세스 전환이 발생하면 진행하던 작업을 저장하고 CPU를 반환한다.

  → 이때 작업의 진행상황을 모두 PCB에 저장 !

- 다시 CPU를 할당받게 되면 PCB에 저장되어 있던 내용을 불러와 이전에 종료됐던 시점부터 다시 작업을 수행한다

- 한 줄 요약

  - Context Switching을 할 때 프로세스에 대한 정보를 PCB에 저장하거나 빼온다!

![스크린샷 2021-09-08 오후 11.33.31.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dfb5db1c-8753-460a-96b3-239f654d68fa/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2021-09-08_%EC%98%A4%ED%9B%84_11.33.31.png)

- PCB에 저장되는 정보
  - 프로세스 식별자 (PID)
  - [프로세스 상태 (new, ready, running, waiting, terminated)](https://www.notion.so/506b28a8c8904e8ea2bb76ebed421216)
  - PC : 프로세스가 다음에 실행할 명령어의 주소
  - CPU 레지스터
  - CPU 스케줄링 정보 ( 프로세스 우선순위, 스케줄 큐에 대한 포인터 등)
  - 메모리 관리 정보 ( 페이지 테이블 또는 세그먼트 테이블 등과 같은 정보를 포함)
  - 입출력 상태 정보 ( 프로세스에 할당된 입출력 장치들과 열린 파일 목록 )
  - 어카운팅 정보 ( 사용된 CPU 시간, 시간제한, 계정 번호 등)

## 2. Context Switching

### 2-1 Context Switching 개요

- CPU가 **현재 실행하는 Task** (프로세스 또는 스레드)의 상태를 **저장하고**
- **다음 진행할 Task**의 상태 및 Register 값들에 대한 정보를 **읽어**
- 새로운 Context 정보로 **교체하는 과정**
- 'CPU가 이전의 프로세스 상태를 PCB에 보관하고, 또 다른 프로세스의 정보를 PCB에서 읽어 레지스터에 적재하는 과정'
- '다중 프로그래밍 시스템에서 CPU가 할당되는 프로세스를 변경하기 위해 현재 CPU를 사용하여 실행되고 있는 프로세서의 상태 정보를 저장하고 제어권을 인터럽트 서비스 루틴(ISR)에게 넘기는 작업

### 2-2 Context Switching이 왜 필요할까?

- 멀티 프로세싱, 멀테스레딩같은 다중 프로그래밍 시스템을 구현하기 위함
- CPU가 Task를 바꿔가며 실행하기 위해서 Context Switching이 필요하다.

### 2-2 Context Switching의 수행과정

- Context Switching이 발생하는 상황
  - 인터럽트가 발생
  - 실행중인 CPU 사용 허가 시간을 모두 소모
  - 입출력을 위한 대기를 해야하는 경우
  - 즉 프로세스에서 특정 상태전이가 발생할 때!
    - Ready → Running
    - Running → Ready
    - Running → Block

1. Task의 대부분 정보는 레지스터에 저장되고 PCB로 관리가 되고 있습니다.
   - 여기서 Task가 프로세스일 경우와 쓰레드일 경우는 조금 상이하다.
     - 프로세스의 경우 PCB는 OS에 의해 스케줄링되는 Process Control Block
     - 스레드의 경우 프로세스 TCB(Task Control Bolck)이라는 내부 구조를 통해 관리된다.
2. 현재 실행하고 있는 Task의 PCB 정보를 저장하게 됩니다.
   - 여기서 Task의 PCB 정보는 Process Stack, Ready Queue라는 자료구조로 관리된다.
   - Context Switching 시 PCB 정보를 바탕으로 이전에 수행하던 작업 혹은 신규 작업의 수행이 가능해진다.
3. 다음 실행할 Task의 PCB 정보를 읽어 레지스터에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행할 수 있게 됩니다.

### 2-3 Context Switching의 Cost

- 잦은 Context Switching은 성능 저하를 가져온다.
  - Cache 초기화
  - Memory Mapping 초기화
  - 메모리의 접근을 위해서 커널은 항상 실행되어야 한다.
- **성능 : 멀티 스레딩 > 멀티 프로세싱**
  - 레지스터 개수가 많은 수록, 프로세스 별로 관리되어야 할 데이터 종류가 더 많을 수록 부담된다.
  - 컨텍스트 스위칭에 소요되는 시간을 줄이려면 저장하고 복원하는 컨텍스트 정보의 개수를 줄여주면 된다.
  - **메모리 구조 관점**
    - 프로세스 - Code, Data, Heap, Stack 영역이 독립적
    - 스레드 - Code, Data, Heap을 공유
    - 스레드는 공유하는 영역이 많기 때문에 컨텍스트 스위칭이 빠르다.
  - **캐시 관점**
    - 캐시란?
      - 캐시는 CPU와 메인 메모리 사이에 위치하며 CPU에서 한번 이상 읽어들인 메모리의 데이터를 저장하고 있다가, CPU가 다시 그 메모리에 저장된 데이터를 요구할 때, 메인메모리를 통하지 않고 데이터를 전달해주는 용도이다.
    - 프로세스 - 공유하는 데이터가 없으므로 지금껏 쌓아놓은 데이터들이 무너지고 새로운 캐시 정보를 쌓아야한다. 모든 캐시 정보를 초기화해야 한다.
    - 스레드 - 저장된 캐시 데이터는 쓰레드가 바뀌어도 공유하는 데이터가 있으므로 의미있다.

### 2-4 Context Switching과 시간 할당량

- 인터럽트 관련
  - CPU는 하나의 프로세스 정보만 기억한다!
  - 다중 프로그래밍 환경에서는 Context Switching을 통해 프로세스의 정보를 바꿔준다!
  - 프로세스의 저장과 복귀는 프로세스의 중단과 실행을 의미한다.
  - 프로세스의 중단과 실행 시 인터럽트가 발생하므로 **Context Switching이 많이 일어난다는 것은 인터럽트가 많이 발생한다는 것**을 의미!
- 프로세스들의 시간 할당량은 시스템 성능에 중요한 역할을 한다.
  - **시간할당량이 적을 수록**
    - 사용자 입장에서는 여러 개의 프로세스가 동시에 수행되는 느낌을 갖는다 👍
      - = 높은 Concurrency 제공
    - 인터럽트의 수와 문맥 교환 수, 오버헤드가 늘어난다. 😭
      - = 성능 저하
  - **시간 할당량이 커질 수록**
    - 문맥 교환 수, 인터럽트 횟수, 오버헤드가 감소한다.  👍
      - = 성능 improve
    - 여러 개의 프로세스가 동시에 수행되는 느낌을 갖지 못한다. 😭
      - = Concurrency 감소

------

- 참고 자료

  https://velog.io/@adam2/인터럽트

  https://ko.wikipedia.org/wiki/프로세스_제어_블록#cite_note-2

  https://nesoy.github.io/articles/2018-11/Context-Switching

  https://github.com/Songwonseok/CS-Study/blob/main/OS/PCB%20Context%20Switching.md