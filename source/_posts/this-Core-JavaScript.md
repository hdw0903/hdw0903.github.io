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
  * [this 정리](/2020/04/27/this-Core-JavaScript/#this_point) 

<!-- more -->

------
<h2 id="this">상황에 따라 달라지는 this</h2>

* `this`는 기본적으로 <mark>실행 컨텍스트가 생성될 때 함께 결정됩니다.</mark>
실행 컨텍스트는 함수를 호출할 때 생성되므로, `this`<mark>는 함수를 호출할 때 결정된다고 할 수 있습니다.</mark>

------
### 전역 공간에서의 this

전역 공간에서 `this`는 전역 객체를 가리킵니다. 브라우저 환경에서 전역객체는 `window`이고 Node.js 환경에서는 `global`입니다.

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

  * `var` 변수로 선언한 경우 : delete window 형식으로 삭제 불가,
  delete 변수명 형식으로도 삭제 불가
  * `window` 프로퍼티에 직접 할당한 경우: delete window 형식으로 삭제 가능,
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

`this`에는 호출한 주체에 대한 정보가 담깁니다. 어떤 함수를 메서드로서 호출하는 경우 호출 주체는 바로 함수명(프로퍼티 명)앞의 객체입니다. 점 표기법의 경우 마지막 점 앞에 명시된 객체가 곧 `this`가 됩니다.

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

어떤 함수를 함수로서 호출할 경우 `this`가 지정되지 않습니다.(this는 호출한 주체에 대한 정보가 담깁니다.)
함수로서 호출하는 것은 호출 주체(객체지향 언어에서의 객체)를 명시하지 않고 실행한 것이기 때문에 호출 주체의 정보를 알 수 없는 것입니다. 
<mark>this가 지정되지 않은 경우 this는 전역 객체를 참조합니다.</mark>
따라서 함수에서의 `this`는 전역 객체를 가리킵니다.

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

* 유사배열객체인 경우 `call` 또는 `apply` 메서드를 이용해 배열 메서드를 사용할 수 있습니다.

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

`class` 내부에 다른 `class`와 공통된 내용이 있을 경우 `call` 또는 `apply`를 이용해 다른 `class`를 호출하면 간단하게 반복을 줄일 수 있습니다.

```js class 내부에서 다른 class 호출
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
}
function Student(name, gender, school) {
  Person.call(this, name, gender);
  this.school = school;
}
function Employee(name, gender, company) {
  Person.apply(this, [name, gender]);
  this.company = company;
}
var by = new Student('보영', 'female', '단국대');
var jn = new Employee('재난', 'male', '구골');
```

Student, Employee class함수 내부에서 Person 함수를 호출해서 instance 속성을 정의하게 했습니다.

------
#### 여러 인수를 묶어 하나의 배열로 전달하고 싶을 때

`apply`메서드를 사용해 하나의 배열로 인수들을 전달할 수 있습니다.

```js 여러 인수를 받는 메서드(Math.max/Math.min)에 apply 적용
var numbers = [10, 20, 3, 16, 45];
var max = Math.max.apply(null, numbers);
var min = Math.min.apply(null, numbers);
console.log(max, min); // 45 3
```

ES6에서는 `spread` 연산자를 이용하면 `apply`를 적용하는 것보다 더욱 간편하게 작성할 수 있습니다.

```js
const numbers = [10, 20, 3, 16, 45];
const max = Math.max(...numbers);
const min = Math.min(...numbers);
console.log(max, min); // 45 3
```

------
### bind 메서드

>Function.prototype.bind(thisArg[,arg1[, arg2[, ...]]])

`bind` 메서드는 ES5에서 추가된 기능으로, call과 비슷하지만 즉시 호출하지는 않고 넘겨받은 this 및 인수들을 바탕으로 새로운 함수를 반환하기만 하는 메서드입니다.
다시 새로운 함수를 호출할 때 인수를 넘기면 그 인수들은 기존 `bind` 메서드를 호출할 때 전달했던 인수들의 뒤에 이어서 등록됩니다.
즉 `bind`메서드는 함수에 this를 미리 적용하는 것과 부분 적용 함수를 구현하는 두 가지 목적을 모두 지닙니다.

```js bind메서드의 this 지정과 부분 적용 함수 구현
var func = function(a, b, c, d) {
  console.log(this, a, b, c, d);
};
func(1, 2, 3, 4); // Window{ ... } 1 2 3 4

1. var bindFunc1 = func.bind({ x: 1 });
bindFunc1(5, 6, 7, 8); // { x: 1 } 5 6 7 8

2. var bindFunc2 = func.bind({ x: 1 }, 4, 5);
bindFunc2(6, 7); // { x: 1 } 4 5 6 7
bindFunc2(8, 9); // { x: 1 } 4 5 8 9
```

1. `bindFunc1` 변수에 `func` 변수에 `this`를 `{x : 1}`로 지정하는 새로운 함수가 할당됩니다.
다음 줄에서 `bindFunc1`을 호출하면 지정된 `this`값과 함께 반환됩니다.

2. `bindFunc2` 변수에는 `func` 변수에 `this`를 `{x : 1}`로 지정하고, 파라미터를 차례대로 4, 5로 지정한 함수 새로운 함수가 할당됩니다. 다음 코드를 호출하면 `this`로 지정해준 값과 파라미터에 지정해준 값이 적용되고 그 다음 함수를 호출하며 넘겨준 파라미터 값이 붙습니다 `{x : 1} 4 5 6 7`, `{x : 1} 4 5 8 9` 형태로 반환됩니다.
<mark>이것이 bind의 부분 적용 함수 구현법입니다.</mark>

------
#### name 프로퍼티

<u>bind 메서드를 적용해서 새로 만든 함수는 한 가지 독특한 성질이 있습니다.</u>
name 프로퍼티에 동사 bind의 수동태인 `bound`라는 접두어가 붙습니다.

함수의 name 프로퍼티가 `bound xxx`이라면 함수명이 xxx인 원본 함수에 `bind`메서드를 적용한 새로운 함수라는 의미가 되므로 call 과 apply 메서드에 비해 코드 추적이 용이한 점이 있습니다.

```js
var func = function(a, b, c, d) {
  console.log(this, a, b, c, d);
};
var bindFunc = func.bind({ x: 1 }, 4, 5);
console.log(func.name); // func
console.log(bindFunc.name); // bound func
```

------
#### 상위 컨텍스트의 this를 내부함수나 콜백 함수에 전달하기

메서드의 내부함수에서 메서드의 `this`를 그대로 바라보게 하기 위한 방법으로
self 등의 변수를 활용하거나 화살표 함수를 이용한 우회법이 있었는데
`call`, `apply`, `bind` 메서드를 이용하면 더 깔끔하게 처리할 수 있습니다.

```js 내부함수에 this 전달 - call
var obj = {
  outer: function() {
    console.log(this); //{outer : f}
    var innerFunc = function() {
      console.log(this); //{outer : f}
    };
    innerFunc.call(this);
  },
};
obj.outer();
```

```js 내부함수에 this 전달 -bind
var obj = {
  outer: function() {
    console.log(this); //{outer : f}
    var innerFunc = function() {
      console.log(this); //{outer : f}
    }.bind(this); // 호출x, 새로운 함수 바인드
    innerFunc(); // 호출
  },
};
obj.outer();
```

* 또한 콜백 함수를 인자로 받는 함수(메서드) 중에서 기본적으로 콜백 함수 내에서의 `this`에 관여하는 함수(메서드)에 대해서도 `bind` 메서드를 사용하면 `this`값을 지정하여 바꿀 수 있습니다.

```js bind 메서드 - 내부 함수에 this 전달
var obj = {
  logThis: function() {
    console.log(this);
  },
  logThisLater1: function() {
    setTimeout(this.logThis, 500);
  },
  logThisLater2: function() {
    setTimeout(this.logThis.bind(this), 1000);
  },
};
obj.logThisLater1(); // Window { ... }
obj.logThisLater2(); // obj { logThis: f, ... }
```

------
### 화살표 함수의 예외사항

ES6에 새롭게 도입된 화살표 함수 `=>`는 실행 컨텍스트 생성 시 this를 바인딩 하지 않습니다.
즉 화살표 함수 `=>` 내부에는 `this`가 아예 없으며, `this`에 접근하고자 하면 스코프체인상 가장 가까운 `this`에 접근하게 됩니다.

```js 화삺표 함수 내부에서의 this
var obj = {
  outer: function() {
    console.log(this);
    var innerFunc = () => {
      console.log(this);
    };
    innerFunc();
  },
};
obj.outer();
```

* `call`, `apply`, `bind`를 사용했던 예제의 내부함수를 `=>`함수로 바꾼 것입니다.
더욱 간결해졌습니다.

------
### 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this)

콜백 함수를 인자로 받는 메서드에는 `this`로 지정할 객체(`thisArg`)를 인자로 지정할 수 있는 것들이 있습니다. 이러한 메서드를 이용하여 `this`값을 원하는대로 변경할 수 있습니다.

이러한 메서드는 내부 요소에 대해 같은 동작을 반복 수행해야하는 배열 메서드에 많이 존재하고, 같은 이유로 ES6에 추가된 `Set`, `Map`등의 메서드에도 일부 존재합니다.

* 콜백 함수와 함께 thisArg를 인자로 받는 메서드
  | Array.prototype                        | Set.prototype                | Map.prototype                |
|----------------------------------------|------------------------------|------------------------------|
| `forEach`(callback[, thisArg])           | `forEach`(callback[, thisArg]) | `forEach`(callback[, thisArg]) |
| `map`(callback[, thisArg])               |                              |                              |
| `filter`(callback[, thisArg])            |                              |                              |
| `some`(callback[, thisArg])              |                              |                              |
| `every`(callback[, thisArg])             |                              |                              |
| `find`(callback[, thisArg])              |                              |                              |
| `findIndex`(callback[, thisArg])         |                              |                              |
| `flatMap`(callback[, thisArg])           |                              |                              |
| `from`(arrayLike[, callback[, thisArg]]) |                              |                              |

* 대표적인 배열 메서드인 `forEach` 예시

```js forEach
var report = {
  sum: 0,
  count: 0,
  add: function() {
    var args = Array.prototype.slice.call(arguments);
    args.forEach(function(entry) {
      this.sum += entry;
      ++this.count;
    }, this);
  },
  average: function() {
    return this.sum / this.count;
  },
};
report.add(60, 85, 95);
console.log(report.sum, report.count, report.average()); // 240 3 80
```

* `60, 85, 95`를 인자로 삼아 `add` 메서드를 호출하면 `slice.call`메서드가 인자를 받아 새로운 배열로 반환합니다. 이후 `forEach` 메서드가 실행됩니다.

* 콜백 함수 내부의 `this`는 `forEach` 함수의 두 번째 인자로 전달해준 `this`(add 메서드의 this)가 전달된 상태이므로 `add` 메서드의 `this(report)`를 가리킵니다.

* 따라서 배열의 요소들을 순회 반복하며 `report.sum` , `report.count`값이 차례로 바뀌게 됩니다.
출력 결과로 `report.sum`에 240, `report.count`에 3이 반환되어 출력됩니다.


------
<h2 id="this_point">this 정리</h2>

명시적 `this` 바인딩이 없는 한 항상 성립하는 규칙

  * 전역공간에서의 `this`는 전역객체를 참조 (브라우저에서는 `window`, Node.js에서는 `global`)

  * 함수를 메서드로 호출한 경우 `this`는 메서드 호출 주체를 참조 (메서드명 앞의 객체)

  * 함수를 함수로 호출한 경우 `this`는 전역객체를 참조 (메서드의 내부함수에서도 동일)

  * 콜백 함수 내부에서의 `this`는 해당 콜백 함수의 제어권을 넘겨받은 함수가 정의한 바를 참조
`this`가 정의되어 있지않다면 전역객체를 참조

  * `class` 함수에서 `this`는 생성될 `instance`를 참조

위 규칙에 부합하지 않는 경우 명시적 `this` 바인딩 규칙으로 예측할 수 있습니다.

  * `call`, `apply` 메서드는 `this`를 명시적으로 지정하면서 함수 또는 메서드를 호출

  * `bind` 메서드는 `this` 및 함수에 넘길 파라미터를 일부 지정해 새로운 함수를 만듭니다.

  * 콜백 함수를 반복 순회,호출 하는 일부 메서드는 별도의 인자로 `this`를 받기도 합니다.
