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

1. 대칭키 암호화 (비공개키 암호방식)

- 클라이언트와 서버가 동일한 비밀 키를 공유한다
- 암호화, 복호화에 사용되는 키가 동일하다.  
- 키가 노출되면 매우 위험하지만 연산 속도가 빠르다

![대칭키암호화](https://user-images.githubusercontent.com/61952198/174466374-2384ca30-f6d8-41c3-9349-7f4bf0c94d61.png)


2. 비대칭키 암호화 (공개키 암호방식)

- 클라이언트와 서버 한쪽이 비밀 키와 공개 키의 쌍, 공개 키 인프라(PKI) 기반을 갖는다.
  - 공개키: 모두에게 공개 가능한 키
  - 개인키: 나만 가지고 알아야 하는 키
- 암호화, 복호화에 사용되는 키가 다르다. 
- 키가 노출되어도 비교적 안전하지만 연산 속도가 느리다

> 공개키로 암호화
  >> 상대방이 보낸 공개키로 데이터를 암호화 하고 전송 -> 데이터를 받은 사람은 자신의 개인키로 수신한 데이터를 복호화   
  >> 공개키는 널리 배포될 수 있기 때문에 많은 사람들이 한 명의 개인키 소유자에게 데이터를 보낼 수 있다.

> 개인키로 암호화
  >> 개인키의 소유자가 개인키로 데이터를 암호화하고 공개키와 함께 전달
  >> 이 과정에서 공개키와 데이터를 획득한 사람은 공개키를 이용하여 복호화가 가능
  >> 데이터 보호의 목적 보다는 공개키 데이터 제공자의 신원을 보장해주기 때문에 사용하는 방식
  >> 공인인증체계의 기본 바탕이 되는 전자 서명이 이 방식을 사용한다.

![비대칭키암호화](https://user-images.githubusercontent.com/61952198/174466392-ee6c3a2b-8179-4242-9566-454c37541dc0.png)


### HTTP의 한계

---

- TCP/IP 구조의 통신은 전부 통신 경로 상에서 패킷을 엿보거나 도청할 수 있는 문제점이 있는데, HTTP는 평문(TEXT)으로 통신하는 프로토콜이기 때문에 패킷의 의미까지 파악이 가능하기 때문에 이는 보안상으로 큰 문제가 있다.
- HTTP는 상태가 없기 때문에 (stateless) 통신하는 상대를 확인하지 않으며 알 수 있는 방법도 없기 때문에 통신 상대측에서 위장이 가능하게 되고 이는 보안상 문제가 될 수 있다.
- HTTP는 상태가 없기 때문에 (stateless) 전송하는 정보의 정확성도 보장할 수 없다. 패킷이 전송되는 도중에 변조되더라도 클라이언트나 서버 측은 이를 알 방법이 없다.

### SSL/TLS

---

#### SSL
SSL(Secure Socket Layer)은 보안 소켓 계층이다.
- 데이터를 안전하게 전송하기 위한 인터넷 암호화 통신 프로토콜
- SSL은 공개키와 대칭키의 장점을 혼합한 방식을 사용한다

#### TLS
TLS 는 SSL의 업데이트 버전으로 SSL의 최종버전인 3.0과 TLS의 최초버전의 차이는 크지않다.

> SSl/TLS 를 사용하는 웹사이트 URL은 HTTP 대신 HTTP가 사용된다.

#### SSL/TLS 의 작동 방식

SSL은 클라이언트와 서버간에 _*핸드셰이크*_ 를 통해 인증이 이루어지며, 데이터에 디지털 서명을 하여 데이터가 의도적으로 도착하기 전에 조작된 여부를 확인한다. (데이터 무결성)

#### SSL/TLS 핸드셰이크

- 핸드셰이크는 클라이언트와 서버간의 메세지 교환이라 볼 수 있다.
- 이 과정을 통해서 상대방이 존재하는지, 또 상대방과 데이터를 주고 받기 위해서는 어떤 방법을 사용해야하는지 파악한다.  
- 핸드셰이크 HTTPS 웹에 처음 커넥션할 때 진행된다.

일반적으로는 SSL/TLS 핸드셰이크는 **RSA 키 교환** 알고리즘이 사용된다.

1. 클라이언트 -> 서버 메세지 전송 (**client hello**)
  - 핸드셰이크의 시작
  - 이 메세지에는 TLS 버전, 암호화 알고리즘(클라이언트와 서버가 지원하는 암호화 방식이 서로 다를 수 있기 때문에 클라이언트 측에서 자신이 사용할 수 있는 암호 방식을 전송한다), 무작위 바이트 문자열이 포함된다.

2. 서버 -> 클라이언트 메세지 전송 (**server hello**)
  - 클라이언트의 메세지에 응답
  - 서버의 SSL인증서, 선택한 암호화 알고리즘, 서버에서 생성한 무작위 바이트 문자열을 포함한 메세지를 전송한다.

3. 인증 
  - 서버의 SSL 인증서가 CA(certificate authority)에 의해서 발급된 것인지를 확인하기 위해서 클라이언트에 내장된 CA의 공개키를 이용해서 인증서를 복호화한다.
  - 클라이언트는 2번 단계를 통해서 받은 서버의 랜덤 데이터와 클라이언트가 생성한 랜덤 데이터를 조합해서 **pre master secret** 이라는 키를 생성한다.

+) pre master secret 키
- 이 키는 세션단계에서 데이터를 주고 받을 때 암호화하기 위해서 사용된다. 
- 이 때 사용할 암호화 기법은 대칭키이기 때문에 pre master secret 값은 제 3자에게 절대로 노출되어서는 안된다. 
- pre master secret 값을 서버에 전달할때 사용하는 방법이 바로 *공개키* 방식이다. 
- 서버로부터 받은 인증서 안에 들어있는 서버의 공개키로 pre master secret 값을 암호화해서 서버로 전송하면 서버는 자신의 비공개키로 안전하게 복호화 할 수 있다.

4. 개인 키 사용 
  - 서버가 pre master secret 키를 개인 키를 통해 복호화한다. (개인 키로만 복호화 가능)
  - 이렇게 서버와 클라이언트가 모두 pre master secret 값을 공유하게 된다.

5. 세션 키 생성
  - 클라이언트와 서버는 클라이언트가 생성한 무작위 키, 서버가 생성한 무작위 키, pr emaster secret 키를 통해 세션 키를 생성한다.
  - 양쪽은 같은 키가 생성되어야 한다.

6. 클라이언트 완료 전송
  - 클라이언트는 세션 키로 암호화된 완료 메세지를 전송한다.

7. 서버 완료 전송 
  - 서버도 세션 키로 암호화된 완료 메세지를 전송한다.

8. 핸드셰이크 완료
  - 핸드셰이크가 완료되고, 세션 키를 이용해 통신을 진행한다.
  +) 클라이언트와의 연결이 끊겼을때는 사용하던 session key를 폐기한다.
  
![ssl:tsl](https://user-images.githubusercontent.com/61952198/174466416-f04f18fd-eae2-4d54-95f4-adf5ca39a6ab.png)


<br>

Reference

---

- [HTTP에서 HTTPS로 전환하기 위한 완벽 가이드](https://webactually.com/2018/11/16/http%EC%97%90%EC%84%9C-https%EB%A1%9C-%EC%A0%84%ED%99%98%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/)
- [SSL이란?, SSL과 TLS 정의 및 차이](https://kanoos-stu.tistory.com/46#:~:text=TLS%20%EB%8A%94%20SSL%EC%9D%98%20%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8,%EB%B3%80%EA%B2%BD%EC%9D%84%20%EC%9C%84%ED%95%B4%EC%84%9C%EC%98%80%EB%8B%A4%EA%B3%A0%20%ED%95%9C%EB%8B%A4.)
- [[SSL 제대로 이해하기 4탄] SSL 동작 과정](https://lbm93.tistory.com/18)
- [[Security] 공개키(Public key) vs 개인키(private key), 대칭키 vs 비대칭키](https://spidyweb.tistory.com/310)


