# Synchronous, Asynchronous / Blocking, Non-Blocking

<br />

### 헷갈린다. 너란 **Synchronous, Asynchronous / Blocking, Non-Blocking**

---

- 자바스크립트를 공부하면서 동기(Synchronous)와 비동기(Asynchronous)라는 단어 자체는 참 많이 들어봤고 코드상에서 사용은 많이 하면서 `동기는 함수들의 실행 순서가 보장` 되고 `비동기는 보장되지 않는다` 정도로만 알고있었다.
- 그래서 동기와 비동기를 더 찾아보았는데, 동기와 비동기에서 끝나는 것이 아니라 Blocking, Non-Blocking이라는 개념도 같이 등장하면서 오히려 혼돈의 구멍으로 뺘져들었다. 🤪

<br />

- 처음에는 비슷한 개념이 아닌가? 하는 생각이 들었는데 단어의 뜻을 이해해보고 예시로 풀어보니 **Synchronous, Asynchronous / Blocking, Non-Blocking** 은 완전히 **다른 개념**이며 두 개념이 바라보는 관점 자체가 다르다는 것을 깨달았다.
- 이렇게 각각의 개념이 다르기 때문에, 코드를 짜다보면 `두가지 중에 하나만 써야지!` 가 아니라 두 개념이 섞여서 녹아들 수 밖에 없다. 필자의 코드에도 동기 비동기만으로 이분법적으로 나눠져 있는 것이 아니라 blocking, non-blocking 개념도 함께 녹아져 있다는 것을 알게되었다.

<br />

