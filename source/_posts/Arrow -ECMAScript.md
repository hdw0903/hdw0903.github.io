---
title: Arrow -ECMAScript
date: 2020-03-17 11:51:03
categories: ECMAScript6
disqusId: tunas-blog-1
tag: 
- ECMAScript6
- JavaScript
- Arrow function
- setTimeout
- arguments
- arguments binding
- Array - like
- new 연산자
- prototype
- block
- param
- this
toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right
  - type: tags
    position: right
  - type: adsense
    position: right
sidebar:
  right:
    sticky: true
---

`arrow`(화살표) 함수는  
`function(param) {코드}` 형태를 축약한 것으로

> (param) => {코드}

형태로 작성합니다.

화살표 함수 표현(arrow function expression)은 `function` 표현에 비해 구문이 짧고 자신의 `this`, `arguments`, `super` 또는 `new.target`을 바인딩 하지 않습니다.  
**화살표 함수는 항상 익명입니다. 이 함수 표현은 메소드 함수가 아닌 곳에 가장 적합합니다. 그래서 생성자로서 사용할 수 없습니다.**

<!-- more -->

* * *

### 기본 구문

> (param1, param2, …, paramN) => { statements }  
> (param1, param2, …, paramN) => expression  
> // 다음과 동일함: => { return expression; }

> // 매개변수가 하나뿐인 경우 괄호()는 선택사항:  
> (singleParam) => { statements }  
> singleParam => { statements }

> // 매개변수가 없는 함수는 괄호()가 필요:  
> () => { statements }

* * *

### 고급 구문

```js
// 객체 리터럴 표현을 반환하기 위해서는 함수 본문(body)을 괄호 속에 넣음:  
params => ({foo: bar})
```

```js
// 나머지 매개변수 및 기본 매개변수를 지원함  
(param1, param2, …rest) => { statements }  
(param1 = defaultValue1, param2, …, paramN = defaultValueN) => { statements }
```
```js
// 매개변수 목록 내 비구조화도 지원됨  
var f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;  
f(); // 6
```

```js 예시
var materials = [  
 'Hydrogen',  
 'Helium',  
 'Lithium',  
 'Beryllium'  
];  
  
materials.map(function(material) {   
 return material.length;   
}); // [8, 6, 7, 9]  
  
// 위에 있는 함수를 arrow 함수를 이용해 아래와 같이 표현할 수 있다  
materials.map((material) => {  
 return material.length;  
}); // [8, 6, 7, 9]  
  
materials.map(({length}) => length); // [8, 6, 7, 9]  
```

* * *

### 줄바꿈  
    화살표 함수는 파라미터와 화살표 사이에 개행 문자를 포함 할 수 없습니다.

> var func = (a, b, c)  
> => 1;  
> // SyntaxError: expected expression, got ‘=>’

하지만, 보기 좋은 코드를 유지하고 싶다면, 아래에 보는 것처럼 괄호나 개행을 둠으로써 이를 수정할 수 있습니다.

```js
var func = (  
a,  
b,  
c  
) => (  
1  
);  
// SyntaxError가 발생하지 않습니다.
```

* * *

### new 연산자 사용  
화살표 함수는 생성자로서 사용될 수 없으며 `new`와 함께 사용하면 오류가 발생합니다.

> var Foo = () => {};  
> var foo = new Foo(); // TypeError: Foo is not a constructor

* * *

### prototype 속성 사용  
화살표 함수는 `prototype` 속성이 없습니다.

> var Foo = () => {};  
> console.log(Foo.prototype); // undefined

* * *

### 함수 호출  
`arrow` 함수는 함수 이름이 없는 무명(혹은 익명) 함수 입니다.  
따라서 함수를 호출 하려면 함수 표현식과 같이 변수에 할당해야 합니다.

> let fn = (param) => {코드}

위 처럼 `arrow` 함수로 생성한 `function` 오브젝트를 **할당할 변수를 작성해야합니다.**

```js
"use strict";  
debugger;  
  
//arrow 함수로 생성된 function 오브젝트를 할당할 변수 es6 작성  
let es6 = (one, two) => { //파라미터 (one,two) 작성  
 return one + two;  
};  
let result = es6(1, 2);  
console.log(result); // 3  
```

* * *

## 블록,파라미터

------
### arrow 함수에서 함수 블록{}을 사용하지 않고 한 줄에 작성할 수 있습니다.

```js {}없이 한줄 작성
"use strict";  
debugger;  
  
let total = (one, two) => one + two;  
let result = total(1, 2);   
console.log(result); // 3  
```

`블록{}`을 사용하지 않은 형태입니다.  
`()`안의 `one,two`가 `total` 함수의 파라미터가 됩니다.  
`one + two;` 는 `return`을 생략한 것입니다. `return one + two;` 와 동일합니다.

------
### 호출 받는 파라미터가 하나인경우 소괄호()을 제외하고 작성할 수도 있습니다.

```js ()제외
let get = value => value + 10; // = (value) + 10;  
let result = get(20);  
console.log(result); // 30  
```

------
### 파라미터 없이 함수 호출만 이루어 진다면 ()만 작성합니다.

```js
let noParam = () => 3 + 4;  
let result = noParam();  
console.log(result); // 7  
```

* * *

## {key: value} 형태의 Object 오브젝트 반환

`{key: value}` 형태의 `Object` 오브젝트 반환하려면 ()안에 {key: value}를 작성합니다.

