---
title: Number 오브젝트 -ECMAScript
date: 2020-03-26 11:01:34
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - Number Object
  - 64bit 유동소수점
  - EPSILON
  - 진수 리터럴
  - isNaN
  - isInteger
  - isSafeInteger
  - isFinite
  - Binary
  - Octal
  - 2 진수
  - 8 진수
  - Number.MAX_SAFE_INTEGER
  - Number.MIN_SAFE_INTEGER

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

* * *

`ES6`에서 `Number` 오브젝트에 상수와 메서드가 추가되었습니다.  
2진수가 추가되었고 8진수가 재정의 되었습니다.

*   Number 오브젝트
    *   [Number 상수](/2020/03/26/Number%20오브젝트%20-ECMAScript/#Number_상수)
        *   64비트 유동 소수점
    *   [EPSILON](/2020/03/26/Number%20오브젝트%20-ECMAScript/#Number_EPSILON)
    *   [진수 리터럴](/2020/03/26/Number%20오브젝트%20-ECMAScript/#진수_리터럴)
        *   2진수(Binary)
        *   8진수(Octal)
    *   [isNaN(): NaN 여부](/2020/03/26/Number%20오브젝트%20-ECMAScript/#Number_isNaN)
    *   [isInteger(): 정수 여부](/2020/03/26/Number%20오브젝트%20-ECMAScript/#Number_isInteger)
    *   [isSafeInteger(): 안정 정수 여부](/2020/03/26/Number%20오브젝트%20-ECMAScript/#Number_isSafeInterger)
    *   [isFinite(): 유한 값 여부](/2020/03/26/Number%20오브젝트%20-ECMAScript/#Number_isFinite)

<!-- more -->

* * *

<h2 id="Number_상수">Number 상수</h2>

`ES6`에 다음과 같이 두 개의 `Number` 상수가 추가되었습니다.

| 상수 이름               | 값                                    |
|-------------------------|---------------------------------------|
| Number.MAX_SAFE_INTEGER | 9007199254740991 (253 - 1) 최댓값     |
| Number.MIN_SAFE_INTEGER | -9007199254740991 (-(253 - 1)) 최솟값 |


`safe integer`(안전 정수)란, 지수(`e`)를 사용하지 않고 나타낼 수 있는 값을 의미합니다. 2의 64승이 아닌 2의 53승입니다. 위와 같이 최댓값과 최솟값이 있습니다.

### 64비트 유동 소수점

자바스크립트는 `IEEE`(Institute of Electrical and Electronics Engineers) `754`에 정의된 “doble-precision floating-point format numbers”로 숫자 값을 표현합니다.  
64비트 유동 소수점으로 정수와 소수를 포함하여 64개의 비트로 값을 표현합니다.

![https://commons.wikimedia.org/wiki/File:IEEE_754_Double_Floating_Point_Format.svg](https://upload.wikimedia.org/wikipedia/commons/7/76/General_double_precision_float.png)

*   사인 비트(`sign bit`)  
    MSB(most significant bit): 최상위 비트, 즉 제일 왼 쪽에 있는 63번 비트를 뜻하며 0이면 양수, 1이면 음수를 나타낸다.

    
*   지수(`exponent`): 62번 비트에서 52번 비트까지의 11개 비트입니다.


*   유효 숫자(`fraction`): 51번 비트부터 0번 비트까지의 52개 비트에 사인 비트를 포함시켜 53개 비트입니다.

```js
// 9007199254740991  
console.log("1:", Number.MAX_SAFE_INTEGER);  
console.log("2:", Math.pow(2, 53) - 1);  
  
// -9007199254740991  
console.log("3:", Number.MIN_SAFE_INTEGER );  
console.log("4:", -(Math.pow(2, 53) - 1));  
  
1: 9007199254740991  
2: 9007199254740991  
3: -9007199254740991  
4: -9007199254740991  
```

자바스크립트 안정 정수 최댓값과 최솟값을 출력한 결과입니다.

* * *

자바스크립트에는 64비트 형식 `IEEE 754` 정의 값 외에도  
상징적인 값을 표현하는 `Infinity`, `NaN`(숫자가 아님)이 있습니다.

**Number 타입의 값 중에는 두 가지 방식으로 표현할 수 있는 유일한 값이 있는데, 0 이다. 0 은 -0 이나 +0 으로 표시할 수 있다. (“0” 은 물론 +0 이다.) 실제로 이러한 방식은 거의 효력이 없다. 그 예로, +0 === -0 은 true 이다. 하지만 0으로 나누는 경우 그 차이가 눈에 띌 보일 것입니다.**

```js Javascript / 0
42 / +0  
// Infinity  
42 / -0  
// -Infinity  
```

* * *

<h2 id="Number_EPSILON">EPSILON</h2>

*   `Number.EPSILON` 속성(`property`)은 `Number` 형으로 표현될 수 있는 1과 1보다 큰 값 중에서 가장 작은 값의, 차이 값입니다.

*   `EPSILON` 속성은 대략 2.2204460492503130808472633361816E-16 또는 2-52의 값을 갖습니다.

    * [[Writable]]: false, 

    * [[Enumerable]]: false, 

    * [[Configurable]]: false 값을 갖습니다.



* 참고 :
    
    [부동소수점에 대한 이해](https://thrillfighter.tistory.com/349)  
    [Number.EPSILON IEEE754 표현식의 문제](https://perfectacle.github.io/2016/12/24/ES6-Number-object-and-function/#Number-EPSILON)  
    [Number.EPSILON은 왜 2.220446049250313e-16인가?](https://perfectacle.github.io/2017/08/04/ES6-EPSILON/)


```js
1. let total = 0.1 + 0.2;  
console.log(total);  
  
2. let result = (Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON);  
console.log(result);  
  
3. let value = (Math.pow(10, 1) * 0.1) + (Math.pow(10, 1) * 0.2);  
console.log(value / 10 === 0.3);  
// 0.30000000000000004  
// true  
// true  
```

1.  **자바스크립트에서 `0.1` 과 `0.2` 를 더하면 `0.3`이 아니라**


2.  30000000000000004이 출력됩니다. 이는 자바스크립트가 2진 유동 소수점 방식으로 값을 계산하기 때문입니다. 이와 같이 미세한 값 차이로 인해 값이 일치하지 않을 때  
`Number.EPSILON`을 사용합니다. ~~0.1+0.2 == 0.3 // false~~


3.  `0.1`과 `0.2`를 더하고 `0.3`을 뺸 절댓값이 `Number.EPSILON` 값보다 작으면 2진 유동 소수점으로 인한 차이로 처리합니다. `Number.EPSILON` 값보다 작으므로 `true`가 출력됩니다.


4.  소수를 정수로 변환하여 계산한 후, 소수를 정수로 변환할 때 곱했던 값으로 나누면 차이가 나지 않습니다. ES5에서 사용가능한 방법입니다.

* * *

<h2 id="진수_리터럴">진수 리터널</h2>

* * *

### 2진수(Binary)

첫 번째 숫자 0을 작성하고 두 번째에 소문자 b 또는 대문자 B를 작성합니다.  
세 번째 부터 값을 0 또는 1로 작성합니다.  
예시로 5값은 0b0101 또는 0B0101 형태의 값 입니다.

* * *

### 8진수(Octal)

첫 번째 숫자 0을 작성하고 두 번째에 소문자 o 또는 대문자 O를 작성합니다.  
세 번째 부터 값을 0에서 7까지 작성합니다. 0o0105 또는 0O0105 형태입니다.  
ES5에서는 첫 번째에 o 또는 O를 작성 했었습니다.

```js literal
// 2진수  
let two = 0b0101;  
console.log(two);  
  
// 8진수  
let eight = 0o0101;  
console.log(eight);  
// 5 65  
```

0b0101는 2진수로 값이 5이며, 0o0101는 8진수로 값이 65입니다.  
웹 개발자에게는 CSS에서 #0012FF 형태의 16진수를 사용하므로 16진수가 더 익숙합니다.

* * *

<h2 id="Number_isNaN">isNaN() : NaN 여부</h2>

**`Number.isNaN()` 메서드는 주어진 값이 NaN인지 판별합니다.**  

(NaN === NaN)의 체크 결과가 false로 반환되는 문제가 있어  
ES5에서 글로벌 오브젝트 isNaN()을 추가했지만 이 또한 문제가 많아  
`ES6`에서 `Number.isNaN()` 이 새로 추가되었습니다.

기존부터 존재한 전역 isNaN() 함수의 더 엄격한 버전입니다.

> Number.isNaN(value)

파라미터에 비교 대상 값을 지정합니다.

주어진 값이 `Number`이고 값이 `NaN`이면 `true`, 아니면 `false를` 반환합니다.

*   글로벌 오브젝트 `isNaN()` 함수와 달리, `Number.isNaN()`은 강제로 매개변수를 숫자로 변환하는 문제를 겪지 않습니다. 
이는 보통 `NaN`으로 변환됐을 값이 안전하게 전달되지만, 실제로는 `NaN`과 같은 값이 아님을 의미합니다. 이는 또한 오직 숫자형이고 또한 `NaN`인 값만이 `true`를 반환함을 뜻합니다. (엄-격)

```js isNaN
// 예를 들면 아래 예시는 global isNaN()으로 true가 됐을 것임  
Number.isNaN("NaN");      // false  
Number.isNaN(undefined);  // false  
Number.isNaN({});         // false  
Number.isNaN("blabla");   // false  
```

**NaN의 재미난? 특징은 자바스크립트에서 유일하게 자기자신과 값이 같지 않다는 것입니다.**  
`NaN === NaN; // false`

* * *

<h2 id="Number_isInteger">isInteger(): 정수 여부</h2>

**파라미터 값이 정수이면 true, 아니면 false를 반환합니다.**

```js isInteger
console.log("1:", Number.isInteger(0));// 정수  
console.log("2:", Number.isInteger(1.0));// 정수  
console.log("3:", Number.isInteger(-123));// 정수  
  
console.log("4:", Number.isInteger("12"));// false  
console.log("5:", Number.isInteger(1.02));// false  
console.log("6:", Number.isInteger(NaN));// false  
console.log("7:", Number.isInteger(true));// false  
```

파라미터 값이 `Number` 타입의 정수가 아니면 모두 `false`를 반환합니다.

* * *

<h2 id="Number_isSafeInterger">isSafeInteger(): 안정 정수 여부</h2>

```js isSafeInteger
console.log(Number.isSafeInteger(7.0)); //true  
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER));//true  
console.log(Number.isSafeInteger(Number.MIN_SAFE_INTEGER));//true  
```

정수 값 이면서 안정 정수의 범위 이면 `true`를 반환합니다.  
그 외에는 모두 `false`를 반환합니다.

* * *

<h2 id="Number_isFinite">isFinite(): 유한 값 여부</h2>

파라미터에 비교 대상 값을 지정합니다.  
**그 값이 유한 값이면 true, 아니면 false를 반환합니다.**

글로벌 오브젝트의 isFinite()와 값 차이가 있으므로 주의합니다.

```js isFinite
console.log("1:", Number.isFinite(Infinity), isFinite(Infinity));  
console.log("2:", Number.isFinite(-Infinity), isFinite(-Infinity));  
console.log("3:", Number.isFinite(0), isFinite(0));  
  
///////////////////////////////////////  
console.log("4:", Number.isFinite("0"), isFinite("0"));  
  
console.log("5:", Number.isFinite(null), isFinite(null));  
console.log("6:", Number.isFinite(NaN), isFinite(NaN));  
  
console.log("7:", Number.isFinite(undefined), isFinite(undefined));  
console.log("8:", Number.isFinite(true), isFinite(true));  
/*  
1: false false  
2: false false  
3: true true  
//////////////////  
4: false true  
5: false true  
6: false false  
7: false false  
8: false true  
*/  
```
