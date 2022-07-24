# OAuth 2.0

`OAuth2.0 (Open Authorization 2.0) 는 접근 위임을 위한 개방형 프로토콜로, 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상 ( 예를들어, Facebook 이나 Twitter )의 자신들의 정보에 대해 다른 애플리케이션(데스크톱, 웹, 모바일 등) 에서도 사용할 수 있게 한다`

```
💡 아래는 Band의 회원가입 화면으로 Band 자체의 회원가입 없이
   로그인을 제공하는 플랫폼의 (아래는 Naver와 Facebook) 계정이 존재한다면 
   외부 서비스에서도 인증을 가능하게 한다 
```

<img width="406" alt="Screen Shot 2022-07-24 at 9 54 55 AM" src="https://user-images.githubusercontent.com/61952198/180631127-7192bfc0-e1a9-40ad-8b03-2b896bca5899.png">

<caption><a href="https://auth.band.us/sign_up_form?next_url=https%3A%2F%2Fband.us%2F#">Band 회원가입 페이지</a> </caption>

---

## OAuth 와 로그인

- OAuth 와 로그인은 분리해서 이해해야 한다

```
💡 OAuth와 로그인을 이해하기 위한 한가지 예시를 살펴보자

    - 사원증을 이용해 출입할 수 있는 회사를 생각해 보면 외부 손님이 그 회사에 방문할 일이 있을 것이다. 
    - 회사 직원이 건물에 출입하는 것이 로그인이라면 OAuth는 외부 손님이 방문증을 수령한 후 회사에 출입하는 것에 비유할 수 있다.
    
    => '방문증'이란 사전에 정해진 곳만 다닐 수 있도록 하는 것이다. 
    => '방문증'을 가진 사람이 출입할 수 있는 곳과 '사원증'을 가진 사람이 출입할 수 있는 곳은 다르다. 
    => 역시 직접 서비스에 로그인한 사용자와 OAuth를 이용해 권한을 인증받은 사용자는 각각 할 수 있는 일이 다르다.
```

- OAuth 에서 'Auth' 는 'Authentication'(인증)뿐만 아니라 'Authorization'(인가) 또한 포함하고 있다
- 그렇기 때문에 OAuth 인증을 진행할 때 해당 서비스 제공자는 '*제 3자가 어떤 정보나 서비스에 사용자의 권한으로 접근하려 하는데 허용하겠느냐*' 라는 안내 메시지를 보여준다

---

## OAuth 2.0 용어 및 역할

- OAuth 2.0의 주요 용어

| 용어 | 설명 |
| --- | --- |
| Authentication (인증) | 접근 자격이 있는지 검증하는 단계이다. |
| Authorization (인가) | 자원에 접근할 권한을 부여하는 것으로 인가가 완료되면 리소스 접근 권한이 담긴 Access Token 이 클라이언트에게 제공된다. |
| Access Token | 리소스 서버에게서 리소스 소유자의 보호된 자원을 획득 할 때 사용되며 만료 기간이 있는 Token 이다. |
| Refresh Token | Access Token 만료시 재발급 받기위한 용도로 사용하는 Token 으로 일반적으로 Access Token 보다 만료기간이 길다. |
- OAuth 2.0의 역할

| 역할 | 설명 |
| --- | --- |
| Resource Owner (리소스 소유자) | - 리소스 소유자, 즉 사용자이다. 본인의 보호된 자원에 접근할 수 있는 자격을 부여하는 주체이다. </br> - OAuth 2.0 흐름에서 Client를 인증하는 역할을 수행한다. </br> - 인증이 완료되면 권한 획득 자격 (Authorization Grant)을 Client 에게 부여한다. </br> - 개념적으로는 리소스 소유자가 자격을 부여하는 것이지만 일반적으로 권한 서버가 리소스 소유자와 클라이언트 사이에서 중개 역할을 한다. |
| Client (클라이언트) | - Resource Owner, 사용자의 보호된 자원을 사용하려고 접근 요청을 하는 어플리케이션 이다. |
| Resource Server  | - Resource Owner, 사용자의 보호된 자원이 저장되어 있는 서버이다. |
| Authorization Server (권한 서버) | - 인증 / 인가를 수행하는 서버로 클라이언트의 접근 자격을 확인하고 Access Token 을 발급하여 권한을 부여한다. |

---

## OAuth 2.0 프로토콜 흐름도

<img width="530" alt="Screen Shot 2022-07-24 at 12 01 54 PM" src="https://user-images.githubusercontent.com/61952198/180631130-941699d9-c573-4a3d-9339-6004dbe9b49e.png">


1. 클라이언트(Client)가 사용자(Resource Owner)에게 `Authorization` 을 요청 (request)한다
    - Authorization Request 는 사용자(Resource Owner)에게 바로 요청될 수 있으나, 보통 중개 역할을 하는 Authorization server를 통해 간접적으로 이루어진다.
