---
title: ES6에 추가된 Operation -ECMAScript
date: 2020-03-22 07:14:37
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
- Operation
- Exponentiation
- Computed property name
- Default Value
- for-of
- ES7
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

`ES6`에 추가된 다양한 형태의 오퍼레이션(`Operation`) 과 `for-of` 문 과 `ES7`에 추가된 거듭제곱 연산자를 살펴봅니다.

*   Operation
    *   [프로퍼티 이름조합](/2020/03/22/ES6에%20추가된%20Operation%20-ECMAScript/#프로퍼티_이름조합)
    *   [Default Value](/2020/03/22/ES6에%20추가된%20Operation%20-ECMAScript/#Default_Value)
    *   [Default 파라미터](/2020/03/22/ES6에%20추가된%20Operation%20-ECMAScript/#Default_파라미터)
    *   [for-of](/2020/03/22/ES6에%20추가된%20Operation%20-ECMAScript/#for-of)
    *   [거듭 제곱 연산자 (**)](/2020/03/22/ES6에%20추가된%20Operation%20-ECMAScript/#거듭_제곱_연산자_(**))

<!-- more -->

* * *

<h2 id="프로퍼티_이름조합">프로퍼티 이름조합</h2>

문자열과 변수를 조합하여 **오브젝트의 프로퍼티 이름으로 사용하는 것**을  
`Computed property name`(프로퍼티 이름 조합)이라고 합니다.

### 문자열 조합

**문자열을 조합하여 오브젝트의 프로퍼티 키로 사용할 수 있습니다.**

```js
let item = {  
 ["one" + "two"]: 12  
};  
console.log(item.onetwo); // 12  
```

조합하려는 이름을 `대괄호[]`안에 문자열로 작성한 형태입니다.  
`“one”`과 `“two”`를 조합한 `onetwo`가 프로퍼티 키가 됩니다.
`item.onetwo` 형태로 프로퍼티 값을 구할 수 있습니다.

### 변수 값과 문자열 조합

```js
let item = "tennis";  
let sports = {  
 [item]: 1,  
 [item + "Game"]: "윔블던",  
 [item + "Method"](){  
 return this[item];  
 }  
};  
console.log(sports.tennis); // 1  
console.log(sports.tennisGame); // 윔블던  
console.log(sports.tennisMethod()); // 1  
```

*   `[]`안에 **변수 이름을 작성하면 변수 값을 프로퍼티 키로 사용합니다.**  
    `item` 변수 값 `“tennis”`가 프로퍼티 키가 되고 1이 값이 됩니다.  
    따라서 `sports.tennis` 형태로 값을 구할 수 있습니다.


*   `item` 변수 값인 `“tennis”`와 문자열 `“Game”`을 조합하면  
    `“tennisGame”`이 되며 **이를 프로퍼티 키 값으로 사용하고** `“윔블던”`**이 값이 됩니다.**  
    `sport.tennisGame` 형태로 값을 구할 수 있게 됩니다.


*   `[item + “Method”] (){}`
    `[]`안에 변수 이름과 문자열을 조합하고 여기에 함수를 나타내는 소괄호`()`를 넣어 함수 이름으로 사용한 형태입니다.  
    소괄호()는 대괄호[]밖에 작성합니다.  
    `sports.tennisMethod()` 형태로 호출할 수 있습니다.

------
### 디스트럭처링과 프로퍼티 이름 조합

**프로퍼티 이름을 조합하고 조합한 이름을 분할 할당하여 값을 넘겨줄 수 있습니다.**

```js
let one = "sports";  
let {[one]: value} = {sports: "농구"};  
console.log(value); //농구  
```

변수 이름 `one`을 `[]`안에 작성하여 프로퍼티 키로 사용하고 그 값은 `sports`가 됩니다.  
`{[one]: value}`코드는 `{sports: value}`와 같습니다.

`{sports: value} = {sports: “농구”};`  
`value`에 “농구”가 할당됩니다.

* * *

<h2 id="Default_Value">Default Value</h2> 

변수, 파라미터, 프로퍼티에 값이 할당되지 않을 때 사전에 정의한 값이 할당됩니다.  
이 값을 `Default value`라고 합니다.

일반적으로 사용되는 디폴트 값과는 차이가 있습니다.  
예를 들어 let 변수로 선언하고 할당은 하지 않은 경우 디폴트 값으로 `undefined`가 설정됩니다.

여기서의 `Default value`는 변수를 선언하고 할당은 되지 않은 경우 (=undefined 형태일 경우) 기본으로 할당될 값을 지정해 주는 형태 입니다.

### Default 파라미터

```js
function multiply(a, b) {  
 return a * b;  
}  
  
multiply(5, 2); // 10  
multiply(5);    // NaN !  
```

`multiply(5)` 함수 호출 형태는 매개 변수 b에게 값을 할당하지 않습니다  
그러므로 b는 `Defalut value`값 undefined 되어 `return a*b`를 평가할 때 NaN이 반환됩니다.

*   **ES6에서 기본 매개변수라면, 함수 body 내 검사는 더 이상 필요치 않습니다. 이제, 간단히 함수 머리(head)에 b의 기본값으로 1을 둘 수 있습니다**

```js default value
function multiply(a, b = 1) {  
 return a*b;  
}  
  
multiply(5, 2); // 10  
multiply(5); // 5  
multiply(5, undefined); // 5  
```

`호출하는 함수에서 파라미터 값을 넘겨주지 않거나 undefined를 넘겨주면 디폴트 값이 적용됩니다.`

### 파라미터 디스트럭처링

```js
let getTotal = ([one, two] = [10, 20]) => one + two;  
console.log(getTotal()); // 30  
```

함수 파라미터에 디스트럭처링과 디폴트 값을 작성한 형태입니다.  
`getTotal()`을 호출하면서 **파라미터 값은 넘겨주지 않으므로 호출 받는 함수의 디폴트값**이 적용됩니다.  
디폴트 값 [10, 20]이 분할 할당되어 10이 one에 20이 two에 할당됩니다.

```js
let getValue = ({two: value} = {two: 20}) => value;  
console.log(getValue()); // 20  
```

함수 파라미터에 디스트럭처링과 **디폴트 값을 오브젝트로 작성한 형태입니다**.  
디폴트 값 `{two: 20}`이 분할 할당되어 20이 `value`에 할당됩니다.

*   왼쪽에서 오른쪽으로 디폴트 값이 적용됩니다.

```js 디폴트 값 적용 순서
let [one, two = one + 1, five = two + 3] = [1];  
console.log(one, two, five);   
// 1 2 5  
```

오른쪽의 `[]`에서 1이 `one`에 할당됩니다. 이어서 `two=one+1`을 실행하고 할당됩니다.  
다음으로 `five=two+3`을 실행하고 할당합니다.

* * *

<h2 id="for-of">for-of</h2>

`for-of` 문은 이터러블 오브젝트를 반복하여 처리합니다.  
**반복하는 자체는 `for-in`문과 차이가 없지만 대상과 방법에서 차이가 있습니다.**

> for (variable of iterableObject) {  
>       코드  
> }

*   variable  
    반복 할때 마다 서로 다른 속성값이 variable에 할당됩니다.

------
### Array 반복

```js
let iterable = [10, 20, 30];  
  
for (let value of iterable) {  
 console.log(value);  
}  
// 10  
// 20  
// 30  
```

블록 내부 변수를 변경하고 싶지 않은경우, `let` 대신 `const`를 사용할 수 있습니다,.

```js const
let iterable = [10, 20, 30];  
  
for (const value of iterable) {  
 console.log(value);  
}  
// 10  
// 20  
// 30  
```

### String 반복

```js String
let iterable = "boo";  
  
for (let value of iterable) {  
 console.log(value);  
}  
// "b"  
// "o"  
// "o"  
```

### TypedArray 반복

```js TypedArray
let iterable = new Uint8Array([0x00, 0xff]);  
  
for (let value of iterable) {  
 console.log(value);  
}  
// 0  
// 255  
```

### Map 반복

```js Map
let iterable = new Map([["a", 1], ["b", 2], ["c", 3]]);  
  
for (let entry of iterable) {  
 console.log(entry);  
}  
// [a, 1]  
// [b, 2]  
// [c, 3]  
  
for (let [key, value] of iterable) {  
 console.log(value);  
}  
// 1  
// 2  
// 3  
```

### Set 반복

```js Set
let iterable = new Set([1, 1, 2, 2, 3, 3]);  
  
for (let value of iterable) {  
 console.log(value);  
}  
// 1  
// 2  
// 3  
```

### DOM 컬렉션 반복

`NodeList` 같은 `DOM` 컬렉션에 대해 반복

document.querySelectorAll()같은 `DOM` 메서드를 실행하여  
반환된 `NodeList`를 반복할 수 있습니다

*   주의: 이는 NodeList.prototype[Symbol.iterator]가 구현된 플랫폼에서만 작동합니다.

```html DOM-Symbol.iterator
<ul>  
 <li>첫 번째</li>  
 <li>두 번째</li>  
 <li>세 번째</li>  
</ul>  
<script>  
let nodes = document.querySelectorAll("li");  
for (var node of nodes) {  
 console.log(node.textContent);  
 //첫 번째  
 //두 번째  
 //세 번째  
};  
</script>  
```

### 생성기(Generator) 반복

`Generator`에 대해서도 반복할 수 있습니다

```js Generator
function* fibonacci() { // 생성기 함수  
 let [prev, curr] = [1, 1];  
 while (true) {  
 [prev, curr] = [curr, prev + curr];  
 yield curr;  
 }  
}  
  
for (let n of fibonacci()) {  
 console.log(n);  
 // 1000에서 수열을 자름  
 if (n >= 1000) {  
 break;  
 }  
}  
```

### 다른 반복가능 객체에 대해 반복

`iterable` 프로토콜을 명시해서 구현하는 객체에 대해서도 반복할 수 있습니다.

```js 
var iterable = {  
 [Symbol.iterator]() {  
    return {  
        i: 0,  
        next() {  
            if (this.i < 3) {  
            return { value: this.i++, done: false };  
            }  
        return { value: undefined, done: true };  
        }  
    };  
 }  
};  
  
for (var value of iterable) {  
 console.log(value);  
}  
// 0  
// 1  
// 2  
```

### 디스트럭처링

**이터러블 오브젝트 구조에 맞춰 `for-of` 문에 변수를 작성하면 디스트럭처링을 할 수 있습니다.**

```js
let values = [  
 {item: "선물1", amount: {apple: 10, candy: 20}},  
 {item: "선물2", amount: {apple: 30, candy: 40}}  
];  
  
for (var {item: one, amount: {apple: two, candy: five}} of values){  
 console.log(one, two, five);  
};  
// 선물1 10 20  
// 선물2 30 40  
```

`values` 배열의 첫 번째 엘리먼트에서 `item` 프로퍼티 값인 “선물1”이 `one`에 할당됩니다. `amount`는 구조이며 양쪽이 모두 같습니다.  
`apple` 프로퍼티 값 10이 `two`에 할당되고  
`candy` 프로퍼티 값 20이 `five`에 할당됩니다.

`values`의 두 번째 엘리먼트도 같은 방법으로 진행됩니다.

* * *

### for-of와 for-in 차이

**for-in 문의 대상은 Object**이며 열거 가능한 프로퍼티가 대상입니다.

즉 프로퍼티의 `enumerable` 속성 값이 `false`이면 반복에서 제외됩니다.

**for-of 문의 대상은 이터러블 오브젝트** 이며 `prototype`에 연결된 프로퍼티는 대상이 아닙니다.

```js
1. let values = [10, 20, 30];  
  
2. Array.prototype.music = function(){  
 return "음악"  
};  
  
3. Object.prototype.sports = function(){  
 return "스포츠"  
};  
  
4. for (var key in values) {  
 console.log(key, values[key]);  
/*  
0 10  
1 20  
2 30  
music ƒ (){  
 return "음악"  
}  
sports ƒ (){  
 return "스포츠"  
}  
*/  
};  
  
5. for (var value of values) {  
 console.log(value);  
/*  
10  
20  
30  
*/  
};  
```

1.  values [10, 20, 30] 는 `Array` 오브젝트 이므로  
    `Array.prototype`에 연결된 프로퍼티가 `values.__proto__`에 첨부됩니다.


2.  `Array.prototype`에 `music` 메서드를 추가하면  
    `values.__proto__`와 `Array.prototype`이 연동되므로  
    `values.music()`형태로 호출할 수 있습니다.  
    ~~빌트인 오브젝트 prototype에 메서드를 추가하는 것은 좋은 방법이 아닙니다. 위는 예시를 위한 표현입니다.~~


3.  `Object.prototype`에 메서드를 추가하는 것 역시 `values.sports()` 식으로 호출 할수 있게 해줍니다.


4.  `for-in` 문으로 `values` 배열을 열거하면 `Array.prototype`에 추가된 `music`과 `Object.prototype`에 추가된 `sports`가 출력됩니다.  
    `Array.prototype`에 빌트인으로 설정된 메서드는 열거되지 않고, 개발자 코드로 추가한 메서드만 열거됩니다.


5.  `for-of` 문으로 `values` 배열을 열거하면 `prototype`에 연결된 프로퍼티가 열거되지 않습니다. 이점이 `for-in`과 `for-of`의 차이입니다.

* * *

### for-of로 Object 열거 하는 방법

<u>오브젝트는 이터러블 오브젝트가 아니므로 for-of 문으로 열거할 수 없습니다.</u>

**개발자 코드로 사전처리 해줌으로써 for-of문에서 오브젝트를 열거할 수 있습니다.**

```js
let sports = {  
 soccer: "축구",  
 baseball: "야구"  
};  
  
let keyList = Object.keys(sports);  
for (var key of keyList){  
 console.log(key, sports[key]);  
  
// soccer 축구  
// baseball 야구  
};  
```

`Object.keys(sports)`는 파라미터의 `sports` 오브젝트에서 프로퍼티 키를 배열로 반환합니다. 배열은 이터러블 오브젝트이므로 이를 `for-of`문으로 반복하면서 `sports[key]`형태로 프로퍼티 값을 구할 수 있습니다.

* * *

<h2 id="거듭_제곱_연산자_(**)">거듭 제곱 연산자 **</h2>

거듭 제곱(`Exponentiation`) 연산자는 곱하기 문자`(*)`를 연속하여 2개 작성한 형태`(**)`입니다. `ES7` 스펙에 추가되었습니다.

```js
console.log(3**2);  
console.log(3**3);  
console.log(Math.pow(3, 3));  
/*  
9  
27  
27  
*/  
```

`3**2`는 3의 2승 값 9 입니다.  
`3**3`은 3의 3승 값 27 입니다.  
`**`은 `Math.pow()`메소드와 같습니다.
