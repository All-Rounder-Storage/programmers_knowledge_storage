# HTTP 특징 및 HTTP vs HTTPS

## 1. HTTP의 특징
> 1. HyperText Transfer Protocol 의 약자로 서버-클라이언트 모델을 따른다.
> 2. 애플리케이션 레벨의 프로토콜로 TCP/IP 위에서 작동하며 기본 포트는 80번이다.
> 3. request/response 구조로 정보를 주고받는 프로토콜이다.
> 4. [request](#request) 는 start line, headers, body로 이루어져 있다.
> 5. [response](#response) status line, headers, body로 이루어져 있다.
> 6. 가장 큰 특징은 [_Connectionless_](#Connectionless) 와 [_Stateless_](#Stateless) 이다.

### request

---

##### 1. Start line
`
  Example)  GET / search HTTP / 1.1
`
- method : request 가 의도한 action
- path : request 가 전송되는 URI
- HTTP version(protocol) : 사용되는 프로토콜 버전

##### 2. headers

해당 request 에 대한 정보를 담고 있는 부분이다.
- key : value 로 되어 있다.

```
  Example)
  Accept: */* // 해당 request가 받을 수 있는 response 타입
  Accept-Encoding: gzip, deflate
  Connection: keep-alive // 해당 request가 끝난 후에 클라이언트와 서버가 계속해서 네트워크 커넥션을 유지할 것인지 아니면 끊을 것인지에 대해 지시하는 부분
  Content-Type: application/json
  Content-Length: 257 // body의 길이
  Host: google.com // request가 전송되는 url
  User-Agent: HTTPie/0.9.3 // request를 보내는 클라이언트에 대한 정보
```

##### 3. body

해당 request 에 대한 정보를 담고 있는 부분이다. 
- body가 없는 request도 존재한다. (ex. GET)

### response

---
##### 1. Status line
`
Example)  HTTP/1.1 200 OK 
`
- HTTP version(protocol version) : 사용되는 프로토콜 버전 
- status code : response 상태를 나타내는 숫자 값
- status message : response 상태를 간략하게 설명해주는 문자 값

##### 2. headers

- request의 heders와 동일하다
- 다만, response에서만 사용되는 headers 값들이 있다. (User-Agent -> Server 헤더 사용)

##### 3. body

- request의 body와 동일하다
- body가 없는 response도 존재한다 (데이터를 전송할 필요가 없을경우)

### Connectionless

---

- HTTP는 서버에 연결 후 request에 response를 받으면 연결을 끊어버린다.
- 실제 동시 접속을 최소화하여 더 많은 유저의 요청을 처리할 수 있다.

### Stateless

---

- 연결을 바로 끊어버리기때문에 이전 상태를 알 수 없다.
- 이런 이전 상태를 알 수 없는점을 해결하고자, _cookie, session, jwt_ 등이 도입되었다.

## 2. HTTP vs HTTPS

> 1. 둘다 웹 서버가 브라우저로부터 요청을 받아 응답할 때 사용되는 프로토콜이다.

### HTTP
***
HTTP(Hypertext Transfer Protocol): TCP/IP 위에서 작동하는 인터넷상에서 데이터를 주고받기 위한 서버/클라이언트 모델을 따르는 프로토콜이다.

### HTTPS
***
HTTPS(Hypertext Transfer Protocol Secure): 기존의 HTTP 프로토콜에 보안을 위한 SSL(Secure Socket Layer)과 TSL(Transport Layer Security)을 추가한 프로토콜이다.

#### 대칭키 암호화와 비대칭키 암호화

HTTPS는 대칭키 암호화 방식과 비대칭키 암호화 방식을 모두 사용한다.

1. 대칭키 암호화

- 클라이언트와 서버가 동일한 키를 사용해 암호화/복호화를 진행한다
- 키가 노출되면 매우 위험하지만 연산 속도가 빠르다


2. 비대칭키 암호화

- 1개의 쌍으로 구성된 공개키와 개인키를 암호화/복호화 하는데 사용한다
  - 공개키: 모두에게 공개 가능한 키
  - 개인키: 나만 가지고 알아야 하는 키
- 키가 노출되어도 비교적 안전하지만 연산 속도가 느리다

### HTTP의 한계

---

- TCP/IP 구조의 통신은 전부 통신 경로 상에서 패킷을 엿보거나 도청할 수 있는 문제점이 있는데, HTTP는 평문(TEXT)으로 통신하는 프로토콜이기 때문에 패킷의 의미까지 파악이 가능하기 때문에 이는 보안상으로 큰 문제가 있다.
- HTTP는 상태가 없기 때문에 (stateless) 통신하는 상대를 확인하지 않으며 알 수 있는 방법도 없기 때문에 통신 상대측에서 위장이 가능하게 되고 이는 보안상 문제가 될 수 있다. => 클라이언트는 [**SSL**](#SSL) 인증서를 사용하여 통신 상대를 인증하여 확인한다.
- HTTP는 상태가 없기 때문에 (stateless) 전송하는 정보의 정확성도 보장할 수 없다. 패킷이 전송되는 도중에 변조되더라도 클라이언트나 서버 측은 이를 알 방법이 없다. => [**TSL**](#TSL) 프로토콜을 사용하여 보안을 유지한다

### SSL

---
