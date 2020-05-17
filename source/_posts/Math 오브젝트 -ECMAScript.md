---
title: Math 오브젝트 -ECMAScript
date: 2020-03-29 05:11:55
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
- Math
- Math Object
- BigInt
- Number
- math method
- math.e
- math.LN2
- math.LN10
- math.LOG2E
- math.LOG10E
- math.PI
- math.SQRT2
- math.SQRT1_2
- math.abs
- math.acosh
- math.asin
- math.asinh
- math.atan
- math.atanh
- math.atan2
- math.cbrt
- math.ceil
- math.clz32
- math.cos
- math.cosh
- math.exp
- math.expm1
- math.floor
- math.fround
- math.hypot
- math.imul
- math.log
- math.log1p
- math.log10
- math.log2
- math.max
- math.min
- math.pow
- math.random
- math.round
- math.sign
- math.sin
- math.sinh
- math.sqrt
- math.tan
- math.tanh
- math.toSource
- math.trunc

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

**`Math`는 수학적인 상수와 함수를 위한 속성과 메서드를 가진 내장 객체입니다. 함수 객체가 아닙니다.**

`Math`는 `Number` 자료형만 지원하며 [BigInt](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/BigInt)와는 사용할 수 없습니다.

`BigInt`는 `Number` 원시 값이 안정적으로 나타낼 수 있는 최대치인 253 - 1보다 큰 정수를 표현할 수 있는 내장 객체입니다.

다른 전역 객체와 달리 `Math`는 생성자가 아닙니다. **`Math`의 모든 속성과 메서드는 정적입니다.**

<!-- more -->

* * *

## 속성

*   `math.E`  
    오일러의 상수이며 자연로그의 밑. 약 2.718.


*   `math.LN2`  
    2의 자연로그. 약 0.693.


*   `math.LN10`  
    10의 자연로그. 약 2.303.


*   `math.LOG2E`  
    밑이 2인 로그 E. 약 1.443.


*   `math.LOG10E`  
    밑이 10인 로그 E. 약 0.434.


*   `math.PI`  
    원의 둘레와 지름의 비율. 약 3.14159.


*   `math.SQRT1_2`  
    ½의 제곱근. 약 0.707.


*   `math.SQRT2`  
    2의 제곱근. 약 1.414.

* * *

## 메서드

*   참고: 삼각 함수(`sin()`, `cos()`, `tan()`, `asin()`, `*acos()`, `atan()`, `atan2()`)는 매개변수와 반환값 모두 호도법(라디안)을 사용합니다. 

    라디안 값을 각도 값으로 변환하려면 (Math.PI / 180)으로 나누세요. 반대로 각도 값에 곱하면 라디안 값이 됩니다.

*   참고: 많은 수의 `Math` 함수 정확도는 구현에 따라 다를 수 있습니다. 즉, 각 브라우저의 결과가 다를 수 있으며, 서로 같은 `JS` 엔진이라도 운영체제나 아키텍쳐에 따라서 불일치하는 값을 반환할 수 있습니다.
    

*   `math.abs(x)`  
    숫자의 절댓값을 반환합니다.
    

*   `math.acos(x)`  
    숫자의 아크코사인 값을 반환합니다.


*   `math.acosh(x)`  
    숫자의 쌍곡아크코사인 값을 반환합니다.


*   `math.asin(x)`  
    숫자의 아크사인 값을 반환합니다.


*   `math.asinh(x)`  
    숫자의 쌍곡아크사인 값을 반환합니다.


*   `math.atan(x)`  
    숫자의 아크탄젠트 값을 반환합니다.


*   `math.atanh(x)`  
    숫자의 쌍곡아크탄젠트 값을 반환합니다.


*   `math.atan2(y, x)`  
    인수 몫의 아크탄젠트 값을 반환합니다.


*   `math.cbrt(x)`  
    숫자의 세제곱근을 반환합니다.


*   `math.ceil(x)`  
    인수보다 크거나 같은 수 중에서 가장 작은 정수를 반환합니다.


*   `math.clz32(x)`  
    주어진 32비트 정수의 선행 0 개수를 반환합니다.


*   `math.cos(x)`  
    숫자의 코사인 값을 반환합니다.


*   `math.cosh(x)`  
    숫자의 쌍곡코사인 값을 반환합니다.


*   `math.exp(x)`  
    Ex 를 반환합니다. x는 인수이며 E 는 오일러 상수(2.718…) 또는 자연로그의 밑입니다.


*   `math.expm1(x)`  
    exp(x)에서 1을 뺀 값을 반환합니다.


*   `math.floor(x)`  
    인수보다 작거나 같은 수 중에서 가장 큰 정수를 반환합니다.


*   `math.fround(x)`  
    인수의 가장 가까운 단일 정밀도 표현을 반환합니다. 32비트 유동 소수 값


*   `math.hypot([x[, y[, …]]])`  
    인수의 제곱합의 제곱근을 반환합니다.


*   `math.imul(x, y)`  
    두 32비트 정수의 곱을 반환합니다.


*   `math.log(x)`  
    숫자의 자연로그(loge 또는 ln) 값을 반환합니다.


*   `math.log1p(x)`  
    숫자 x에 대해 1 + x의 자연로그(loge 또는 ln) 값을 반환합니다.


*   `math.log10(x)`  
    숫자의 밑이 10인 로그를 반환합니다.


*   `math.log2(x)`  
    숫자의 밑이 2인 로그를 반환합니다.


*   `math.max([x[, y[, …]]])`  
    0개 이상의 인수에서 제일 큰 수를 반환합니다.


*   `math.min([x[, y[, …]]])`  
    0개 이상의 인수에서 제일 작은 수를 반환합니다.


*   `math.pow(x, y)`  
    x의 y 제곱을 반환합니다.


*   `math.random()`  
    0과 1 사이의 난수를 반환합니다.


*   `math.round(x)`  
    숫자에서 가장 가까운 정수를 반환합니다.


*   `math.sign(x)`  
    x의 양의 수인지 음의 수인지 나타내는 부호를 반환합니다.


*   `math.sin(x)`  
    숫자의 사인 값을 반환합니다.


*   `math.sinh(x)`  
    숫자의 쌍곡사인 값을 반환합니다.


*   `math.sqrt(x)`  
    숫자의 제곱근을 반환합니다.


*   `math.tan(x)`  
    숫자의 탄젠트 값을 반환합니다.


*   `math.tanh(x)`  
    숫자의 쌍곡탄젠트 값을 반환합니다.


*   `math.toSource()`  
    문자열 “Math”를 반환합니다.


*   `math.trunc(x)`  
    숫자의 정수 부분을 반환합니다.