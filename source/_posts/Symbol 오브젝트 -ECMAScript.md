---
title: Symbol 오브젝트 -ECMAScript
date: 2020-04-06 10:35:07

categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - Symbol Object
  - primitive
  - Symbol 사용 형태
  - JSON.stringify
  - Symbol-keyed property
  - Symbol-keyed

toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right

sidebar:
  right:
    sticky: true
---

*   Symbol Object
    *   [primitive](/2020/04/06/Symbol%20오브젝트%20-ECMAScript/#primitive)
    *   [Symbol()](/2020/04/06/Symbol%20오브젝트%20-ECMAScript/#Symbol_값_생성)
    *   [Symbol 값 변경](/2020/04/06/Symbol%20오브젝트%20-ECMAScript/#Symbol_값_변경)
    *   [Symbol 오브젝트 생성](/2020/04/06/Symbol%20오브젝트%20-ECMAScript/#Symbol_Object)
    *   [오브젝트에서 Symbol 사용](/2020/04/06/Symbol%20오브젝트%20-ECMAScript/#Object_Symbol)
    *   [Symbol 사용 형태](/2020/04/06/Symbol%20오브젝트%20-ECMAScript/#Symbol_ex)

<!-- more -->

* * *

<h2 id="primitive">primitive</h2>

자바스크립트에 프리미티브(`primitive`) 개념이 있습니다. 이는 오브젝트가 아닌 값 입니다.  
`ES5`에 `string`, `number`, `boolean`, `null`, `undefined`가 있으며  
`ES6`에 `Symbol`이 추가되었습니다.

* ex) (var num = 123;)을 실행하면 num에 123이 할당됩니다.  
123은 number 타입의 프리미티브 값입니다.

`string`, `number`, `boolean` 은 래퍼(`Wrapper`) 오브젝트가 있습니다.  
각각 `String`, `Number`, `Boolean` 오브젝트가 래퍼 오브젝트 입니다.  
`ES6`에서 `symbol`의 `Symbol` 오브젝트가 추가되었습니다.  
`undefined`, `null`은 래퍼 오브젝트가 없습니다.

**valueOf()로 래퍼 오브젝트의 프리미티브 값을 구할 수 있습니다.**  
**단, Symbol은 값을 반환하지 않습니다.**

* * *

<h2 id="Symbol_값_생성">Symbol()</h2>

**Symbol 값 생성**

> let sym = Symbol();

형태로 작성하며 `Symbol` 값을 생성하여 `sym`에 할당합니다.

`new` 연산자는 사용할 수 없습니다.

**Symbol()로 생성된 값은 프로그램 전체를 통해 유일하며 값을 변경할 수 없습니다.**

생성한 `Symbol`에 프로퍼티를 설정할 수 없으며 `strict mode`에서 `TypeError`가 발생합니다.

`Symbol()`로 반환된 값이 오브젝트가 아니므로 오브젝트를 생성한다고 할 수 없습니다.  
`Symbol`을 생성한다는 것은 뉘앙스에 차이가 있습니다. `Symbol` 값을 생성한다는 표현이 적절합니다.

`Symbol`은 `String””`, `Array[]`, `Object{}`, `Boolean(true/false)`와 같이 **오브젝트를 생성하는 리터럴이 없습니다.** undefined, null과 같이 그 자체가 값이 되는 것도 아닙니다.

**`Symbol()`과 같이 함수로 호출해야 값을 생성하며 반환합니다.**

```js
const sym = Symbol();  
console.log("1:", sym);  
console.log("2:", typeof sym);  
console.log("3:", Symbol("주석"));  
  
console.log("4:", sym == Symbol());  
// 1: Symbol()  
// 2: symbol  
// 3: Symbol(주석)  
// 4: false  
```

*   Symbol()을 호출하면 Symbol 값을 생성하여 반환합니다.  
    생성한 값은 변경할 수 없으므로 const 변수에 할당해도 됩니다.


1.  Symbol()로 값을 생성했는데 값이 출력되지 않고 실행 결과 Symbol()이 출력됩니다.  
    Symbol 값을 구하면 Symbol()로 생성한 값을 반환하지 않고,  
    **Symbol값을 생성했던 형태를 반환합니다. 브라우저 개발자 도구에서도 값을 볼 수 없습니다.**  
    **이것이 Symbol의 특징입니다.**


2.  Symbol()로 생성한 값의 typeof는 symbol 입니다.


3.  Symbol()의 파라미터는 선택사항으로 Symbol()로 생성한 값의 설명이나 주석을 문자열로 작성합니다. Symbol 값을 생성하는데 영향을 미치지 않습니다. Symbol 값을 외부에 제공하지 않으므로 디버깅할 때 유용합니다.  
    파라미터를 작성하지 않으면 undefined로 인식합니다. Symbol(“주석”)형태로 반환됩니다.


4.  <mark>앞에서 Symbol()로 생성한 값(sym)과 다시 Symbol()로 생성한 값을 비교하면  
    false가 반환됩니다.Symbol()을 실행할 때마다 프로그램 전체를 통해 유일한 값을  
    생성하므로 값이 같을 수 없습니다. 이것이 Symbol의 특징입니다.</mark>

* * *

<h2 id="Symbol_값_변경">Symbol 값 변경</h2>

`Symbol()`로 생성한 `Symbol` 값을 변경할 수 없습니다. `Symbol` 값에 문자열을 연결할 수 있으나  
`String()` 또는 `toString()`을 사용해야 합니다. 이 형태는 `Symbol` 값이 연결되는 것은 아니며  
`Symbol` 값을 생성할 때의 형태를 연결합니다.

**`Symbol` 값을 템플릿에 사용할 수 없고, `+` 연산자로 문자열을 연결하면 에러가 발생합니다.**

------
### Symbol +연산자, or연산자 사용불가

```js Symbol +연산자, or연산자 사용불가
let sym = Symbol();  
try {  
 +sym;  
} catch (e) {  
 console.log("+sym 사용 불가");  
};  
  
try {  
 sym | 0;  
} catch (e) {  
 console.log("sym | 0 사용 불가");  
};  
// +sym 사용 불가  
// sym | 사용 불가  
```

*   단항 + 연산자를 사용하여 Number 타입으로 변환하면 에러가 발생합니다.  
    Symbol과 비트 or 연산자를 함께 사용하면 에러가 발생합니다.

------
### Symbol 문자열 연결

```js Symbol 문자열 연결
let sym = Symbol();  
try {  
 sym + "문자열";  
} catch (e) {  
 1. console.log("문자열 연결 불가");  
};  
  
2. console.log(String(sym) + "연결");  
3. console.log(sym.toString() + "연결");  
// 문자열 연결 불가  
// Symbol() 연결  
// Symbol() 연결  
```

1.  Symbol()로 생성한 값에 + 연산자로 문자열을 연결하면 에러가 발생합니다.


2.  String(sym) 으로 Symbol을 문자열로 변환하면 문자열을 연결할 수 있습니다.  
    Symbol 값이 문자열로 연결되지는 않고 Symbol 값을 생성한 형태인  
    “Symbol()”이 연결됩니다. “Symbol() 연결”이 출력됩니다.


3.  toString()으로 Symbol()을 문자열로 변환하면 문자열을 연결할 수 있습니다.  
    String()과 마찬가지로 Symbol()을 생성한 형태가 문자열에 연결됩니다.

------
### Symbol 템플릿

```js Symbol 템플릿
let sym =Symbol("123");  
try {  
 `${sym}`;  
} catch (e) {  
 console.log("`${sym} 불가`");  
}  
// `${sym} 불가`  
```

**템플릿에서 Symbol 값을 사용하면 에러가 발생합니다.**  
Symbol 값이 템플릿에 반영되면 문자열 값으로 변환되어 외부에 노출되기 때문입니다.  
(Symbol 값을 생성한 형태도 반영되지 않습니다. Symbol 값을 외부에 노출시키지 않는 것이 Symbol의 특징입니다.)

* * *

<h2 id="Symbol_Object">Symbol 오브젝트 생성</h2>

`Object()`에 `Object(123)`처럼 파라미터 값 `123`을 작성하면 `Number` 오브젝트를 반환합니다.  
마찬가지로 `Object()`의 파라미터에 `Symbol` 값을 작성하면 `Symbol` 오브젝트를 반환합니다.

*   Symbol 오브젝트에
    *   Symbol 메서드,
    *   Symbol.prototype,
    *   prototype에 연결된 프로퍼티가 설정됩니다.

```js Symbol Object
1. let sym = Symbol("123");  
const obj = Object(sym);  
console.log(obj);  
  
2. console.log(obj == sym);  
console.log(obj === sym);  
  
/* Symbol  
description: (...)  
>__proto__: Symbol  
[[PrimitiveValue]]: Symbol(123)  
*/  
  
// true  
// false  
```

1.  `Symbol(“123”)`으로 `Symbol` 값을 생성하고 이를 `Object(sym)` 파라미터에 지정합니다.  
    `Symbol` 오브젝트를 생성하여 반환하게 됩니다. 다음은 생성한 `Symbol` 오브젝트 구성 입니다.
    <img src="/images/SymbolObj.JPG">
    
    1.  Symbol.prototype에 연결된 프로퍼티가 &#95;&#95;proto&#95;&#95;에 첨부됩니다.  
        이를 통해 Symbol.prototype의 메서드와 프로퍼티를 확인할 수 있습니다.
        
    2.  [[PrimitiveValue]]에 Symbol 값을 생성한 형태인 Symbol(“123”)이 설정됩니다.  
        ~~Symbol 값이 설정되지 않고 값을 생성한 형태가 설정됩니다.~~ 
        &#95;&#95;proto&#95;&#95;에 있는 valueOf()로 [[PrimitiveValue]]에 설정된 값을 반환받을 수 있습니다.
        

2.  `Symbol(“123”)`으로 생성한 값 `sym` 과 `Object()`의 파라미터에 지정하여 생성한 `obj`를 비교하면 `true`를 반환합니다. `===` 연산자로 타입까지 비교하면 `false`를 반환합니다. 

**값은 [[PrimitiveValue]] 값으로 비교하여 true를 반환하지만 타입은 Symbol 과 Object 의 타입이 비교되기 때문입니다.**

* * *

<h2 id="Object_Symbol">오브젝트에서 Symbol 사용</h2>

프로그램에서 유일한 값을 갖은 `Symbol`의 특징을 이용하여  
**`Symbol` 값을 오브젝트의 프로퍼티 키로 사용하면 프로퍼티 키가 중복되지 않습니다.**

*   [Symbol()]  
    형태와 같이 대괄호[] 안에 Symbol()을 작성합니다.
    

*   { [Symbol()]: 123}  
    프로퍼티 키에 작성한 Symbol()을 `symbol-keyed property`라고 합니다.  
    Symbol 값은 문자열이 아니며 값이기 때문에 []안에 {“Symbol”} 형태로 작성하지 않고  
    {Symbol} 형태로 작성합니다. **이 형태를 `symbol-keyed property` 라고 하는 것입니다.**

```js
let sym = Symbol("123");  
let obj = {[sym]: "456"};  
// Symbol("123")을 변수에 할당하고 []안에 변수 이름을 작성하여  
// Object 의 프로퍼티 키로 사용  
console.log(obj);  
  
// 프로퍼티 키에 해당하는 값 출력  
console.log(obj[sym]);  
console.log(obj.sym);  
// 대괄호[]를 사용하지 않고 작성하면 에러가 발생하지 않고  
// undefined가 반환됨.  
  
/* Object  
Symbol(123): "456"  
>__proto__: Object  
*/  
  
// 456  
// undefined  
```

* * *

<h2 id="Symbol_ex">Symbol 사용 형태</h2>

`fon-in` 문에서 `symbol-keyed` 프로퍼티가 열거되지 않습니다.  
**Symbol 이 [[ Enumberable]]: false 이기 때문입니다.**

`Symbol-keyed` 프로퍼티를 열거 하려면 `Object.getOwnPropertySymbols()`를 사용해야 합니다.

------
### 클래스 메서드 이름으로 Symbol 사용

```js method-name
const symbolOne = Symbol("symbol one");  
const symbolTwo = Symbol("symbol two");  
  
class Sports {  
 1. static [symbolOne]() {  
 return "Symbol-1";  
 }  
 2. [symbolTwo](){  
 return "Symbol-2";  
 }  
}  
3. console.log(Sports[symbolOne]());  
  
4. let obj = new Sports();  
console.log(obj[symbolTwo]());  
// Symbol-1  
// Symbol-2  
```

1.  `Symbol(“symbol one”)`으로 생성한 값을 `static` 메서드 이름으로 사용한 형태입니다.  
    대괄호 []안에 Symbol 값을 할당한 변수 이름을 작성하고 이어서 소괄호()를 작성합니다.


2.  `Symbol(“symbol two”)`로 생성한 값을 메서드 이름으로 사용한 형태입니다.  
    `static` 키워드를 작성하지 않았으므로 `Sports.prototype`에 `symbolTwo{}`가 연결됩니다.


3.  `symbolOne`은 `static` 메서드 입니다. 클래스 이름에 이어 대괄호[]안에 메서드 이름을 작성하고 소괄호()를 작성하면 메서드가 호출됩니다.


4.  `symbolTwo{}`가 `Sports.prototype`에 연결되어 있으므로 호출하기 위해 인스턴스를 생성합니다. 인스턴스에 이어 대괄호[]안에 메서드 이름을 작성하고 소괄호()를 작성하면 메서드로 호출됩니다.

------
### JSON.stringify()에서 Symbol 사용

`JSON.stringify()`는 `Object` 오브젝트의 프로퍼티를 `{“key”: “value”}`형태로 변환합니다.  
`key는` 문자열로 변환되고 `value는` 타입에 따라 문자열로 변환되지 않을 수도 있습니다.  
**클라이언트에서 서버로 데이터를 `JSON`형태로 전송할 때 사용합니다.**

`Symbol` 값은 외부에 노출되지 않도록 조치 되어 있기 때문에 `{[sym]: “값”}`과 같이  
`Symbol` 값을 프로퍼티로 작성하여 `JSON.stringify()`를 실행하면 
**프로퍼티 키와 프로퍼티 값이문자열로 변환되지 않고 빈 Object {}가 반환됩니다.**


<mark>이를 위한 대처방법으로 Object.getOwnPropertySymbols()를 사용하는 방법이 있습니다.</mark>
