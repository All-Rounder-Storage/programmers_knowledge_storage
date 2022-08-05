# CORS

<br />

## CORS (Cross Origin Resource Sharing, 교차 출처 리소스 공유)란?

- 브라우저는 전통적으로 SOP(Same Origin Policy, 동일 출처 정책)를 가지고 있다.
    - SOP란?
        - 내가 지금 접속해 있는 사이트 (클라이언트라 하자) 에서 외부 사이트 (서버라 하자)로 어떤 특정 리소스를 요청을 했을 때 **출처 (Origin)** 가 같은 경우에만 그 리소스를 받아보고 사용할 수 있게끔 만들어둔 보안 정책이다.
    - 출처(Origin)란?

      [URL 이미지]
        - 출처는 우리가 흔히 볼 수 있는 URL의 `Protocol`, `Host`, `Port` 로 구성돼있다.
            - **같은 출처** (**Same-Origin**) : 내가 접속한 사이트와 다른 사이트의 Protocol, Host, Port 가 **전부 다 동일**
            - 교차 출처, 즉 **다른 출처** (**Cross-Origin**) : 내가 접속한 사이트와 다른 사이트의 Protocol, Host, Port 중 **하나라도 다른 경우**
        - 브라우저의 개발자 콘솔도구에서 `Location` 객체가 가지고 있는 `origin` 프로퍼티를 찍어보면 현재 출처를 알아낼 수 있다.

          ```jsx
            console.log(location.origin); // https://github.com
          ```


- 따라서, 내가 지금 접속해 있는 사이트에서 출처가 다른 사이트로 어떤 특정 정보를 가져오는 요청을 보내고 그에 대한 응답이 돌아왔을 때, 브라우저는 이 응답이 출처가 다른 외부에서 날라 온 데이터라고 판별하고 **클라이언트는 응답을 받아 볼 수 없게된다.**
- 하지만 외부 사이트로의 요청이 많이 필요 없었던 옛날과 달리, 이제는 내가 접속하고 있는 사이트에서 다른 사이트들과 많은 상호작용이 필요해졌기 때문에 이를 해결할 방법이 필요해졌고 이러한 상황을 해결하기 위해 나타난 것이 **CORS**이다.
- **CORS 를 활용하면 안전하게 다른 출처의 자원을 사용할 수 있게 된다**.

- MDN 에서는 아래와 같이 CORS 를 설명한다.

  > **CORS 란 ?**
  >
  >
  > 교차 출처 리소스 공유(Cross-Origin Resource Sharing, CORS)는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제입니다. 웹 애플리케이션은 리소스가 자신의 출처(도메인, 프로토콜, 포트)와 다를 때 교차 출처 HTTP 요청을 실행합니다.

- 즉,  **CORS는 HTTP 통신을 요청할 때 HEADER에 CORS와 관련된 정보를 추가적으로 입력하여 다른 출처의 자원에 접근할 수 있도록 하는 것이다**.

<br />

## CORS 의 흐름

- CORS는 **브라우저**에 의해서 동작한다
    1. 내가 접속한 사이트 (클라이언트)에서 다른 출처의 사이트 (서버)로 리소스를 얻기 위한 HTTP Request 를 보낸다
    2. 브라우저는 그 사이에서 **Request Headers** 에  `Origin` 이라는 필드에 `요청을 보내는 출처` (내가 지금 접속하고 있는 사이트)를 보낸다

        ```
        Origin: https://github.com
        ```

    3. 서버에서는 이 Request를 받아 **Response Headers**에 `Access-Control-Allow-Origin`
       이라는 필드에 접근이 허용되는 출처를 담아 응답한다.

        ```
        // 보통 가장 많이 사용되는 값은 *(와일드 카드)이며 이는 모든 출처를 허용함을 의미한다
        Access-Control-Allow-Origin: *
        ```

    4. 브라우저는 해당 Response에 `Access-Control-Allow-Origin` 와 Request에 `Origin` 을 비교하여 이 응답이 CORS가 허용되는 응답인지 아닌지 결정한다.   

- 내가 접속한 사이트가 요청이 허용되는 출처가 아니라면 브라우저는 아래와 같은 Error 내용을 보여줄 것이며 응답 값은 버려진다. (아마 많은 개발자가 이러한 상황을 마주쳤을것이라 예상한다)

    ```
    Access to XMLHttpRequest at '외부 사이트' from origin '내가 접속한 사이트' has been 
    blocked by CORS policy: No 'Access-Control-Allow-Origin' header is 
    present on the requested resource
    ```

<br />

## CORS의 세가지 시나리오

- 위의 CORS의 기본적인 흐름을 바탕으로 CORS의 접근 제어 시나리오는 세가지로 나뉜다.
    1. 단순 요청(Simple Requests)
    2. 프리플라이트 요청 (Preflighted Requests)
    3. 인증정보를 포함한 요청 (Credentialed Requests)

