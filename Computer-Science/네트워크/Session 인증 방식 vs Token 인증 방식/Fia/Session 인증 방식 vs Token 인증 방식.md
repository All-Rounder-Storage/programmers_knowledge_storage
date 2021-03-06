# Session 인증 방식 vs Token 인증 방식

## Session 기반 인증 방식 이란 ?
- **서버** 측 에서 사용자들의 정보를 세션을 통해 기억하고, 이후의 기능을 서비스 시 정보를 활용한다.
- 세션은 주로 **서버의 메모리, 디스크 혹은 DB**에 저장된다. 
  (트래픽 규모에 따라 Redis 나 Memcached 와 같은 메모리 관리 시스템이 필요할 때가 있다. )
  
## 인증 절차 진행 방식
1. 사용자의 인증 요청 시 서버에서 세션을 생성, 유지 한다.
2. 사용자가 요청을 보낼 경우 만들어둔 세션에서 정보를 읽고, 응답을 제공한다.

![session](https://user-images.githubusercontent.com/49216939/178099435-52f64111-1166-4a38-b0eb-b2f35d63a743.png)

## Session 기반 인증 방식의 장단점
```markdown
장점 
- 서버 측에서 상태를 유지하기 때문에 클라이언트의 제어가 가능하다.
- 클라이언트의 변조에 따라 영향을 받기가 어렵다.
- 서버 측에서 사용자의 로그인 상태 확인이 분명하고 수월하다.
단점
- 사용자 마다의 정보를 저장하고 있어야 하기 때문에, 사용자가 많을 경우 서버에 부하가 발생한다.
- 트래픽이 증가하여 서버를 확장해야 할 때 세션을 분산 시키는 것도 고려를 해야하기 때문에 확장성이 떨어진다.
- 쿠키는 단일, 서브 도메인에서만 작동하기에, 여러 도메인에서는 관리가 번거롭다. (cross-origin resource sharing)
```
## Token 기반 인증 방식 이란 ?
- 인증 받은 사용자들에게 토큰을 발급하여, 서버에 Request 를 보낼 때 Token 을 함께 전송하여
  유효성 검사를 진행해 인증된 사용자 만 서비스 이용이 가능하게 한다.
- 사용자의 인증 정보를 서버나 세션에 유지하지 않고 클라이언트가 보내는 정보 만으로 작업을 처리하는 방식이다.
- 세션 기반의 단점을 해결할 수 있었던 방식이다.

## 인증 절차 진행 방식
1. 사용자가 아이디와 비밀번호로 로그인 시 서버 측에서 해당 정보를 검증한다.
2. 정보가 일치할 경우 사용자에게 Signed token 을 발급한다.(서버에서 정상적으로 발급된 토큰 임을 나타내는 signature 를 가짐)
3. 클라이언트는 발급 받은 토큰을 이용해 서버에 요청 시 header 에 함께 보낸다.
4. 서버는 토큰을 검증하고, 요청에 응답한다.

![token](https://user-images.githubusercontent.com/49216939/178100206-615786bf-3f43-4237-b9da-c7b82c55c47b.png)

## JWT 란 ? 
- Json Web Token 의 약자로 **인증에 필요한 정보들을 암호화 시킨 JSON 토큰**을 의미한다.
- Base64 URL-safe Encode 를 통해 인코딩, 직력화 처리를 하고 위변조 방지를 위해 개인키를 통한 전자서명도 포함된다.
  이를 통해 서버가 서명을 검증하고, 검증이 완료되면 요청한 응답을 반환한다. (비대칭 암호화 알고리즘을 이용한다. 즉 암호화/복호화 키가 다르다.)
## 구성 요소
- Header
  + 토큰 타입, 해싱 알고리즘(토큰 검증시 사용되는 signature 에서 이 정보를 참조한다)
  + decoding 시 alg, typ 로 구성된다.
    + alg : **서명 암호화 알고리즘(ex. SHA 256, RSA, HMAC)**
    + typ : 토큰의 유형
- payload
  + 토큰명, 발급자, 만료시간, 고유식별사 등에 대한 정보
    + decoding 시 토큰에서 사용할 claim 이 담겨 있다.
      + claim 이란 ? ---> KEY-VALUE 형식으로 이뤄진 한쌍의 정보
    + 정해진 데이터 타입은 없지만 대표적으로 3가지로 나눌 수 있다.
      + Register claims (미리 정의된 클레임)
        + iss : 발행자(issuer)
        + exp : 만료 시간(expiration time)
        + sub : 제목(subject)
        + iat : 발행 시간(issued At)
        + jti : JWI ID
      + Public claims (사용자가 정의할 수 있는 클레임)
        + 공개용 정보 전달을 위해 사용한다.
      + Private claims(해당하는 상호 간에 정보를 공유하기 위해 만들어진 사용자 지정 클레임)
        + 해당 유저를 특정할 수 있는 정보들을 담는다.
- signature
  + header 와 payload 의 데이터 무결성, 변조 방지를 위한 서명
  + header 의 encoding 값 + Payload 의 encoding 값 ---> 비밀키로 해쉬 값을 생성한다.(서버만 아는 정보)
  + header 가 정의한 alg 알고리즘을 활용한다.
  
**위 3가지의 구성요소는 "." 을 구분자로 나누어 가진다.**
![jwt](https://user-images.githubusercontent.com/49216939/178101581-ca70abc6-082d-4200-bde3-794ab66487bb.png)

**JWT 를 이용한 인증 과정**
![jwt flow](https://user-images.githubusercontent.com/49216939/178103462-f58784d1-41fa-45ea-a193-76285e96ac8a.png)

**access Token / Refresh Token 이란 ?** 
- 2가지 모두 동일한 형태의 JWT 이다. 
  
차이점
- 유효기간에서 차이가 있다.
  + 탈취를 방지하기 위해 Access Token 의 유효 기간을 짧게 하여 생성한다.
      + 사용자는 로그인을 자주해서 token 을 다시 발급 받아야 하는 불편함이 있다.
        이를 Refresh Token 을 활용하여 해결하였다.
  + Refresh Token 은 Access Token 과 동일하지만 **유효 기간이 더 길다.**
      + Access Token 의 유효기간이 만료되었을 때 새롭게 발급 해주는 역할을 한다.
- 유효기간이 짧은 이유
  + token 이 탈취 당할 경우 악의적으로 이용 당할 수 있기 때문이다.
  
## Token 기반 인증 방식의 장단점
```markdown
장점
- 토큰은 클라이언트 측에 저장되어 서버는 stateless 하다. 이를 통해 **확장하기가 매우 편리**하다.
- 인증된 토큰을 통해서만 데이터를 이용할 수 있기 때문에 보안성이 높다.
- 토큰에 부여하는 권한을 선택적으로 조정할 수 있어서 extensibility 가 높다. (첫번째 확장성과는 또 다른 의미이다)
- 여러 플랫폼 및 도메인에서의 서비스 지원이 가능하다.(토큰을 통한 검사를 진행 한 후에 처리 할 수 있기 때문이다.)

단점
- 토큰이 탈취 당할 경우 허가받지 않은 사용자의 서비스 이용이 가능해진다.
- 서버에서 상태를 기억하지 않기 때문에 통제가 불가능하다.
- 토큰에 signature 등 의 부가적인 정보가 담겨 있어서 세션 인증 방식 보다 주고 받는 데이터가 많다.
- 토큰 인증을 위해 매번 조회해야 함으로 상황에 따라 성능 부하가 있을 수 있다.
```