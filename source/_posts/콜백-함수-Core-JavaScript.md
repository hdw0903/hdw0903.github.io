---
title: 콜백 함수 -Core JavaScript
disqusId: tunas-blog-1
tags:
  - Core JavaScript
  - JavaScript
  - 코어 자바스크립트
  - 콜백 함수
  - callback function
  - this
  - param
  - bind
  - 콜백 지옥
  - 비동기 제어
  - 제어권
  - setInterval
  - clearInterval
  - 고유 ID
  - asynchronous
  - synchronous
date: 2020-04-30 20:03:50
categories: Core JavaScript
toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right
  - type: adsense
    position: right
sidebar:
  right:
    sticky: true
---

* 콜백 함수
  * [콜백 함수란?](/2020/04/30/콜백-함수-Core-JavaScript/#callback)
  * [제어권](/2020/04/30/콜백-함수-Core-JavaScript/#callback_제어권)
    * 인자
    * this
  * [콜백 함수는 함수다.](/2020/04/30/콜백-함수-Core-JavaScript/#callback_function)
  * [콜백 함수 내부의 this에 다른 값 바인딩하기](/2020/04/30/콜백-함수-Core-JavaScript/#callback_this_binding)
  * [콜백 지옥과 비동기 제어](/2020/04/30/콜백-함수-Core-JavaScript/#callback_hell)

<!-- more -->

------
<h2 id="callback">콜백 함수란?</h2>

콜백 함수(`callback function`)는 다른 코드의 인자로 넘겨주는 함수 입니다.

`callback`은 부르다, 호출하다, 실행하다의 의미인 `call` 과 되돌아오다 `back`의 합성어로,
되돌아 호출하다라는 의미로 이해할 수 있습니다.

특정 `함수a`를 호출하면서 '특정 조건일때 `함수b`를 실행해서 알려달라는'요청을 보내는 것입니다.
`함수a`의 입장에서는 해당 조건이 갖춰졌는지 여부를 스스로 판단하고 `함수b`를 직접 호출합니다.

이처럼 콜백 함수는 다른 코드(함수 또는 메서드)에게 인자를 넘겨줌으로써 그 <mark>제어권도 함께 위임한 함수</mark>입니다. (**콜백 함수를 위임받은 코드는 자체적인 내부 로직에 의해 이 콜백 함수를 적절한 시점에 실행합니다**.)

------
<h2 id="callback_제어권">제어권</h2>

몇 가지 예제

------
### 호출 시점

```js setInterval
1. var count = 0;
2. var timer = setInterval(function() {
  console.log(count);
  if (++count > 4) clearInterval(timer);
}, 300);
```

우선 `setInterval` 메서드의 형태를 살펴보면 다음과 같습니다.
> var 참조변수(interval ID) = scope.setInterval(func, delay[, param1, param2, ...]);

* scope :
  `Window` 객체 또는 `Worker`의 인스턴스가 들어올 수 있습니다. 두 객체 모두 `setInterval` 메서드를 제공하기 때문입니다. 일반적인 브라우저 환경에서는 `window`를 생략하고 함수처럼 사용할 수 있습니다.


* 매개변수
  `func`, `delay` 값을 반드시 전달해야 합니다.
  세 번째 매개변수 부터는 선택적 파라미터로 `func` 함수를 실행할때 전달할 파라미터입니다.
  `func`에 넘겨준 함수는 매 `delay(ms)`마다 실행되며, 그 결과로 어떤 값도 반환하지 않습니다.


* `setInterval`을 실행하면 반복적으로 실행되는 내용 자체를 특정할 수 있는 고유 ID 값이 반환됩니다.
이를 변수에 담는 이유는 반복 실행되는 중간에 종료(`clearInterval`)할 수 있게 하기 위해서 입니다.

1. `count` 변수를 선언하고 0을 할당합니다.

2. `timer` 변수를 선언하고 setInterval 결과를 할당했습니다.

**위에 코드를 콜백 함수를 확인하기 쉽게 수정했습니다.**

```js callback - setInterval
var count = 0;
var cbFunc = function() {
  console.log(count);
  if (++count > 4) clearInterval(timer);
};
var timer = setInterval(cbFunc, 300);

// -- 실행 결과 --
// 0  (0.3초)
// 1  (0.6초)
// 2  (0.9초)
// 3  (1.2초)
// 4  (1.5초)
```

*  `timer`변수에는 `setInterval`의 ID 값이 담기게 됩니다.


* `setInterval`에 전달한 첫 번째 인자인 `cbFunc`함수(이 함수가 곧 콜백함수입니다.)는 0.3초마다 자동으로 실행될 것입니다. 


* 콜백 함수 내부에서는 `count` 값을 출력하고 1씩 증가시키며, `count`값이 4보다 크면 반복 실행이 종료됩니다.


* 제어권
`setInterval` 메서드에 첫 번째 인자로 `cbFunc` 함수를 넘겨주자 제어권을 넘겨받은 `setInterval` 메서드는 0.3초마다 (지정된 시점) 이 익명 함수를 실행했습니다. 이처럼 <u>콜백 함수의 제어권을 넘겨받은 코드</u>는 <mark>콜백 함수 호출 시점에 대한 제어권</mark>을 가집니다.

### 인자 (파라미터)

```js
var newArr = [10, 20, 30].map(function(currentValue, index) {
  console.log(currentValue, index);
  return currentValue + 5;
});
console.log(newArr);

// -- 실행 결과 --
// 10 0
// 20 1
// 30 2
// [15, 25, 35]
```
1. `newArr` 변수를 선언하고 오른쪽에 배열 `[10, 20, 30]`에 `map` 메서드를 호출하고 그 결과를 할당합니다.

  `map`메서드의 동작 방식부터 살펴보도록 하겠습니다.
  > Array.prototype.map(callback[, thisArg])
callback : function(currentValue, index, array)

  `map` 메서드는 Array.prototype에 담긴 메서드입니다.

  * 파라미터
    * 첫 번째 인자: `callback` 함수를 받습니다
    * 두 번째 인자: 생략가능한 파라미터이며, 콜백 함수 내부에서 `this`로 인식할 대상을 지정합니다. 생략시 일반적인 함수와 마찬가지로 전역객체를 참조하게 됩니다.

  * 반환 값
    * `map`메서드는 메서드의 대상이 되는 배열의 모든 요소들을 차례대로 불러와 콜백 함수를 반복 호출하고, <u>콜백 함수의 실행 결과를 모아 새로운 배열</u>을 반환합니다.

  * `callback`함수
  콜백 함수의 첫 번째 인자에는 배열의 요소중 `현재값`, 두 번째 인자에는 `현재값의 index`, 세 번째 인자에는 `map`메서드의 `대상이 되는 배열`이 담깁니다.


2. `map` 메서드는 배열`[10, 20, 30]`의 각 요소를 차례대로 꺼내어 콜백 함수를 실행합니다.


3. 첫 번째 요소에 대한 콜백 함수는 `currentValue`에 10이, `index`에는 index 0이 담긴 채 실행됩니다 `return currentValue + 5;` 코드를 실행하여 `(10 + 5)` 값인 `15`가 반환됩니다.


4. 두 번째 요소에 대한 콜백 함수는 `currentValue`에 20이, `index`에는 index 1이 담긴 채 실행됩니다 `return currentValue + 5;` 코드를 실행하여 `(20 + 5)` 값인 `25`가 반환됩니다.


5. 세 번째 요소에 대한 콜백 함수는 `currentValue`에 30이, `index`에는 index 2이 담긴 채 실행됩니다 `return currentValue + 5;` 코드를 실행하여 `(30 + 5)` 값인 `35`가 반환됩니다.


6. 모든 요소에 대한 콜백 함수를 마치고 나면 `[15, 25, 35]`라는 새로운 배열이 만들어져 `newArr`변수에 할당됩니다.


7. `console.log(newArr)` 코드에서 `newArr`변수에 할당된 새로운 배열이 출력됩니다.


**중요 포인트**

`콜백 함수`의 파라미터 순서를 바꾸면 안됩니다. `currentValue`, `index`순 이어야하며
바뀐다면 전혀 다른 값을 반환할 것입니다.

~~엔진은 파라미터로 받은 값의 단어(name)를 인식하는 것이 아니라 순서(첫 번째, 두 번째)에 의해서만 각각을 구분하고 인식합니다.~~ 

-------
### this

콜백 함수 내부에서의 `this`

```js
1. setTimeout(function() {
  console.log(this);
}, 300); // (1) Window { ... }

2. [1, 2, 3, 4, 5].forEach(function(x) {
  console.log(this); // (2) Window { ... }
});

document.body.innerHTML += '<button id="a">클릭</button>';
3. document.body.querySelector('#a').addEventListener(
  'click',
  function(e) {
    console.log(this, e); // (3) <button id="a">클릭</button>
  } // MouseEvent { isTrusted: true, ... }
);
```

1. `setTimeout`은 내부에서 콜백 함수를 호출할 때 `call`메서드의 첫 번째 인자로 전역객체를 넘기게 됩니다. 콜백 함수 내부에서 `this`가 전역객체를 가리키게됩니다. 


2. `forEach`는 '별도의 인자로 this를 받는 경우'에 해당하지만 별도로 `this`를 지정해주지 않았기 때문에 전역객체를 가리키게됩니다.


3. `addEventListener`는 내부에서 콜백 함수를 호출할 때 `call`메서드의 첫 번째 인자에 `addEventListener`메서드의 `this`를 넘기도록 정의되어 있습니다. 때문에 콜백 함수 내부에서의 `this`는 `addEventListener`를 호출한 주체 `HTML` 엘리먼트를 가리키게 됩니다. 

------
<h2 id="callback_function">콜백 함수는 함수다.</h2>

**콜백 함수로 객체의 메서드를 전달하더라도 그 메서드는 메서드가 아닌 함수로 호출됩니다.**

```js 메서드를 콜백 함수로 전달한 경우
1. var obj = {
  vals: [1, 2, 3],
  logValues: function(v, i) {
    console.log(this, v, i);
  },
};
2. obj.logValues(1, 2); // { vals: [1, 2, 3], logValues: f } 1 2
3. [4, 5, 6].forEach(obj.logValues); // Window { ... } 4 0
// Window { ... } 5 1
// Window { ... } 6 2
```

1. `obj` 객체의 `logValues`는 메서드로 정의됐습니다.


2. 메서드의 이름 앞에 점(.)이 있으니 메서드로서 호출한 것 입니다. `this`는 `obj`를 가리키고 파라미터로 넘겨준 1, 2와 함께 출력됩니다.


3. `obj.logValues`메서드를 `forEach` 함수의 콜백 함수로서 지정했습니다.
`obj`를 `this`로 하는 메서드를 그대로 전달한 것이 아니라, `obj.logValues`가 가리키는 함수만 전달한 것입니다.
이 함수는 메서드로서 호출할 때가 아닌 한 `obj`와의 직접적인 연관이 없어집니다.
`forEach`에 의해 콜백이 함수로서 호출되고, 별도로 `this`를 지정하지 않았으므로 `this`는 전역객체를 참조합니다.

**중요 포인트**

어떤 함수의 인자(파라미터)에 객체의 메서드를 전달하더라도 메서드가 아닌 함수일 뿐입니다.
이 차이를 정확히 이해하는 것이 중요합니다.

------
<h2 id="callback_this_binding">콜백 함수 내부의 this에 다른 값 바인딩 하기</h2>

콜백 함수에서 `this`를 지정하지 않으면 전역객체를 참조하게 되므로
객체의 메서드를 콜백 함수로 전달하면 해당 객체를 `this`로 참조할 수 없게됩니다.

별도의 인자(`thisArg`)로 `this`를 받는 함수의 경우에는 원하는 값을 넘겨주면 되지만 그렇지 않은 경우에는 `this`의 제어권도 넘겨주게 되므로 사용자가 임의로 값을 변경할 수 없습니다.

그래서 <makr>전통적으로는 `this`를 다른 변수에 담아 콜백 함수로 활용할 함수에서는 `this`대신 그 변수를 사용하게 하고, 이를 클로저로 만드는 방식이 많이 쓰였습니다.</mark>

```js 콜백 함수 내부의 this에 다른 값을 바인딩 - 전통적인 방법(변수 사용)
var obj1 = {
  name: 'obj1',
  func: function() {
    var self = this;
    return function() {
      console.log(self.name);
    };
  },
};
var callback = obj1.func();
setTimeout(callback, 1000);
```

1. `obj1.func` 메서드 내부에서 `self` 변수에 `this`를 담고, 익명 함수를 선언하고 반환했습니다.


2. `obj1.func`를 호출하면 앞서 선언한 내부함수가 반환되어 `callback` 변수에 할당됩니다.


3. 이 `callback`변수를 `setTimeout` 함수에 인자로 전달하면 1초(1000ms) 뒤 `callback`이 실행되면서 `"obj1"`을 반환할 것입니다.

**중요 포인트**
이 방식은 `this`를 다른 변수에 담아 함수내에서 `this` 대신 그 변수를 사용하게하여 실제로는 `this`를 사용하지 않을뿐더러 번거롭습니다.

* ES5에 등장한 `bind`메서드를 사용하면 더욱 간편히 `this`를 바인딩할 수 있습니다.
```js 콜백 함수 내부의 this에 다른 값 바인딩 - bind 메서드 사용
var obj1 = {
  name: 'obj1',
  func: function() {
    console.log(this.name);
  },
};
setTimeout(obj1.func.bind(obj1), 1000);

var obj2 = { name: 'obj2' };
setTimeout(obj1.func.bind(obj2), 1500);
```

* `bind` 메서드의 첫 번째 인자(`thisArg`)로 `this`로 지정할 값을 전달합니다.
함수가 실행될때 `this`는 `thisArg`로 전달받은 값을 참조하게 됩니다.

------
<h2 id="callback_hell">콜백 지옥과 비동기 제어</h2>

* 콜백 지옥(`callback hell`)이란?

  콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상.
  주로 이벤트 처리나 서버 통신과 같이 비동기적인 작업을 수행하기 위해 이런 형태가 자주 등장하곤 하는데, 가독성이 떨어질 뿐더러 코드를 수정하기도 힘들다.


* 비동기(`asynchronous`)란?
  
  동기(`synchronous`)의 반대말로 <mark>동기적인 코드는 현재 실행 중인 코드가 완료된 후에야 다음 코드를 실행하는 방식</mark>
  비동기적 코드는 <mark>현재 실행 중인 코드의 완료 여부와 무관하게 즉시 다음 코드로 넘어갑니다.</mark>
  

  1. 사용자의 요청에 의해 특정 시간이 경과되기 전까지 어떤 함수의 실행을 보류한다거나 -`setTimeout`

  2. 사용자의 직접적인 개입이 있을 때 어떤 함수를 실행하도록 대기한다거나 -`addEventListener`

  3. 웹브라우저 자체가 아닌 별도의 대상에 무언가를 요청하고 그에 대한 응답이 왔을 때 어떤 함수를 실행하도록 대기하는 등 -`XMLHttpRequest`

  ~~CPU의 계산에 의해 즉시 처리가 가능한 대부분의 코드는 동기적인 코드입니다.~~
  <mark>별도의 요청, 실행 대기, 보류 등과 관련된 코드는 비동기적인 코드입니다.</mark>

* 현대의 자바스크립트는 웹의 복잡도가 높아진 만큼 비동기적인 코드의 비중이 예전보다 훨씬 높아진 상황입니다. (콜백 지옥에 빠지기도 훨씬 쉬워졌습니다.)

------
**간단한 콜백 지옥 예시를 살펴봅시다.**

```js callback-hell
setTimeout(function(name) {
    var coffeeList = name;
    console.log(coffeeList);

    setTimeout(function(name) {
        coffeeList += ', ' + name;
        console.log(coffeeList);

        setTimeout(function(name) {
            coffeeList += ', ' + name;
            console.log(coffeeList);

            setTimeout(function(name) {
                coffeeList += ', ' + name;
                console.log(coffeeList);
              }, 500, '카페라떼');
          }, 500, '카페모카');
      }, 500, '아메리카노');
  }, 500, '에스프레소');
```

* 0.5 초마다 커피 목록을 수집하고 출력합니다. 각 콜백은 커피 이름을 전달하고 `coffeeList`에 추가합니다. 

* 실행에는 지장이 없는 코드입니다만, 들여쓰기 수준이 과도하게 깊어졌을 뿐더러 수정하기도 불편하고 값이 전달되는 순서가 아래에서 위로 향하고 있어 어색하게도 느껴집니다.

가독성 문제와 어색함을 동시에 해결하는 방법은 익명 콜백함수를 모두 기명함수로 전환하는 방법이 있습니다...?!

------
```js 기명함수로 변환
var coffeeList = '';

var addEspresso = function(name) {
  coffeeList = name;
  console.log(coffeeList);
  setTimeout(addAmericano, 500, '아메리카노');
};
var addAmericano = function(name) {
  coffeeList += ', ' + name;
  console.log(coffeeList);
  setTimeout(addMocha, 500, '카페모카');
};
var addMocha = function(name) {
  coffeeList += ', ' + name;
  console.log(coffeeList);
  setTimeout(addLatte, 500, '카페라떼');
};
var addLatte = function(name) {
  coffeeList += ', ' + name;
  console.log(coffeeList);
};

setTimeout(addEspresso, 500, '에스프레소');
```

* 익명함수를 기명함수로 변환하므로서 콜백 지옥을 해결한 예시입니다.

함수 선언과 함수 호출을 구분한다면 코드의 가독성을 높여 코드를 위에서 아래로 읽어내려가는데 어려움이 없습니다. 변수가 전역으로 전개되긴 했지만 즉시 실행 함수 등으로 감싸면 해결할 수 있는 문제입니다.

하지만 코드명을 일일이 따라다녀야 하므로 오히려 헷갈릴 여지도 있습니다.

* 자바스크립트는 비동기적인 작업들을 동기적으로(혹은 동기적인 것처럼 보이도록) 처리해주는 방법을 고안했고, 그 결과 `ES6`에서 `Promise`, `Generator`등이 도입됐으며, `ES8(ES2017)`에서는 `async`, `wait`가 도입됐습니다.

------
**이들을 활용한 표현법도 알아봅시다.**

```js 비동기 작업의 동기적 표현 -Promise
new Promise(function(resolve) {
  setTimeout(function() {
    var name = '에스프레소';
    console.log(name);
    resolve(name);
  }, 500);
})
  .then(function(prevName) {
    return new Promise(function(resolve) {
      setTimeout(function() {
        var name = prevName + ', 아메리카노';
        console.log(name);
        resolve(name);
      }, 500);
    });
  })
  .then(function(prevName) {
    return new Promise(function(resolve) {
      setTimeout(function() {
        var name = prevName + ', 카페모카';
        console.log(name);
        resolve(name);
      }, 500);
    });
  })
  .then(function(prevName) {
    return new Promise(function(resolve) {
      setTimeout(function() {
        var name = prevName + ', 카페라떼';
        console.log(name);
        resolve(name);
      }, 500);
    });
  });
```

`ES6`의 `Promise`를 이용한 방법입니다. `new`연산자와 함께 호출한 `Promise`의 인자로 넘겨주는 콜백 함수는 호출할 때 바로 실행되지만 내부에 `resolve` 또는 `reject` 함수를 호출하는 구문이 있을 경우 둘 중하나가 충족되어 실행되기 전까지는 `.then` 또는 `.catch` 구문으로 넘어가지 않습니다.

따라서 비동기 작업이 완료될 때 `resolve` 또는 `reject`를 호출하는 방법으로 비동기 작업의 동기적 표현이 가능합니다.

------
다음은 위에 코드의 반복적인 내용을 함수화 하여 더욱 짧게 표현한 것입니다.

```js Promise 동기적표현 함수화
var addCoffee = function(name) {
  return function(prevName) {
    return new Promise(function(resolve) {
      setTimeout(function() {
        var newName = prevName ? prevName + ', ' + name : name;
        console.log(newName);
        resolve(newName);
      }, 500);
    });
  };
};
addCoffee('에스프레소')()
  .then(addCoffee('아메리카노'))
  .then(addCoffee('카페모카'))
  .then(addCoffee('카페라떼'));
```

------
* Generator 활용

```js 비동기 작업의 동기적 표현 -Generator
var addCoffee = function(prevName, name) {
  setTimeout(function() {
    coffeeMaker.next(prevName ? prevName + ', ' + name : name);
  }, 500);
};
var coffeeGenerator = function*() {
  var espresso = yield addCoffee('', '에스프레소');
  console.log(espresso);
  var americano = yield addCoffee(espresso, '아메리카노');
  console.log(americano);
  var mocha = yield addCoffee(americano, '카페모카');
  console.log(mocha);
  var latte = yield addCoffee(mocha, '카페라떼');
  console.log(latte);
};
var coffeeMaker = coffeeGenerator();
coffeeMaker.next();
```

`ES6`의 `Generator`를 이용한 방법입니다.
`Generator` 함수를 실행하여 `Iterator`을 반환받고 `Iterator`의 메서드 `next`를 사용할 수 있습니다

`next` 메서드를 호출하면 `Generator` 함수 내부에서 첫 번째 `yield`를 만나면 함수 실행을 멈추게 되고 다시 `next`를 호출하면 멈춘 부분부터 그 다음에 등장하는 `yield`에서 함수 실행을 멈추게 됩니다.

즉, 비동기 작업이 완료되는 시점마다 `next` 메서드를 호출해준다면 `Generator` 함수 내부의 소스가 순차적으로 진행되게 합니다.

------
* Promise + async/await

```js
var addCoffee = function(name) {
  return new Promise(function(resolve) {
    setTimeout(function() {
      resolve(name);
    }, 500);
  });
};
var coffeeMaker = async function() {
  var coffeeList = '';
  var _addCoffee = async function(name) {
    coffeeList += (coffeeList ? ',' : '') + (await addCoffee(name));
  };
  await _addCoffee('에스프레소');
  console.log(coffeeList);
  await _addCoffee('아메리카노');
  console.log(coffeeList);
  await _addCoffee('카페모카');
  console.log(coffeeList);
  await _addCoffee('카페라떼');
  console.log(coffeeList);
};
coffeeMaker();
```

`ES8(ES2017)`에서 추가된 `async/await`는 비동기 작업을 수행하고자 하는 함수 앞에 `async`를 표기하고, 함수 내부에서 비동기 작업이 필요한 위치마다 `await`를 표기하는 것만으로 뒤의 내용을 `Promise`로 자동 전환하고, 해당 내용이 `resolve`된 이후에야 다음으로 진행합니다.

`Promise`의 `then`과 비슷한 효과를 얻을 수 있습니다.

------
<h2 id="callback">정리</h2>

* 콜백 함수는 다른 코드에 인자를 넘겨줌으로써 <mark>제어권도 함께 위임</mark>.


* 제어권을 넘겨받은 코드는 
  1. 콜백 함수를 호출하는 시점을 지정.

  2. 콜백 함수를 호출할 때 <mark>인자로 넘겨줄 값의 순서를 변경하지 말 것.</mark>

  3. 콜백 함수는 `this`를 지정하여 사용할 수 있는 함수가 존재, 지정하지 않을 경우 전역객체를 참조
  사용자 임의로 `this`를 변경하고 싶은 경우 `bind` 메서드 활용


* 함수에 인자로 메서드를 전달하더라도 이는 <mark>함수로서 실행됨</mark>.


* 비동기 제어를 위해 콜백 함수를 사용하다 보면 콜백 지옥에 빠지기 쉬움.
최근의 자바스크립트에서는 <mark>Promise, Generator, async/await</mark> 등 콜백 지옥을 벗어날 수 있는 방법들이 등장함.