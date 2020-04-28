---
title: this -Core JavaScript
disqusId: tunas-blog-1
tags:
  - Core JavaScript
  - JavaScript
date: 2020-04-27 21:50:04
categories: Core JavaScript
---

자바스크립트에서의 this는 어디서든 사용할 수 있습니다.
this는 상황에 따라 참조하는 대상이 달라질 수 있습니다.
함수와 객체(메서드) 구분이 느슨한 자바스크립트에서 이 둘을 구분하는 유일한 기능입니다.

* this
  * [상황에 따라 달라지는 this](/2020/04/27/this-Core-JavaScript/#this)
    * 전역 공간에서의 this
    * 메서드로서 호출할 때 메서드 내부의 this
      * 함수 vs 메서드
      * 메서드 내부에서의 this
    * 함수로서 호출할 때 그 함수 내부에서의 this
      * 함수 내부에서의 this
      * 메서드의 내부함수에서의 this
      * 메서드 내부 함수에서의 this를 우회하는 방법
      * this를 바인딩하지 않는 함수
    * 콜백 함수 호출시 그 함수 내부에서의 this
    * Class 함수 내부에서의 this 
  * [명시적으로 this를 바인딩하는 방법](/2020/04/27/this-Core-JavaScript/#this_binding)
    * call 메서드
    * apply 메서드
    * call / apply 메서드의 활용
      * 생성자 내부에서 다른 생성자를 호출
      * 여러 인수를 묶어 하나의 배열로 전달
    * bind 메서드
      * name 프로퍼티
      * 상위 컨텍스트의 this를 내부함수나 콜백 함수에 전달하기
    * 화살표 => 함수의 예외사항
    * 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this) 

<!-- more -->

------
<h2 id="this">상황에 따라 달라지는 this</h2>

* this는 기본적으로 <mark>실행 컨텍스트가 생성될 때 함께 결정됩니다.</mark>
실행 컨텍스트는 함수를 호출할 때 생성되므로, <mark>this는 함수를 호출할 때 결정된다고 할 수 있습니다.</mark>

------
### 전역 공간에서의 this

전역 공간에서 this는 전역 객체를 가리킵니다. 브라우저 환경에서 전역객체는 window이고 Node.js 환경에서는 global입니다.

  * 참고 : 전역 변수
전역 변수를 선언하면 자바스크립트 엔진은 전역객체의 프로퍼티로 할당시킴.
```js 전역객체 window의 프로퍼티
var a = 1;
console.log(a); // 1
console.log(window.a); // 1 (전역객체의 프로퍼티)
console.log(this.a); // 1 (this는 현재 전역객체 참조)
```
<mark>사실 자바스크립트의 모든 변수는 실은 특정 객체의 프로퍼티로 동작합니다.</mark>
이 특정 객체란 실행 컨텍스트의 `LexicalEnvironment`를 말합니다.
실행 컨텍스트는 변수를 수집하여 `LexicalEnvironment`의 프로퍼티로 지정합니다.
이후 변수를 호출하면 `LexicalEnvironment`를 조회하여 일치하는 프로퍼티가 있을 경우 그 값을 반환합니다.

  전역 컨텍스트의 `LexicalEnvironment`는 전역객체를 참조하므로 var a 선언/할당 이후 `window.a` 와 `this.a`가 1이 나오는 이유는 당연합니다.
<mark>a를 직접 호출했을 때도 1이 나오는 이유는 무엇일까요?</mark>
변수 a에 접근하려고 하면 스코프 체인에서 a를 검색하다가 가장 마지막에 도달하는 전역 스코프의 `LexicalEnvironment`에서 해당 프로퍼티 a를 조회하여 그 값을 반환하기 때문입니다.

* 전역 공간에서 var로 변수를 선언하는 대신 window의 프로퍼티에 직접 할당하더라도 결과적으로 똑같이 동작합니다. 하지만 `delete` 연산자를 사용하는 경우 다른 결과를 반환합니다.

  * var 변수로 선언한 경우 : delete window 형식으로 삭제 불가,
  delete 변수명 형식으로도 삭제 불가
  * window 프로퍼티에 직접 할당한 경우: delete window 형식으로 삭제 가능,
  delete 변수명 형식으로도 삭제 가능

------
### 메서드로서 호출할 때 그 메서드 내부에서의 this

#### 함수 vs 메서드

함수를 실행하는 방법 중에는 함수로 호출하는 경우와 메서드로서 호출하는 경우가 있습니다.
함수와 메서드를 구분하는 유일한 차이는 <u>독립성</u>에 있습니다.

* 함수는 그 자체로 독립적인 기능을 수행합니다.

* 메서드는 자신을 호출한 대상 객체에 관한 동작을 수행합니다.

<mark>어떤 함수를 객체의 프로퍼티에 할당한다고 해서 무조건 메서드가 되는 것이 아니라
객체의 메서드로서 호출할 경우에만 메서드로 동작하고, 그렇지 않으면 함수로 동작합니다.</mark>

```js 메서드로 호출 (점 표기법, 대괄호 표기법)
var obj = {
  method: function(x) {
    console.log(this, x);
  },
};
obj.method(1); // { method: f } 1
obj['method'](2); // { method: f } 2
```

다시 말해 점 표기법이든 대괄호 표기법이든, 어떤 함수를 호출할 때 그 함수 이름 앞에
객체가 명시돼 있는 경우 메서드로 호출한 것이고, 그렇지 않은 경우에는 함수로 호출한 것입니다.

#### 메서드 내부에서의 this

this에는 호출한 주체에 대한 정보가 담깁니다. 어떤 함수를 메서드로서 호출하는 경우 호출 주체는 바로 함수명(프로퍼티 명)앞의 객체입니다. 점 표기법의 경우 마지막 점 앞에 명시된 객체가 곧 this가 됩니다.

```js 메서드 내부에서의 this
var obj = {
  methodA: function() {
    console.log(this);
  },
  inner: {
    methodB: function() {
      console.log(this);
    },
  },
};
obj.methodA(); // { methodA: f, inner: {...} }    ( === obj)
obj['methodA'](); // { methodA: f, inner: {...} } ( === obj)

obj.inner.methodB(); // { methodB: f }            ( === obj.inner)
obj.inner['methodB'](); // { methodB: f }         ( === obj.inner)
obj['inner'].methodB(); // { methodB: f }         ( === obj.inner)
obj['inner']['methodB'](); // { methodB: f }      ( === obj.inner)
```

------
### 함수로서 호출할 때 그 함수 내부에서의 this

------
#### 함수 내부에서의 this

어떤 함수를 함수로서 호출할 경우 this가 지정되지 않습니다.(this는 호출한 주체에 대한 정보가 담깁니다.)
함수로서 호출하는 것은 호출 주체(객체지향 언어에서의 객체)를 명시하지 않고 실행한 것이기 때문에 호출 주체의 정보를 알 수 없는 것입니다. 
<mark>this가 지정되지 않은 경우 this는 전역 객체를 참조합니다.</mark>
따라서 함수에서의 this는 전역 객체를 가리킵니다.

------
#### 메서드의 내부함수에서의 this

메서드 내부에서 정의하고 실행한 함수에서의 this는 자바스크립트 초심자들이 this에 관해 가장 자주 혼란을 느끼는 점입니다.

<mark>내부함수 역시 이를 함수로 호출했는지 메서드로 호출했는지만 파악하면 this의 값을 정확히 맞출 수 있습니다.</mark>

```js 내부함수에서의 this
1. var obj1 = {
  3. outer: function() {
    4. console.log(this); // (1)
    5. 7. var innerFunc = function() {
      8. console.log(this); // (2) (3)
    };
    6. innerFunc();

    9. 11. var obj2 = {
      12. innerMethod: innerFunc,
    };
    10. obj2.innerMethod();
  },
};
2. obj1.outer();
// (1): obj1
// (2): 전역객체(window)
// (3): obj.innerMethod
```

(2)는 `innerFunc`를 호출한 결과를, (3)은 `obj2.innerMethod`를 호출한 결과입니다.

1. 객체를 생성하는데 내부에 outer 프로퍼티가 있습니다. outer 프로퍼티에 익명함수가 연결되고 생성된 객체를 변수 `obj1`에 할당합니다.


2. `obj1.outer()`를 호출합니다.


3. `obj1.outer` 함수의 실행 컨텍스트가 생성되면서 호이스팅하고, 스코프 체인 정보를 수집하고, this를 바인딩합니다.
이 함수는 호출될 때 함수명 `outer` 앞에 점(.)이 있었으므로 메서드로서 호출된 것입니다.
따라서 `this`는 점(.)앞에 객체인 `obj1`을 바인딩합니다.


4. `this`가 바인딩된 `obj1` 객체 정보가 출력됩니다.


5. 호이스팅된 변수 `innerFunc`는 outer 스코프 내에서만 접근할 수 있는 지역변수 입니다. 이 변수에 익명 함수를 할당합니다.


6. `innerFunc()`를 호출합니다.


7. `innerFunc` 함수의 실행 컨텍스트가 생성되면서 호이스팅,스코프 체인 수집, this 바인딩 등을 수행합니다.
이 함수는 호출될 때 ~~함수명 앞에 점(.)이 없었습니다.~~ 함수로서 호출되었습니다.
따라서 this가 지정되지 않았고, 자동으로 스코프 체인상의 최상위 객체인 전역객체(window)가 바인딩 됩니다.


8. `this`가 바인딩된 `window` 객체 정보가 출력됩니다.


9. `obj2` 역시 outer 스코프 내부에서만 접근할 수 있는 지역변수 입니다. 
`obj2` 변수에는 `object`를 할당하는데, object 안에 `innerMethod`라는 프로퍼티가 존재하고, 프로퍼티 값으로 앞서 정의된 변수 `innerFunc`와 연결된 익명 함수가 지정됩니다.


10. `obj2.innerMethod()`를 호출합니다.


11. `obj2. innerMethod` 함수의 실행 컨텍스트가 생성됩니다. 이 함수는 호출할 때 함수명인 `innerMethod` 앞에 점(.)이 있었으므로 메서드로서 호출한 것입니다.
따라서 `this`에는 마지막 점 앞의 객체인 `obj2`가 바인딩 됩니다.


12. `obj2` 객체 정보가 출력됩니다.

* 정리하자면 
<mark>this 바인딩에 관해서는 함수를 실행하는 당시의 주변 환경(메서드 내부인지, 함수 내부인지 등)은 중요하지 않고 오직 해당 함수를 호출하는 구문 앞에 점(.) 또는 대괄호[] 표기가 있는지 없는지가 관건입니다.</mark>

------
#### 메서드의 내부 함수에서의 this를 우회하는 방법

호출 주체가 없을 때 자동으로 전역객체를 바인딩하지 않고 호출 당시 주변 환경의 `this`를 그대로 상속받아 사용하고 싶다면
간단하고 대표적인 방법으로 변수를 활용하는 방법이 있습니다.

```js 내부함수에서의 this를 변수를 활용하여 우회하기
var obj = {
  outer: function() {
    console.log(this); // (1) { outer: f }
    var innerFunc1 = function() {
      console.log(this); // (2) Window { ... }
    };
    innerFunc1();

    var self = this;
    var innerFunc2 = function() {
      console.log(self); // (3) { outer: f }
    };
    innerFunc2();
  },
};
obj.outer();
```

1. `innerFunc1` 내부에서 `this`는 전역객체를 가리킵니다.

2. outer 스코프에서 `self`라는 변수에 `this`를 저장한 상태에서 호출한 `innerFunc2`의 경우 `self`에는 객체 obj가 출력됩니다.

~~그저 상위 스코프의 this를 저장해서 내부함수에서 활용하려는 수단이므로 변수명은 달라도 무관합니다.~~

------
#### this를 바인딩하지 않는 함수

ES6에서는 함수 내부에서 `this`가 전역객체를 바라보는 문제를 보안하고자, 
`this`를 바인딩하지 않는 <mark>화살표 함수(Arrow function)</mark>를 새로 도입했습니다. 

화살표 함수는 실행 컨텍스트를 생성할 때 `this` 바인딩 과정 자체가 빠지게 되어, 상위 스코프의 `this`를 그대로 활용할 수 있습니다.

```js 화살표 함수
var obj = {
  outer: function() {
    console.log(this); // (1) { outer: f }
    var innerFunc = () => {
      console.log(this); // (2) { outer: f }
    };
    innerFunc();
  },
};
obj.outer();
```

이 밖에도 `call`, `apply` 등의 메서드를 활용해 함수를 호출할 때 명시적으로 `this`를 지정하는 방법이 있습니다.

------
### 콜백 함수 호출 시 그 함수 내부에서의 this

* `함수 A`의 제어권을 `다른 함수(또는 메서드) B`에게 넘겨주는 경우 `함수 A`를 `콜백 함수`라고 합니다.

* 이때 `함수 A`는 `함수 B`의 내부 로직에 따라 실행되며, `this` 역시 `함수 B` 내부 로직에서 정한 규칙에 따라 값이 결정됩니다.

* `콜백 함수` 역시 함수이므로 기본적으로 `this`가 전역객체를 참조하지만, 제어권을 받은 함수 (함수 B)에서 `콜백 함수`에 별도로 `this`가 될 대상을 지정한 경우에는 그 대상을 참조하게 됩니다.

```js 콜백 함수 내부에서의 this
setTimeout(function() {
  console.log(this);
}, 300); // (1) window

[1, 2, 3, 4, 5].forEach(function(x) {
  // (2) window
  console.log(this, x);
});

document.body.innerHTML += '<button id="a">클릭</button>';
document.body.querySelector('#a').addEventListener('click', function(e) {
  // (3) <button id="a">클릭</button>
  console.log(this, e);
});
```

1. 0.3초 뒤 전역객체가 출력됩니다.

2. 배열의 각 요소를 차례대로 콜백 함수의 첫 번째 인자로 삼아 전역객체와 배열의 각 요소가 총 5회 출력됩니다.

3. 지정한 `HTML` 엘리먼트에 `'click'` 이벤트가 발생할 때 마다 그 이벤트 정보를 콜백 함수의 첫 번째 인자로 삼아 함수를 실행합니다. 버튼을 클릭하면 앞서 지정한 엘리먼트와 클릭 이벤트에 관한 정보가 담긴 객체가 출력됩니다.

이 처럼 콜백 함수에서의 `this`는 한가지로 정의할 수 없습니다.
<mark>콜백 함수의 제어권을 가지는 함수(메서드)가 콜백 함수에서의 this를 무엇으로 정할지 결정하며</mark>,
따로 정의하지 않은 경우 기본적으로 전역객체를 참조합니다.

------
### Class 함수 내부에서의 this

객체지향 언어에서 생성자를 클래스(`class`), 
클래스를 통해 만든 객체를 인스턴스 (`instance`)라고 합니다.

프로그래밍적으로 `class`는 구체적인 `instance`를 만들기 위한 일종의 틀입니다.
틀 안에는 해당 `class`의 공통 속성들이 준비되어 있고, 추가로 개별 `instance`를 만들 수 있습니다.

`new` 키워드와 함께 함수를 호출하면 해당 함수가 `class`로서 동작합니다.
`class`로서 함수가 호출된 경우 내부에서의 `this`는 `instance`가 됩니다.

* `class` 함수를 `new`키워드로 호출 하면 엔진은 `class`의 `prototype` 프로퍼티를 참조하는 `__proto__` 프로퍼티 `instance`를 만들고 공통 속성 및 특성 들을 해당 객체(`this`)에 부여합니다.
`instance`가 생성됩니다.

```js class 함수
var Cat = function(name, age) {
  this.bark = '야옹';
  this.name = name;
  this.age = age;
};
var choco = new Cat('초코', 7);
var nabi = new Cat('나비', 5);
console.log(choco, nabi);

/* 결과
Cat { bark: '야옹', name: '초코', age: 7 }
Cat { bark: '야옹', name: '나비', age: 5 }
*/
```

1. `new` 키워드와 함께 Cat 함수를 호출하여 변수 choco, nabi에 각각 할당 했습니다.

2. `console.log` 출력 결과 `this`가 각각 Cat `class`의 `instance`를 참조하여 반환합니다.
(`choco instance`, `nabi instance`)

------
<h2 id="this_binding">명시적으로 this를 바인딩하는 방법</h2>

상황에 따라 this에 바인딩 되는 값들을 살펴봤지만 이런 규칙을 무시?하고 this에 대상을 지정하여 바인딩하는 방법도 있습니다.

------
### call 메서드

> Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])

