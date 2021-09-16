# HTTP?

- Hyper Text Transfer Protocol
- OSI 7 레이어의 가장 상위 레이어인 Application Layer에 속하는 프로토콜
- **웹**의 어플리케이션 레이어 프로토콜 → TCP(Transport Layer) 위에서 작동
- 클라이언트 - 서버 모델
    - 클라이언트
        - HTTP 프로토콜을 통해 요청을 하거나 응답을 받음
        - 웹 오브젝트를 표시
    - 서버
        - 요청을 받고 그에 대한 응답 오브젝트를 전송

## 웹페이지

- 오브젝트로 이루어짐
- several referenced objects를 포함한 base HTML 파일로 이루어짐
- **오브젝트**: HTML 파일, 이미지, 자바 applet, 오디오 파일, ...
    - 각 오브젝트는 URL 주소로 표현할 수 있다.

# HTTP Connections

## non-persistent HTTP (HTTP/1.0)

- TCP 커넥션을 통해 최대 하나의 오브젝트만 전송 가능 → 전송 완료되면 커넥션 종료

## persistent HTTP (HTTP/1.1 & HTTP/2)

- 하나의 TCP 커넥션에서 여러 개의 오브젝트 전송 가능
- 장점: CPU ⬇️, Memory ⬇️, RTT(패킷 왕복 시간) ⬇️, Congestion(혼잡) ⬇️
- 단점: 클라이언트가 모든 데이터를 받고 나서 TCP 커넥션을 끊을 수 있기 때문에, 그 전까지 서버는 커넥션을 열고 있어야 한다 → 다른 클라이언트는 사용 불가능

# 메시지 포맷

- Request
- Response

<img width="679" alt="스크린샷 2021-09-16 오후 3 25 52" src="https://user-images.githubusercontent.com/40057032/133563983-5cd203df-3be6-4873-95f5-fd85755b625c.png">


## Request Methods

<img width="639" alt="스크린샷 2021-09-16 오후 3 26 42" src="https://user-images.githubusercontent.com/40057032/133563997-447cf955-3b8e-4f73-8b20-e16e94da5614.png">


## Response Status Code

<img width="610" alt="스크린샷 2021-09-16 오후 3 26 51" src="https://user-images.githubusercontent.com/40057032/133564003-21118959-08d0-4bd5-92fc-7b7996a9e474.png">


# HTTPS (HTTP Secure)

## HTTP's Security Concerns

### Privacy

- HTTP 메시지는 ASCII 텍스트이므로 누구나 메시지 내용 열람 가능

### Integrity

- HTTP 메시지 암호화가 지원되지 않음 → 통신 중간에서 누군가 도청하거나 공격할 수 있음

### Authentication

- 통신 중인 상대가 누구인지 분명하지 않음, 누군가 인증 메시지를 갈취했을 수도 있음

## HTTP Secure

<img width="349" alt="스크린샷 2021-09-16 오후 3 32 56" src="https://user-images.githubusercontent.com/40057032/133564027-39785f62-f680-4fc9-9862-8c63663604df.png">


- HTTP와 TCP 사이에 Sublayer가 추가됨 (TLS/SSL)
    - HTTPS = HTTP + TLS / SSL
    - SSL(Secure Socket Layer), formerly
    - TLS(Transport Layer Security)
    - SSLv3 ≒ TLS → TLS로 명칭 변경됨
        - 데이터 암호화 수행
        - 무결성을 위한 MAC을 생성 → 의도한 사이트와 통신 중임을 인증
        - 참고: [https://itwiki.kr/w/TLS(SSL)](https://itwiki.kr/w/TLS(SSL))

### 특징

- URL이 `https://` 로 시작함
- 443번 포트를 사용 (HTTP는 80번 사용)
- 전송 전에 메시지를 암호화하고, 도착하면 메시지를 복호화함
- HTTP보다 약간 느림, 계산 + 네트워크 오버헤드 존재
- SEO 혜택 → 구글이 HTTPS 사이트에 검색 가산점