```js (o)
let get = param => ({sports: "축구"});  
let result = get();  
console.log(result); // Object {sports: "축구"}  
```

`JavaScript`는 소괄호() 안의 코드를 표현식으로 인식합니다.  
그래서 소괄호()안에 작성된 {sports: “축구”}를 반환할 수 있습니다.


```js (x)
let sports = () => {};  
let result = sports();  
console.log(result); // undefined  
```

위 코드는 `sports()` 함수를 호출하면 `{key: value}` 형태의  
빈 `Object` 오브젝트를 반환하는 것이 목적입니다.

하지만, `arrow (=>)` 다음의 `블록{}`을 `함수 블록`으로 인식하고  
함수 블록{}안에 `return`문을 작성하지 않은 것으로 인식되어  
함수 안에 `return`문을 작성하지 않았을 때의 디폴트 값 `undefined`가 반환됩니다.

<mark>arrow 함수이기 때문에 undefined값이 반환 되는것이 아니라
ES5 기준으로 함수블록으로 인식되었기 때문입니다.</mark>

* * *

## arguments

자바스크립트에서는 함수를 호출할 때 인수들과 함께 암묵적으로 `arguments` 객체가 함수 내부로 전달된다.

`arguments` 객체는 함수를 호출할 때 넘긴 인자들이 배열 형태로 저장된 객체를 의미한다.

특이한 점은 실재 배열이 아닌 마치 배열 형태처럼 숫자로 인덱싱된 프로퍼티가 있는 객체다.

이러한 객체를 <mark>배열과 유사하다 하여 유사 배열 객체</mark>라고 부른다.

*   arguments 객체는 세 부분으로 구성되어 있다.

1.  함수를 호출할 때 넘겨진 인자(배열 형태): 첫 번째 인자는 0, … n-1번 인덱스
    
2.  length 프로퍼티: 호출할 때 넘겨진 인자의 개수
    
3.  callee 프로퍼티: 현재 실행 중인 함수의 참조값
    

`arguments은 유사 배열 객체로써 배열과 유사하게 동작하지만, 배열은 아니므로 배열 메서드를 사용하면 에러가 발생한다.`

------
### 바인딩 되지 않은 arguments

화살표 함수는 `arguments` 객체를 바인드 하지 않습니다.  
때문에 `arguments` 프로퍼티를 사용할 수 없습니다.

```js
let sports = () => {  
 try {  
 let args = arguments; //ReferenceError  
 } catch (error) {  
 console.log("사용 불가");  
 }  
}  
sports(1, 2);  
```

<mark>**ES6에서는 arguments 대신에 rest 파라미터를 사용합니다.**</mark>

> let sports = (…rest) => {코드}

()안에 …을 작성하고 이어서 파라미터를 작성합니다.

```js
function foo(n) {  
 var f = (...args) => args[0] + n;  
 return f(2);  
}  
  
foo(1); // 3  
```

* * *

## this와 setTimeout()

`arrow`함수가 간단한 코드작성을 할 수 있어 편리하지만 `this`의 참조 경우를 고려해야 합니다.

```js 예시
let Sports = function(){  
 this.count = 20;  
};  
Sports.prototype = {  
 plus: function(){  
 this.count += 1;  
 },  
 get: function(){  
 setTimeout(function(){  
 console.log(this === window);  
 console.log(this.plus);  
 }, 1000);  
 }  
};  
// newSports 변수에 new 연산자로 생성된 Sports 인스턴스를 생성하여 할당합니다.  
let newSports = new Sports();  
//get() 함수에 작성된 setTimeout() 함수가 실행되어  
//1초 후에 콜백 함수가 실행됩니다.  
newSports.get();  
// true  
// undefined  
```

*   `setTimeout()`이 `window` 오브젝트 함수 이므로 `this`가 `window` 오브젝트를 참조하게 되어 `true`가 출력됩니다.


*   중요한 점은 `newSports.get()`형태로 호출 하였기 때문에 `this`가 `newSports` 인스턴스를 참조 하지 않고 `window` 오브젝트를 참조 한다는 것 입니다.


*   `this`가 `newSports` 인스턴스를 참조하지 못하므로  
    `setTimeout()`을 실행하기 전에 `newSports` 인스턴스를 변수에 할당하고,  
    `setTimeout()` 콜백 함수에서 변수의 인스턴스를 사용하는 형태를 취했습니다.


*   `console.log(this.plus)`코드의 목적은  
    `this`로 `newSports` 인스턴스를 참조하여 `plus` 메서드를 반환받는 것입니다.  
    그런데 `this`가 `window` 오브젝트를 참조하므로 `undefined`값이 출력됩니다.

------
### this 참조값 해결법

```js 해결방법
let Sports = function(){  
 this.count = 20;  
};  
Sports.prototype = {  
 plus: function(){  
 this.count += 1;  
 },  
 get: function() {  
 setTimeout(() => { // 콜백 함수를 arrow함수로 작성합니다.  
 this.plus();  
 console.log(this.count);  
 }, 1000);  
 }  
};  
let newSports = new Sports();  
newSports.get(); // 21  
```

*   `setTimeout()` 함수의 콜백 함수를 `arrow`함수로 작성하면  
    `this`가 `newSports.get()`형태에서 `newSports` 인스턴스를 참조할 수 있게 됩니다.