<br />

### 1. 단순 요청 (Simple Requests)

- 단순 요청은 이 다음으로 다룰 Preflighted Requests (예비 요청) 을 보내지 않고 서버에 바로 본 요청을 보내는 방법이다.
    - 위에서 설명했던 CORS 기본 흐름과 같은 흐름이다.

[simple request image]

- 클라이언트에서 서버에 `Origin` 값을 보냈을 때 서버는 이에 대한 응답으로 `Access-Control-Allow-Origin`  값을 보내주고 브라우저가 `CORS` 정책 위반여부를 검사한다.
- 단순 요청은 예비 요청이 필요없는 말 그대로 단순 요청이나, 단순 요청이 일어나는데에는 아래와 같은 조건이 존재하며 **모두** 충족되어야 단순 요청이 가능해진다.
    1. 요청의 Method 가 `GET`, `POST` , `HEAD` 중에 하나여야 한다.
    2. 수동으로 설정할 수 있는 헤더는 오직 [Fetch 명세에서 “CORS-safelisted request-header”로 정의한 헤더](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)뿐이다.
        - `Accept` , `Accept-Language` , `Content-Language` , `Content-Type`
    3. `Content-Type`을 사용하는 경우에는 아래의 값 들만 허용된다.
        - `application/x-www-form-urlencoded` , `multipart/form-data` , `text/plain`

  ⇒ 위 조건을 모두 다 충족 시킨다는 것은 쉽지 않기 때문에 사실상 **단순 요청**이 가능한 상황은 많지 않다.

<br />

### 2. 프리플라이트 요청 (Preflighted Requests)

- 가장 많이 마주치게 되는 시나리오로 브라우저가 **본 요청을 보내기 이전에 Preflighted Requests (예비 요청) 을 먼저** 보내는 경우이다.
- 예비 요청을 미리 보냄으로써 본 요청을 보낼 서버에서 어떤 출처를 허용하고 어떤 메서드를 허용하는지 미리 확인할 수 있다.

[preflighted request image]

- 예비 요청에는 HTTP method 중 `OPTIONS`가 사용된다.
    - OPTIONS 의 HEADERS 에는 아래와 같은 값이 보내진다.

    ```
    Origin: 출처 (현재 Request를 보내는 사이트)
    Access-Control-Request-Method : 본 요청에서 보내질 HTTP METHOD
    Access-Control-Request-Headers : 본 요청에서 보내질 HTTP의 HEADERS
    
    - 실제 본 요청에서는 **Access-Control-Request-*** 값들은 포함되지 않는다
    ```

- 서버는 이 예비 요청에 대한 응답으로 아래와 같은 값을 돌려보낸다.

    ```
    Access-Contorl-Allow-Origin : 서버가 허락한 출처들 (Protocol + Host + Port)
    Access-Control-Allow-Methods : 서버에서 허락한 HTTP Method
    Access-Control-Allow-Headers : 서버에서 허용되는 HEADERS
    Access-Control-Max-Age : preflight 요청에 대한 응답의 수명 (초 단위)
    	
    - 브라우저는 preflight 요청을 캐싱하여 해당 시간 동안은 다시 preflight 요청을 보내지 않고
    바로 본 요청을 보내는 것이 가능해진다.
    ```

- 브라우저가 예비 요청의 응답을 미리 확인 후 본 요청이 유효할것이라 판단이 되면 본 요청을 보낸다.

<br />

### 3. 인증정보를 포함한 요청 (Credentialed Requests)

- 인증된 요청을 사용하는 방법이다. 이 시나리오는 CORS의 기본적인 방식이라기 보다는 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는 방법이다.
- 브라우저가 제공하는 리소스 요청인 `XMLHttpRequest` 객체나 `fetch` 를 이용한 HTTP 통신에서 출처가 다른 경우 기본적으로 쿠키나 HTTP 인증 같은 자격 증명(credential)을 요청에 바로 담지않는다.
    - HTTP 요청의 경우 대개 쿠키가 함께 전송되는데, 자바스크립트를 사용해 만든 크로스 오리진 요청은 예외이다.
- `credentials`옵션을 적용해야지 요청에 자격 증명 정보를 함께 보낼 수 있다.
    - `credentials` 의 옵션은 3가지가 존재하는데 이는 요청을 보낼 때 설정 할 수 있다.
    
    | same-origin (기본값) | 같은 출처 (Protocol + Host + Port) 간의 요청에만 인증 정보를 담을 수 있다. |
    | --- | --- |
    | include | 출처와 상관없이 모든 요청에 인증 정보를 담을 수 있다. |
    | omit | 모든 요청에 인증정보를 담을 수 없다. |
    
    ```jsx
    fetch('서버주소', {
        credentials: 'include',
    });
    ```


