# TCP 3 way handshake & 4 way handshake

생성일: 2021년 8월 29일 오후 9:29
속성: 2021년 9월 9일 오후 9:04
태그: 예진, 완료

# TCP?

- OSI TCP/IP 7계층 중 4번째 레이어 Transport 계층에 속하는 프로토콜
- 전송 제어 프로토콜(Transmission Control Protocol)

    → 인터넷 상에서 데이터를 메시지 형태로 보내기 위해 IP와 함께 사용하는 프로토콜

**패킷**: 데이터를 여러 개의 조각으로 나누어 전송할 때 그 조각

## 특징

- 3단계 통신
    1. 연결 수립 (Connection Establish)
    2. 데이터 전송 (Data Transfer)
    3. 연결 종료 (Connection Termination)
- 서버 - 클라이언트 간 긍정 확인응답 방식 (Positive ACK Mechanism)

**긍정 확인응답**: **ACK** 신호, 수신 측에서 메시지를 에러 없이 정상적으로 수신했다 / 송신해도 된다는 것을 송신 측에 알리는 신호
**부정 확인응답**: **NACK** 혹은 **NAK** 신호, 정상적으로 수신되지 않았음을 송신측에 알림 → 잘 쓰이지 않는다

- Packet Switching 방식
    - *Circuit Switching vs Datagram Packet Switching*

        **Circuit Switching**

        <img width="492" alt="스크린샷_2021-09-09_오후_5 04 25" src="https://user-images.githubusercontent.com/40057032/133558556-35ef1245-22d2-42f8-a614-80e66ca75154.png">

        
        - 하나의 회선을 할당받아 데이터를 주고받는 방식, 물리적 연결
        - 회선 전체를 독점하므로 끼어들 수 없고, 속도 성능 일정
        - 전화 등의 실시간 통신에 사용

        **Datagram Packet Switching**

        <img width="542" alt="스크린샷_2021-09-09_오후_5 04 48" src="https://user-images.githubusercontent.com/40057032/133558562-d4822f2a-29d1-4e86-a450-bc4b28f85848.png">


        - 데이터를 패킷 단위로 쪼개어 전송, 라우터 알고리즘으로 여러 라우터를 거쳐 최종 목적지에 도달
        - 물리적 연결이 아님
        - TCP의 경우, 메시지 수신 성공 여부를 송신자가 알 수 있음

## 제공하는 기능

- **Connection-oriented (연결 지향 프로토콜)**
    - 데이터가 전송되기 전에 통신 세션 또는 반영구적 연결이 설정되어 데이터가 올바른 순서로 네트워크에 전달되는지 확인
- 믿을 수 있고 순서가 보장됨
- **point-to-point** → 송신자와 수신자가 일대일 연결됨
- **full duplex service** → 쌍방향 통신, 데이터 송수신을 위해 독립된 회선을 사용 (동시에 송수신 가능)
- **piplined** → 한 window 안의 패킷은 ACK 받지 않고 한 번에 보내기 가능, 패킷의 수를 줄일 수 있다.

    <img width="216" alt="스크린샷_2021-09-09_오후_5 12 13" src="https://user-images.githubusercontent.com/40057032/133558605-28663cc2-1223-414f-9c93-fa24e95cb63f.png">


- **flow controlled** → 흐름 제어, 송신측의 속도를 조절하여 너무 많은 패킷이 전송되지 않게 한다
    - *Sliding Window*

        <img width="622" alt="스크린샷_2021-09-09_오후_5 37 33" src="https://user-images.githubusercontent.com/40057032/133558620-1f2762e3-05f5-4b6c-b6b6-4f8e28dbb5a9.png">


        현재 윈도우에 포함된 모든 패킷 전송 → 전달 확인되는대로(ACK) 윈도우를 옆으로 옮김(Slide) → 윈도우 내 전송되지 않은 패킷 전송

# TCP 연결 관리

<img width="773" alt="스크린샷_2021-09-09_오후_5 13 57" src="https://user-images.githubusercontent.com/40057032/133558639-3f32d414-c519-4029-95ca-2de8a8fbd056.png">


Finite State Machine

<img width="524" alt="스크린샷_2021-09-09_오후_4 56 17" src="https://user-images.githubusercontent.com/40057032/133558661-71e7b8ee-6ea5-4d26-b3b3-c8fd269921a3.png">


전체 전송 과정

## 연결 수립 - 3 way handshake

<img width="964" alt="스크린샷_2021-09-09_오후_5 14 58" src="https://user-images.githubusercontent.com/40057032/133558756-8174d766-30ef-44a4-818c-5ec1e90c01f1.png">



- TCP 통신을 위하여 네트워크를 연결하는 과정
- 포트를 확인하고 연결하기 위하여 세 번의 요청과 응답이 이루어짐

### 연결 과정

