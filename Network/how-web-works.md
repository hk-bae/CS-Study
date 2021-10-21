# 웹의 동작 방식 - 주소창에 www.naver.com 을 입력하면 일어나는 일

- 웹에 연결된 컴퓨터의 종류
    - **클라이언트**
        - 일반적 웹 사용자의 인터넷이 연결된 장치(모바일, 컴퓨터 등)
        - 웹브라우저와 같은 소프트웨어
    - **서버**
        - 웹페이지, 사이트, 앱 등을 저장하는 컴퓨터
        - 클라이언트가 웹페이지 접근을 요청하면, 서버가 웹페이지의 사본을 보내줌

## 웹 주소를 입력하면 생기는 일

![image](https://user-images.githubusercontent.com/40057032/138212486-98e336b2-dea8-46c3-8c2a-185163788efd.png)

1. 사용자가 URL 주소를 입력하면, 브라우저는 DNS 서버에 웹사이트가 있는 서버의 진짜 주소를 찾는다 (도메인 이름 ⇒ IP)
    1. URL 주소 내의 domain name 부분을 DNS 서버에서 검색한다.
    2. DNS 서버는 해당 도메인의 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 전달한다.
2. 1에서 찾은 주소로 웹사이트의 사본을 보내달라는 HTTP 요청 메시지를 서버로 전송한다.
    1. 모든 요청과 데이터는 TCP/IP 연결로 전송된다.
3. 서버가 클라이언트의 요청 메시지를 받아 승인하고, "200 OK" 메시지를 전송한다. 그 다음 웹사이트의 파일을 데이터 패킷으로 쪼개어 클라이언트 브라우저로 전송한다.
    1. 서버에 요청 메시지가 도착하면 HTTP 프로토콜에 따라 메시지는 웹 페이지 URL 정보로 변환된다.
    2. 웹 서버는 URL 정보에 해당하는 데이터를 검색한다.
    3. 찾은 웹 페이지 데이터를 HTTP 프로토콜을 따라 응답 메시지를 생성하고 전송된다.
4. 브라우저는 데이터 패킷을 웹 사이트로 조립하여 표시한다.

### (참고) URI & URL

- **URI(Uniform resource identifier)**: 통합 자원 식별자, 서버 리소스의 이름
- **URL(Uniform resource locator)**: 통합 자원 지시자, URI의 한 종류
    - ex) https://media.vlpt.us/images/jch9537/post/6cecfb8e-13d5-462d-bf68-bd748c7eaaf1/image.png
    - 특정 서버의 한 리소스에 대한 구체적 위치의 서술
    - 리소스가 정확히 어디에 있고 어떻게 접근 가능한지 확인 가능
    - 한계) 리소스가 옮겨지면 해당 URL은 더이상 사용 불가능하며, 어디로 이동했는지 알 수 없다
- **URN(Uniform resource name)**: 통합 자원 이름
    - ex)
        
        ![image](https://user-images.githubusercontent.com/40057032/138212502-96dc5ad4-4da9-4150-b86d-c82ebbd6f3a9.png)
        
        ex) `urn:ietf:rfc:2141` (RFC 2141 인터넷 표준 문서)
        
    - 리소스의 위치에 영향을 받지 않는 유일무이한 이름

## 서버에서 일어나는 일

![image](https://user-images.githubusercontent.com/40057032/138212663-a9c1698a-ddf8-4734-a31c-4d60cb72a65c.png)

- 웹 서버 (소프트웨어)
    - 클라이언트의 HTTP 요청을 받아들이고, HTML 등 웹 페이지나 오브젝트(이미지나 파일 등)를 반환하는 서비스 프로그램
    - 클라이언트의 요청 HTTP 메시지를 확인하고 이에 맞게 데이터를 처리하여 클라이언트에 응답함
    - 아파치 웹 서버, GWS, IIS 등
- WAS(Web Application Server)
    - 웹 어플리케이션을 수행하는 **미들웨어**, 웹 서버를 보조하여 동작
    - 요청에 필요한 **동적** 페이지 로직을 처리하거나 DB에서 데이터 정보를 가져옴
    - 아파치 톰캣, 레진, 제이런 등
- DB(Data Base): 자원을 정리하여 저장하는 보관소

### 동작 과정

1. 클라이언트가 보낸 HTTP 요청 메시지를 웹 서버가 받아 확인한다
2. 요청에 필요한 처리를 위해 웹 서버가 WAS에 처리를 요청한다   
    1. 페이지 로직 처리   
    2. 데이터베이스 연동: WAS가 DB에 SQL 질의 => DB가 WAS로 요청에 맞는 응답을 보냄   
3. WAS가 처리를 끝내면 그 결과를 웹 서버로 보낸다
4. 웹 서버는 요청에 필요한 데이터를 HTTP 응답 메시지로 클라이언트에게 전송한다

![스크린샷 2021-10-21 오후 1 57 03](https://user-images.githubusercontent.com/40057032/138214052-21805427-0115-4552-aa22-8231f3569b4f.png)

### (참고) WAS 

- DB 조회 등 **동적인 컨텐츠**를 제공하기 위한 어플리케이션 서버
    - 웹 서버는 정적 컨텐츠 담당, WAS는 동적 컨텐츠 담당(DB 조회 및 로직 처리)
- 웹 컨테이너(Web Container), 서블릿 컨테이너(Servlet Container)라고도 불림
    - 컨테이너: JSP, Servlet을 실행시킬 수 있는 소프트웨어
    - JSP
        - JavaServer Pages
        - **HTML에 자바 코드를 삽입**해 웹 서버에서 동적으로 웹 페이지를 생성하는 서버 사이드 스크립트 언어
        - 웹 컨테이너에서 서블릿으로 변환됨
        - ![스크린샷 2021-10-21 오후 2 03 54](https://user-images.githubusercontent.com/40057032/138214651-f659c8a9-8e80-40c4-baf5-e79f38912e61.png)
    - Servlet
        - 자바를 이용해 웹페이지를 동적으로 생성하는 서버 프로그램
        - **자바 코드 안에 HTML을 포함**
        - ![스크린샷 2021-10-21 오후 2 06 34](https://user-images.githubusercontent.com/40057032/138214928-9258ebd6-2548-4b3b-8a91-57f5f8a14a5e.png)


## 참고자료

- [https://developer.mozilla.org/ko/docs/Learn/Getting_started_with_the_web/How_the_Web_works](https://developer.mozilla.org/ko/docs/Learn/Getting_started_with_the_web/How_the_Web_works)
- [http://tcpschool.com/webbasic/works](http://tcpschool.com/webbasic/works)
- https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EC%84%9C%EB%B8%94%EB%A6%BF
- https://swimjiy.github.io/2019-11-03-How-Web-Works
- https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html
- https://galid1.tistory.com/488