- `credentials` 의 옵션이 `include`인 경우, 즉 요청에 인증 정보가 담겨있는 상태에서 다른 출처의 리소스를 요청하게 되면 브라우저는 CORS 정책 위반 여부를 검사하는 룰에 다음 두 가지를 추가하게 된다.
    1. `Access-Control-Allow-Origin`에는 명시적인 URL 이 적혀 있어야 하며 `*` 를 사용할 수 없다.
    2. 서버의 응답 헤더에는 반드시 `Access-Control-Allow-Credentials: true`가 존재해야한다.
        - Preflighted Requests 에 대한 응답의 일부로 사용 되는 경우 인증정보를 포함하여 본 요청을 수행 할 수 있는지를 나타낸다. (모든 룰을 통과한다면 실제 본 요청이 이루어지며 본 요청에는 자격증명 (Credentials) 을 담아서 보내게 된다)
        - Simple GET 요청은 Preflighted Requests (예비 요청)을 하지 않으므로 자격 증명이 있는 리소스를 요청했을 때,  `Access-Control-Allow-Credentials: true` 라는 헤더가 리소스가 포함된 응답에 존재하지 않으면, 브라우저에서는 그 응답을 무시하고 웹 콘텐츠로 응답을 반환하지 않는다.

<br />

## CORS 해결방법

- CORS 정책 위반으로 error가 나타났을 때는 아래와 같은 방법으로 해결할 수 있다.

1. 서버
    1. **Access-Control-Allow-Origin 세팅**
        - 가장 많이 사용 되는 방법으로 서버측 응답에서 접근 권한을 주는 헤더를 추가하여 CORS를 해결한다.

        ```jsx
        // node 서버
        app.use((req, res, next) => {
          res.header("Access-Control-Allow-Origin", "*"); // 모든 도메인
            // 특정 도메인
            //res.header("Access-Control-Allow-Origin", "https://github.com");
        });
        ```

    2. **cors 모듈 활용**
        - 아무런 옵션없이 cors 모듈을 사용하게 되면 모든 cross-origin 요청에 대해 응답한다

        ```jsx
        const cors = require("cors");
        const app = express();
        
        app.use(cors());
        ```

        ```jsx
        const options = {
          origin: "http://github.com", // 접근 권한을 부여하는 출처
          credentials: true, // 응답 헤더에 Access-Control-Allow-Credentials 값 추가
        };
        
        app.use(cors(options));
        ```


2. 클라이언트
    1. **Webpack Dev Server 로 프록시 설정**
        - **리액트 개발환경**에서 실제로 response 를 받아오는 백엔드 서버쪽 세팅 (예를들어 외부 API)을 제어 (Access-Control-Allow-Origin 설정) 할 수 없을 때 사용할 수 있다.
        - 백엔드 서버에 요청을 보내기전에 프론트에서 중개 역할을 하는 서버 (프록시)를 설정하고, 프론트에서 백엔드 서버로 바로 API 호출을 하는것이 아니라 설정해 둔 프록시를 통해 우회하여 호출하는 방법이다.

        ```jsx
        // webpack.config.js
        
        module.exports = {
          devServer: {
            proxy: {
              "/api": {
                target: "domain.com",
                changeOrigin: true, //target이 위와 같이 실제 IP 주소가 아닌 경우, 즉 가상 이름(domain.com)인 경우에는 이 옵션을 추가해준다
              },
            },
          },
        };
        ```

        - 프록시를 사용하지 않았을 때 기본적인 webpack dev server 와 API 요청의 흐름은 아래와 같다. (이미지 출처 : [웹팩 핸드북](https://joshua1988.github.io/webpack-guide/devtools/webpack-dev-server.html#%ED%94%84%EB%A1%9D%EC%8B%9C-proxy-%EC%84%A4%EC%A0%95))
            - localhost:8080(클라이언트) -> domain.com (서버) : **CORS Error**

       [noProxy img]

        - 프록시 설정 후
            - 브라우저에서는 localhost:8080/api/login 으로 리소스를 요청한다.
            - 중간에서 설정된 프록시 서버가 domain.com 서버로 위장한다.
            - domain.com (서버) 서버에서는 같은 도메인에서 날라 온 요청으로 인식하여, CORS Error가 뜨지 않는다.

       [proxy img]


### Reference

---

- [교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EB%8B%A8%EC%88%9C_%EC%9A%94%EC%B2%ADsimple_requests)
- [[WEB] CORS 개념 정리](https://velog.io/@jimmy0417/WEB-CORS-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%A0%81%EC%9A%A9-%EB%B0%A9%EB%B2%95)