1. 클라이언트에서 서버로 SYN 데이터 전송 (연결 요청)
2. 서버에서 SYN 데이터 받으면 요청을 받던 포트가 LISTEN → SYN_RCV로 상태 변경
3. 서버가 클라이언트에게 ACK + SYN 전송
    - ACK → 요청 정상적으로 잘 받았다
    - SYN → 포트를 열어달라
4. 클라이언트가 ACK+SYN 받으면 closed → ESTABLISHED로 상태 변경하고 서버에 ACK 전송
5. 서버가 ACK 받으면 상태가 SYN_RCV → ESTABLISHED로 상태 변경
6. 서로의 포트가 ESTABLISHED 되면 연결 완료

## 연결 해제 - 4 way handshake

<img width="899" alt="스크린샷_2021-09-09_오후_5 15 20" src="https://user-images.githubusercontent.com/40057032/133558745-b40a5a6e-e602-4082-826c-eaaca4437f2a.png">



### 해제 과정

1. 클라이언트가 FIN 플래그 전송 → FIN-WAIT 상태
2. 서버가 FIN 플래그 수신, 클라이언트에 ACK 전송 → CLOSE_WAIT 상태 (자신이 보낼 데이터 다 보낼 때까지 대기)
3. 서버가 연결 종료할 준비가 되었으면 클라이언트에 FIN 전송 → LAST_ACK 상태
4. 클라이언트가 FIN 수신, 해지 준비 되었으면 ACK 전송 → TIME-WAIT 상태
5. 클라이언트는 MSL * 2만큼 기다린 후에 CLOSED 상태

### MSL - Maximum Segment Lifetime

<img width="705" alt="스크린샷_2021-09-09_오후_5 18 14" src="https://user-images.githubusercontent.com/40057032/133558717-6c546eb5-ac1d-438f-b0a7-fdf0880c530d.png">


- TCP 세그먼트가 네트워크 시스템에 존재할 수 있는 최대 시간 (보통 30초, 1분, 2분)
- 서로 FIN 보내고 그 ACK를 받고 나면, 클라이언트는 MSL 만큼 기다린 후에 완전히 연결 해제

**이렇게 하는 이유**

1. 연결 종료 시에 신뢰성 보장을 위해 서로 CLOSE된 것을 확인하기 위함
    - 위 이미지처럼 연결 종료 중 ACK가 손실된 경우 FIN 메시지를 재전송한다. 위 이미지에서 클라이언트가 ACK N+1 보낸 직후 바로 CLOSE 되었다면, 서버는 클라이언트에게 ACK 받지 못하고 RST 세그먼트만 받을 수 있을 것임 (RST: 유효하지 않은 요청)
2. 네트워크에 있는 중복 세그먼트를 소멸시키기 위함
    - 연결되어 있던 서버-클라이언트가 연결 해제되었는데, 그 연결에서 라우터 오류로 처리되지 않은 패킷이 있다면? 서버-클라이언트 새로 연결되었을 때 처리되지 않은 패킷이 목적지로 도착하게 되는 오류가 발생할 수 있음.
    - TIME_WAIT 상태에 있는 동안은 같은 연결이 발생하지 못함 → MSL 만큼 기다리는 동안 해당 연결에 보내진 패킷이 모두 소멸되었다고 확신 가능함
    - 만일 TIME_WAIT 중 도착하는 패킷이 있다면 그 패킷은 drop
- 참고: [https://otsteam.tistory.com/264](https://otsteam.tistory.com/264)

# TCP State

- [https://coding-factory.tistory.com/614](https://coding-factory.tistory.com/614)
- **LISTEN :**서버의 데몬이 떠서 접속 요청을 기다리는 상태
- **SYN-SENT :**로컬의 클라이언트 어플리케이션이 원격 호스트에 연결을 요청한 상태
- **SYN_RECEIVED :**서버가 원격 클라이언트로부터 접속 요구를 받아 클라이언트에게 응답을 하였지만 아직 클라이언트에게 확인 메시지는 받지 않은 상태
- **ESTABLISHED :**3 way-handshaking 이 완료된 후 서로 연결된 상태
- **FIN-WAIT1, CLOSE-WAIT, FIN-WAIT2 :**서버에서 연결을 종료하기 위해 클라이언트에게 종결을 요청하고 회신을 받아 종료하는 과정의 상태
- **TIME-WAIT :**연결은 종료되었지만 분실되었을지 모를 느린 세그먼트를 위해 당분간 소켓을 열어두고 있는 상태
- **CLOSING :**흔하지 않지만 주로 확인 메시지가 전송도중 분실된 상태
- **CLOSED :**완전히 종료

# TCP 헤더

![스크린샷_2021-09-08_오후_8 19 37](https://user-images.githubusercontent.com/40057032/133558814-a18ce7b6-a7fc-4b86-8486-dbd419e57e94.png)


# 제어 비트

![스크린샷_2021-09-08_오후_8 19 50](https://user-images.githubusercontent.com/40057032/133558809-01cd14d6-5407-4d91-8363-8e325a708dc6.png)
