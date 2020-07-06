---
title: 전개(Spread) 연산자 -ECMAScript
date: 2020-03-20 08:49:05

categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - Spread
  - spread
  - iterable
  - 유사 배열
  - Array-like
  - rest
  - 전개 연산자

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


* * *

## 개요

전개(`Spread`) 연산자는 **이터러블 오브젝트의 엘리먼트를 하나씩 분리하여 전개합니다.** <mark>(복사)</mark>

<mark>객체(혹은 배열)를 가리키는 것이 아닌 내부의 값을 복사하여 새로운 객체(혹은 배열)를 만들어 내기 때문에
불변성을 지켜줘야하는 곳에 자주 사용됩니다.</mark>

전개한 결과를 변수에 할당하거나 호출하는 함수의 파라미터 값으로 사용할 수 있습니다.

* 기본형

> [...iterableObject]

* function에서

> function(...iterableObject);

* array에서

> [...iterableObj, ‘4’, ‘five’, 6];

* object 에서

> let objClone = { ...obj };  
> //ECMAScript 2018에서 추가되었습니다.

`spread` 연산자는 `“...”`을 작성하고 뒤에 이터러블 오브젝트를 작성합니다.

<!-- more -->

* * *

### 1. 배열

**`[]` 대괄호 안에 `spread` 연산자로 배열을 작성한 형태.**

```js
let one = [11, 12];  
let two = [21, 22];  
let spreadObj = [51, ...one, 52, ...two];  
  
1. console.log(spreadObj);   
/*   
0: 51  
1: 11  
2: 12  
3: 52  
4: 21  
5: 22  
= [51, 11, 12, 52, 21, 22]  
*/  
2. console.log(spreadObj.length); // 6  
```

1.  `one` 배열의 엘리멘트 `[11, 12]`에서 11과 12가 분리된 후,  
    `spreadObj = [51, ...one, 52, ...two];`에 `...one`위치에 설정됩니다.  
    `two` 배열역시 같은방식으로 `spreadObj`에 설정되고  
    [51, 11, 12, 52, 21, 22] 형태로 `spreadObj`에 할당됩니다.


2.  `spreadObj`의 `length` 값은 6이 됩니다.  
    (one 과 two 배열의 엘리멘트가 분리되어 할당되었기 때문)  
    엘리멘트가 분리되지 않고 할당되었다면 length 값은 4가 돼야 합니다.

* * *

### 2. 문자열

**`[]` 대괄호 안에 `spread` 연산자로 문자열을 작성한 형태.**

```js
let spreadObj = [..."music"];  
console.log(spreadObj);  
/*  
0: "m"  
1: "u"  
2: "s"  
3: "i"  
4: "c"  
= ["m", "u", "s", "i", "c"]  
*/  
console.log(spreadObj.length); // 5  
```

* * *

### 3. 함수 파라미터

호출하는 함수의 파라미터 값을 `spread` 연산자로 작성하면,  
**함수를 호출하기 전에 파라미터 값을 분리, 전개합니다.** 
호출받는 함수의 파라미터에 이름을 작성하면 각 엘리먼트 값이 파라미터 이름에 설정됩니다.

```js
const values = [10, 20, 30];  
get(...values); // 10, 20, 30 으로 각각 분리됩니다.  
  
function get(one, two, three){  
 /* 호출받은 함수 파라미터에  
 one: 10  
 two: 20  
 three: 30 이 설정됩니다.  
 */  
 console.log( one + two + three); // 60  
};  
```

함수의 파라미터 값이 분리된 형태를 `spread 파라미터` 라고 합니다.

* * *

## rest 파라미터

`function(...rest)`와 같이 함수 파라미터에 `spread`연산자로 파라미터를 작성한 형태를 `rest`파라미터 라고 합니다.

> function(param, paramN, ...rest);

* * *

호출하는 함수의 파라미터에 3개의 파라미터 값을 작성하고 호출받는 함수의 파라미터에 파라미터 이름을 하나만 작성하면  

나머지 2개의 파라미터 값이 설정되지 않습니다.

------
### rest 미사용 

```js ...rest 미사용
let get = (one) => {  
 console.log(one);  
}  
get(...[1, 2, 3]); // 1  
```

* `get(...[1, 2, 3])`이 `get(1, 2, 3)`형태로 전개되고 파라미터 `one`에 1 값이 설정됨.
호출받는 `get()`함수가 화살표 함수이므로 
호출한 `get(...[1, 2, 3])` 함수에서 보낸 파라미터가 `arguments`에 설정되지 않음.  
따라서 [1, 2, 3]에서 2,3을 받지 못하게 됩니다.

