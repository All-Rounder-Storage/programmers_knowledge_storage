# HTTP 메서드

`클라이언트가 서버로 HTTP Request를 보낼 때 request가 의도한 action을 HTTP 메서드라고 한다.`

- Example)  *GET* / search HTTP / 1.1
- 해당 주소에 대한 어떤 동작을 하려고 할 때 알맞은 HTTP 메서드를 사용해야 한다.

## 1. HTTP 메서드의 속성

1. 안전성 (Safe Methods)
    - 계속해서 메소드를 호출해도 리소스를 변경하지 않는다.
    - HTTP 메서드: GET, HEAD, OPTIONS, TRACE
2. 멱등성 (Idempotent Methods)
    - 계속해서 메소드를 호출해도 결과가 똑같다.
    - 직접 호출하는 것에 한하여 멱등한지를 판별한다. (외부 요인은 무시한다.)
    - HTTP 메서드: GET, PUT, DELETE, HEAD, OPTIONS, TRACE
3. 캐시 가능 (Cacheable Methods)
    - 캐싱하여 데이터를 효율적으로 가져올 수 있다.
    - HTTP 메서드: GET, HEAD, POST, PATCH

## 2. HTTP 메서드의 종류

- HTTP 메소드의 종류는 총 9가지가 있다.

#### 주요 메소드 5가지

1. GET : 데이터 조회에 사용한다.
   - GET 요청은 멱등하기 때문에 계속해서 호출해도 항상 같은 결과를 받는다.
   - 리소스 조회 요청이기 때문에 Body 값과 Content-Type 은 비워져있다.
   - 데이터를 변경하는 요청에 사용하면 안된다.
   - 캐싱이 가능하여 같은 데이터를 조회할 경우에 캐싱한 값을 사용하여 조회 속도가 빨라진다.
   - 서버에 전달하고 싶은 데이터가 있을 때는 query parameter 를 통해서 전달한다.
   
   ```HTML
   GET /user/1
   GET /user?name=jade // query parameter 를 통한 데이터 전달
   ```
    
2. POST : 주로 데이터 등록에 사용한다.
   - POST 요청은 멱등하지 않기 때문에 항상 같은 결과물이 보장되지 않는다.
   - 데이터를 생성: Body 값과 Content-Type 값을 작성해야한다.
   - 데이터는 URL 을 통해서 받지 않고, Body 값을 통해서 받는다.
   - POST 를 사용하여 BODY에 조회할 데이터를 전달할 수 있지만, POST 는 캐싱하기에 어려운 문제가 있기 때문에 권장하지 않는다.

    ```HTML
   POST /user
   body: {name : "Jade", age: 16}
   Content-Type : "application/json"
   ```
   
3. PUT : 리소스를 대체, 해당 리소스가 없으면 생성한다. ➡ **기존 리소스를 덮어쓴다.️**
   - PUT 요청은 멱등하기 때문에 동일한 데이터를 호출해도 같은 결과를 받는다.
   - 데이터를 수정: 요청시에 Body 값과 Content-Type 값을 작성해야한다  
   - URL 을 통해서 어떤 리소스를 수정할지를 받는다.
   - 수정할 데이터 값은 Body 값을 통해서 받는다.

   Before resource
   ```JSON
   {
    "name": "Jade",
    "age" : 16
   }
   ```

   ```HTML
   PUT /user/1
   body: {age : 26}
   Content-Type : "application/json"
   ```

   After resource
    ```JSON
   {
    "age": 26
   }
   ```
   
4. PATCH : PUT 과는 다르게 리소스를 **일부만 변경** 가능하다.
   - PATCH 는 멱등하지 않다. (사실상, 구현방법에 따라 멱등하고 멱등하지 않게 둘다 가능하다.)
     - PATCH 메소드에 수정할 리소스의 일부분만 담아서 보내는 경우에는 멱등성이 보장된다.
      ```HTML
         PATCH /user/1
         body: {age : 26}
         Content-Type : "application/json"
      ```
     - 하지만, 특정 부분을 추가로 더하는 식으로도 보낼 수 있기 때문에 멱등하지 않다.
     - API가 호출될때마다 나이가 1씩 증가한다.
     ```HTML
        PATCH users/1
        body:{  
            $increase: 'age', // 증가시키고 싶은 속성
            value: 1 // 속성을 얼마나 증가시킬지
        }
        Content-Type : "application/json"
      ```
     
   - 데이터를 수정: 요청시에 Body 값과 Content-Type 값을 작성해야한다
   - URL 을 통해서 어떤 리소스를 수정할지를 받는다.
   - 수정할 데이터 값은 Body 값을 통해서 받는다.

5. DELETE : 리소스 삭제에 사용한다.
   - DELETE 요청은 멱등하기 때문에 응답 코드는 달라질 수 있어도 요청한 리소스의 삭제가 일어나는 것은 같다.
   - 데이터를 삭제: 요청시에 Body 값과 Content-Type 값이 비워져있다.
   - URL 을 통해서 어떤 리소스를 삭제할지를 받는다. 

   ```HTML
   DELETE /user/1
   ```

#### 기타 메소드 4가지

1. HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환한다.
2. OPTIONS: 대상 리소스에 대한 통신 가능 옵션을 설명(주로 CORS에서 사용)한다.
3. CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정한다.
4. TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행한다.

<br>

Reference

---

- [HTTP 메서드](https://www.zerocho.com/category/HTTP/post/5b3723477b58fc001b8f6385)
- [PATCH의 멱등성](https://www.inflearn.com/questions/110644)