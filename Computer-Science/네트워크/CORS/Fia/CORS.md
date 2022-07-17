# CORS (Cross Origin Resource Sharing)

## 이게 무엇인가요 ? 
- 브라우저가 보안적인 이슈로 cross-origin http 요청을 제한할 때, 서버에 허가를 받으면 이를 승인할 수 있습니다. 
  이러한 메커니즘을 cors 라고 부르며 http 헤더를 이용해서 동작시킬 수 있습니다.
  기존에는 SOP 원칙에 의거하여 다른 도메인에서 리소스를 가져오는 것이 불가능 했지만, 
  어플리케이션 개선과 수월한 개발을 위해서는 외부 리소스 요청이 필수적이다.
  

  
- 브라우저에서 cross-origin 요청을 안전하게 할 수 있도록 하는 메커니즘 

## cross-origin 이란 ? 
- 프로토콜 / 도메인 / 포트 번호  이 중에서 한개라도 다른 것을 의미 합니다.


## 왜 필요한가요 ? 
- 이러한 규약 없이 모든 곳에서 데이터를 요청할 수 있을 경우에는, 하나의 사이트를 동일하게 만들어 버릴 수도 있습니다.
- 동일한 화면 구성으로 로그인 했던 세션을 탈취해 정보 유출, 다른 사람의 정보 입력 등의 공격이 가능합니다.
  이러한 점을 불가능하게 만들기 위해서 브라우저에서 1차적으로 보호하고 필요한 경우에만 서버의 승인을 받아 사용할 수 있도록 하기 위함 입니다.
  
## 동작 flow

1. Simple Request 인 경우
    - 이게 뭔가요 ? 
      > 요청 메서드 는 GET / HEAD / POST 중 하나
      > Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width 만 사용
      > Content type 은 application/x-www-form-urlencoded, multipart/form-data, text/plain 중 하나 사용
- 서버에 요청 후 서버의
## 