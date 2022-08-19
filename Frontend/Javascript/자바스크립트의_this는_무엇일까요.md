# 자바스크립트의 this는 무엇일까요

```
자바스크립트에서 this는 다른 자바나 C++ 같은 클래스 기반 객체지향 언어와는 달리
**함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다.**
```

- 그렇다면 도대체 `this`는 무엇이고 `this 바인딩` 이며 `동적으로 결정`된다는게 어떤 말일까?


- 다른 언어를 사용해 봤다면 매번 가리키는 값이 달라지는 자바스크립트의 this가 천덕꾸러기처럼 느껴질 수 있다. (필자는 그렇게 느꼈다,,)
- 그래서 this에 대해 정리해보고 차근차근 배워보려 한다.

<br />

## this

- [자바스크립트의 객체는 무엇일까요](https://github.com/dbwjd5864/programmers_knowledge_storage/blob/feature/jade-week_9_frontend/Frontend/Javascript/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98_%EA%B0%9D%EC%B2%B4%EB%8A%94_%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C%EC%9A%94.md) 에서 객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드로 구성돼있다고 했다.
- 여기서 동작을 나타내는 메서드 (아래 예시의 `greeting`) 는 자신이 속해있는 객체의 상태 (`name`) 를 참조할 수 있어야 하는데 객체의 상태(프로퍼티)를 참조한다는건 우선적으로 객체를 가리키는 식별자 (`person` )를 참조해야 한다는 말이기도 하다.
- 그래야 객체안의 상태를 가리키는 프로퍼티에 접근가능 하기 때문이다.
- 이때, `객체 리터럴로 생성`한 객체의 경우 메서드 안에서 자신이 속한 객체를 가리키는 `식별자를 재귀적으로 참조할 수 있다!`

    ```jsx
    const person = {
        name: '유정',
        greeting(){ 
            return `안녕 나는 ${person.name}이라고해`;  
        }
    }
    ```

    - 이런 재귀적인 참조는 어떻게 가능한 것일까?
    - 위 예제의 객체 리터럴은 person 변수에 할당되기 이전에 평가된다.
    - 즉, greeting 메서드가 호출되는 시점에는 이미 객체가 생성됐고 person 식별자에 객체가 할당이 되있는 것이다.
    - 그렇기 때문에 greeting 메서드 안에서 person 식별자를 참조할 수 있는 것이다.

❗️ 하지만 이런 재귀적인 참조 방식은 일반적이지도 않고 권장 되지도 않는다. 이런 재귀적인 참조는 생성자 함수 방식으로 생성된 인스턴스에서는 `사용 불가` 하다.

```jsx
// 생성자 함수
function Person(name){
    ???.name = name;
}

// 생성자 함수로 인스턴스 생성
const person = new Person('유정');
```

- 생성자 함수로 인스턴스를 생성 `const person = new Person('유정');` 하려면 먼저 생성자 함수를 정의해야 한다.
- 생성자 함수를 정의하는 `function Person(name){...}` 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
- 이때 자바스크립트가 제공하는 `this` 라는 `특수한 식별자`를 사용한다.

```
1. this는 자신이 속한 객체 또는 자신이 생성할 인스턴트를 가리키는 자기 참조 변수(self-referencing variable)다.
2. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
3. this는 코드 어디서든 참조 가능하여 전역에서도 함수 내부에서도 참조할 수 있다.
4. **this가 가리키는 값, 즉 this 바인딩(식별자와 값을 연결시키는 과정)은 함수 호출 방식에 의해 동적으로 결정된다.**
```

<br />

## 함수 호출 방식과 this 바인딩

```
this의 가장 큰 특징 중에 하나는 **this 바인딩(식별자와 값을 연결시키는 과정)이 함수 호출 방식에 의해 동적으로 결정된다는 것이다.**
```

- 이때 알아야 할점은 자바스크립트에서는 동일한 함수도 다양한 방식으로 호출할 수 있다는 것이다.
- 함수를 호출하는 방식은 이와 같은데 아래 예시의 example 함수가 호출되는 방식을 보면서 이해해보자!
    1. 일반 함수 호출
    2. 메서드 호출
    3. 생성자 함수 호출
    4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

    ```jsx
    // example 이라는 함수 하나가 있다고 해보자
    const example = function(){
        // 매개변수로 전달된 객체의 속성을 출력한다
        console.dir(this);
    }
    
    // **동일한 함수도 다양한 방식으로 호출할 수 있다!**
    
    // 1. 일반 함수 호출
    // example 함수를 일반적인 방법으로 호출
    // example 함수 내부의 **this는 전역 객체 window**를 가리킨다.
    example(); // window
    
    // 2. 메서드 호출
    // example 함수를 obj의 프로퍼티 값으로 할당하여 호출
    // example 함수 내부의 **this는 메서드를 호출한 객체 obj**를 가리킨다.
    const obj = { example };
    obj.example(); // obj
    
    // 3. 생성자 함수 호출
    // example 함수를 new 연산자와 함께 생성자 함수로 호출
    // 일반적으로 생성자 함수는 대문자로 시작하여 생성자 함수로 구분해주는게 암묵적인 규칙이지만 
    // 함수에 new를 붙여서 생성자 함수처럼 사용하는 것이 불가능한 것은 아니다.
    // example 함수 내부의 **this는 생성자 함수가 생성한 인스턴스**를 가리킨다.
    new example(); // example {}
    
    // 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
    // example 함수 내부의 **this는 인수에 의해 결정**된다.
    const person = {name: 'yujeong'};
    
    example.call(person); // person
    example.apply(person); // person
    example.bind(person)(); // person
    ```

<br />

💡 이제 각각의 호출 방식에 대해 조금 더 자세히 알아보자

<br />

### 1. 일반 함수 호출

---

```
기본적으로 **this**에는 **전역 객체가 바인딩**된다.
```

- 전역 함수는 물론 중첩 함수 또한 **일반 함수로 호출**하면 **함수 내부의 this에는 전역 객체가 바인딩**된다.

    ```jsx
    function outer(){
        console.log('outer 함수의 this: ', this); // window
        function inner(){
            console.log('inner 함수의 this: ', this); // window
        }   
        inner();
    }
    outer();
    ```

- 사실상 this는 자기 참조 변수이기 때문에 일반 함수에서는 의미가 없다라고 보면 된다.
- 그렇기 때문에 ‘use strict’을 사용한 함수 내부에서는 this에 `undefined`가 바인딩된다.

    ```jsx
    function outer(){
        'use strict';
            
        console.log('outer 함수의 this: '. this); // undefined
        function inner(){
            console.log('inner 함수의 this: ', this); // undefine
        }   
        inner();
    }
    outer();
    ```

- 메서드 내에서 정의한 중첩 함수 또한 일반 함수로 호출된다면 this에는 전역 객체가 바인딩된다.

    ```jsx
    // var 키워드로 선언한 전역 변수 name은 전역 객체의 프로퍼티이다.
    var name = 'Jade';
    
    const person = {
        name: '유정',
        showName(){
            console.log("showName's this: ", this); // {name: '유정', showName: f}
            console.log("showName's this.name: ", this.name); // 유정
    		
            // 메서드 내에서 정의한 중첩 함수
            function innerShowName(){
                console.log("innerShowName's this: ", this); // window
                console.log("innerShowName's this.name: ", this.name); // Jade
            }
            
            // 메서드 내의 중첩 함수도 일반 함수로 호출하면 this에는 전역 객체가 바인딩된다.
            innerShowName();    
        }
    };
    
    person.showName();
    ```

- 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다.

    ```jsx
    var name = 'Jade';
    
    const person = {
        name: '유정',
        showName(){
            console.log("showName's this: ", this); // {name: '유정', showName: f}
            
            setTimeout(function(){
                console.log("callback's this: ", this); // window
                console.log("callback's this.name: ", this.name); // Jade
            }, 100);
        }
    };
    
    person.showName();
    ```

- 즉! 어떤 함수라도 일반 함수로 호출되면 this에는 전역 객체가 바인딩된다.
- 하지만 중첩 함수 또는 콜백 함수는 외부 함수를 돕는 헬퍼의 역할을 하고자 하는 경우가 대부분이기 때문에 중첩 함수 또는 콜백 함수의 this와 외부 함수의 this가 일치하지 않는 것은 헬퍼의 역할을 수행해내기 힘들게 된다.
    - 따라서 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 외부 함수의 this 바인딩과 일치시키는 방법이 존재한다!   
    <br />
    
    1. **this 바인딩을 that에다가 할당하기**

    ```jsx
    var name = 'Jade';
    
    const person = {
        name: '유정',
        showName(){
            // 외부에서 this 바인딩을 that에다가 할당한다.
            const that = this;
            
            setTimeout(function(){
                console.log(that.name); // 유정
            }, 100);
        }
    };
    
    person.showName();
    ```

    2. **Function.prototype.apply/call/bind 메서드를 활용하여 this를 명시적으로 바인딩하기** (이와 관련 돼서는 아래에서 더 자세하게 살펴볼 것이다!)

    ```jsx
    var name = 'Jade';
    
    const person = {
        name: '유정',
        showName(){
            // 콜백 함수에 명시적으로 this를 바인딩한다.
            setTimeout(function(){
                console.log(that.name); // 유정
            }.bind(this), 100);
        }   
    };
    
    person.showName();
    ```

    3. **화살표 함수를 사용해서 this 바인딩하기**

    ```jsx
    var name = 'Jade';
    
    const person = {
        name: '유정',
        showName(){
            // **화살표 함수 내부의 this**는 **상위 스코프의 this**를 가리킨다.
            setTimeout(() => {
                console.log(this.name); // 유정
            }, 100);
        }
    };
    
    person.showName();
    ```

<br />

### 2. 메서드 호출

---

```
**메서드 내부의 this에는 메서드를 호출한 객체**, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 객체가 바인딩된다.
```

- **메서드 내부의 this**는 메서드를 소유한 객체가 아닌 **메서드를 호출한 객체에 바인딩**된다는 것을 주의해야한다!
    - 이는 메서드가 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 함수 객체이기 때문이다.
    - 즉, 아래의 예시에서 getName이 person 객체에 포함된 것이 아니라 getName 프로퍼티가 `별도의 함수 객체를 가리키고 있는 것`이다.

        ```jsx
        const person = {
            name: '유정',
            getName(){
                // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
                return this.name;
            }
        }
        
        // 메서드 getName을 호출한 객체는 person이다.
        console.log(person.getName()); // 유정
        ```

    - 그렇기 때문에 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 함수로 호출될 수도 있다.

        ```jsx
        const friend = {
            name: 'Jade'
        }
        
        // getName 메서드를 friend 객체의 메서드로 할당
        friend.getName = person.getName;
        
        // 메서드 getName을 호출한 객체는 friend 이다.
        console.log(friend.getName()); // Jade
        
        /* --------------------------------------- */
        
        // getName 메서드를 변수에 할당
        const nameVariable = person.getName;
        
        // getName 메서드를 일반함수로 호출
        console.log(nameVariable()); // ''
        ```

    - 일반 함수로 호출된 **getName 함수 내부의 this.name**은 브라우저 환경에서 **window.name**과 같다.
        - 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본 값은 ‘’이다.
        - Node.js 환경에서 this.name은 undefined이다.

<br />

### 3. 생성자 함수 호출

---

```
생성자 함수 내부의 this에는 **생성자 함수가 후에 생성할 인스턴스가 바인딩**된다.
```

- [자바스크립의 객체는 무엇일까요의 생성자 함수](https://github.com/dbwjd5864/programmers_knowledge_storage/blob/feature/jade-week_9_frontend/Frontend/Javascript/자바스크립트의_객체는_무엇일까요.md#생성자-함수)에서 알아본 것처럼 생성자 함수는 말 그대로 객체(인스턴스)를 생성하는 함수이다.
- new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작하는 것을 명심해야 한다!

    ```jsx
    // 생성자 함수
    function Person(name, gender) {
        // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다
        this.name = name;           // public
        this.gender = gender;       // public
        this.sayHello = function(){ // public
            console.log('Hi! My name is ' + this.name);
        };
    }
    
    // 인스턴스의 생성
    const yujeong = new Person('Yujeong', 'female');
    
    console.log(yujeong.sayHello()); // Hi! Ma name is Yujeong
    
    /* --------------------------------------- */
    
    // new 연산자와 함께 쓰지 않는 경우 (일반함수로 동작)
    const friend = Person('Summer', 'female');
    
    // 일반 함수로 호출된 friend에는 반환문이 따로 없기 때문에 undefined를 반환한다.
    console.log(friend); // undefined
    
    // 일반 함수로 호출된 내부의 this는 전역 객체를 가리킨다!
    // 따라서 전역객체에 name이 할당된 것을 볼 수 있다.
    console.log(name); // Summer
    ```

<br />

### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

---

```
apply, call, bind 메서드는 Function.prototype의 메서드로서 모든 함수가 사용할 수 있다.
```

- Function.prototype.apply와  Function.prototype.call 메서드는 **this로 사용할 객체와 인수 리스트를 인수로 전달받는다.**
    - apply와 call 메서드의 **본질적인 기능은 함수를 호출**하는 것이다.
    - 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.
    - **apply와 call 메서드는 호출한 함수에 인수를 전달하는 방법만 다르다.**
        - apply: 배열로 전달
        - call: 쉼표로 구분한 리스트 형식으로 전달

    ```jsx
    // this로 사용할 객체
    const thisArg = {name : '유정'}
    
    const sayHello = function (englishName){
        return `안녕 나는 ${this.name}이라고해. 내 영어이름은 ${englishName}야.`
    }
    
    // sayHello를 그냥 호출했을 때는 this에 전역 객체가 바인딩 되어 있으므로 
    // 결과 값으로 
    // 안녕 나는 이라고해. 내 영어이름은 Jade야.
    console.log(sayHello('Jade'));
    
    // 명시적으로 this에 thisArg 값을 바인딩 시켜주기 때문에
    // 결과 값으로 
    // 안녕 나는 유정이라고해. 내 영어이름은 Jade야.
    console.log(sayHello.apply(thisArg, ['Jade']));  
    console.log(sayHello.call(thisArg, ['Jade']));  
    ```

- Function.prototype.bind 메서드는 apply와 call 메서드와 달리 **함수를 호출하지 않고** **this로 사용할 객체만 전달한다.**
    - 다시말해 bind 함수는 함수가 가리키는 **this**만 바꾸고 호출하지는 않는다.

    ```jsx
    // this로 사용할 객체
    const thisArg = {name : '유정'}
    
    const sayHello = function (englishName){
        return `안녕 나는 ${this.name}이라고해. 내 영어이름은 ${englishName}야.`
    }
    
    const hello = sayHello.bind(thisArg);  
    
    // bind 메서드는 함수를 호출하지 않기 때문에 명시적으로 호출해야 한다.
    console.log(hello('Jade')); // 안녕 나는 유정이라고해. 내 영어이름은 Jade야.
    ```

    - bind 메서드는 위에서 언급 했던 것처럼 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결할 때 유용하게 사용된다.

💡 지금까지 함수 호출 방식에 따라 this 바인딩이 동적으로 결정되는 천덕꾸러기 this에 대해 알아보았다. 마지막으로 다시 정리해 보자면 이렇게 this 바인딩이 결정됨을 볼 수 있다. ( 이 글을 읽는 모두가 this와 조금 더 친해지는 시간이였기를 바란다! )

| 함수 호출 방식 | this 바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 후에 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출  | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달된 객체 |

<br />

### Reference

---

- [함수 호출 방식에 의해 결정되는 this](https://poiemaweb.com/js-this)

