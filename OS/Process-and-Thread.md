# 프로세스 & 쓰레드

## 1. 프로세스

### 1-1. 프로세스의 정의

<img width="619" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-01%20%EC%98%A4%ED%9B%84%2010 47 00" src="https://user-images.githubusercontent.com/68215452/133197574-bd7846a0-e022-4538-a52e-3439a3ae9cd6.png">

- **실행중인 프로그램의 인스턴스**
- 프로세스는 Active와 Dynamic한 상태
    - 프로그램은 static, passive한 상태로 디스크에 저장되어 있는 상태
    - 디스크에 저장된 프로그램이 메모리에 올라와 **시스템 리소스(CPU, 주소 공간, 메모리, 파일 등)을 할당받을 수 있는 상태**가 되면  프로세스!
- C++/C#/Java
    - Class → 'Program'
    - Object → 'Process'

### 1-2. 프로세스 특징

- **각 프로세스는 개별 메모리 주소 공간을 할당 받는다.**
    - `Code`
        - instruction 등을 저장
        - Program Counter가 다음 실행될 명령을 가리킨다.
    - `Data` : 전역 변수 저장 등
    - `Heap` : 런타임에 동적으로 할당되는 메모리 공간
    - `Stack` : 임시 데이터 포함
        - 함수 파라미터, 리턴 주소, 지역 변수 등
        - Stack Pointer가 가리키고 있음

- 프로세스 상태전이

    <img width="686" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-01%20%EC%98%A4%ED%9B%84%2010 54 02" src="https://user-images.githubusercontent.com/68215452/133197579-79cf8786-48b2-4688-8ac5-40eec030b839.png">
    
    - `new` : 생성
        - 프로세스 최초 상태.
        - 주메모리가 아닌 보조메모리에 저장되어 있음
    - `ready` : 준비
        - 프로세스가 CPU를 차지하기 위한 준비가 된 상태
        - 프로세스 우선순위에 따라 대기 큐에 정렬됨
    - `running` : 실행
        - 프로세스가 CPU를 점유하여 실행중인 상태
    - `waiting` : 대기
        - I/O 이벤트 등이 들어와서 대기중인 상태
    - `terminated` : 종료
        - 프로세스 실행이 완료되어 자원을 반납한 상태

## 2. 쓰레드

### 2-1. 쓰레드 정의 및 특징

- **프로세스가 할당받은 자원을 이용하여 실행되는 여러 흐름의 단위**
    - 모든 프로세스는 한 개 이상의 쓰레드를 갖게 된다.
    - Main Thread

- **쓰레드들은 프로세스의 주소 공간을 공유하면서 자기 자신의 Stack 영역을 갖는다.**
    - `Code`, `Data`, `Heap` - 같은 프로세스 내의 쓰레드들이 공유
    - `Stack` - 개별 스택영역을 가짐
        - 개별 Program Counter, Stack Pointer를 보유!

        <img width="566" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-01%20%EC%98%A4%ED%9B%84%2011 29 02" src="https://user-images.githubusercontent.com/68215452/133197585-57c318d7-de44-4545-bdd7-9791acfae00d.png">

        - 여러 개의 실행흐름을 만들어 내기 위해서 Stack 영역, Program Counter, Stack Pointer를 독립적으로 갖는 것은 당연하다.
        - 스택 메모리 공간이 독립적이라는 것은 독립적인 함수 호출이 가능하다는 것을 의미하고 이는 독립적인 실행흐름이 추가되는 것이다.
        - PC를 독립적으로 갖는 이유는 쓰레드가 CPU를 할당받았다가 스케줄러에 의해 선점을 당할 수 있기 때문에 명령어가 연속적으로 수행되지 못한다. 따라서 어느 부분까지 수행 했는지 쓰레드 별로 기억을 할 필요가 있다.

### 2-2. 멀티쓰레드

