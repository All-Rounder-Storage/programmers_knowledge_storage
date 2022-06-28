# var, let, const 는 무엇일까요

## var

ES5까지 변수를 선언할 수 있는 유일한 방법은 var 키워드였다.

#### var 키워드의 특징
1. 변수 중복 선언 허용
   ```Javascript
   var x = 1;
   var y = 1;
   
   // var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
   // 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
   var x = 100;
   // 초기화문이 없는 변수 선언문은 무시한다.
   var y;
   
   console.log(x);  // 100
   console.log(y);  // 1
   ```
 
2. 함수 레벨 스코프 
   - 대부분의 프로그래밍 언어는 함수 몸체만이 아니라 모든 코드 블럭 (if, for, while, try/catch 등)이 지역 스코프를 만든다. ➡️ **블록 레벨 스코프**
   - 하지만, var 키워드로 선언한 변수는 **함수의 코드 블록** 만을 지역 스코프로 인정한다.
   - 즉, **함수 내부** 에서 선언된 변수만이 지역 스코프를 가진다.
    
   ```Javascript
   // x, y는 전역변수이다.
   var x = 1;
   var y = 1;
   
   if(true){
    // var 키워드로 선언된 변수는 함수의 코드 블록 (함수 몸체)만을 지역 스코프로 인정한다.
    // 즉, 코드 블럭인 if 안에서 선언된 x는 독자적인 지역 스코프를 가지는 것이 아닌 전역변수로 선언된 x의 값을 덮어 씌운다.
    var x = 10;
   }
   
   // for문에서 var 키워드로 선언된 y도 독자적인 지역 스코프를 가지는 것이 아닌 전역변수로 선언된 y의 값을 덮어 씌운다.
   for(var y=0; y<5; y++){
    console.log(y); // 0 1 2 3 4
   }
   
   console.log(x); // 10
   console.log(y); // 5
    ```
   
3. 변수 호이스팅
    - var 키워드로 변수를 선언하면 **변수 호이스팅** 에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.   
      💡 변수 호이스팅이란?
        - 자바 스크립트 엔진은 런타임 이전에 소스코드의 평가 과정을 거치면서 소스코드를 실행하기 위한 준비를 한다.
        - 이때 변수 선언이 이뤄진다. ➡️ 즉, 변수 선언이 소스코드의 어디에 있든 상관없이 다른 코드보다 먼저 실행한다.
        - 변수 선언뿐 아니라 *var, let, const, function, function**, *class* 키워드를 사용해서 선언하는 모든 식별자 (변수, 함수, 클래스등)는 호이스팅 된다.
        - 하지만, ES6에 도입된 let과 const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.
         
          ```javascript
            // 변수 선언문인 var x; 보다 변수를 참조하는 코드인 console.log(x);가 앞에 있지만,
            // console.log(x)가 실행되는 시점에는 이미 x = undefined로 선언 후 초기화돼있다.
            // 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 변수 호이스팅에 의해 Reference에러가 발생하지 않는다.
            console.log(x); // undefined
            var x; //변수 선언문
          ```
    - 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.
    
   ```Javascript
   // var로 선언된 변수 호이스팅에 의해 이미 foo 변수가 선언되었다.
   // var 키워는 선언과 동시에 초기화가 일어난다
   // 실제 선언문이전에는 undefined로 초기화된다.
   console.log(x); // undefined
   
   // 변수에 값을 할당
   x = 1;
   console.log(x); // 1
   
   // 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
   var x;
   ```

## let

ES6에서는 새로운 변수 선언 키워드인 let 과 const 를 도입하였다.

#### let 키워드의 특징
1. 변수 중복 선언 금지
   ```Javascript
   let x = 1;
   
   // let이나 const로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
   // SyntaxError
   let x = 2;
   ```

2. 블록 레벨 스코프 

   - 모든 코드 블럭 (if, for, while, try/catch 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
   ```javascript
   let x = 1; // 전역 변수
   
   {
   // let 키워드로 선언된 변수는 블록 레벨 스코프를 따른다.
   // 즉, 전역에서 선언된 x(1)과 코드 블록 내에서 선언된 x(2) 변수는 다른 별개의 변수다.
   // 또한, y 또한 블록 레벨 스코프를 가지는 지역 변수이다. 따라서 전역에서는 y변수를 참조할 수 없다.
    let x = 2; // 지역 변수
    let y = 3; // 지역 변수
    var z = 4; // 전역 변수
   }
   
   console.log(x); // 1
   console.log(y); // ReferenceError
   console.log(z); // 4
   ```
   
3. 변수 호이스팅
   - let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
     - 하지만 여전히 호이스팅은 일어나고 있기 때문에 Reference Error가 발생한다.
     ```Javascript
     let x = 1; // 전역 변수
     
     {
        console.log(x) // ReferenceError
        let x = 2; // 지역변수
     }
     ```
   - 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 "선언 단계"와 "초기화 단계"기 한번에 진행된 var 키워드와는 달리,
   - *let 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행된다.*
   > 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만, 초기화 단계는 변수 선언문에 도달했을 때 실행된다.
   > let 키워드로 선언한 변수는 스코프의 시작 시점부터 초기화 단계 시작 지점 (변수 선언문)까지 변수를 참조할 수 없다. ➡️ **일시적 사각지대**
   
   ![IMG_B8FB02E77DDB-1](https://user-images.githubusercontent.com/61952198/175774246-d13bfa05-301a-4156-996e-c7e1fd31dd7e.jpeg)
   
   ```Javascript
   console.log(x); // ReferenceError: x is not defined
   let x;
   ```
   
4. 전역 객체(window)와 let
   - var 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 된다.
   - 전역 객체의 프로퍼티를 참조할 때는 window 를 생략하고도 가능하다.
   - 하지만 let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
   
   ```javascript
   var x = 1; // 전역 변수
   y = 2; // 암묵적 전역
   function outer(){} // 전역 함수
   
   let z = 3;
   
   console.log(x) // 1
   console.log(window.x) // 1
   
   console.log(y) // 2
   console.log(window.y) // 2
   
   console.log(outer) // f outer(){}
   console.log(window.outer) // f outer(){}
   
   console.log(z) // 3
   console.log(window.z) // undefined
   ```

## const

ES6에서는 새로운 변수 선언 키워드인 let 과 const 를 도입하였다.

#### const 키워드의 특징

1. 선언과 초기화
   - const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.
   ```javascript
   const x = 1; // good
   const y; // SyntaxError
   ```
   
2. let과 동일하게 블록 레벨 스코프를 가진다.

3. let과 동일하게 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

4. 재할당 금지
   ```javascript
   const x = 1;
   x = 2; // TypeError
   ```
