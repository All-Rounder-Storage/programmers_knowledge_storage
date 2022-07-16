# OAuth 2.0

## 무엇인가요 ? 
- Open Authorization 으로 다양한 플랫폼에서 자원 활용에 대한 권한 부여를 위해 만들어진 산업 표준 프로토콜
  
- 자격 증명을 공유하지 않고 클라이언트 앱이 사용자를 리소스 사용에 대한 액세스를 제공할 수 있는 개방형 표준 프로토콜 
  (온라인 인증을 위한 사실상 업계 표준)
  
- 주요 사용 플랫폼은 웹 이지만 다른 클라이언트 유형도 지원한다.

- OAUTH 1.0 의 구조적인 문제점과 세션 고정 공격을 보완하여 출시 되었다.

## Oauth 가 태어난 이유
- **서로가 서로를 믿을 수 없다**
```markdown
1. 사용자 : 다른 사이트의 id/pw 를 내가 만든 사이트에 공개하는 것에 대해 신뢰할 수 없음
2. 다른 사이트 : 내가 만든 사이트를 신뢰할 수 없음
3. 내가 만든 사이트 : id/pw 를 받았으니 문제가 발생하면 다 내 탓..?? 그리고 정확한 정보인지도 신뢰할 수 없음
```
## Oauth 2.0 용어를 알아보자

```markdown
Resource Server
- oauth 서비스를 제공하고 자원을 관리하는 서버 (ex. google, naver, etc.. )

Resource Owner, 자원 소유자
- resource server 의 계정을 소유하고 있는 사용자

Authorization Server, 권한 서버
- Client Resource Server 가 가진 자원을 사용할 수 있게 인증 및 토큰을 발생시켜주는 서버

Access Token
- 자원 서버에 자원을 요청할 수 있는 token

Refresh Token
- Access Token 만료 시 권한 서버에 접근 토큰을 요청할 수 있는 토큰
```

## 인증 절차의 흐름도
![OAUTH FLOW](https://user-images.githubusercontent.com/49216939/179351431-e6bfba28-0483-42cc-955e-a0a3ac0be977.png)

1. 클라이언트 -> 자원 소유자 : 인증 요청
    - 이 때 인증의 방식은 아래의 4가지 중 하나로 이용한다.
    
2. 자원 소유자 -> 클라이언트 : 인증 권한 부여

3. 클라이언트 -> 권한 서버 : 부여받은 인증 권한으로 access token 발급 요청

4. 권한 서버 -> 클라이언트 : 유효성 검사 후 access token 발급

5. 클라이언트 -> 자원 서버 : access token 으로 자원 소유자의 자원에 접근 요청

6. 자원 서버 -> 클라이언트 : access token 검사 후 통과 시 요청에 대한 응답 전송

## 인증 절차의 종류
```markdown
Authorization Code Grant
- 리소스 접근을 위해 서버에서 받은 권한 코드로 리소스에 대한 액세스 토큰을 받는 방식
- 다른 절차 중 보안성이 가장 높아서, 가장 복잡함에도 불구하고 가장 많이 쓰인다.
```
![AUTH CODE GRANT](https://user-images.githubusercontent.com/49216939/179352677-3bed27bc-0c11-4f28-937e-8f893250fe90.png)
```markdown
Implicit Grant
- 권한 코드를 교환하는 단계가 있다.
- access Token 을 즉시 반환 받아서 이를 인증에 이용하는 방식이다.
```
![implicit grant](https://user-images.githubusercontent.com/49216939/179352767-48a5ded1-113c-494c-bd0e-8aa504106210.png)
```markdown
Resource Owner Password Credentials Grant
- resource owner 가 id/pw 를 전달 받아서 resource server 에 인증하는 방식
- 신뢰할 수 있는 client 로만 가능한 방식이다.
```  
![password credentials](https://user-images.githubusercontent.com/49216939/179353017-50ff7a99-f48e-4311-8e1e-8b599d4b8576.png)

```markdown
Client Credentials Grant
- 클라이언트가 외부에서 액세스 토큰을 얻어 특정 리소스에 접근 요청을 할 때 사용하는 방식
```
![client authentication](https://user-images.githubusercontent.com/49216939/179352988-18b00d99-d6b0-4cb4-96bc-5a1071c4c0c8.png)


## 그럼 OAUTH 2.0 이 앱에서 실질적으로 사용되는 순간은 ?

```markdown
상황 : 사용자가 앱 실행 후 facebook 으로 로그인 하기 선택

이후 발생하는 흐름
1. facebook 이 제공하는 ui 를 통해 로그인 시도

2. 로그인 성공 시, 해당 앱(페이스북으로 로그인을 시도한 앱)애서 필요로 하는 권한을 알리는 ui 노출
   - 여기까진 facebook 이 처리(우리 앱 개발자는 문서에 따라 facebook 로그인을 연결 해두면 된다, ui 직접 구현하지 않음)

3. 유저가 모두 확인을 하면 redirect url 과 authorization 코드를 발급해 사용자에게 전달

4. 우리 서버는 redirect url 과 authorization code 를 가지고 페이스북 서버에 접근

5. 페이스북의 서버는 전달받은 정보를 바탕으로 access Token / Refresh Token 발급
```


## Reference
- [eng document](https://datatracker.ietf.org/doc/html/rfc6749)
  
- [블로그](https://doqtqu.tistory.com/295 )