`call` 메서드는 호출 주체인 함수를 즉시 실행하도록 합니다.
`call` 메서드의 첫 번째 인자를 `this`로 바인딩 하고, 이후에 인자들을 호출할 함수의 매개변수로 사용합니다.

`call`메서드를 이용하여 `this` 값으로 참조할 객체를 지정할 수 있습니다. 

```js call 메서드
var func = function(a, b, c) {
  console.log(this, a, b, c);
};

func(1, 2, 3); // Window{ ... } 1 2 3
func.call({ x: 1 }, 4, 5, 6); // { x: 1 } 4 5 6
```

* `func.call`에서 메서드의 첫 번째 인자 `{x : 1}`를 `this`가 참조할 값으로 던져줍니다.

* 객체의 메서드를 호출하면 `this`는 객체를 참조하게 되지만, `call` 메서드는 이렇듯 임의의 객체를 `this`로 지정할 수 있습니다.

------
### apply 메서드

> Function.prototype.apply(thisArg[, argsArray])

`apply` 메서드는 기능적으로 `call` 메서드와 완전히 동일합니다.

`apply`메서드는 <mark>두 번째 인자를 배열로 받아 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다는 점에서 차이가 있습니다.</mark>

```js apply 메서드
var func = function(a, b, c) {
  console.log(this, a, b, c);
};
func.apply({ x: 1 }, [4, 5, 6]); // { x: 1 } 4 5 6

var obj = {
  a: 1,
  method: function(x, y) {
    console.log(this.a, x, y);
  },
};
obj.method.apply({ a: 4 }, [5, 6]); // 4 5 6
```

