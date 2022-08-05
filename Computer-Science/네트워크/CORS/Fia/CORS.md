# CORS (Cross Origin Resource Sharing)

## 이게 무엇인가요 ? 
- 브라우저에서 **cross-origin 요청을 안전하게 할 수 있도록 하는 메커니즘**

- 브라우저가 **보안적인 이슈**로 cross-origin http 요청을 제한할 때, 서버에 허가를 받으면 이를 승인할 수 있습니다.

- 이러한 메커니즘을 CORS 라고 부르며 **http 헤더를 이용**해서 동작시킬 수 있습니다.

- 기존에는 SOP 원칙에 의거하여 다른 도메인에서 리소스를 가져오는 것이 불가능 했지만, 어플리케이션 개선과 수월한 개발을 위해서는 외부 리소스 요청이 필수적이다. 
  이를 가능하게 하는 메커니즘이 **CORS** 이다.
  
## cross-origin 이란 ? 
- **프로토콜 / 도메인 / 포트 번호** 이 중에서 한개라도 다른 것을 의미 합니다.

## 왜 필요한가요 ? 
- 이러한 규약 없이 모든 곳에서 데이터를 요청할 수 있을 경우에는, 하나의 사이트를 동일하게 만들어 버릴 수도 있습니다.
- 동일한 화면 구성으로 로그인 했던 **세션을 탈취해 정보 유출, 다른 사람의 정보 입력 등의 공격**이 가능합니다.
    - 이러한 점을 불가능하게 만들기 위해서 브라우저에서 1차적으로 보호하고 **필요한 경우에만 서버의 승인을 받아 사용할 수 있도록 하기 위함** 입니다.

## 알고 가야 할 점
- 서버는 CORS 를 위반하더라도 정상적인 응답을 내려줄 수 있다. 
    - 이때 응답의 파기 여부는 **브라우저에서 결정**한다.
- CORS 는 **브라우저의 구현 스펙**이다. 해결하기 위해선 서버와의 협업이 필요하지만 **브라우저 정책으로 인한 이슈**이다. (서버적 결함이 아님)

## 동작 flow

1. Simple Request 인 경우
    - 이게 뭔가요 ? 
        - 요청 메서드 는 **GET / HEAD / POST** 중 하나여야 합니다.
        - 헤더로는 **Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width** 만 사용해야 합니다.
        - Content type 은 **application/x-www-form-urlencoded, multipart/form-data, text/plain** 중 하나를 사용해야 합니다.

- 서버에 요청 후 서버가 내려준 Access-Control-Allow-Origin 헤더 값을 확인하여, 교차 출처를 허용 여부를 결정 합니다.

