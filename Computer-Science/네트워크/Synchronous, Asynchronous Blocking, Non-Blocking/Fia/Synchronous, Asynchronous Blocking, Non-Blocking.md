# Synchronous, Asynchronous / Blocking, Non-Blocking

## 이게 뭔가요 ?
- 네트워크 및 I/O 에서 사용하는 개념이다.
- Synchronous, Asynchronous 작업의 실행 순서 보장에 관한 개념
- Blocking, Non-Blocking 작업 진행 시 프로세스의 유휴 상태에 대한 개념
- 서로 관련이 있는 개념은 아니나 두가지를 조합하여서 쓰는 개념이 많아 같이 설명할 뿐, 혼동하지 않아야 한다!

## Synchronous / Asynchronous
> - 요청에 대한 결과를 받을 때 사용되는 개념이며, 결과물을 받는 시점에 차이가 있다.
> - 작업의 수행 순서 보장에 관한 메커니즘

`Synchronous` **동기 방식**

- 작업이 **순차적인 흐름을 가지고 있음**을 의미한다.
- 동기식 전송으로 **한번의 요청에 대한 응답이 올 때까지 기다린 후** 결과물을 가져온다.

![sync](https://user-images.githubusercontent.com/49216939/185898464-faf0968c-45c0-4007-8569-f3136a8eb75e.png)

`Asynchronous` 비동기 방식

- 실행한 작업이 나중에 완료가 되면 그때 결과물을 가져온다.
- 주로 Callback 함수를 통해서 결과물을 가져오게 된다.

![async](https://user-images.githubusercontent.com/49216939/185898592-fa65132f-61ec-4f53-a3d6-671cdd1ffbe1.png)

## Blocking / Non-Blocking
> - 프로그램을 실행하는 순서의 관점에서 보아야 하는 개념이다.
> - 하나의 작업을 실행 한 후에 발생하는 일이 달라진다.

`Blocking`

- 시작한 작업이 끝날 때까지 다른 작업은 대기해야하고, 시작한 작업이 완료되면 그때 결과를 return 합니다.
- 작업이 **wait Queue** 에 들어간다.
- 먼저 시작한 작업 도중 다른 작업이 실행될 경우, 먼저 시작한 작업은 하던 일을 멈추고 다른 작업에게 제어권을 넘기고 그 작업이 완료 될 때까지 기다려야 합니다.
- 완료 후 제어권은 다시 넘어오며, 그때 멈췄던 작업을 개제 합니다.
    - **제어권을 가지고 있는지 없는지 여부에 따라 작업 진행 여부가 결정 됩니다.**

- ex. scanf 함수는 사용자가 콘솔에 입력할 때까지 대기하고, 입력하면 동작한다.

![blocking](https://user-images.githubusercontent.com/49216939/185898137-851a6fdc-1ba1-4220-9c3f-14d567378d1f.png)

`Non-Blocking`

- 작업이 완료 되지 않아도 대기하지 않고 return 합니다.
- 작업이 **wait Queue** 에 들어가지 않는다.
- 먼저 시작한 작업 도중 다른 작업이 실행될 경우, 처음 실행한 작업이 제어권을 다른 작업으로 넘기지 않고 가지고 있으며, 
  다른 작업은 실행을 시켜둔 후 자신의 남은 작업을 실행합니다.

![non-blocking](https://user-images.githubusercontent.com/49216939/185898296-316e12b5-317c-4bfb-b610-c54c694a0b86.png)

## 이제 2가지를 조합해서 사용하는 방식을 알아보자 

`제일 흔하게 접하는 방식 2가지`

#### Synchronous + Blocking (동기 + 블럭킹)
- 가장 흔하게 접하는 방식으로 하나의 작업이 실행된 후, 다음 작업이 실행된다.

![sync-blocking](https://user-images.githubusercontent.com/49216939/185865851-a813a4f4-73e6-46b1-b4c7-3cbd68418d1f.png)

```kotlin
fun makeSteak(){
    seasoningToMeat()
    heatTheFan()
    cookingMeat()
}

fun seasoningToMeat(){
    print("소금 후추 챱챱")
    print("올리브오일 쓱쓱")
}

fun heatTheFan(){
    for(temperature in 1..71){
        print("온도가 올라 갑니다...현재 $temperature 도 입니다.")
    }
}

fun cookingMeat(){
    print("고기를 올립니다.")
    print("치이익-")
}
```

- makeSteak() 호출 후 결과는 ?
> - "소금 후추 챱챱"
> - "올리브 오일 쓱쓱"
> - "온도가 올라 갑니다...현재 1 도 입니다."
> - "온도가 올라 갑니다...현재 2 도 입니다."
> -  ". . ."
> - "온도가 올라 갑니다...현재 70 도 입니다."
> - "고기를 올립니다"
> - "치이익-"

`Key Point`

- `먼저 실행된 직업의 결과 와 다음 작업의 시작 이 동일`해지는 방식이다. 
- `응답이 오기 까지 대기하고(Synchronous) 해당 작업이 끝난 후 다음 작업(Blocking)` 을 시작한다.


#### Asynchronous + Non-Blocking (비동기 + 논블럭킹)
- 흔히 아는 `비동기 프로그래밍 방식`이다.
- 먼저 실행된 작업의 응답을 기다리지 않고 다른 동작을 수행할 수 있다. 

![async-nonBlocking](https://user-images.githubusercontent.com/49216939/185865988-b61727b7-1b13-4371-8bcc-9d9cbcc6189c.png)

```kotlin
fun sendOrderToKitchen(menu : Menu, showMessage : (String) -> Unit){
    GlobalScope.launch {
        val order = this.async {
            delay(2000)
            makeFood(menu.foodName)   
        }
        val cookingComplete = order.await()
        showMessage(cookingComplete.foodName)
    }
}

fun serveOrder(){
    val menu = Menu(name = "떡볶이")
    print("주문 수령 !")
    sendOrderToKitchen(menu) { foodName ->
        print("$foodName 조리 완료 !")
    }
    print("테이블 정리 !")
}
```
- serveOrder() 호출 후 결과는 ?
> - "주문 수령 !"
> - "테이블 정리 !"
> - "떡볶이 조리 완료 !"

- 같은 맥락이지만 좀 더 친숙한 js 예제

```jsx
function employee (maxDollCount = 1, callback) {
  let dollCount = 0;
  const interval = setInterval(() => {
    if (dollCount > maxDollCount) {
      callback();
      clearInterval(interval);
    }
    dollCount++;
    console.log(`직원: 인형 눈알 붙히기 ${dollCount}번 수행`);
  }, 10);
}

function boss () {
  console.log('사장: 출근');
  employee(100, () => console.log('직원: 눈알 결산 보고'));
  console.log('사장: 퇴근');
}

boss();
```
- boss() 호출 후 결과는 ?
> - "사장: 출근"
> - "사장: 퇴근"
> - "직원 인형 눈알 붙이기 1번 수행"
> - "직원 인형 눈알 붙이기 2번 수행"
> - ". . ."
> - "직원 인형 눈알 붙이기 100번 수행"
> - "직원 눈알 결산 보고"


`흔하지 않은 2가지 방식`

#### Synchronous + Non-Blocking (동기 + 논블럭킹)
- 이 방식은 2가지 작업이 실행될 때 먼저 시작한 작업이 다음 작업에게 제어권을 넘겨주지 않고, 다음 작업의 실행에 대한 결과 값만 꾸준히 확인한다.

![sync-nonBlocking](https://user-images.githubusercontent.com/49216939/185866126-ff225162-bcae-4913-ba47-56ea84d0902b.png)

> - 엄마가 요리 하는 중에 마늘을 빻아달라고 요청했다.
> - 엄마 : "다했니?"
> - 나 : "빻는 중.."
> - 엄마 : "다했니?"
> - 나 : "빻는 중.."
> - 엄마 : "다했니?"
> - 나 : "응 여기.."
> - 엄마가 하는 요리 : 제어권을 가진 작업
> - 내가 마늘을 빻는 일 : 제어권을 가지지 못한채 실행된 작업

#### Asynchronous + Blocking (비동기 + 블럭킹)
- 이 방식은 end 단 개발 부분에선 잘 사용되지 않고, 리눅스나 유닉스 같은 OS 에서 I/O 다중처리 같은 작업에서 사용되고 있다.
- 코드의 흐름은 유지하면서 비동기로 여러 I/O 를 처리하기 위한 방식이다.

![async-blocking](https://user-images.githubusercontent.com/49216939/185866295-3a20f140-6b06-485e-93a4-6d89a7669589.png)


> - 손님은 키오스크를 통해 4번의 주문을 하였다.
> - 키오스크는 4가지 주문을 주방장에게 넘겨준다.
> - 매니저는 4가지가 다 나오는지를 모니터링하고 4가지 모두 준비가 되면 손님에게 서빙 한다.
> - 키오스크 : 결과와 상관 없이 주문을 받아서 넘김 (Asynchronous)
> - 매니저 : 결과가 모두 나오길 기다린 후 다음 동작을 수행(Blocking)


## 요약하자면

#### Synchronous + Blocking (동기 + 블럭킹)
- 작업 1의 결과 반환과 다음 작업인 작업 2의 시작 시점이 같음

#### Asynchronous + Non-Blocking (비동기 + 논블럭킹)
- 작업 1의 결과 반환과 상관 없이 할일을 하다 결과가 반환되면 그에 맞는 일을 처리함

#### Synchronous + Non-Blocking (동기 + 논블럭킹)
- 작업 1 도중 작업 2를 실행했지만 제어권을 뺏기지 않고 작업 2의 결과 반환을 수시로 체크하며 할 일을 처리함 

#### Asynchronous + Blocking (비동기 + 블럭킹)
- 필요한 작업의 모든 결과 반환을 기다렸다가 반환되면 다음 동작을 수행함 


## reference
- [document](https://developer.ibm.com/articles/l-async/)
  
- [post + 제어권 이미지 출처](https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC)
- [post + jsx 예제 코드 참고](https://evan-moon.github.io/2019/09/19/sync-async-blocking-non-blocking/)
- [post](https://velog.io/@nittre/%EB%B8%94%EB%A1%9C%ED%82%B9-Vs.-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0-Vs.-%EB%B9%84%EB%8F%99%EA%B8%B0)
- [post](https://incheol-jung.gitbook.io/docs/q-and-a/java/or-or-or)
  