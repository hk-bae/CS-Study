# CORS: 교차 출처 리소스 공유 (Cross-Origin Resource Sharing)

<img width="535" alt="스크린샷 2021-10-28 오후 2 41 48" src="https://user-images.githubusercontent.com/40057032/139194149-7b3b42b5-153a-46a6-ba82-a1ebf2099d02.png">

- **교차 출처(Cross-Origin):** 다른 출처를 말함
- **출처(Origin):** Scheme(Protocol) + Host + Port
    - 출처가 같으려면, **Scheme, Host, Port**가 같아야 한다.
    - Port는 생략 가능하다. HTTP, HTTPS 프로토콜 기본 포트 넘버가 정해져 있기 때문에 http Scheme에서 Port가 생략되면 80번 포트라고 가정한다.
      ![image](https://user-images.githubusercontent.com/40057032/139194002-9d382432-b0aa-4432-aa39-37c4135a9e5a.png)
      ⇒ 정책에 따라 다른 출처로 자원을 요청하면 CORS 오류가 발생할 수 있다.  
    - 출처 비교 로직은 브라우저에 구현되어 있다. 서버 CORS 정책에 위배되지 않아서 응답을 보내주었더라도 브라우저가 CORS 정책에 위반된다고 판단하면 응답을 버릴 수도 있음 ⇒ 이 경우 서버 로그에는 정상 응답 로그만 찍힌다

![image](https://user-images.githubusercontent.com/40057032/139194016-bb070804-c118-445e-a793-c0f04818ad96.png)

- 적용되는 리소스
    - XMLHttpRequest, Fetch API 호출
    - 웹 폰트 (`@font-face`)
    - WebGL 텍스처
    - drawImage()로 캔버스에 그린 이미지/비디오 프레임
    - 이미지로부터 추출하는 CSS Shapes

## CORS 정책

### SOP (Same-Origin Policy)

- 같은 출처에서만 리소스를 공유할 수 있다 [[RFC 6454](https://tools.ietf.org/html/rfc6454#page-5)]
    - Scheme, host, port 모두 일치
- 예외 조항에 해당하는 리소스 요청은 출처가 달라도 허용한다.
    - CORS 정책을 지킨 리소스 요청

### CORS

- 기본적으로 자신의 출처와 동일한 리소스만 불러올 수 있다.
- **다른 출처**의 리소스를 불러오려면 그 출처에서 올바른 CORS 헤더를 포함한 응답을 반환해야 한다.


## 쓰는 이유

- 브라우저 - 서버 간의 안전한 데이터 요청 및 전송을 지원하기 위함
- 웹 클라이언트 어플리케이션은 사용자 공격에 매우 취약하다
    - DOM 정보, 리소스 출처, 서버와의 통신 내용을 모두 열람할 수 있음
    - 자바스크립트 난독화가 되어 있다 하더라도 절대로 이해할 수 없게 되어있는 것은 아님
- 다른 출처 어플리케이션을 제한하지 않으면, 다른 사람이 코드를 심어서 사용자 정보를 탈취 가능
    - CSRF(Cross-Site Request Forgery), XSS(Cross-Site Scripting) 등

## CORS의 동작

CORS 동작 방식에는 세 가지의 시나리오가 있다: 🔺 Preflight Request 🔺Simple Request 🔺 Credentialed Request

### CORS 에러 경험해보기

- 개발자 도구를 열고, Console 탭에서 아래 코드 입력

```jsx
const headers = new Headers({
  'Content-Type': 'text/xml',
});
fetch('https://github.com/ooooorobo', { headers });
```

### 기본 흐름

1. 클라 ⇒ 서버: HTTP 요청 헤더 Origin 필드에 요청 출처를 담는다
2. 서버 ⇒ 클라: HTTP 응답 헤더 Access-Control-Allow-Origin 필드에 이 리소스에 접근이 허용된 출처를 담아서 보낸다
3. 브라우저는 요청의 Origin과 응답의 Access-Control-Allow-Origin을 비교하고 응답의 유효 여부를 결정한다.
    1. 응답이 유효하지 않다고 판단하면 CORS 에러가 발생한다.
        
        ![image](https://user-images.githubusercontent.com/40057032/139194040-5c21f917-4185-4c7d-8123-e63531580974.png)
        
        ![image](https://user-images.githubusercontent.com/40057032/139194054-94967683-0423-49c3-b229-cf96b9e78ece.png)
        
        개발자 도구 콘솔 탭과 네트워크 탭에서 CORS error 메시지를 볼 수 있다.
        

### 1. Preflight Request

![image](https://user-images.githubusercontent.com/40057032/139194243-792c09d4-0e79-4bbf-9991-8fc450de0b7d.png)

- 브라우저가 요청을 보낼 때, 본 요청 이전에 예비 요청(Preflight)을 전송
- Preflight 요청
    - HTTP 메소드 중 Options 메소드를 사용
    - 트래픽 증가 우려 → 서버 헤더 설정으로 캐싱 가능
    - `Access-Control-Request-*` 형태 헤더
        - `Access-Control-Request-Method`: 본 요청에서 사용할 메소드
        - `Access-Control-Request-Headers`: 본 요청에서 전송할 헤더 종류
1. 클라 ⇒ 서버: 본 요청을 보내려는 URI에 Option 메소드(Preflight) 요청을 보낸다.
    
    ```jsx
    // 본 요청
    const headers = new Headers({
      'Content-Type': 'text/xml',
    });
    fetch('https://roborobo.tistory.com', { headers });
    ```
    
    - Preflight 헤더에는 본 요청 메소드 종류, 사용할 헤더 등의 정보를 담음
    
    ```HTTP
    ...
    Access-Control-Request-Headers: content-type
    Access-Control-Request-Method: GET
    Connection: keep-alive
    Host: roborobo.tistory.com
    Origin: https://www.naver.com
    Referer: https://www.naver.com/
    ...
    ```
    
2. 서버 ⇒ 클라: 서버의 CORS 정책에 따라 Preflight에 대해 메소드, 헤더, 쿠키 등의 허용 여부에 대해 응답
    
    ```HTTP
    Access-Control-Allow-Origin: https://roborobo.tistory.com
    Content-Encoding: gzip
    Content-Length: 5060
    Content-Type: text/html; charset=utf-8
    Date: Thu, 28 Oct 2021 05:08:23 GMT
    P3P: CP='ALL DSP COR MON LAW OUR LEG DEL'
    Vary: Accept-Encoding
    X-UA-Compatible: IE=Edge
    ```
    
    - 응답에서 요청을 허용했다면 본 요청을 전송

### 2. Simple Request

- **과정**
    1. 클라 ⇒ 서버: 예비 요청을 보내지 않고 바로 본 요청을 보낸다.
    2. 서버 ⇒ 클라: 응답 헤더에 `Access-Control-Allow-Origin` 값 등을 보낸다.
    3. 브라우저가 CORS 정책 위반 여부를 검사한다.
- 예비 요청을 생략하기 위해서는 특정 조건을 만족해야 한다. (매우 드문 경우임)
    1. 요청 메소드가 `GET, HEAD, POST` 중 하나
    2. **`Accept`**, **`Accept-Language`**, **`Content-Language`**, **`Content-Type`**, **`DPR`**, **`Downlink`**, **`Save-Data`**, **`Viewport-Width`**, **`Width`**를 제외한 헤더를 사용하면 안된다.
    3. 만약 **`Content-Type`**를 사용하는 경우에는 **`application/x-www-form-urlencoded`**, **`multipart/form-data`**, **`text/plain`**만 허용된다.

### 3. Credentialed Request

- `credentials` 옵션
    - 요청에 인증과 관련된 정보를 담을 수 있게 한다.
    - 기본적으로 `XMLHttpRequest`, `fetch API`는 브라우저 쿠키 정보나 인증 관련 헤더를 요청에 담지 않는다.
    - 값의 종류
      - `same-origin` (default): 같은 출처 간 요청만 인증 정보를 담음
      - `include`: 모든 요청에 인증 정보를 담음
      - `omit`: 모든 요청에 인증 정보를 담지 않음
- `credentials: include` 사용 시에는 브라우저가 조건을 추가로 검사한다.
    - `include` 모드에서는 요청의 `Access-Control-Allow-Origin` 헤더에 `*`를 사용할 수 없고, 명시적 URL이어야 한다.
    - 응답 헤더에 `Access-Control-Allow-Credentials: true`가 있어야 한다.

## CORS 해결 방법

1. 동일 출처 사용하기
2. 서버에서 Access-Control-Allow-Origin 세팅
    - 와일드카드 `*` 사용하기 ⇒ 보안 위험
    - 허용할 출처의 이름을 명시해주기
3. 프록시 서버 사용
    
    ![image](https://user-images.githubusercontent.com/40057032/139194284-a22717a8-ed54-441d-9d6e-fa10dad46516.png)
    
    - 클라 - 서버 사이에서 헤더를 추가하거나 요청을 허용/거부하는 역할
    - 로컬에서 개발 시, `[localhost:3000](http://localhost:3000)` 과 같은 주소를 사용하게 되는데, 서버에서 이와 같은 범용 주소를 `Access-Control-Allow-Origin`에 잘 넣어주지 않음
    - [Webpack Dev Server](https://www.npmjs.com/package/webpack-dev-server) 세팅
        
        ```jsx
        module.exports = {
          devServer: {
            proxy: {
              '/api': {
                target: 'https://domain.com',
                changeOrigin: true,
                pathRewrite: { '^/api': '' },
              },
            }
          }
        }
        
        // 혹은 package.json에서
        {
        	...
        	"proxy" : "https://api.domain.com"
        	...
        }
        ```

        

### 참고

- [https://developer.mozilla.org/ko/docs/Web/HTTP/CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
- [https://evan-moon.github.io/2020/05/21/about-cors/](https://evan-moon.github.io/2020/05/21/about-cors/)
- [http://wiki.gurubee.net/display/SWDEV/CORS+(Cross-Origin+Resource+Sharing)](http://wiki.gurubee.net/display/SWDEV/CORS+%28Cross-Origin+Resource+Sharing%29)
- [https://velog.io/@jesop/SOP와-CORS](https://velog.io/@jesop/SOP%EC%99%80-CORS)
- [https://react.vlpt.us/redux-middleware/09-cors-and-proxy.html](https://react.vlpt.us/redux-middleware/09-cors-and-proxy.html)
