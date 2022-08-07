# async/await 은 무엇일까요

- async/await은 [프로미스는 무엇일까요](https://github.com/dbwjd5864/programmers_knowledge_storage/blob/main/Frontend/Javascript/프로미스는_무엇일까요.md) 에서 다뤘던 Promise를 기반으로 동작하면서 **비동기 처리를 동기 처리처럼 동작하도록 구현**하게 해준다.
    - 즉, Promise의 후속처리 메서드인 then/catch/finally에 콜백함수를 전달하여 후속 처리를 하지 않고도 프로미스의 처리 결과를 사용할 수 있다.

<br />

## async 함수

- `async` 는 function 앞에 위치한다.
- `async` 가 붙은 function 은 항상 **프로미스를 반환한다.**
- `async` 함수가 명시적으로 프로미스를 반환하지 않거나 프로미스가 아닌값을 반환 하더라도, `async` 함수는 암묵적으로 반환값을 **resolve** 하는 프로미스를 반환한다.

    ```jsx
    // 아래의 함수를 호출하면 result가 1인 이행 프라미스가 반환된다.
    // 함수 선언문 ✅
    async function example(x) {
      return x;
    }
    
    example(1).then(result => console.log(result)); // 1
    ```

    - async 함수는 다양한 형태의 함수로 정의가 가능하나 클래스의 **constructor 메서드는 async 메서드가 될 수 없다.**

    ```jsx
    // 함수 표현식 ✅
    const example = async function(x) {
      return x;
    }
    
    example(2).then(result => console.log(result)); // 2
    ```

    ```jsx
    // 화살표 함수 ✅
    const example = async (x) => {
      return x;
    }
    
    example(3).then(result => console.log(result)); // 3
    ```

    ```jsx
    // async 메서드 ✅
    const obj = {
      async example(x){
    		return x;
    	}
    }
    
    obj.example(4).then(result => console.log(result)); // 4
    ```

    ```jsx
    // async 클래스 메서드 ✅
    class ExampleClass {
    	async example(x) {
    		return x;
    	}
    }
    
    const myExample = new ExampleClass();
    myExample.example(5).then(result => console.log(result)); // 5
    ```

    ```jsx
    // constructor 메서드 ❌
    
    // async 함수는 언제나 프로미스를 반환해야하는데
    // constructor 메서드는 인스턴스를 반환하기 때문이다.
    class ExampleClass {
    	async constructor() {} 
    	// **SyntaxError: Class constructor may not be an async method**
    }
    
    const myExample = new ExampleClass();
    ```

<br />

## await 키워드

- `await` 은 async 함수 안에서만 동작한다.
- `await` 키워드는 반드시 프로미스 앞에서 사용해야 한다.

    ```jsx
    // 일반 함수 안에서는 await을 사용할 수 없다.
    function example() {
      let result = await Promise.resolve(1); 
    	// **SyntaxError: await is only valid in async functions and the top level bodies of modules**
    }
    ```

- `await` 은 프로미스가 settled 상태, 즉 비동기 처리가 수행된 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
    - **프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 변수에 할당된다.**

    ```jsx
    async function example() {
      // 1초이후 이행되는 프로미스
      let promise = new Promise((resolve, reject) => {
        setTimeout(() => resolve("끝이지롱"), 1000)
      });
    
      let result = await promise; // **프라미스가 이행 (settled) 될 때까지 기다린다
    
      // 프로미스가 이행 될 때까지 아래 코드들은 중단된다.**
    
      console.log(result); // "끝이지롱"
    }
    
    example();
    ```

    - example 함수를 호출하고 함수 본문이 실행되는 도중에 `let result = await promise;`에서 그 다음의 코드 실행이 잠시 '**중단**’된다.
    - 프로미스가 처리된 후에 중단됐던 함수의 실행이 재개되고, 프라미스 객체의 `result` 값이 변수 result 에 할당된다.
        - 따라서 위 예시를 실행하면 1초 뒤에 ‘끝이지롱’이 출력된다.
    - `await(기다리다)`는 말 그대로 프로미스가 settled 될 때까지 함수 실행을 지연시키고 프로미스의 결과값이 주어졌을 때 실행을 재개시킨다. 
    -  프로미스가 처리되길 기다리는 동안에 자바스크립트 엔진은 다른 일 (다른 스크립트를 실행하거나 이벤트를 처리하는 등) 을 처리할 수 있기 때문에 CPU 리소스는 낭비되지 않는다.
    - `await`는 단지 `promise.then` 보다 더 세련되게 `result` 값을 얻을 수 있도록 해주는 문법이며 `promise.then` 보다 가독성이 뛰어나고 사용하기도 더 쉽다.

<br />

## 에러 처리

- async/await 은 `try …catch`문을 사용하여 에러 처리를 할 수 있다.

    ```jsx
    const example = async () => {
        try{
            const wrongUrl = '나는 잘못된 url~';
  
            const response = await fetch(wrongUrl);
            const data = await response.json();
            
            console.log(data);  
        }catch(err){
            console.error(err) // **TypeError: Failed to fetch**
        }
    } 
    ```

    - 위 예제의 함수는 catch 문의 HTTP 통신뿐만아니라 try 코드 블록 내의 모든 에러를 캐치 할 수 있다.
- **async 함수 내에서 catch 를 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.**
    - async/await도 프로미스 기반이기 때문에 이렇게 발생한 에러를 async 함수를 호출하고 프로미스의 후속 처리 메서드(catch) 를 통해서도 잡을 수 있다.

    ```jsx
    const example = async () => {
        const wrongUrl = '나는 잘못된 url~';
    
        const response = await fetch(wrongUrl);
        const data = await response.json();
    		
        return data;
    } 
    
    // 하지만 보통 이렇게 많이 사용되지 않고 try ...catch 구문을 이용한다.
    // +) 하지만 만약에 async function 안이 아니라 await 키워드를 사용하지 못한다면 then/catch를 사용하기도 한다.
    example()
    .then(result => console.log(result))
    .catch(err => console.error(err)); 
    // **TypeError: Failed to fetch** 
    ```
  
<br />

### Reference

---

- [async와 await](https://ko.javascript.info/async-await)
- [제너레이터와 async/await](https://poiemaweb.com/es6-generator)