- 아마 필자처럼 **Synchronous, Asynchronous / Blocking, Non-Blocking** 검색을 해봤다면 아래의 사진은 한번쯤 만나봤을 것이다.

  ![sync,asyn,block,non-block](https://user-images.githubusercontent.com/61952198/188845930-db88d9eb-921e-4daf-8003-92859bb7bdf6.png)

  이미지 출처: [https://interconnection.tistory.com/141](https://interconnection.tistory.com/141)

- 그런데 솔직히 이 사진만 봤을 때는 이걸 어떻게 이해해야 하지 싶을 것이다. 지금은 이렇게 같이 사용된다라는 정도만 알아두고 **Synchronous, Asynchronous / Blocking, Non-Blocking** 각각을 먼저 알아본 후 어떻게 서로 함께 사용되는지 밑에서 더 알아보면 좋을 것 같다.

<br />

### 바라보는 관점이 다른 **Synchronous, Asynchronous / Blocking, Non-Blocking**

---

- 말 그대로 두가지 개념은 각각 다른 관점을 가지고 있다.

<br />

**Synchronous, Asynchronous : 전체적인 흐름속에서 각각의 작업들에 완료 여부를 신경 쓰느냐 안쓰느냐에 따른 작업들의 순서가 보장되는지에 대한 관점**

**Blocking, Non-Blocking : 전체적인 흐름속에서 한 작업을 수행했을 때 그 작업이 전체적인 흐름을 막는지 안막는지에 대한 관점**

<br />

자, 조금 더 자세하게 들어가보자.

<br />

**Synchronous와 Asynchronous**

---

- **Synchronous**의 사전적 의미를 찾아보면 이렇게 나온다.

    
> [happening or existing at the same time](https://www.oxfordlearnersdictionaries.com/definition/english/synchronous)
  - `동시에 발생하거나 존재하는`


- 이와 반대로 **Asynchronous**의 사전적 의미는 이렇다.

> [not existing or happening at the same time](https://www.oxfordlearnersdictionaries.com/definition/english/asynchronous?q=asynchronous)
  - `동시에 발생하거나 존재하지 않는`

<br />

- 사실 **Synchronous 와 Asynchronous**는 형용사이기 때문에 `무엇이 동시에 발생하거나 존재하는지`에 무엇은 상황에 따라 달라질 수 있다.
- 필자는 **Synchronous, Asynchronous** 에서의 무엇을 **한 작업의 완료와 다음 작업의 수행** 으로 표현해보려한다.

<br />

- **Synchronous**에서 무엇을 필자가 말한 상황에 대입해보면 **Synchronous는 한 작업의 완료와 다음 작업의 수행이 동시에 발생한다로 볼 수 있고, Asynchronous는 한 작업의 완료와 다음 작업의 수행이 동시에 발생하지 않는다로 볼 수 있다.**


![sync,async](https://user-images.githubusercontent.com/61952198/188846733-181b41ac-a1da-4b6f-8e3b-8e1b3c2f6764.png)


- 이렇게 한 작업의 완료와 다음 작업의 수행이 동시에 일어난다는 것은 `다음으로 수행돼야 하는 작업`이 `이전 작업의 완료 여부에 대해서 관심`을 가지고 지켜보고 있다라는 말이 된다.
- 또한, 어떤 작업이 끝나고 다음 작업을 실행하는 것이기 때문에 `작업들이 어떠한 순서를 가지고 진행됨`을 의미하게 되는 것이다.

<br />

그렇기 때문에 **Synchronous, Asynchronous는 작업들의 순서가 보장되는지에 대한 관점이라고 보는 것이다.**

그렇다면 **Blocking, Non-Blocking의 어떤 작업이 전체적인 흐름을 막는지 안막는지에 대한 관점은 무슨 말일까?**

<br />

**Blocking과 Non-Blocking**

---

- 우리가 실생활에서도 자주 쓰는 단어인 **Blocking**은 `무엇인가를 막는다`라는 의미를 가지고 있고, **Non-Blocking**은 `무엇인가를 막지 않는다` 라는 의미를 가지고 있다.

<br />

- 여기서 무엇을 막는다라는 것에 `무엇`은 `한 작업을 실행하고 있을 때 다른 작업의 실행을 막는다`로 이해해보려 한다.
- 컴퓨터에서 작업을 실행시키는 것을 막는다는 것은 `제어권`이 누구한테 있느냐와 관련이 있다. ( `여기서 **제어권은 작업을 실행 시킬수 있는 권리**정도로 보면 좋을 것 같다.` )

<br />

- 간단하게 A 함수와 B 함수를 가지고 예시를 들어보자면

    ```
    제어권을 가지고 있는 A 함수가 B 함수를 호출해서 제어권을 B 함수에 넘겨준다면, 
    A 함수는 B 함수 실행 코드 이후의 코드들은 더이상 실행하지 못하고 B 함수가 제어권을 다시 넘겨줄 때까지
    기다려야 한다. 이러한 상황을 **Blocking** 이라고 한다.
    
    이와 반대로, A 함수가 B 함수를 호출해도 제어권은 그대로 자신이 가지고 있으면서 
    B 함수 실행 코드 이후의 코드들을 계속 실행한다면 이는 곧 **Non-Blocking** 이라고 한다.
    ```


![블락킹](https://user-images.githubusercontent.com/61952198/188846097-b651775c-0edc-4d14-ada4-08946a129b95.png)
![논블락킹](https://user-images.githubusercontent.com/61952198/188846194-369ee57b-7277-4853-8c2a-40b7a2277fc6.png)

이렇게 관점이 다른 동기, 비동기 / 블로킹, 논블로킹은 어떻게 같이 사용 되게 될까?

<br />

### 다시 돌아온 **Synchronous, Asynchronous / Blocking, Non-Blocking**

---

- 글 초기 이미지에서 봤던것처럼 **Synchronous, Asynchronous / Blocking, Non-Blocking** 조합은 4가지로 나눌 수 있다.
    1. **Synchronous + Blocking**
    2. **Synchronous + Non-Blocking**
    3. **Asynchronous + Non-Blocking**
    4. **Asynchronous + Blocking**

<br />

1. **Synchronous + Blocking (동기 + 블로킹)**

---

- 동기 방식이기 때문에 **작업의 흐름순서가 보장**되지만, 블로킹 방식이기 때문에 한 작업이 수행 될 때 다른 작업을 동시에 진행될 수 없고 기다려야 한다.

    ```jsx
    function watchOnlineClass () {
        let minute = 100;
        while(minute > 0){
            console.log(`강의 종료까지 ${minute}분 남았습니다.`);
            minute--;
        }
    }
    
    function study () {
        console.log('공부 시작!');  
        watchOnlineClass();
        console.log('공부 끝!');
    }
    
    study();
    ```

    ```
    공부 시작!
    강의 종료까지 100분 남았습니다.
    강의 종료까지 99분 남았습니다.
    ...
    강의 종료까지 2분 남았습니다.
    강의 종료까지 1분 남았습니다.
    공부 끝!
    ```

- 위의 예시에서 볼 수 있듯이 작업들은 흐름을 가지고 진행되게 되며, 온라인 강의를 다 듣고 나서야 공부를 끝마칠 수 있게된다.
    - 공부 시작! 을 외치면서 `study` 함수를 시작하게 되고
    - `watchOnlineClass` 함수를 실행 시키면서 `watchOnlineClass` 가 끝날때까지 기다린다. (**Blocking**)
    - `watchOnlineClass` 함수가 끝나고 나서야 비로서 `study` 함수는 공부 끝!을 외치고 작업을 마칠 수 있게된다. (**Synchronous**)


위에서의 예시가 `watchOnlineClass` 를 실행하여 강의 시청중에는 아무것도 하지 못하는 블로킹이였다면 강의 시청중에 노트 필기까지 같이 하려고 한다면 이는 곧 **Synchronous + Non-Blocking (동기 + 논블로킹) 를 의미한다.**

<br />

2. **Synchronous + Non-Blocking (동기 + 논블로킹)**

---

- 동기 방식이기 때문에 **작업의 흐름순서가 보장**되지만, 논블로킹 방식이기 때문에 한 작업이 수행하면서 다른 작업도 동시에 수행할 수 있다.

💡 자바스크립트의 제너레이터를 이용하면 동기 + 논블로킹을 구현 해낼 수 있다.

- 제너레이터를 더 자세하게 알고싶다면 [여기](https://ko.javascript.info/generators)를 참고하면 좋을 것 같다.
- 우선은 제너레이터는 제너레이터를 호출한 호출자에게 제어권을 넘겨줄 수 있다 정도로 알고 예시를 보도록 하자

```jsx
function* watchOnlineClass () {
  let minute = 100;
    while(minute > 0){
        console.log(`강의 종료까지 ${minute}분 남았습니다.`);
        minute--;
        yield;
	}
}

function study () {
    console.log('공부 시작!');
    
    const generator = watchOnlineClass();
    let watch = {};
        
    while (!watch.done) {
        console.log(`틈틈이 강의 노트 필기~`);
        watch = generator.next();
    }
    console.log('공부 끝!');
}

study();
```

```
공부 시작!

틈틈이 강의 노트 필기~
강의 종료까지 100분 남았습니다.
틈틈이 강의 노트 필기~
강의 종료까지 99분 남았습니다.

...

강의 종료까지 2분 남았습니다.
틈틈이 강의 노트 필기~
강의 종료까지 1분 남았습니다.
틈틈이 강의 노트 필기~

공부 끝!
```

- 위의 예시에는 `watchOnlineClass` 함수를 실행하여 강의 시청을 하면서 강의 시청중에 노트 필기까지 같이 하고 있다.
    - 공부 시작! 을 외치면서 `study` 함수를 시작하게 되고
    - `watchOnlineClass` 함수를 실행 시키면서 강의 시청과 동시에 노트 필기(**Non-Blocking**) 를 하면서 주기적으로 강의 시청이 끝났는지를 확인한다.
    - 강의 시청이 끝나지 않았다면 계속해서 노트 필기를 하게 되고 강의 시청이 끝나기전까지는 공부 끝!을 외칠 수 없다. (이는 곧 작업들이 순차적인 흐름을 가지고 있음을 의미한다. **Synchronous**)
    - `watchOnlineClass` 함수가 끝나고 나서야 비로서 `study` 함수는 공부 끝!을 외치고 작업을 마칠 수 있게된다.

<br />

3. **Asynchronous + Non-Blocking (비동기 + 논블로킹)**

---

- 비동기 방식이기 때문에 다른 작업이 끝났는지 끝나지 않았는지에 대해서는 신경쓰지 않고, 논블로킹 방식이기 때문에 한 작업이 수행하면서 다른 작업도 동시에 수행할 수 있다.

```jsx
function watchOnlineClass () {  
    let minute = 100;
    
    while(minute > 0){
        console.log(`강의 종료까지 ${minute}분 남았습니다.`);
        minute--;
    }
}

function study () {
    console.log('공부 시작!');
    console.log('오늘은 공부가 하기싫은데 강의는 들어야 하고,,');
    console.log('강의만이라도 틀어둘까..?');
    
    setTimeout(() => {
        watchOnlineClass();
    }, 0);
	
    console.log('몰라, 공부 끝!');
}

study();
```

```
공부 시작!
오늘은 공부가 하기싫은데 강의는 들어야 하고,,
강의만이라도 틀어둘까..?
몰라, 공부 끝!

강의 종료까지 100분 남았습니다.
강의 종료까지 99분 남았습니다.
...
강의 종료까지 2분 남았습니다.
강의 종료까지 1분 남았습니다.
```

- 위의 예시에는 `watchOnlineClass` 함수를 실행하여 강의를 틀어두기만 하고 강의가 끝나는지에 대한 여부에 대해서는 신경쓰지 않고 있다.
    - 공부 시작! 을 외치면서 `study` 함수를 시작하게 되고
    - 강의만 틀어둘까 고민하다가 `watchOnlineClass` 함수를 실행 시켜버린채 강의가 끝났는지에 대한 여부와 상관없이(**Asynchronous**) 강의만 틀어져 있는동안 몰라, 공부 끝!을 외쳐버리고(**Non-Blocking)** 공부를 마치게 된다.

<br />
- 비동기와 논블로킹 방식은 여러 개의 작업을 동시에 처리할 수 있지만 너무 복잡하게 얽힌 비동기 처리는 흐름을 읽기 어렵게 만들기 때문에 자바스크립트에서 `Promise`나 `async/await` 문법을 활용하여 흐름을 더 직관적으로 만들고자 하는 것이다.

<br />
<br />
<br />

4. **Asynchronous + Blocking (비동기 + 블로킹)**

---

- 비동기와 블로킹 방식은 흔하게 접할 수 있는 조합은 아니며  Linux와 Unix 운영체제의 I/O 다중화 모델 정도에서 사용된다고 한다.
- 아래의 이미지가 비동기 + 블로킹을 검색했을 때 가장 많이 나오는 이미지인데 중간쯤에 보면 Select 함수가 있다.
- 이 Select 함수에 대해 간단하게 설명하자면, 이 함수가 바로 작업을 블로킹하고 비동기 방식이기 때문에 여러 개의 I/O를 동시에 감시하며 처리할 수 있다.
    - 자신이 감시하고 있는 파일들에서 I/O가 발생하는지를 확인하며 Select 함수가 종료되면 감시하던 파일들 중에서 처리해야할 파일의 개수를 반환해 주면 블로킹이 풀리게 되는 방식이다.

  ![비동기,블로킹](https://user-images.githubusercontent.com/61952198/188846523-45f1e904-0018-41f4-bee2-8421ccd38109.png)

<br />

- 지금까지 **Synchronous, Asynchronous / Blocking, Non-Blocking** 에 대해서 정리해봤는데 확실히 헷갈렸던 개념이지만 이번 기회에 어느 정도 정리가 되면서 혼돈의 구멍에서 조금은 빠져 나올 수 있었던 것 같다.
- 완벽하게 차이를 가지고 이해하고 설명하기에는 어려운 개념이기에 필자의 이번 글을 통해 **Synchronous, Asynchronous / Blocking, Non-Blocking 이 바라보는 관점이 완전히 다르며 서로 다른 개념이라는 정도만이라도 이해할 수 있는 시간이였으면 좋겠다.**

<br />

### Reference

---

- [Blocking or Non-Blocking, Synchronous and Asynchronous](https://interconnection.tistory.com/141)
- [블로킹 Vs. 논블로킹, 동기 Vs. 비동기](https://velog.io/@nittre/%EB%B8%94%EB%A1%9C%ED%82%B9-Vs.-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0-Vs.-%EB%B9%84%EB%8F%99%EA%B8%B0)
- [동기(Synchronous)는 정확히 무엇을 의미하는걸까?](https://evan-moon.github.io/2019/09/19/sync-async-blocking-non-blocking/#%EB%8F%99%EA%B8%B0-%EB%B0%A9%EC%8B%9D--%EB%85%BC%EB%B8%94%EB%A1%9D%ED%82%B9-%EB%B0%A9%EC%8B%9D)
- [[10분 테코톡] 🎧 우의 Block vs Non-Block & Sync vs Async](https://www.youtube.com/watch?v=IdpkfygWIMk)
- [[10분 테코톡] 🐰 멍토의 Blocking vs Non-Blocking, Sync vs Async](https://www.youtube.com/watch?v=oEIoqGd-Sns)