------
### rest 사용시

```js ...rest 사용시
let get = (...numRest) => {  
 console.log(numRest); // [1, 2, 3]  
 console.log(Array.isArray(numRest));// 배열 인지 확인해주는 메서드 true  
}  
get(...[1, 2, 3]);  
```

**rest 파라미터는 get(1, 2, 3) 형태로 호출된 파라미터 값을 배열의 엘리멘트로 설정합니다.(함수 안에서 numRest에 설정된 값을 배열로 사용할 수 있습니다.)**

------
### 한개의 값만 설정하고 rest 사용시

```js 한개의 값만 설정 하고 rest 사용시
let get = (one, ...rest) => {  
 console.log(one); // 1  
 console.log(rest); // [2, 3]  
}  
get(...[1, 2, 3]);  
```

* `get(1, 2, 3)` 형태로 호출되어 파라미터 `one`에 1값 설정됨.  
나머지 `2,3`은 `rest`파라미터에 `[2, 3]`형태로 설정됩니다.

* * *

## spread와 rest 파라미터 구분

`(...)형태는 같지만 기능이 다르므로 구분해야 합니다.`

*   spread 파라미터는  
    <u>호출하는 함수의 파라미터</u>에 사용하며 Array( 배열[] )를 엘리먼트 단위로 전개합니다.
    
*   rest 파라미터는  
    <u>호출받는 함수의 파라미터</u>에 사용합니다.  
    **호출하는 함수의 파라미터 순서에 맞춰 호출받는 파라미터에 값을 설정합니다.**  
    설정되지 못하고 남은 파라미터 값은 배열의 엘리먼트로 설정합니다.
    

`spread는 배열을 엘리먼트로 분리,전개. rest는 전개된 엘리먼트를 다시 배열에 설정.`

* * *

## Array-like (유사배열)

배열은 아니지만 배열처럼 사용할 수 있는 `Object`를 `Array-like`(혹은 유사배열 객체)라고 합니다.

배열은 인덱스(`index`)를 갖고 있어 인덱스 순서대로 읽을 수 있으며,  
인덱스 번호를 이용해 엘리먼트 값을 수정, 삭제할 수 있습니다.

*   `Object`의 `{key:values}`형태를 배열도 `object`라는 점을 이용한  
    `Array`(배열) 처럼 사용 하는 트릭입니다.
    

*   `Object`의 프로퍼티 `“key”` 를 `Array`의 `index` 처럼 사용하고  
    `value[key]` 형태로 프로퍼티를 읽어올 수 있습니다.
    

*   `Object`의 `value`(프로퍼티 값)은 `Array`의 엘리먼트 처럼 사용합니다.
    
```js
let values = {  
// {key:value}  
 0: "zero",  
 1: "one",   
 2: "two",  
  
// {key:value} 형태에 맞추어 object전체 수 length 값을 작성합니다.  
// 이 형태가 Array-like (유사 배열)입니다.   
 length: 3};  
  
for (var key in values){ //  for-in  
 console.log(key, ':', values[key]);  
};  
  
for (var k = 0; k < values.length; k++){ // for()  
 console.log(values[k]);  
};  
/*실행결과  
--------------  
for-in  
0 : zero  
1 : one  
2 : two  
length : 3  
--------------  
for()  
zero  
one  
two  
*/  
```

*   `for-in` 문은 `Object` 오브젝트를 전개할 때 사용합니다.  
    `for-in` 문으로 `values` 오브젝트를 전개하면 `length` 프로퍼티도 같이 전개됩니다.  
    즉, `for-in`문으로 `Array-like`를 전개한다면 `length` 프로퍼티를 제외시키는 처리가 필요합니다.
    

*   **배열을 전개할 때는 주로 for()문을 사용합니다.**  
    `for()`문으로 `values` 오브젝트를 전개해도 `length`프로퍼티가 전개되지 않습니다.
    

* * *

### 유사배열은 배열 메서드를 사용할 수 없습니다.

일반적으로 유사배열은 배열이 아니므로 배열 메서드를 사용할 수 없습니다.  
이럴 때 메서드를 빌려와 유사배열에도 배열 메서드를 사용할 수 있는 방법은

<mark>apply, call, form등 을 이용해 Array 메소드를 가져와 쓰는 경우가 있습니다.</mark>
