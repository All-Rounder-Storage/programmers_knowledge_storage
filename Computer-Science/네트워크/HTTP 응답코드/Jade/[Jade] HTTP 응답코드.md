# HTTP 응답코드

`HTTP 상태코드(Status Code) 라고도 불리며 클라이언트가 보낸 HTTP Request에 대한 서버의 Response 코드 이다. 코드에 따라 요청의 성공 / 실패 여부를 알 수 있다.`

## HTTP 응답코드의 분류

`상태 코드의 첫 번째 숫자 (example. 1XX, 2XX)에 따라 크게 5가지 종류로 분류된다.`

- [**1XX**](#1XX) : 정보 (Informational)
- [**2XX**](#2XX) : 성공 (Success)
- [**3XX**](#3XX) : 리다이렉트 (Redirection)
- [**4XX**](#4XX) : 쿨라이언트 에러 (Client Error)
- [**5XX**](#5XX) : 서버 에러 (Server Error)


### 1XX

`조건부 응답`

- 요청을 받았으며 작업을 계속한다는 것을 의미한다 (Request received, continuing process)

|Status Code|Description|
|---|---|
|100 (계속, Continue)|클라이언트가 지금까지 보낸 응답의 문제가 없으면 계속해서 요청을 보내도 된다는것을 의미한다. (이미 요청을 완료한 경우에는 무시해도 괜찮다는 것을 의미한다.)|
|101 (프로토콜 전환, Switching Protocols)|클라이언트가 보낸 요청 헤더에 대한 응답 부분이며 서버에서 프로토콜을 변경할 것임을 알려주는 상태코드이다.|

### 2XX

### 3XX

### 4XX

### 5XX

<br>

Reference

---

- [HTTP 상태코드](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)
- [MDN HTTP 상태코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)