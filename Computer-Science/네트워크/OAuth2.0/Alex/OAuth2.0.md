## OAuth란 무엇일까?

- 비밀번호를 제공하지 않고 타사의 사이트 혹은 애플리케이션의 접근 권한을 부여하는 수단
- 접근 위임을 위한 개방형 표준 프로토콜

<br>

## OAuth의 성장과정

- OAtuh 1.0에서 토큰 승인 과정에서 세션고정 공격이라는 보안 결함이 발견
  - 세션을 주입할 코드를 설치 후 유도하여 인증을 통과한 유효 세션을 탈취하는 방법
- OAuth1.0의 보안 문제 등을 개선한 버전으로 **OAuth 2.0 탄생** (하위 호환성 미지원)

#### OAuth 1.0 -> 2.0
1. API 요청 시 클라이언트 인증 방법 변경 : 서명 → HTTPS 의무화
2. Refresh Token 도입을 통한 접근 토큰(Access Token) 유효기간(Life-time) 설정으로 인한 단축
   → 보안성 개선
3. 기존 서비스 제공자를 자원 서버, 권한서버로 분리하여 다수의 서버로 구성된 웹 서비스에서 발생 가능한 권한 동기화 문제를 개선
4. 다양한 확장성 지원

   <img src="images/OAuth1vs2.png" width="70%" height="70%"/>
  
   이미지 출처 : [https://velog.io/@devsh/OAuth-2.0-에-대하여](https://velog.io/@devsh/OAuth-2.0-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)

<br>

## OAuth 인증은 어떻게 이루어질까?

- OAuth 인증은 소비자 ↔ 서비스 제공자 사이에 인증 과정을 통해 이루어진다

    ```markdown
    ex) 소비자: 애플리케이션 <->  서비스 제공자: Facebook
    ```

- `📢 일반적인 인증 흐름을 간략하게 알아보고 아래에서 실제 권한부여 방식을 자세히 알아보겠다`
1. 소비자 → 서비스 제공자 (접근 요청)
2. 서비스 제공자 → 소비자 (인증 요청)
3. 소비자가 사용자를 서비스 제공자로 Redirect 이동 (사용자 인증 수행)
4. 서비스 제공자가 사용자를 소비자로 이동 (인증 완료)

`💡 여기까지 아래 그림 1, 2번에 해당`
  
5. 소비자가 접근토큰을 요청
6. 서비스 제공자가 접근토큰 발급 (Access Token)

`💡 여기까지 아래 그림 3, 4번에 해당`

7. 발급받은 접근 토큰(Access Token)을 이용하여 소비자에서 사용자 정보(자원)에 접근
8. 발급받은 접근 토큰(Access Token)을 통해 자원 서버(Resource Server)에서는 인가를 진행
9. 접근 권한이 있어 인가가 성공하면 요청 데이터 반환, 인가가 실패할 경우 403 에러

`💡 여기까지 아래 그림 5, 6번에 해당`
   

  <img src="images/OAuth_Process.png" width="70%" height="70%"/>

<br>

## OAuth 관련 용어

### Resource Owner (자원 소유자)

- 서비스 이용자
- 보호 자원에 접근 권한을 부여할 수 있는 개체

### Resource Server (자원 서버)

- API 서버
- 보호 자원에 대한 서비스 API를 제공하는 서버

### Authentication (인증)

- 사용자인지 아닌지를 확인 (로그인)
- 권한을 요청하기 전 로그인 주체가 누구인지 판별하는 단계

### Authorization (인가)

- 자원에 접근할 권한이 있는지 확인 (Admin)
- 사용자인지는 확인됐으나 해당 자원에 접근할 권한이 있는지 확인하는 단계

### Authorization Server(권한 서버)

- 권한 관리 및 부여 서버
- Access Token, Refresh Token 등을 발급해주는 역할

### Client

- Web/App
- 자원 서버(API 서버)에서 보호 자원을 요청하고 관련 서비스를 제공하는 애플리케이션

### Access Token (접근 토큰)

- 인증 후에 사용자가 서비스 제공자가 아닌 소비자를 통해 자원에 접근하기 위한 키를 포함한 값
- 만료기간이 존재 (Life-Time)

<br>

## OAuth 2.0 권한 부여 방식

- OAuth 2.0의 권한 부여 방식은 주로 4가지가 사용된다.
1. 권한 부여 승인 코드 방식 (주로 사용)
2. 암묵적 승인 방식
3. 자원 소유자 자격 증명 승인 방식
4. 클라이언트 자격증명 승인 방식
- OAuth 2.1에서는 legacy들은 버려질 예정이다 (개발 진행중)
  - ~~2. 암묵적 승인 방식~~
  - ~~3. 자격증명 승인 방식~~

<br>

### 1. Authorization Code Grant (권한 부여 승인 코드 방식)

- 어플리케이션이 인증 서버에 요청해 브라우저를 열어서 사용자가 인증을 진행하게 하는 방식
  - `response_type=code, grant_type = authorization_code`
- Access Token 요청 시 Authorization code를 요청하는 단계가 있어서 보안에 효과적
- Refresh Token 사용 가능
- 가장 복잡하지만, 가장 많이 사용되는 방식


<img src="images/payco_oauth.png" width="70%" height="70%"/>

이미지 출처 : [https://developers.payco.com/guide](https://developers.payco.com/guide)


<img src="images/authorization_code_grant1.png" width="40%" height="40%"/> <img src="images/authorization_code_grant2.png" width="40%" height="40%"/>

이미지 출처 : [msaschool](http://www.msaschool.io/operation/design/design-seven/) ,  [kimjaejung96](https://velog.io/@kimjaejung96/OAuth2.0-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%9E%91%EB%8F%99%EB%B0%A9%EC%8B%9D)

<br>

### 2. Implicit Grant (암묵적 승인 방식) : Legacy (삭제예정)

- 1번 방식(권한 부여 승인코드)에서 **Authorization code를 요청하는 프로세스를 제거**한 방식
  - `response_type=token`
- 로그인(인증) 성공 시 URL로 Access Token을 즉시 반환 → 보안 위험
- 자격 증명을 안전하게 저장하기 힘든 클라이언트(JavaScript)이거나 외부에 있는 OAuth 서버가 cors를 지원하지 않을 때 사용하지만 권장하지 않음
- 따라서 신뢰성 있는 앱 또는 단말기에서만 사용해야 함
- Refresh Token  사용 불가

  <img src="images/implicitGrant.png" width="60%" height="60%"/>

  이미지 출처 : [https://velog.io/@kimjaejung96/OAuth2.0-개념-및-작동방식](https://velog.io/@kimjaejung96/OAuth2.0-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%9E%91%EB%8F%99%EB%B0%A9%EC%8B%9D)

<br>

### 3. Resource Owner Password Credentials Grant (자원 소유자 자격 증명 승인 방식) : Legacy (삭제예정)

- username, password로 Access Token을 받는 방식
  - `grant_type = password`
- 클라이언트가 자신의 서비스에서 제공하는 애플리케이션일 경우에만 사용
  - 클라이언트가 직접 유저정보를 가지고 있고 내 서비스만 인증할 때
- 전통적으로 이름과 비밀번호로 인증
- Refresh Token 사용 가능

  <img src="images/Password_Credentials_Grant.png" width="60%" height="60%"/>

  이미지 출처 : [https://velog.io/@kimjaejung96/OAuth2.0-개념-및-작동방식](https://velog.io/@kimjaejung96/OAuth2.0-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%9E%91%EB%8F%99%EB%B0%A9%EC%8B%9D)

<br>

### 4. Client Credentials Grant (클라이언트 자격증명 승인 방식)

- 사용자가 아닌 응용프로그램(클라이언트)이 인증을 요청할 때
  - `grant_type = client_credentials`
- 자격증명을 안전하게 보관할 수 있는 관리자용 Desktop App이나 Mobile App에서 사용
- Refresh Token 사용 불가

  <img src="images/Client_Credentials_Grant.png" width="60%" height="60%"/>

  이미지 출처 : [https://velog.io/@kimjaejung96/OAuth2.0-개념-및-작동방식](https://velog.io/@kimjaejung96/OAuth2.0-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%9E%91%EB%8F%99%EB%B0%A9%EC%8B%9D)

<br>

## Access Token만으로 충분한가?

- OAuth 2.0은 Access Token을 통해 애플리케이션에 비밀번호를 제공하지 않으면서 실제 서비스를 이용할 수 있게 도움을 준다.
- 하지만 API 요청이 들어올때마다 전달 받은 Access Token이 유효한지 계속 확인해야 한다면 어떨까?
- MSA 아키텍쳐로 구성되어 있는 많은 서비스에서 각각의 서버가 Access Token 유효성 검사를 매 요청마다 한다면, 자원 서버에서는 서버 부하로 인해 인가 단계에서 버티지 못하고 병목 현상 등이 발생할 것이다.
- 이를 해결하는 방법으로 [JWT를 사용](#access-token을-jwt-토큰을-사용할-때의-장점)한다.

<br>

## JWT란 무엇인가?

- Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token
  - Claim : 사용자 정보나 데이터 속성 등
- JWT는 사용자 정보를 안전하게 보관 → 토큰 자체를 정보로 사용하는 Self-Contained 방식 사용

```markdown
💡 예시를 들어보자

토큰을 해석하면 이런 정보들이 담겨져 있다 (Claim 기반 토큰) -> 커스텀 클레임(비공개 클레임)을 예시로 들겠다
{
	"id" : "giibeom",
	"name" : "Alex",
	"role" : ["백엔드노예", "취준지망생", "회사탈출 예정러"]
}
```

<br>

## JWT 사용 방법

- 애플리케이션 실행 시, 클라이언트에서는 JWT 토큰을 static 변수와 로컬 스토리지에 저장
  - 매 HTTP 통신마다 헤더에 담아 보내야 하기 때문에 static에 저장
  - 로컬 스토리지에서 계속 불러올 경우 오버헤드 발생 위험
- 서버에서는 요청 데이터에서 JWT 토큰이 허가된 것인지를 검사
- 로그아웃 할 경우
  - 클라이언트 : 로컬 스토리지에 저장된 JWT 데이터 제거
  - 서버 : 사용했던 토큰을 blacklist DB 테이블에 넣어 해당 토큰의 접근 제한

<img src="images/JWT.png" width="60%" height="60%"/>

이미지 출처 : [https://mangkyu.tistory.com/56](https://mangkyu.tistory.com/56)

<br>

## JWT 구조

- JWT는 Header, Payload, Signature로 3가지의 부분으로 이루어져 있다.
- Json 형태인 각 부분은 “.” 구분자로 나눠서 Base64로 인코딩 되어 표현된다.

```markdown
💡 예시를 들어보자

해당 토큰은 실제 JWT로 만든 토큰이다
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6ImdpaWJlb20iLCJuYW1lIjoiQWxleCIsInJvbGUiOlsi67Cx7JeU65OcIOuFuOyYiCIsIuydtOyngSDsp4Drp53sg50iLCLrvYDroZzroZwiXX0.NGFDoN4IIcHENZ8ayO6cVvDiGIk2mszhV7CRHv_hyrw

해당 토큰을 해석하면 아래와 같다!
```

<img src="images/JWT_Token.png" width="60%" height="60%"/>

<br>

### Header

- JWT를 어떻게 검증(Verify)하는가에 대한 내용을 담고 있다.
- 토큰의 헤더는 두가지로 구성된다
  - alg : 서명(Signature)을 해싱하기 위해 지정한 알고리즘
  - typ : 토큰의 타입을 지정

  <img src="images/JWT_Header.png" width="50%" height="50%"/>

<br>

### PayLoad

- 토큰에서 사용할 정보의 조각들인 클레임(Claim)이 담겨 있다.
- Json(Key/Value) 형태로 다수의 정보를 넣을 수 있다.
- 클레임은 총 3가지로 나누어 진다
  - 등록된 클레임
  - 공개 클레임
  - 비공개 클레임


#### 1. 등록된 클레임 (Registered Claim)

- 토큰 정보를 표현하기 위해 이미 이름이 정해진 데이터 종류
- 모두 선택적 (optional)이므로 꼭 사용하지 않아도 된다

  | 클레임 명 | 의미                                                                     |
  |------------------------------------------------------------------------| --- |
  | iss | 토큰 발급자 (issuer)                                                        |
  | sub | 토큰 제목 (subject) - 유니크한 값을 사용하므로 주로 사용자 이메일을 사용                         |
  | aud | 토큰 대상자 (audience)                                                      |
  | exp | 토큰 만료 시간(expiration), NumericDate 형식으로 되어 있어야 함 <br> ex) 1480849147370 |
  | nbf | 토큰 활성 날짜(not before), 이 날이 지나기 전의 토큰은 활성화되지 않음                         |
  | iat | 토큰 발급 시간(issued at), 토큰 발급 이후의 경과 시간을 알 수 있음                           |
  | jti | JWT 토큰 식별자(JWT ID), 중복 방지를 위해 사용하며, 일회용 토큰(Access Token) 등에 사용         |

#### 2. 공개 클레임 (Public Claim)

- 사용자 정의 클레임으로 공개용 정보를 위해 사용된다
- 충돌이 방지된 (collision-resistant) 이름을 가지고 있어야 한다.
- 따라서 충돌을 방지하기 위해서 URI 포맷을 사용한다.

```markdown
{
    "https://github.com/giibeom" : true
}
```

#### 3. 비공개 클레임 (Private Claim)

- 사용자 정의 클레임으로 서버와 클라이언트 사이에 임의로 지정한 정보를 저장 (커스텀 클레임)
- 공개 클레임과는 달리 이름이 중복되어 충돌될 수 있어서 사용에 유의해야 함

<img src="images/JWT_Private.png" width="40%" height="40%"/>

```markdown
📢 서명된 토큰의 경우 토큰 내 정보는 변조로부터는 보호되지만 누구나 읽을 수 있습니다
따라서 암호화 되지 않은 중요한 비밀 정보를 PayLoad나 Header에 넣는것은 지양해야 한다
```

<br>

### Signature

- 서명(Signature)은 토큰을 인코딩하거나 유효성 검증을 할 때 사용되는 고유한 암호화 코드
- 메시지가 도중에 변경되지 않았는지 확인하는데 사용
- 인코딩된 Header + 인코딩된 PayLoad + 인코딩된 비밀 키를 이용해 Header에서 정의한 알고리즘으로 해싱하고, 이 값을 다시 BASE64로 인코딩하여 생성
- 아래 서명 방식은 HS256 알고리즘으로 해싱하는 방식이다.

<img src="images/Signature.png" width="40%" height="40%"/>

<br>

## JWT 토큰 예시

- Header(빨강색).PayLoad(보라색).Signature(파란색)으로 이루어져 있다.

  <img src="images/JWT_Token_ex.png" width="40%" height="40%"/>
  

- 실제 토큰 사용 : `Authorization: Bearer <token>`

```markdown
생성된 토큰은 HTTP 통신을 할 때 Authorization이라는 key의 value로 사용된다!
일반적으로 value에는 "Bearer " 라는 prefix가 붙여진다.

{
	"Authorization": "Bearer {생성된 JWT 토큰값}"
}
```

<br>

## JWT 단점 및 고려사항

### 1. Self-Contained 방식 사용

- 토큰 자체에 정보를 담고 있다는 것은 양날의 검이 될 수도 있다.

### 2. 토큰 길이

- 토큰의 PayLoad에 3종류의 클레임을 저장하기 때문에, 데이터 정보가 많아질수록 토큰의 길이가 늘어난다.
- JWT 토큰은 HTTP 매 요청마다 헤더에 들고 가기 때문에 네트워크에 부하를 줄 수 있다.

### 3. Stateless

- JWT는 상태를 저장하지 않기 때문에 한번 생성되면 제어가 불가능하다.
- 따라서 토큰을 임의로 삭제하는 것은 불가능하므로 보안을 위해 토큰 만료시간은 꼭 설정해주어야 한다.

### 4. Store Token

- 생성된 JWT 토큰은 클라이언트 측에서 관리해야 하기 때문에 토큰을 클라이언트에 저장함으로써 보안에 취약하다
- 로컬 스토리지에 저장하면 XSS 공격에 취약하고, 쿠키에 저장하면 CSRF 공격에 취약하다.

<br>

## Access Token을 JWT Token으로 사용할 때의 장점
- 일반 토큰을 사용할 시에는 매 요청마다 토큰을 검증해야 한다
- 토큰 검증은 인증 DB를 통해서 진행한다 -> 서버 부하 야기
- JWT Token의 구성요소 중 하나인 Signature는 서명만으로도 바로 토큰의 무결성을 체크 할 수 있다
- 또한 JWT Token의 특징인 Self-Contained 방식으로 토큰 내 저장되어 있는 데이터를 통해 추가 작업을 실행 할 수 있다
- 이로써 DB Connection 감소함으로써 서버 및 DB의 부하를 감소시키는 장점이 있다



<br>
<br>

### Reference

- [위키백과](https://ko.wikipedia.org/wiki/OAuth)
- [OAuth2.0 개념 및 작동방식](https://velog.io/@kimjaejung96/OAuth2.0-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%9E%91%EB%8F%99%EB%B0%A9%EC%8B%9D)
- [Session Fixation 취약점](https://reiphiel.tistory.com/entry/session-fixation-vulnerability)
- [MSA School](http://www.msaschool.io/operation/design/design-seven/)
- [JWT(Json Web Token)란?](https://mangkyu.tistory.com/56)
- [JWT 토큰을 어디에 저장해야 하는가?](https://velog.io/@mokyoungg/JWT-%ED%86%A0%ED%81%B0%EC%9D%84-%EC%96%B4%EB%94%94%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94%EA%B0%80)
