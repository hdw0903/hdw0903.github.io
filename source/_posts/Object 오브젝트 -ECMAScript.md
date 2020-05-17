---
title: Object 오브젝트 -ECMAScript
date: 2020-03-23 10:28:44
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
- Object
- get
- set
- getter
- setter
- is()
- assign
- setPrototypeOf
- __proto__
- use strict
- debugger
- Descriptor
- Operation
- prototype
- property
- Object is()

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


`ES6`에서 `Object` 오브젝트에 작성하고 제어하는 방법이 추가 되었습니다.  
어떤것은 ES5와 다르게 변경된 것도 있습니다.

*   Object
    
    * [오퍼레이션](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_Operation)
    * [디스크립터](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_Descriptor)
    * [get, set 속성](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_get_set)
    * [getter](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_getter)
    * [setter](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_setter)
    * [is(): 값과 값 타입 비교](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_is)
    * [assign(): 오브젝트 프로퍼티 복사](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_assign)
    * [assign() 필요성](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_assign_necessity)
    * [assign() 고려사항](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_assign_consider)
    * [assign() getter](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_assign_getter)
    * [setPrototypeOf():&#95;&#95;proto&#95;&#95;에 첨부](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_setPrototypeOf)
    * [&#95;&#95;proto&#95;&#95;](/2020/03/23/Object%20오브젝트%20-ECMAScript/#Object_proto)
   
<!-- more -->

* * *

<h2 id="Object_Operation">오퍼레이션</h2>

* * *

### Object에 같은 key 사용

(var obj= {key: value}) 형태에서 key 값이 같은 프로퍼티를 두 개 작성했을 때  
자바스크립트 에디션(버젼)별로 차이가 있습니다.

**ES3에서는 key 값이 같더라도 추가되고 ES5의 strict 모드에서는 에러가 발생합니다.**

**ES6에서는 strict 모드에 관계없이 에러가 발생하지 않으며 나중에 작성한 프로퍼티 값으로 대체됩니다.**

```js ES6 같은 key값 사용
"use strict";  
debugger;  
  
let sameKey = {one: 1, one: 2};  
console.log(sameKey);  
// Object {one:2}  
```

오브젝트 프로퍼티 키 값이 one인 프로퍼티 두 개를 작성 했습니다.  
`ES6 버젼에서 첫 번째의 one 프로퍼티 값 1이 두 번째 프로퍼티 값 2로 대체됩니다.`  
`나중에 작성된 프로퍼티는 값만 대체되고 추가되지 않습니다.`  
`{one:2}가 두 개 작성되지 않습니다.`

### 변수 이름으로 값 설정

변수 이름을 사용하여 `Object`의 프로퍼티 값을 설정할 수 있습니다.

```js
let one = 1, two = 2;  
let values = {one, two};  
console.log(values);  
// Object {one: 1, two:2}  
```

(let values = {one, two})에서 one이 프로퍼티 이름이 되면서 one 변수 값인 1이  
프로퍼티 값으로 설정됩니다.  
two 역시 프로퍼티 이름이 되면서 변수 값인 2가 프로퍼티 값으로 설정됩니다.

{one, two} 의 형태가 변수의 이름을 사용하여 프로퍼티 이름이 되면서  
변수의 값이 프로퍼티 키로 할당되어 {one:1, two:2}형태로 변환 됩니다.

### Object에 function 작성

*   `ES5` 에서는 `Object`에 함수를 아래와 같은 형태로 작성합니다.

```js ES5
let obj = {  
 getTotal: function(param){  
 return param + 123;  
 }  
};  
console.log(obj.getTotal(400));  
//523  
```

*   `ES6` 에서 `Object`에 함수(메서드)를 다른 방법으로 작성할 수 있습니다.

```js ES6
let obj = {  
 getTotal(param){  
 return param + 123;  
 }  
};  
console.log(obj.getTotal(400));  
//523  
```

`getTotal(param) {}` 형태와 같이 클론(`;`)과 `function` 키워드를 작성 하지 않습니다.  
이 형태의 설명은 다음에 다루고 여기서는 바뀐 형태와 그에 따른 코드 작성의 편리함 정도만 알고 넘어갑니다.

* * *

<h2 id="Object_Descriptor">디스크립터</h2>

디스크립터(`Descriptor`)는 ES5에서 제시되었으며 이를 바탕으로  
`ES6`에서 여러 기능들이 추가 되었습니다.

내용의 연결을 위해 간단하게 요점만 다룹니다.

```js
Object.defineProperty({}, "book", {  
 value : 123  
 enumerable: true  
});  
```

“book”은 프로퍼티 이름입니다. 프로퍼티 이름 이외에  
{value: 123, enumerable: true}가 프로퍼티 디스크립터입니다.  
프로퍼티 디스크립터는 속성 이름(`enumerable`)과 속성 값(`true`)으로 구성됩니다.

프로퍼티 디스크립터는 데이터 프로퍼티 디스크립터 타입과 엑세스(`access`) 디스크립터 타입으로 분류됩니다.

------
### 프로퍼티 디스크립터

| 타입   | 속성 이름    | 속성 값 형태           | 디폴트 값 | 개요                        |
|--------|--------------|------------------------|-----------|-----------------------------|
| 데이터 | value        | Javascript 데이터 타입 | undefined | 프로퍼티 값으로 사용        |
|        | writable     | true, false            | false     | false: 속성 값 변경 불가    |
| 엑세스 | get          | function, undefined    | undefined | 프로퍼티 getter 함수        |
|        | set          | function, undefined    | undefined | 프로퍼티 setter 함수        |
| 공용   | enumerable   | true, false            | false     | false: for-in으로 열거 불가 |
|        | configurable | true, false            | false     | false: 프로퍼티 삭제 불가   |


데이터 타입 속성과 엑세스 타입 속성을 같이 작성할 수 없습니다.  
“{value:1, get: function(){}}” 형태과 같이 value 속성과 get 속성을 같이 작성하면 에러가 발생합니다.

스펙에서 `{writable: true}` 형태를 `[[writable]]:true`로 기술하고 있습니다.  
`enumerable`, `configurable`도 같습니다.  
위와 같이 대괄호 두 개 [[]] 사이에 속성 이름을 작성합니다.

* * *

<h2 id="Object_get_set">get, set 속성</h2>

`get` 속성은 `getter` 기능을 제공하고 `set` 속성은 `setter` 기능을 제공합니다.

**아래는 ES5 에서의 사용 형태 입니다.**

-------
### ES5 get

```js ES5 get
var obj = {};  
2. Object.defineProperty(obj, "book", {  
 3. get: function(){  
 4. return "책";  
 }  
});  
  
1. console.log(obj.book); // 책  
```

1.  엔진이 코드를 해석하면
    

2.  `obj` 오브젝트에서 `book` 프로퍼티 작성 여부를 체크합니다.  
    `book` 프로퍼티가 작성되어 있으면 `get` 속성의 존재 여부를 체크합니다.
    

3.  존재한다면 `get` 속성 값인 함수를 실행합니다. **이것이 getter 입니다.**
    

4.  `getter`가 호출되어 호출된 값 `“책”`이 반환됩니다.
    

**getter는 obj.book()과 같이 함수를 호출하는 형태로 작성하지 않고**  
**obj.book과 같이 함수 이름만 작성합니다.**

------
### ES5 set

```js ES5 set
var obj = {};  
Object.defineProperty(obj, "item", {  
 set: function(param){  
 this.sports = param;  
 }  
});  
  
obj.item = "야구";  
console.log(obj.sports); // 야구  
```

1.  `obj` 오브젝트에서 `item` 프로퍼티 작성 여부를 체크합니다.
    

2.  작성되어 있으면 `set` 속성의 존재 여부를 체크합니다. 존재 하면 `set` 속성 값인 함수를 실행합니다.
    

3.  이때 “야구” 값 을 실행하는 함수의 파라미터 값으로 넘겨줍니다.  
    `이것이 setter 입니다.`
    

`setter`가 호출되면 `this.sports`에서 `this`가 `obj` 오브젝트를 참조합니다.  
파라미터로 넘겨받은 `“야구”`를 `obj` 오브젝트의 `sports` 프로퍼티에 할당합니다.

`(obj.sports)` 형태는 `getter`입니다. 하지만 `obj` 오브젝트에 `get` 속성을 작성하지 않았습니다. 디폴트로 `getter가` 호출되어 `obj` 오브젝트의 `sports` 프로퍼티 값을 반환 합니다.

------
### ES6 get

**ES6에서는 보다 직관적으로 getter와 setter를 정의할 수 있습니다.**

```js ES6 getter
let obj = {  
 value: 123,  
 get getValue(){  
 return this.value;  
 }  
};  
console.log(obj.getValue);  
// 123  
```

**ES6에서 getter는 함수(메서드) 이름 앞에 명시적으로 “get”을 작성합니다.**

1.  (`obj.getValue`)와 같이 함수 이름을 작성합니다.
    

2.  `getter getValue`가 함수로 호출 됩니다.
    

3.  `this`는 `obj.getValue`의 `obj` 오브젝트를 참조 합니다.
    

4.  `obj` 오브젝트에 `value` 프로퍼티가 있으므로 값을 반환 합니다.
    
------
### ES6 set

```js ES6 setter
let obj = {  
 set setValue(value){  
 this.value = value;  
 }  
};  
obj.setValue = 123;  
console.log(obj.value);  
// 123  
```

`setter` 또한 `ES6`에서 함수(메서드) 이름 앞에 명시적으로 `“set”`을 작성합니다.

1.  (`obj.setValue = 123`)과 같이 `obj` 오브젝트의 `setValue`를 프로퍼티 키로 하여  
    값을 할당하는 형태로 작성합니다.
    

2.  `setValue`가 `setter`이므로 함수로 호출되고 123을 파라미터 값으로 넘겨줍니다.
    

3.  `this`는 `obj.setValue`의 `obj` 오브젝트를 참조합니다.
    

* * *

<h2 id="Object_is">is(): 값과 값 타입 비교</h2>

`Object.is()` 메서드는 두 개의 파라미터 값과 값 타입을 비교하여 같으면 `true`, 다르면 `false`를 반환 합니다.

**값과 값 타입을 비교하는 것이지 오브젝트를 비교하는 것이 아닙니다.**

배열[]과 배열[] 비교, 오브젝트{}와 오브젝트{} 비교는 false가 반환됩니다.  
단, `window` 오브젝트를 비교하면 `true`를 반환합니다.

값을 비교하는 방법마다 차이가 있습니다.

> 1.  ===  
>     값과 값 타입을 모두 비교합니다.
> 2.  ==  
>     타입은 비교하지 않고 값만 비교합니다.
> 3.  Object.is()  
>     값과 값 타입을 모두 비교합니다.

------
### Object.is() 와 === 의 차이

Object.is()와 ===는 값과 값 타입을 비교하는 점은 같습니다.

차이점.

> *   +0 과 -0을 비교하면
>     *   Object.is()는 false
>     *   ===는 true를 반환합니다.
> *   NaN 과 NaN을 비교하면
>     *   Object.is()는 true를 반환하고
>     *   ===는 false를 반환합니다.

```js 정리
console.log("1:", Object.is(1, "1"));  
//false  
  
console.log("2:", Object.is(NaN, NaN), NaN === NaN);  
// true false  
  
console.log("3:", Object.is(0, -0), 0 === -0);  
// false true  
  
console.log("4:", Object.is(-0, 0), -0 === 0);  
// false true  
  
console.log("5:", Object.is(-0, -0), -0 === -0);  
// true true  
  
console.log("6:", Object.is(NaN, 0/0), NaN === 0/0);  
// true flase  
  
console.log("7:", Object.is(null, null), null === null);  
// true true  
  
console.log("8:", Object.is(undefined, null), undefined === null);  
// false false  
```

* * *

<h2 id="Object_assign">assign(): 오브젝트 프로퍼티 복사</h2>

**`Object.assign()` 메소드는 열거할 수 있는 하나 이상의 출처 객체로부터 대상 객체로 속성을 복사할 때 사용합니다. 대상 객체를 반환합니다.**

> Object.assign(target, …sources)

*   형태 : Object.assign()

    
*   target : 열거 가능한 오브젝트 지정


*   sources : 열거 가능한 오브젝트, 다수 지정 가능, sources 지정안할 시 target 오브젝트 반환
    

두 번째 파라미터의 오브젝트에서 own 프로퍼티만 복사합니다.  
prototype과 프로퍼티 디스크럽터는 복사하지 않습니다.

`own 프로퍼티: 오브젝트 자체에서 작성한 프로퍼티를 나타내며 상속받은 프로퍼티는 포함되지 않습니다.`

------
### 첫 번째 파라미터 null 값

```js 첫 번째 파라미터 null값
try {  
 let obj = Object.assign(null, {x: 1});  
} catch (e) {  
 1. console.log("null 지정 불가");  
}  
  
2. console.log(Object.assign(123));  
// Number {[[PrimitiveValue]]: 123}  
3. console.log(Object.assign(456, 70));  
// Number {[[PrimitiveValue]]: 456}  
```

1.  `Object.assign()`의 첫 번째 파라미터를 지정하지 않거나 `null` 또는 `undefined`로 지정하면 `TypeError`가 발생합니다.  
    `null`로 지정했으므로 `catch(e)`가 실행되어 `“null 지정 불가”`가 출력됩니다.


2.  **Object.assign()의 첫 번째 파라미터에 Number, Boolean, String, Symbol 값을 지정하면 값 타입의 오브젝트를 생성**하고

    파라미터 값을 생성한 오브젝트의 `[[PrimitiveValue]]`에 설정합니다.  
    첫 번째 파라미터가 123 `Number` 오브젝트 이므로 `Number` 오브젝트를 생성하고  
    `[[PrimitiveValue]]`에 123을 설정합니다. 마지막으로 생성한 오브젝트를 반환 합니다.


3.  첫 번째, 두 번째 파라미터 모두 열거 가능한 오브젝트가 아닙니다.  
    첫 번째 오브젝트는 `Number` 오브젝트 이므로 2번과 같은 방식으로 설정되고 반환됩니다.  
    하지만 두 번째 오브젝트는 복사되지 않고 반환됩니다.

------
### 첫 번째 파라미터 String 

```js 첫 번째 파라미터 String
1. console.log(Object.assign("ABC", {one: 1}));  
/*  
String  
0: "A"  
1: "B"  
2: "C"  
one: 1  
length: 3  
__proto__: String  
[[PrimitiveValue]]: "ABC"  
*/  
  
2. console.log(Object.assign(Symbol("ABC"), {one: 1}));  
/*  
Symbol  
description: (...)  
one: 1  
__proto__: Symbol  
[[PrimitiveValue]]: Symbol(ABC)  
*/  
  
try {  
 let obj = Object.assign("ABC", "ONE");  
} catch (e) {  
 3. console.log("파라미터 모두 문자열 사용 불가")  
};  
```

1.  `Object.assign()` 첫 번째 파라미터가 `String` 오브젝트 입니다.  
    `String` 오브젝트를 생성하고 `[[PrimitiveValue]]`에 `“ABC”`가 설정되고  
    `String` 오브젝트는 이터러블 오브젝트 이기 때문에 `{one:1}`을 복사하고 생성한 오브젝트를 반환합니다.


2.  `Object.assign()` 첫 번째 파라미터가 `Symbol` 이므로 `Symbol` 오브젝트를 생성합니다.  
    `[[PrimitiveValue]]`에 `Symbol(“ABC”)`를 설정합니다.  
    생성된 `Symbol` 오브젝트에 `{one:1}`을 복사합니다.


3.  `Object.assign()`의 파라미터가 모두 문자열이므로 `TypeError`가 발생합니다.

------
### 파라미터 값으로 undefined, null 작성시 

```js 파라미터 값으로 undefined, null 작성시
let oneObj = {};  
1. Object.assign(oneObj, "ABC", undefined, null);  
console.log(oneObj);  
/*  
Object  
0: "A"  
1: "B"  
2: "C"  
__proto__: Object  
*/  
let twoObj = {};  
2. Object.assign(twoObj, {key1: undefined, key2: null});  
console.log(twoObj);  
/*  
Object  
key1: undefined  
key2: null  
__proto__: Object  
*/  
```

1.  `Object.assign()` 파라미터에 `undefined`, `null`을 작성하면 복사되지 않습니다.


2.  `Object.assign()` 파라미터에 오브젝트로 작성하고 오브젝트에 프로퍼티 값으로  
    `undefined` 와 `null을` 작성하면 복사됩니다.

**undefined, null를 오브젝트 프로퍼티 값으로 작성시 복사 가능하지만, 파라미터에 값으로 작성하면 복사되지 않습니다.**

* * *

<h2 id="Object_assign_necessity">assign() 필요성</h2>

**일반적으로 `Object` 오브젝트를 변수에 할당하면 프로퍼티가 연동되어 한 쪽의 프로퍼티 값을 바꾸면 다른 한 쪽의 프로퍼티 값이 자동으로 바뀝니다.**

```js 일반적인 오브젝트 프로퍼티 연동 예시
let sports = {  
 event: "축구",  
 player: 11  
}  
1. let dup = sports;  
  
2. sports.player = 55;  
console.log(dup.player);  
// 55  
  
3. dup.event = "농구";  
console.log(sports.event);  
// "농구"  
```

1.  `sports` 오브젝트를 `dup` 변수에 할당하면 `dup` 변수의 오브젝트와 `sports` 오브젝트의 프로퍼티가 연동됩니다.

> console.log(dup);  
> {event: “농구”, player: 55}
> 
> *   event: “농구”
> *   player: 55
> *   &#95;&#95;proto&#95;&#95;: Object

**즉, 한 쪽 오브젝트 프로퍼티 값을 바꾸면 다른 쪽의 프로퍼티 값이 자동으로 변경됩니다.**


2.  `sports` 오브젝트의 `player` 프로퍼티에 55를 할당 합니다.  
    `dup` 오브젝트의 `player` 프로퍼티 값이 55로 변경됩니다.  
    `sports` 오브젝트는 원본이고 `dup` 오브젝트가 복사본입니다.


3.  복사본 `dup` 오브젝트의 `event` 프로퍼티에 값을 할당하면  
    원본 `sports` 오브젝트의 `event` 프로퍼티 값이 자동으로 변경됩니다.

-------
### 값을 연동하여 사용하고 싶지 않은 경우

`Object.assign()`으로 복사하여 사용하면 프로퍼티 값이 연동되지 않습니다.

```js Object.assign() 사용 복사
let sports = {  
 event: "축구",  
 player: 11  
};  
1. let dup = Object.assign({}, sports);  
console.log(dup.player);  
// 11  
  
2. dup.player = 33;  
console.log(dup.player,sports.player);  
// 33 11  
3. sports.event = "수영";  
console.log(dup.event,sports.event);  
// 축구 수영  
```

1.  `Object.assign()` 두 번째 프로퍼티에 지정한 `sports` 오브젝트의 프로퍼티를  
    첫 번째 파라미터에 지정한 빈 오브젝트{}에 복사합니다.  
    그리고 첫 번째 파라미터의 오브젝트를 반환하여 `dup` 변수에 할당합니다.


2.  `dup` 오브젝트의 `player` 프로퍼티에 33을 할당합니다.  
    `sports` 오브젝트의 `player` 프로퍼티 값은 연동되지 않습니다.


3.  `sports` 오브젝트의 `event` 프로퍼티에 “수영”을 할당합니다.  
    `dup` 오브젝트의 프로퍼티 값은 연동되지 않습니다.

* * *

<h2 id="Object_assign_consider">assign() 고려사항</h2>

**`Object.assign()`은 복사한 값이 연동되지 않아 좋지만 고려할 점도 있습니다.**

```js 
let oneObj = {one: 1};  
let twoObj = {two: 2};  
  
1. let mergeObj = Object.assign(oneObj, twoObj);  
console.log(Object.is(oneObj, mergeObj));  
//true  
  
2. mergeObj.one = 456;  
console.log(Object.is(oneObj, mergeObj));  
//true  
```

1.  `twoObj`의 프로퍼티를 `oneObj`에 복사하고 `mergeObj`에 할당합니다.  
    `oneObj` 와 `mergeObj`의 프로퍼티가 같으므로 `ture`가 출력됩니다.


2.  `Object.assign()`으로 복사한 첫 번째 파라미터(`oneObj`)와 두 번째 파라미터(`twoObj`)의 프로퍼티는 연동되지 않지만,

**첫 번째 파라미터 오브젝트(oneObj)를 할당한 오브젝트(mergeObj)는 연동됩니다.**

* * *

<h2 id="Object_assign_getter">assign() getter</h2>

오브젝트의 프로퍼티를 복사할 때 프로퍼티가 `getter`이면  
<u>함수를 복사하지 않고 함수를 호출하여 반환된 값을 복사합니다.</u> 

`return`문을 작성하지 않으면 `undefined`를 반환합니다.

```js
let count = {  
 current: 1,  
 get getCount() {  
 return ++this.current;  
 }  
};  
let mergeObj = {};  
1. Object.assign(mergeObj, count);  
2. console.log(mergeObj);  
// Object {current: 1, getCount: 2}  
```

1.  두 번째 파라미터 `count` 오브젝트에 작성된 순서로 복사합니다.  
    `count` 오브젝트의 `current` 프로퍼티를 복사 하고 그 값은 1입니다.  
    다음 `getCount()`함수를 복사하는 대신 함수를 호출하고 반환된 값을 복사합니다.  
    함수에서 `this.current`에 1을 더하므로 프로퍼티 값은 2가 되고 2를 반환합니다.


2.  `count` 오브젝트의 `current` 프로퍼티를 복사하는 시점의 값은 1입니다.  
    그 후에 `getCount()` 함수를 호출하면서 1을 더하지만  
    원본 프로퍼티 값이 변경되더라도 복사된 `mergeObj`의 프로퍼티 값은 연동되지 않으므로 `current` 값은 1입니다.  
    `getCount`는 함수가 호출되고 반환된 값인 2가 `mergeObj`의 복사되어 출력됩니다.

* * *

<h2 id="Object_setPrototypeOf">setPrototypeOf(): __proto__에 첨부</h2>

`Object.setPrototypeOf()` 메소드는 지정된 객체의 프로토타입 (즉, 내부 [[Prototype]] 프로퍼티)을 다른 객체 또는 `null` 로 설정합니다  
첫 번째 파라미터의 &#95;&#95;proto&#95;&#95;에 두 번째 파라미터를 첨부합니다.

> Object.setPrototypeOf(obj, prototype);

*   obj  
    오브젝트 또는 인스턴스 (프로토타입 설정을 가지는 오브젝트)  
    오브젝트에 프로퍼티를 추가 할 수 없는 오브젝트이면 TypeError 발생
    
*   prototype  
    객체의 새로운 프로토 타입 (오브젝트 or null).
    

<p style="color: red;">경고</p>

오브젝트의 [Prototype]을 변경하는 것은 모든 브라우저와 JavaScript 엔진에서  
단순히 obj.&#95;&#95;proto&#95;&#95; = … 문에 소요 된 시간으로 제한되지 않고  
변경된 [Prototype]에 접근할 수 있는 모든 코드로 확장될 수 있습니다.  
성능에 신경을 쓰면 [[Prototype]] 설정을 피해야 합니다.  
[[Prototype]]을 변경하는 대신 Object.create()를 사용하여 원하는 [[Prototype]]으로 새 오브젝트를 만듭니다.

```js setPrototypeOf() 예제1
let Sports = function(){};  
Sports.prototype.getCount = function(){  
 return 123;  
};  
  
1. let protoObj = Object.setPrototypeOf({}, Sports.prototype);  
  
2. console.log(protoObj.getCount());  
// 123  
```

1.  `setPrototypeOf()`의 두 번째 파라미터인 `Sports.prototype`에 연결된 프로퍼티를 첫 번째 파라미터인 빈 오브젝트{}의 &#95;&#95;proto&#95;&#95;에 첨부합니다.

> *   protoObj: Sports
>     *   &#95;&#95;proto&#95;&#95;: Object ①
>         *   getCount: ƒ ()
>         *   constructor: ƒ ()
>         *   &#95;&#95;proto&#95;&#95;: Object


*   `protoObj`에 &#95;&#95;proto&#95;&#95;가 연결되어 있으며 여기에  
    `Sports.prototype`의 `constructor` 와 `getCount`가 연결되어 있습니다.


2.  `protoObj`의 &#95;&#95;proto&#95;&#95;에 `getCount`가 있으므로  
    `protoObj.getCount()` 형태로 호출할 수 있습니다.

```js setPrototypeOf() 예제2
let Sports = function(){};  
Sports.prototype.getCount = function(){  
 return 123;  
};  
  
1. let fnObj = Object.setPrototypeOf({}, Sports);  
  
2. console.log(fnObj.getCount);  
// undefined  
3. console.log(fnObj.prototype.getCount.call(Sports));  
// 123  
```

1.  예제1에서는 `Object.setPrototypeOf()`의 두 번째 파라미터를  
    `Sports.prototype`를 지정 했습니다. 
    예제 2에서는 `Sports` 함수를 지정합니다.  
    `Sports` 함수를 첫 번째 파라미터 오브젝트의 &#95;&#95;proto&#95;&#95;에 첨부하여 반환합니다.
    

2.  `fnObj.getCount`를 실행하면 `Sports.prototype`에 연결된 `getCount`가 반환되지 않고 `undefined`가 반환됩니다. 이는 `fnObj` 또는 fnObj.&#95;&#95;proto&#95;&#95;에 `getCount`가 없다는 의미입니다.  
    `setPrototypeOf()` 두 번째 파라미터에 `Sports.prototype`이 아닌 `Sports`를 지정하면 `Sports.prototype`에 연결된 메서드를 직접 호출할 수 없습니다.

    
3.  이와 같이 경로를 지정해 호출해줘야 합니다.  
    fnObj.&#95;&#95;proto&#95;&#95;.prototype.getCount()가 전체 경로이지만,  
    &#95;&#95;proto&#95;&#95;는 작성하지 않아도 되므로  
    `fnObj.prototype.getCount.call(Sports)`형태로 작성하면  
    `getCount()`를 호출할 수 있습니다.    

* * *

<h2 id="Object_proto">__proto__</h2>

*   `new` 연산자로 생성된 인스턴스 또는  
    다른 오브젝트의 `prototype`에 연결된 프로퍼티가 &#95;&#95;proto&#95;&#95;에 첨부됩니다.


*   &#95;&#95;proto&#95;&#95;는 엑세스 프로퍼티 입니다.  
    **즉, getter와 setter 기능이 있습니다.**


*   &#95;&#95;proto&#95;&#95;는 [[Enumerable]]: false이고  
    [[Configurable]]: true 입니다.


1.  인스턴스를 생성하면 오브젝트의 `prototype`에 연결된 프로퍼티가  
    인스턴스의 &#95;&#95;proto&#95;&#95;에 첨부됩니다.


2.  **이 환경이 만들어지면 `prototype`에 연결된 메서드를 인스턴스 메서드로 호출할 수 있습니다.**


*   브라우저 개발자 도구에서 &#95;&#95;proto&#95;&#95;에 첨부되는 것처럼 표시되는 것은 개발자가 인식 하기 쉽도록 하기 위한 것으로 실제로는  
    원본 오브젝트의 `prototype`에 연결된 프로퍼티를 참조합니다.  
    이것을 `prototype 공유`(share)라고 합니다. 

    **첨부라는 말은 기능의 표면적 이해를 돕기 위한 것일뿐 실제로 prototype에 프로퍼티를 복사하는 것은 아닙니다.** <mark>참조 혹은 prototype 공유가 맞습니다</mark>.

* * *

<h2>prototype과 __proto__ 차이</h2>

&#95;&#95;proto&#95;&#95;에 있는 메서드는 `object.name()` 형태로 직접 호출할 수 있지만 `prototype`에 연결된 메서드는 `object.prototype.name.call()` 형태로 호출해야 합니다.

```js 
1. let Sports = function(){};  
Sports.prototype.get = function(){};  
let sportsObj = new Sports();  
  
2. sportsObj.__proto__["set"] = function(){};  
3. sportsObj.set();  
  
4. let result = Sports.prototype.set;  
console.log(result);  
// function() {}  
```

1.  `Sports.prototype`에 `get` 메서드를 연결하고 `new` 연산자로 인스턴스를 생성하여  
    `sportsObj`에 할당합니다. sportsObj.&#95;&#95;proto&#95;&#95;에 get()메서드가 첨부됩니다.


2.  생성한 `sportsObj` 인스턴스의 &#95;&#95;proto&#95;&#95;에 `set()` 메서드를 추가합니다. 이때 &#95;&#95;proto&#95;&#95;에 추가하더라도 &#95;&#95;proto&#95;&#95;에 추가되지 않고 `Sports.prototype`에 추가됩니다.  
    **왜냐하면 `set()`메서드를 `Sports`로 생성한 다른 인스턴스에서 공유하기 때문입니다.**


3.  `set()` 메서드가 호출되면 인스턴스의 &#95;&#95;proto&#95;&#95;에 있지만,  
    &#95;&#95;proto&#95;&#95;은 프로퍼티 검색과 경로 제공을 위한 것으로,  
    **실제로 호출되는 메서드는 인스턴스를 생성한 `Sports.prototype`에 연결된 메서드입니다.**  
    <mark>이것이 `prototype`의 프로퍼티 공유 개념이며 자바스크립트의 아키텍처입니다.</mark>


4.  `sportsObj`의 &#95;&#95;proto&#95;&#95;에 set()메서드를 추가했는데,  
    `Sports.prototype.set`으로 코드가 출력된 것은 실제로 `Sports.prototype`에 추가되기 때문입니다.