- [참고] **Concurrency vs Parallelism**
    - `Concurrency`(병행성)은 S/W적인 성질, `Parallelism`(병렬성)은 H/W적인 성질이라고 생각하면 이해하기 쉽다.

    - `Concurrency`
    
        <img width="460" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-01%20%EC%98%A4%ED%9B%84%2011 45 59" src="https://user-images.githubusercontent.com/68215452/133197936-d88994d5-6a89-4eba-800b-44f464539582.png">
        

        - 어떤 프로그램이나 알고리즘이 순서와 상관없이 동시에 수행될 수 있다면 concurrent하다고 한다.
        - 이는 싱글 코어 머신에서 멀티쓰레드로 구현할 수 있다.

            → 사용자에게 동시에 수행되는 듯한 **abstraction**을 제공하는 것

    - `Parallelism`
        <img width="329" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-01%20%EC%98%A4%ED%9B%84%2011 47 39" src="https://user-images.githubusercontent.com/68215452/133197941-e8e17497-1256-4099-ad19-71e0dd0b01b3.png">
        

        - 실제로 작업이 동시에 처리. 멀티코어에서만 가능

- 멀티쓰레딩이란 하나의 응용프로그램을 여러 개의 스레드로 구성하고 각 스레드로 하여금 하나의 작업을 처리하도록 하는 것이다.
- 윈도우, 리눅스 등 많은 운영체제들이 멀티 프로세싱을 지원하고 있지만 멀티 스레딩을 기본으로  하고 있다.
    - [참고] 멀티 쓰레딩 모델

        스레드는 크게 `Kernel thread`와 `User Thread`로 분리할 수 있다.

        - 커널 쓰레드
            - O.S에 의해 직접적으로 관리되는 스레드이며 **스케줄링의 단위**
            - 현대의 모든 OS들은 커널 스레드를 지원한다.
        - 유저 쓰레드
            - 사용자 레벨에서의 스레드 라이브러리에 의해 생성, 관리되는 스레드
            - 커널 보다 위의 단계인 사용자 레벨에서 구현된다.

        - `One-to-One`
        
            <img width="511" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-02%20%EC%98%A4%ED%9B%84%201 06 18" src="https://user-images.githubusercontent.com/68215452/133197908-af42f049-0ba7-4733-bb4f-b6084d4c684e.png">
            

            - 각각의 유저 스레드는 커널 스레드와 매핑되어 있다.
                - 유저 레벨의 스레드를 생성하면 커널 스레드가 함께 생성된다.
            - many-to-one 모델보다 더 높은 Concurrency를 보장
            - 커널 스레드의 과도한 생성으로 인해 오버헤드가 커진다.
            - 리눅스,윈도우 등 `One-to-One` 모델을 채택

        - `Many-to-one`
        
            <img width="284" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-02%20%EC%98%A4%ED%9B%84%201 08 53" src="https://user-images.githubusercontent.com/68215452/133197916-b38bfdb5-5206-4a1e-9942-0f7aebd71ae0.png">
            

            - 다수의 유저 스레드는 하나의 커널 스레드와 매핑되어 있다.
            - 유저 스레드 간의 context switching이 빠르다.
            - 유저 스레드를 처리하던 중 시스템 콜에 의해 blocking된다면 전체 프로세스에 Deadlock이 발생되는 문제점
            - 멀티코어에서 parallel하게 동작하지 않음
                - 커널 스레드가 하나 뿐이기 때문에 Concurrency만 제공
            - Solaris Green Threads, GNU Portable Threads

        - `Many-to-Many`

            <img width="382" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-02%20%EC%98%A4%ED%9B%84%201 12 33" src="https://user-images.githubusercontent.com/68215452/133197921-99e59ec2-c989-4fa1-af80-72dcd9970c2c.png">

            - 다수의 유저 스레드를 다수의 커널 스레드가 처리하는 구조
            - 커널 스레드의 숫자는 유저 스레드의 숫자보다 같거나 작게 할당되어야 한다.

        - `Two-level Model`

            <img width="426" alt="%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-09-02%20%EC%98%A4%ED%9B%84%201 14 19" src="https://user-images.githubusercontent.com/68215452/133197930-8abdadc6-f7ec-470d-baac-a4bbdfa49a1e.png">

            - 중요한 작업은 One-to-One 구조로 처리
            - 나머지는 Many-to-Many로 처리하여 혹시나 있을 중요한 작업에서의 기다림 현상을 줄일 수 있다.