2. 클라이언트(Client)가 사용자(Resource Owner)의 인증을 나타내는 `credential` 즉, `authorization grant` 를 받는다
3. 클라이언트(Client)는 `authorization grant` 을 가지고 `authorization server` 에  `access token` 을 요청한다
4. `authorization server` 는 `authorization grant` 가 유효한지 확인하고 `authorization grant` 가 유효하다면 `access token` 을 발급한다
5. 클라이언트(Client)는 부여 받은 `access token` 을 가지고 인가된 상태를 나타내며 `resource server` 로 사용자(Resource Owner)의 보호된 자원을 요청한다
6. `resource server` 는 `access token` 이 유효한지 확인하고 유효하다면 클라이언트(Client)의 요청을 수행한다

---

## OAuth 2.0 권한 부여 방식

- OAuth 2.0 권한 부여 방식에 따른 프로토콜을 4가지 종류로 구분하여 제공한다.
    1. ****Authorization Code Grant**** (권한 부여 승인 코드 방식)
    2. ****Implicit Grant**** (암묵적 승인 방식)
    3. ****Resource Owner Password Credentials Grant**** (자원 소유자 자격 증명 승인 방식)
    4. ****Client Credentials Grant**** (클라이언트 자격증명 승인 방식)


### 1.  ****Authorization Code Grant**** (권한 부여 승인 코드 방식)

- **권한 부여 승인을 위해 자체 생성한 `authorization code`를 전달하는 방식으로 클라이언트가 사용자를 대신하여 특정 자원에 접근을 요청할 때 사용 되는 방식이다**
- 간편 로그인 기능에서 사용 되는 방식으로 가장 많이 쓰이고 기본이 되는 방식이다
- `refresh token`의 사용이 가능한 방식이다

![Authorization Code Grant](https://user-images.githubusercontent.com/61952198/180631135-e1b127a9-d962-439b-8bb6-06170d878c33.png)


- 권한 부여 승인 요청 시 `response_type`을 `token`으로 설정하여 요청한다
- 클라이언트는 권한 서버에서 제공하는 로그인 페이지를 브라우저를 띄워 출력하고 사용자가 로그인을 하면 권한 서버는 권한 부여 승인 코드 요청 시 전달받은 `redirect_url` 로 `authorization code`를 전달한다
- `authorization code`는 권한 서버에서 제공하는 API를 통해 `access token`으로 교환된다

### 2. ****Implicit Grant**** (암묵적 승인 방식)

- **자격증명을 안전하게 저장하기 힘든 클라이언트(example, JavaScript와 같은 스크립트 언어를 사용한 브라우저)에게 최적화된 방식이다**
- 권한 부여 승인 코드 없이 URL로 바로 `access token`이 발급 된다.
- 바로 `access token` 발급되어 전달 되므로 누출의 위험을 방지하기 위해 만료기간을 짧게 설정한다
- `refresh token`의 사용이 불가능한 방식이다
- 권한 서버는 `client_secret`를 사용해 클라이언트를 인증하지 않고 `access token`을 획득하기 위한 절차가 간소화되어 응답성과 효율성은 높아지지만 보안 위험이 있다

![Implicit Grant](https://user-images.githubusercontent.com/61952198/180631139-9851bcc7-ee4c-4f29-b518-00480404b17b.png)


- 권한 부여 승인 요청 시 `response_type`을 `token`으로 설정하여 요청한다
- 클라이언트는 권한 서버에서 제공하는 로그인 페이지를 브라우저를 띄워 출력하고 사용자가 로그인을 하면 권한 서버는 권한 부여 승인 코드 요청 시 전달받은 `redirect_url` 로 `access token` 을 전달한다

### 3. ****Resource Owner Password Credentials Grant**** (자원 소유자 자격 증명 승인 방식)

- **username, password로 Access Token을 받는 방식이다**
- 클라이언트가 자신의 서비스에서 제공하는 애플리케이션일 경우에만 사용한다
- `refresh token`의 사용이 가능한 방식이다

![Password Grant](https://user-images.githubusercontent.com/61952198/180631141-5cfb9b47-ca85-4f82-bf9a-e27a29091370.png)


### 4. ****Client Credentials Grant**** (클라이언트 자격증명 승인 방식)

- **클라이언트의 자격증명만으로 `access token`을 획득하는 방식이다**
- OAuth 2.0의 권한 부여 방식 중 가장 간단한 방식이다
- 클라이언트 자신이 관리하는 리소스 혹은 권한 서버에 해당 클라이언트를 위한 제한된 리소스 접근 권한이 설정되어 있는 경우 사용한다
- `refresh token`의 사용이 불가능한 방식이다

![Client Credentials](https://user-images.githubusercontent.com/61952198/180631142-88abb46a-3143-424a-8fb3-46d4a8741187.png)


### Reference

---

- [An Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)
- [OAuth 와 춤을](https://d2.naver.com/helloworld/24942)
- [OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749)
