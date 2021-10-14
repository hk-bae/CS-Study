# DNS

- Domain Name System: IP를 이름으로 바꿔주는 서비스 + 프로토콜
- Application layer 프로토콜
- Client-Server 모델 사용
- Transport Layer의 UDP 프로토콜과 함께 동작
  - UDP ==> Transport Layer의 최소 기능만으로 동작
  - 연결 상태 유지하지 않음 ==> 정보 기록 최소화 + 연결 비용이 없음
  - 더 많은 클라이언트 수용 + 빠른 동작

![스크린샷 2021-10-13 오후 9 37 50](https://user-images.githubusercontent.com/40057032/137134081-af71f9b5-b733-4517-aac8-2e4a1ded71db.png)
![image](https://user-images.githubusercontent.com/40057032/137141099-e3a85ca2-1ff6-4dc5-9f27-71a67c615fb4.png)


## DNS Server

- 세 가지의 DNS 서버가 있고, 이들은 `distributed, hierarchical` 데이터베이스
  - 분산 데이터베이스: 데이터베이스 복제 및 분산을 통해 데이터베이스 내의 데이터를 여러 물리적 위치에 분산 배치
  - 계층형 데이터베이스: 트리 형태 구조로 조직되어, 부모-자식 관계를 이룸

![스크린샷 2021-10-13 오후 9 50 38](https://user-images.githubusercontent.com/40057032/137135940-53f831fc-ddba-48b5-badf-d5caf0d1c2ea.png)

- DNS 서버의 종류
  1. Root DNS Servers
  2. Top-Level Domain (TLD) Servers
  3. Authoritative DNS Servers
  - 그 하위의 local DNS Server는 DNS 계층 구조에 공식적으로 속하지 않으며, 이용의 편의를 위하여 구현된 것

- 서버가 분산된 이유
  - A single point of failure. 
    - 하나의 서버에 집중된다면, 해당 서버에 오류 발생 시 피해가 매우 클 것
  - Traffic volume. 
    - 수많은 클라이언트가 DNS 요청을 보내오기 때문에, 트래픽을 분산시켜야 함
  - Distant centralized database. 
    - 단일 DNS만 운영한다면, 해당 서버와 물리적으로 멀리 떨어진 사용자는 속도가 매우 느릴 것임
  - Maintenance. 
    - 점검 등의 이유로 DNS 서버가 중단되면 인터넷이 마비됨

### Root Name Server

- 호스트 이름을 IP 주소로 변환하는 첫 단계
- UDP 패킷의 크기 제한으로 인하여 루트 서버 개수를 13개로 제한하고 있음 (전 세계에 13개 뿐)
- 도메인 네임 쿼리가 도착하면 ==> 요청된 도메인의 TLD 권한을 갖는 네임서버 이름, 주소 정보 제공
- 네임 쿼리가 집중됨 ==> DNS 캐싱 등으로 쿼리 집중 경감

### Top-Level Domain Server (TLD)

- 도메인 네임의 가장 마지막 부분
- 전 세계 모든 도메인 정책과 TLD를 추가 및 관리하는 기관은 [ICANN](www.icann.org)
- Generic domains: com, org, net, edu, aero, jobs, ...
  - 각 등록관리 기관에서 인정한 여러 등록 대행업체를 통하여 관리가 이루어짐 (Verisign 등)
- Country domains: kr, uk, fr, ca, jp ...
  - 일반적으로 각 나라의 도메인 관리센터(NIC: Network Information Center)에서 등록 및 관리


### Authoritative DNS Server

- 조직/기관의 DNS 서버 ==> 실제 개인 도메인과 IP주소의 관계가 기록/저장/변경됨
- 호스트 네임과 IP의 매핑을 제공
- 서비스 제공자나 기관에서 직접 관리됨


### Local Name Server

- 계층 구조에 포함되어 있지는 않고, 프록시처럼 행동함
- 최근에 사용된 name-to-address 정보를 캐싱하고 있음
- 호스트가 DNS 쿼리를 전송하면, Local DNS 서버로 전송됨




## Name Resolution

1. Recursive 쿼리 (기본값)
  - 실제 domain name을 가지고 있는 서버까지 쿼리가 전달되어 IP 주소를 얻음
  - 요청된 쿼리에 대하여 응답을 할 수 없으면, 응답을 줄 수 있는 서버에 직접 요청을 보냄   
  - root 서버에 부담이 될 수 있음   
  ![image](https://user-images.githubusercontent.com/40057032/137142710-c3597e1c-a242-4091-ab34-1cc8aaa6d2e1.png)

2. Iterative 쿼리 (non-recursive)
  - 최종 IP 주소를 받을 때까지 local name server가 요청&응답을 반복
  - 요청된 쿼리에 대하여 응답을 할 수 없으면, 응답을 줄 수 있는 서버에 대한 정보를 알려줌   
  ![image](https://user-images.githubusercontent.com/40057032/137142696-028a125e-d0b7-443e-a666-860d2dc007af.png)

- 실제로는 두 방법을 혼합하여 사용
![image](https://user-images.githubusercontent.com/40057032/137317983-4b2fb95f-b1d6-4fc7-b467-ab4aac36a8e3.png)


### DNS 캐싱
  - 한 번 요청된 DNS 쿼리를 TTL만큼 **메모리**에 저장해 뒀다가, 똑같은 쿼리를 빠르게 처리
  - 퍼포먼스 딜레이 줄이기, 인터넷의 DNS 메시지 줄이기
  - 보통 TLD 서버가 로컬 네임 서버에 캐싱되어 있다 ==> root name server까지 도달하는 경우는 잘 없음
  - **문제)** 네임 호스트의 IP 주소가 바뀐다면?
    - TTL 만료 전까지 알아차리지 못할 수도 있다
    - **해결)** update/notify 메커니즘


## Resource Records (RRs)

`RR format: (name, value, type, ttl)`
- type=A
  - name = hostname
  - value = IP address
- type=CNAME
  - name = alias name for 'canonical' name
  - value = canonical name
- type=NS
  - name = domain
  - value = authoritative name server의 hostname
- type=MX (이메일 서버 관리)
  - value = name의 mail server 이름


# 참고자료

- 아주대학교 소프트웨어학과 노병희 교수님 컴퓨터네트워크 강의 자료 (2019 가을 학기)
- https://wookiist.dev/50
- https://opentutorials.org/module/288/2802
- https://ssudalim.tistory.com/4