## 3. 프로세스 vs 쓰레드

### 3-1. 프로세스, 쓰레드 개념 비교

- 프로세스는 여러 개의 쓰레드를 가질 수 있다.
    - 쓰레드는 단일 프로세스에 속하게 된다.

- 프로세스는 쓰레드가 실행되는 컨테이너 역할을 한다.
    - 프로세스의 Code, Heap, Data 영역을 공유한다.

        → PID, address space, user&group ID, open file descriptors, current working directory ...

        - 모든 쓰레드들은 같은 주소 공간을 바라보고 있다.
        - 쓰레드들은 스케줄링의 단위가 된다.

### 3-2. 멀티프로세스 vs 멀티쓰레드

- **멀티프로세싱**
    - 장점
        - 여러 개의 자식 프로세스 중 하나에 문제가 발생하면 그 자식 프로세스만 죽는 것 이상으로 다른 영향이 확산되지 않는다.
    - 단점
        - Context Switching에서의 오버헤드
            - 프로세스 사이에서 공유하는 메모리가 없음
            - Context Switching이 발생하면 캐시에 있는 모든 데이터를 모두 리셋하고 다시 캐시 정보를 불러와야 한다.
        - 프로세스 사이의 복잡한 통신 기법 (IPC)
            - 프로세스는 각각의 독립된 메모리 영역을 갖기 때문에 하나의 프로그램에 속하는 프로세스들 사이의 변수를 공유할 수 없다.

- **멀티쓰레딩**
    - 장점
        - **자원의 효율성 증대 및 시스템 처리량 증가(처리비용 감소 및 응답시간 단축)**
            - 프로세스를 생성하여 자원을 할당하는 **시스템콜이 줄어들어** 자원을 효율적으로 관리할 수 있다.
                - 프로세스 간의 Context-Switching 시에 단순히 CPU 레지스터의 교체 뿐만이 아니라 RAM과 CPU 사이의 캐시 메모리에 대한 데이터 초기화가 진행 (시스템 콜)
            - 스레드 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어들게 된다.
            - 스레드 사이의 작업량이 작아 **Context Switching이 빠르다.**
                - 스레드는 Stack 영역만 바꿔주면 된다!
            - **간단한 통신 방법**으로 인한 프로그램 응답 시간 단축
                - 스레드는 프로세스 내의 스택 영역을 제외하고 모두 공유하기 때문에 통신 부담이 적다.
    - 단점
        - 주의 깊은 설계를 요함
        - 디버깅이 까다로움
        - 단일 프로세스 시스템의 경우 효과를 기대하기 어렵다.
            - 잦은 context-switching이 일어난다면 오히려 속도가 저하될 우려가 있음
        - 다른 프로세스에서 스레드를 제어할 수 없다.
        - **동기화 문제**
        - 하나의 스레드에 문제가 발생하면 전체 프로세스가 영향을 받는다.

---

- 프로세스란?
- 프로세스의 특징?
- 스레드란?
- 스레드의 특징?
- 프로세스와 스레드의 차이점
- 멀티스레딩의 장단점
- 멀티스레딩과 멀티프로세싱의 차이점

---

* 참고자료

  * 아주대학교 O.S 강의자료

  * https://github.com/Songwonseok/CS-Study

  * https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS

  * https://gmlwjd9405.github.io/tags.html#면접
