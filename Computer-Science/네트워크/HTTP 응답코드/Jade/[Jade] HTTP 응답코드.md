# HTTP 응답코드

`HTTP 상태코드(Status Code) 라고도 불리며 클라이언트가 보낸 HTTP Request에 대한 서버의 Response 코드 이다. 코드에 따라 요청의 성공 / 실패 여부를 알 수 있다.`

## HTTP 응답코드의 분류

`세 자리 숫자 형태중 상태 코드의 첫 번째 숫자 (example. 1XX, 2XX)에 따라 크게 5가지 종류로 분류된다.`

- [**1XX**](#1XX) : 정보 (Informational)
- [**2XX**](#2XX) : 성공 (Success)
- [**3XX**](#3XX) : 리다이렉트 (Redirection)
- [**4XX**](#4XX) : 쿨라이언트 에러 (Client Error)
- [**5XX**](#5XX) : 서버 에러 (Server Error)


### 1XX

`정보`

- 요청을 받았으며 작업을 계속한다는 것을 의미한다 (Request received, continuing process)

|Status Code|Description|HTTP Cats|
|---|---|---|
|100 (계속, Continue)|클라이언트가 지금까지 보낸 응답의 문제가 없으면 계속해서 요청을 보내도 된다는것을 의미한다. (이미 요청을 완료한 경우에는 무시해도 괜찮다는 것을 의미한다.)|![100 cat](https://http.cat/100)|
|101 (프로토콜 전환, Switching Protocols)|클라이언트가 보낸 요청 헤더에 대한 응답 부분이며 서버에서 프로토콜을 변경할 것임을 알려주는 상태코드이다.|![101 cat](https://http.cat/101)|

### 2XX

`성공`

- 서버가 클라이언트의 요청을 성공적으로 처리했다는 것을 의미한다. (The action was successfully received, understood, and accepted)

|Status Code|Description|HTTP Cats|
|---|---|---|
|200 (성공, Ok)|서버가 요청을 성공적으로 처리했음을 의미한다. **성공의 의미** 는 HTTP 메소드에 따라 달라진다.|![200 cat](https://http.cat/200)|
|201 (작성됨, Created)|클라이언트의 요청이 성공적으로 수행되었고, 서버에서 새로운 리소스가 생성됐음을 의미한다. 주로, POST 요청에 대한 응답으로 볼 수 있다.|![201 cat](https://http.cat/201)|
|202 (허용됨, Accepted)|서버가 요청은 성공적으로 받았지만 아직 처리하지 않았음을 의미한다.|![202 cat](https://http.cat/202)|
|203 (신뢰할 수 없는 정보, Non-Authoritative Information)|서버가 요청은 성공적으로 처리했으나, 요청에 대해 돌려받은 메타 정보 세트가 서버의 것과 일치하지 않고 로컬이나 서드 파티 복사본에서 모아졌음을 의미한다.|![203 cat](https://http.cat/203)|

```markdown
HTTP 메소드에 따른 200 상태 코드의 성공의 의미

GET: 리소스를 불러와서 메시지 바디에 전송되었다.
HEAD: 개체 해더가 메시지 바디에 있다.
PUT / POST: 수행 결과에 대한 리소스가 메시지 바디에 전송되었다.
TRACE: 메시지 바디는 서버에서 수신한 요청 메시지를 포함하고 있다.
```

### 3XX

`리다이렉트`

- 요청 완료를 위해서는 또 다른 조치가 필요함을 의미한다. (Further action must be taken in order to complete the request)

|Status Code|Description|HTTP Cats|
|---|---|---|
|300 (여러 선택항목, Multiple Choice)|요청에 대한 응답이 하나 이상 존재함을 의미한다. 사용자는 그 중에 하나를 반드시 선택해야 한다.|![300 cat](https://http.cat/300)|
|301 (영구 이동, Moved Permanently)|요청한 리소스의 URI가 변경되었음을 의미한다. 새로운 URI가 응답에서 주어질 수도 있다.|![301 cat](https://http.cat/301)|
|302 (임시 이동, Found)|리소스의 URI가 일시적으로 변경되었음을 의미한다. 새롭게 변경된 URI는 나중에 만들어질수도 있다.|![302 cat](https://http.cat/302)|

### 4XX

`클라이언트 에러`

- 요청 문법에 오류가 있거나 요청이 처리될 수 없음을 의미한다. (The request contains bad syntax or cannot be fulfilled)

|Status Code|Description|HTTP Cats|
|---|---|---|
|400 (잘못된 요청, Bad Request)|클라이언트가 잘못된 문법 등으로 올바르지 못한 요청을 보내 서버가 요청을 이해할 수 없음을 의미한다.|![400 cat](https://http.cat/400)|
|401 (권한 없음, Unauthorized)|인증되지 않은 사용자가 인증이 필요한 리소스를 요청했음을 의미한다. 보통 로그인이 필요한 API를 비로그인 사용자가 호출했을 때 사용됩니다.|![401 cat](https://http.cat/401)|
|403 (금지됨, Forbidden)|클라이언트가 콘텐츠에 접근할 권한을 가지고 있지 않음을 의미한다. 401과 다른 점은 서버가 클라이언트가 누구인지 알고 있다는 것이다. 보통 특정 IP나 국가가 차단되어 있는 사이트에 접속을 시도한 경우 이를 거절하기 위해 사용된다.|![403 cat](https://http.cat/403)|
|404 (찾을 수 없음, Not Found)|요청한 리소스가 존재하지 않음을 의미한다. 인증되지 않은 클라이언트로부터 리소스를 숨기기 위해 403 대신 이 응답을 전송하기도 한다. 서버와 통신은 되지만 요청한 바를 찾을 수 없다는 뜻이기도 하다.|![404 cat](https://http.cat/404)|
|405 (허용되지 않는 메서드, Method Not Allowed)|현재 리소스에 맞지 않는 메소드를 사용했음을 의미한다. GET으로만 허용되는 요청인데 POST 메서드를 사용했을 때가 이에 경우에 속한다.|![405 cat](https://http.cat/405)|
|408 (요청 시간초과, Request Timeout)|요청에 응답하는 시간이 너무 오래 걸림을 의미한다.|![408 cat](https://http.cat/408)|

### 5XX

`서버 에러`

- 명백히 오류없는 요청을 서버가 수행하지 못함을 의미한다. (The server failed to fulfill an apparently valid request)

|Status Code|Description|HTTP Cats|
|---|---|---|
|500 (내부 서버 오류, Internal Server Error)|서버에 오류가 발생하여 응답할 수 없음을 의미한다. 서버에 오류가 발생했으나 처리 방법을 알 수 없을 경우의 응답이다.|![500 cat](https://http.cat/500)|
|502 (불량 게이트웨이, Bad Gateway)|서버가 요청을 처리하는 데 필요한 응답을 얻기 위해 게이트웨이로 작업하는 동안 잘못된 응답을 수신했음을 의미한다. 보통 서버에 접속하는 사용자가 많아 과부화가 발생할때 해당 응답코드를 볼 수 있다.|![502 cat](https://http.cat/502)|
|503 (서비스를 사용할 수 없음, Service Unavailable)|서버가 요청을 처리할 준비가 되지 않았음을 의미한다. 일반적인 원인으로는 유지보수를 위해 서비스 작동이 중단되거나 혹은 과부하가 걸렸을 때이다.|![503 cat](https://http.cat/503)|

<br>

Reference

---

- [HTTP 상태코드](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)
- [MDN HTTP 상태코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
- [502를 찾아서! HTTP 상태 코드의 비밀](https://www.inflearn.com/pages/weekly-inflearn-23?utm_source=pinpoint&utm_medium=email&utm_campaign=weekly-inflearn&utm_content=23)
- [HTTP Cats](https://http.cat)