------
### call / apply 메서드의 활용

`call` 과 `apply` 메서드의 활용 사례

------
#### 유사 배열객체에 배열 메서드 적용

```js 유사 배열객체에서 배열 메서드 사용하기
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3,
};
Array.prototype.push.call(obj, 'd');
console.log(obj); // { 0: 'a', 1: 'b', 2: 'c', 3: 'd', length: 4 }

var arr = Array.prototype.slice.call(obj);
console.log(arr); // [ 'a', 'b', 'c', 'd' ]
```

* 유사배열객체인 경우 call 또는 apply 메서드를 이용해 배열 메서드를 사용할 수 있습니다.

* 배열 메서드인 `push`를 `객체 obj`에 적용해 프로퍼티 3에 'd'를 추가했습니다.

* 배열 메서드인 `slice`로 얕은 복사하여 객체를 배열로 반환했습니다.

이 밖에도 유사배열객체에는 `call`, `apply` 메서드를 이용해 모든 배열 메서드를 적용할 수 있습니다.
단, 문자열의 경우 `length` 프로퍼티가 읽기 전용이기 때문에 원본 문자열에 변경을 가하는 메서드(`push`, `pop`, `shift`, `unshift`, `splice` 등)는 에러를 던지며, `concat` 처럼 대상이 반드시 배열이어야 하는 경우에는 에러는 나지 않지만 재대로 된 결과를 얻을 수 없습니다.