![simple request](https://user-images.githubusercontent.com/49216939/179394813-2d7f474f-d7f0-4e91-aac1-22894c9a27e1.png)

2. Preflight Request 인 경우
    - **이게 뭔가요 ?**
        - 본 요청 전에 **예비 요청**을 보내서 안전한지 를 판단 한 후 본 요청을 보내는 방법
        - 예비 요청은 **OPTIONS 메서드로 실제 리소스 요청 전**에 보내진다.
   
- 예비 요청에 대한 서버의 Access-Control-Allow-Origin 헤더 값을 확인하여 교차 출처 허용 여부를 결정 합니다.
- 예비 요청을 보냄으로써 **리소스 낭비를 줄일 수 있고, CORS 대비 되지 않은 서버를 보호**할 수 있습니다.

![preflight request](https://user-images.githubusercontent.com/49216939/179394800-0fcb88a6-f702-4a29-a5c7-297cca1f3c82.png)

[이미지 출처](https://velog.io/@hyejeong/CORS-%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D)

3. Credentialed Request 인 경우
     - **이게 뭔가요 ?**
          - cross-origin 요청에는 자격 증명 정보가 담기지 않는데, 이를 허용하여 인증된 요청을 사용하도록 하는 방법(보안 강화)
          - 자격증명이 담긴 header 사용을 서버 측에서 허용하겠음을 명시해야 한다.
     - header 에 **credentials** 옵션을 추가해서, cross origin 요청 시 자격 증명 정보를 보낼 수 있습니다.
        - credentials : **include** -> 출처 불문 모든 요청에 인증 정보 담음
        - credentials : **include** -> 출처 불문 모든 요청에 인증 정보 담음
        - credentials : **omit** -> 모든 요청에 인증 정보 담을 수 없음
     
    - 서버에서 이를 동의 했다면  **Access-Control-Allow-Credentials: true** 해당 header 를 추가해 응답 합니다.
        - **Access-Control-Allow-Origin** 해당 헤더에는 와일드 카드(*)응답을 할 수 없습니다.
        - preflight request 의 응답이었다면, **인증 정보를 포함한 본 요청을 보내도 되는지에 대한 응답**
            - 본 요청에 인증 정보가 포함되어 요청 된다.
        - simple request 의 응답이었다면, **Access-Control-Allow-Credentials: true** 해당 헤더가 포함되어 있을 경우 응답 반환 , 없을 경우엔 브라우저 단에서 응답을 반환하지 않는다.


## 사용되는 headers
- 요청에 포함되는 헤더
    - Origin
   
    - Access-Control-Request-Method
        - preflight 요청시 **실제 요청에서 사용할 메서드**를 서버에 알리기 위함.
   
    - Access-Control-Request-Headers
        - preflight 요청시 **실제 요청에서 어떤 header 를 사용할 것**인지 서버에 알리기 위함.

- 응답에 포함되는 헤더
    -  Access-Control-Allow-Origin
        - 브라우저가 해당 origin 이 자원에 접근할 수 있도록 허용합니다. 
        - 값에 *(와일드 카드)은 **credentials 이 없는 요청에도 모든 origin 에서 접근이 가능하도록 허용**
    
    - Access-Control-Expose-Headers 
        - 브라우저가 접근 가능한 서버 **화이트리스트 헤더**를 허용
    
    - Access-Control-Max-Age 
        - preflight 요청이 **얼마나 오래 캐싱** 될 수 있는지를 알림 
    
    - Access-Control-Allow-Credentials
       - Credentials 가 true 일 때 **요청에 대한 응답 노출 여부**를 나타냄
       - preflight 요청에 대한 응답 일 때, **자격 증명을 사용하여 실제 요청을 수행할 수 있는지**를 나타냄
       - 간단한 GET 요청은 preflight 되지 않음
            - 자격 증명이 있는 리소스를 요청시, 헤더가 리소스와 함께 반환되지 않으면, **브라우저에서 응답을 무시하고 웹 콘텐츠로 반환하지 않음**
    
    - Access-Control-Allow-Methods
        - preflight 요청에 대한 대한 응답으로 **허용되는 메서드**를 나타냄
   
    - Access-Control-Allow-Headers 
        - preflight 요청에 대한 대한 응답, **실제 요청 시 사용할 수 있는 헤더를 알림**

## 안드로이드에서 CORS 상황이 발생하는 경우가 있나요 ? 
- WebView 로 화면을 구성할 때 간혹 발생하는 경우가 있습니다.
    - ex) web 내부의 동작이 자바스크립트 인터페이스를 통해서 안드로이드와도 상호작용이 가능한 경우

- 이때 Android WebView setting code 로 허용을 지정해줄 수 있습니다. 하지만 근본적인 해결책은 아닙니다.

```kotlin
val webViewSetting = webView.settings
    webViewSetting.setAllowFileAccessFromFileURLs(true)
    webViewSetting.setAllowUniversalAccessFromFileURLs(true)
```

## REFERENCE
- [document](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests)

- [reference](https://kamang-it.tistory.com/602)

- [reference](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F)

- [reference](https://evan-moon.github.io/2020/05/21/about-cors/)