* 사실 `call`, `apply`를 이용해 형변환하는 것은 '`this`를 원하는 값으로 지정해서 호출한다'라는 본래의 메서드의 의도와는 어긋나는 활용법이라고 할 수 있습니다.
또한 코드만 봐서는 어떤 의도인지 파악하기 쉽지 않습니다.


* <mark>ES6 에서는 유사배열객체 또는 이터러블한 모든 종류의 데이터 타입을 배열로 전환</mark>하는 `Array.from`메서드가 추가되었습니다.

------
#### Array.from
Array 오브젝트를 생성하고 콜백 함수에서 반환된 값을 엘리먼트 값으로 설정하여 새로운 Array 객체를 반환합니다.

>Array.from(arrayLike[, mapFn[, thisArg]])

* arrayLike
배열로 변환하고자 하는 유사 배열 객체(Array-like)나 반복 가능한 객체(이터러블 오브젝트).

* mapFn (선택적 파라미터)
배열의 모든 엘리먼트 마다 호출할 함수.

* thisArg (선택적 파라미터)
두 번째 파라미터 함수 실행 시에 this로 참조할 값.

* 반환 값
새로운 Array 인스턴스.

```js ES6의 Array.from
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3,
};
var arr = Array.from(obj);
console.log(arr); // ['a', 'b', 'c']
```

------
#### class 내부에서 다른 class를 호출

`class` 내부에 다른 `class`와 공통된 내용이 있을 경우