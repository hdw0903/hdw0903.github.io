---
title: Array 오브젝트 -ECMAScript
date: 2020-03-30 07:55:37
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
- Array Object
- Array method
- iterator
- 이터러블 오브젝트
- Array. from
- Array. of
- 제네릭 함수
- Array. copyWithin
- Array. fill
- Array. entries
- Array. keys
- Array. values
- Array. find
- Array. findIndex
- Array - like
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

`ES6`에서 `Array`오브젝트에 9개의 메서드가 추가되었습니다.  
추가된 메서드를 살펴봅니다.

*   Array 오브젝트
    *   [from(): Array 오브젝트 생성](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_from)
    *   [of(): 배열 엘리먼트 설정](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_of)
    *   [copyWithin(): 범위 값 복사, 설정](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_copyWithin)
    *   [fill(): 범위 값 변경](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_fill)
    *   [entries(): 이터레이터 오브젝트 생성](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_enteries)
    *   [keys(): key 이터레이터 오브젝트 생성](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_keys)
    *   [values(): value 이터레이터 오브젝트 생성](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_values)
    *   [find(): 엘리먼트 값 비교, 반환](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_find)
    *   [findIndex(): 배열 인덱스 반환](/2020/03/30/Array%20오브젝트%20-ECMAScript/#Array_findeIndex)

<!-- more -->

* * *

<h2 id="Array_from">from(): Array 오브젝트 생성</h2>

**새로운 `Array` 오브젝트를 생성하고 콜백 함수에서 반환된 값을 엘리먼트 값으로 설정하여 새로운 `Array` 객체를 반환합니다.**

> Array.from(arrayLike[, mapFn[, thisArg]])

*   arrayLike  
    배열로 변환하고자 하는 유사 배열 객체(Array-like)나 반복 가능한 객체(이터러블 오브젝트).
    
*   mapFn (선택적 파라미터)  
    배열의 모든 엘리먼트 마다 호출할 함수.
    
*   thisArg (선택적 파라미터)  
    두 번째 파라미터 함수 실행 시에 this로 참조할 값.
    
*   반환 값  
    새로운 Array 인스턴스.
    
------
### 중요 포인트

다음과 같은 경우에 `Array.from()`으로 새 `Array`를 만들 수 있습니다.

*   유사 배열 객체 (length 속성과 인덱싱된 요소를 가진 객체)
*   순회 가능한 객체 (Map, Set 등 객체의 요소를 얻을 수 있는 객체)

`Array.from()`은 선택 매개변수인 `mapFn`를 가지는데,배열(혹은 배열 서브클래스)의 각 요소를 맵핑할 때 사용할 수 있습니다. 즉, `Array.from`(obj, mapFn, thisArg)는 중간에 다른 배열을 생성하지 않는다는 점을 제외하면 `Array.from(obj).map`(mapFn, thisArg)와 같습니다. 

**이 특징은 `typed arrays`와 같은 특정 배열 서브클래스에서 중간 배열 값이 적절한 유형에 맞게 생략되기 때문에 특히 중요합니다.**

`from()` 메서드의 length 속성은 1입니다.

`ES2015` 이후, **클래스 구문은 내장 및 새 클래스의 상속을 가능케 했습니다.** 
그 결과로 `Array.from`과 같은 정적 메서드는 `Array`의 서브클래스에 의해 상속되며, `Array` 대신 자신의 인스턴스를 만듭니다.

```js 예제1
1. let arrayObj = Array.from({0: "zero", 1: "one", length: 2});  
console.log(Array.isArray(arrayObj));  
console.log(arrayObj);  
  
2. let stringObj = Array.from("ABC");  
console.log(stringObj);  
  
// true  
// ["zero","one"]  
// ["A", "B", "C"]  
```

1.  `Array.from()` 첫 번째 파라미터에 `Array-like` 오브젝트를 작성했습니다.  
    새로운 `Array` 오브젝트를 생성하고 `Array-like` 오브젝트의 프로퍼티 값을 배열에 추가하여 반환합니다. `{0:”zero”, 1:”one”, length: 2}`에서 프로퍼티 키와 `length` 프로퍼티를 제외한 `“zero”` 와 `“one”`이 생성된 `Array` 오브젝트에 추가됩니다.

2.  `Array.from()` 파라미터 `“ABC”` 를 생성한 `Array` 오브젝트 배열의 엘리먼트에 하나씩 설정하여 반환합니다. 즉 `[“A”, “B”, “C”]` 형태로 반환합니다.

```js 예제2
let arrayLike = {0: 10, 1: 30, length: 2};  
let values = Array.from(arrayLike, function(value) {  
 return value + this.bonus;  
}, {bonus: 100});  
console.log(values);  
// [110, 130]  
```

`Array-like` 오브젝트의 프로퍼티를 하나씩 읽어 갑니다. 프로퍼티를 읽을 때 마다 콜백 함수를 호출 합니다.  
읽은 프로퍼티 값이 콜백 함수의 `value` 파라미터에 설정됩니다.  
콜백 함수에서 `this`로 `from()`의 세 번째 파라미터에 지정한 오브젝트를 참조할 수 있습니다. 
`Array-like` 오브젝트의 `length` 프로퍼티는 참조되지 않습니다.

1.  첫 번째 파라미터의 첫 번째 프로퍼티 값은 10 이며  
    두 번째 파라미터 콜백 함수의 `value` 파라미터에 설정됩니다.  
    `this.bonus`는 `bonus` 값을 참조합니다.

2.  `value + this.bonus`는 110이 반환되고 생성된 `Array` 배열에 추가됩니다.

3.  두 번째 프로퍼티 값인 30도 같은 방법으로 처리되고 최종적으로 생성된 배열을 반환합니다.

------
### 그외 예제

*   `Set`에서 배열 만들기
```js    
const s = new Set([‘foo’, window]);  
Array.from(s);  
// [“foo”, window]
```

*   `Map`에서 배열 만들기
```js    
const m = new Map([[1, 2], [2, 4], [4, 8]]);  
Array.from(m);  
// [[1, 2], [2, 4], [4, 8]]

const mapper = new Map([[‘1’, ‘a’], [‘2’, ‘b’]]);  
Array.from(mapper.values());  
// [‘a’, ‘b’];

Array.from(mapper.keys());  
// [‘1’, ‘2’];
```
    
*   `배열 형태를 가진 객체`(arguments)에서 배열 만들기
```js    
function f() {  
return Array.from(arguments);  
}  
f(1, 2, 3);  
// [1, 2, 3]
```

*   `Array.from`과 화살표 함수 사용하기
 ```js    
Array.from([1, 2, 3], x => x + x);  
// [2, 4, 6]

// 숫자생성  
// Array는 초기화 될때 각 위치마다 ‘undefined’값으로 초기화 됩니다.  
// 아래의 v 의 value 값은 undefined 가 될 것입니다.  
Array.from({length: 5}, (v, i) => i);  
// [0, 1, 2, 3, 4]
```

------
<h2 id="Array_of">of(): 배열 엘리먼트 설정</h2>

**파라미터 값을 새로운 배열의 엘리먼트로 설정하여 반환합니다.**

> Array.of(element0[, element1[, …[, elementN]]])

*   매개변수  
    elementN  
    배열을 생성할 때 사용할 엘리먼트.
    
*   반환 값  
    새로운 Array 객체.
    

```js
let arrayObj = Array.of(1, 2, 3);  
console.log(arrayObj);  
/// [1, 2, 3]  
```

`Array.of()` 파라미터에 새로운 배열의 엘리먼트에 설정할 값을 작성합니다. (콤마로 구분하여 다수를 작성할 수 있습니다.)
`Array.of()`가 호출되면 우선 `Array` 오브젝트를 생성합니다. 이어서 파라미터에 작성한 순서대로 `Array` 오브젝트에 추가한 후, 반환합니다.

*   `Array.from()`은 파라미터에 `Array-like` 또는 `이터러블 오브젝트`를 지정하지만,  
    `Array.of()`는 파라미터에 값을 지정합니다.


*   `Array.of()`와 `Array` **생성자의 차이는 정수형 인자의 처리 방법에 있습니다.** 
    `Array.of(7)`은 하나의 요소 `7`을 가진 배열을 생성하지만 `Array(7)`은 `length` 속성이 `7`인 `빈 배열`을 생성합니다.

```js Array.of
Array.of(7);       // [7]   
Array.of(1, 2, 3); // [1, 2, 3]  
  
Array(7);          // [ , , , , , , ]  
Array(1, 2, 3);    // [1, 2, 3]  
```

* * *

<h2 id="copyWithin">copyWithin(): 범위 값 복사, 설정</h2>

`copyWithin()` <u>메서드는 배열의 일부를 인덱스 범위의 값을 복사하여,</u>
**동일한 배열의 지정한 위치에 덮어쓰고 그 배열을 반환합니다. 이 때, 배열의 길이를 수정하지 않고 반환합니다.**

> Array.copyWithin(target[, start[, end]])

*   target  
    **복사한 값을 설정할 시작 인덱스**. 음수를 지정하면 인덱스를 배열의 끝에서부터 계산합니다.  
    `target`이 `arr.length`보다 크거나 같으면 아무것도 복사하지 않습니다.  
    `target`이 `start` 이후라면 복사한 시퀀스를 `arr.length`에 맞춰 자릅니다.


*   start (선택적 파라미터)  
    복사를 시작할 위치를 가리키는 0 기반 인덱스. 
    음수를 지정하면 인덱스를 배열의 끝에서부터 계산합니다.  
    기본값은 0으로, **start를 지정하지 않으면 배열의 처음부터 복사합니다.**


*   end (선택적 파라미터)  
    복사를 끝낼 위치를 가리키는 0 기반 인덱스.  
    `copyWithin`은 **`end` 인덱스 이전까지 복사하므로 `end` 인덱스가 가리키는 요소는 제외합니다**. 음수를 지정하면 인덱스를 배열의 끝에서부터 계산합니다.  
    기본값은 `arr.length`로, **`end`를 지정하지 않으면 배열의 끝까지 복사합니다.**


*   반환 값  
    수정한 배열.

### 중요 포인트

`copyWithin`은 `C`와 `C++`의 `memmove`처럼 작동하고, **복사와 대입이 하나의 연산에서 이루어지므로** `Array`의 데이터를 이동할 때 사용할 수 있는 고성능 메서드입니다. `TypedArray`의 동명 메서드에서 이 특징이 두드러집니다. 
붙여넣은 시퀀스의 위치가 복사한 범위와 겹치더라도 최종 결과는 원본 배열에서 복사한 것과 같습니다.

`copyWithin` 함수는 [제네릭 함수](https://heecheolman.tistory.com/67)로, `this` 값이 `Array` 객체일 필요는 없습니다.

`copyWithin` 메서드는 변경자 메서드로, `this`의 길이는 바꾸지 않지만 내용을 바꾸며 필요하다면 새로운 속성을 생성합니다.

```js
1. let one = [1, 2, 3, 4, 5];  
console.log(one.copyWithin(0, 3));  
  
2. let two = [1, 2, 3, 4, 5];  
console.log(two.copyWithin(0, 2, 4));  
  
3. let three = [1, 2, 3, 4, 5];  
console.log(three.copyWithin(3));  
// [4, 5, 3, 4, 5]  
// [3, 4, 3, 4, 5]  
// [1, 2, 3, 1, 2]  
```

1.  `copyWithin()` 두 번째 파라미터에 지정한 인덱스 3부터 배열의 끝까지 엘리먼트 값을 복사하여 
첫 번째 파라미터 값인 인덱스 0부터 차례대로 설정합니다. 
`4`와 `5`를 `인덱스 0`부터 설정하므로 [1, 2]가 [4, 5]로 대체되어 `[4, 5, 3, 4, 5]`가 됩니다.


2.  두 번째 파라미터 값인 인덱스 2부터 세 번째 파라미터 값인 인덱스 4 직전까지 엘리먼트 값을 복사한 첫 번째 파라미터 값인 인덱스 0부터 차례대로 설정합니다. `3`과 `4`를 `인덱스 0`부터 설정하므로 [1, 2]가 [3, 4]로 대체되어 `[3, 4, 3, 4, 5]`가 됩니다.


3.  **두 번째와 세 번째 파라미터를 작성하지 않았으므로 배열 전체를 복사**하여 인덱스 3부터 설정합니다. 
    **복사할 엘리먼트 수는 5이지만, 설정할 수 있는 엘리먼트의 수는 두 개 입니다.** 
`[4, 5]`에 `[1, 2]`가 설정되고 나머지 `[3, 4, 5]`는 설정되지 않습니다. 즉 [1, 2, 3, 1, 2]가 출력됩니다.

------
### Array - like

```js Array-like
let arrayLike = {0: "ABC", 1: "DEF", 2: "가나다", length: 3};  
1. let one = Array.prototype.copyWithin.call(arrayLike, 0, 1);  
console.log(one);  
  
2. function two() {  
 return Array.prototype.copyWithin.call(arguments, 3, 0, 2);  
};  
console.log(two(1, 2, 3, 4, 5));  
/*  
Object  
0: "DEF"  
1: "가나다"  
2: "가나다"  
length: 3  
  
Arguments(5)  
callee: (...)  
0: 1  
1: 2  
2: 3  
3: 1  
4: 2  
length: 5  
*/  
```

`Array-like` 는 배열이 아닌 오브젝트이므로 `Array.copyWithin()`형태로 호출할 수 없습니다. 하지만 위와 같이 `call()`을 호출하면서 첫 번째 파라미터에 `Array-like`를 지정하면 `copyWithin()`이 호출됩니다.

1.  `arrayLike` 오브젝트의 프로퍼티 키인 0,1,2를 배열의 인덱스로 사용합니다.  
    세 번째 파라미터인 인덱스 1 부터 끝까지 복사하여 `[“DEF”,”가나다”]`가
    인덱스 0,1 값과 대체됩니다. {0: “DEF”, 1: “가나다”, 2: “가나다”, length: 3}


2.  호출한 함수에서 넘겨준 파라미터 값이 `arguments`에 설정됩니다. `arguments`가 `Array-like` 오브젝트이므로 `call()`의 첫 번째 파라미터에 지정하면 `copyWithin()`을 호출할 수 있습니다. 인덱스 0 부터 인덱스 2 이전까지 복사합니다 [1, 2]  
    이를 인덱스 3부터 설정하여 인덱스 3,4의 값이 대체됩니다. [1, 2, 3, 1, 2]

* * *

<h2 id="Array_fill">fill(): 범위 값 변경</h2>

**같은 배열에서 인덱스 범위의 값을 하나의 지정한 값으로 바꾸어 반환합니다.**

> array.fill(value[, start[, end]])

*   value  
    배열을 채울 값.
    
*   start (선택적 파라미터)  
    시작 인덱스, 기본 값은 0.
    
*   end (선택적 파라미터)  
    범위 끝 인덱스, 기본 값은 this.length.
    
*   반환 값  
    변형한 배열.
    

### 중요 포인트

*   `start`가 음수이면 시작 인덱스는 `[length + start]`입니다. `end`가 음수이면 끝 인덱스는 `[length + end]`입니다.

*   `fill`은 일반 함수이며, `this` 값이 배열 객체일 필요는 없습니다.

*   `fill` 메서드는 변경자 메서드로, **복사본이 아니라 `this` 객체를 변형해 반환합니다.** 
    `value`에 객체를 받을 경우 그 참조만 복사해서 배열을 채웁니다.

```js
let one = [1, 2, 3];  
1. console.log(one.fill(7));  
  
let two = [1, 2, 3, 4, 5];  
2. console.log(two.fill(7, 1));  
  
let three = [1, 2, 3, 4, 5];  
3. console.log(three.fill(7, 1, 3));  
// [7, 7, 7]  
// [1, 7, 7, 7, 7]  
// [1, 7, 7, 4, 5]  
```

1.  범위를 지정해 주지 않았으므로 배열 전체가 변경 대상이 됩니다. 첫 번째 파라미터 7이 변경할 값이 되어 [7, 7, 7]로 변경됩니다.


2.  두 번째 파라미터 인덱스 값1 부터 배열 끝까지가 변경 대상이 됩니다. [1, 7, 7, 7, 7]로 변경됩니다.


3.  두 번째 파라미터 인덱스 값1 부터 세 번째 파라미터 인덱스 3이전 까지가 변경 대상입니다. [2, 3]이 7로 변경되어 [1, 7, 7, 4, 5]이 됩니다.

* * *

<h2 id="Array_enteries">entries(): 이터레이터 오브젝트 생성</h2>

**Array오브젝트를 이터레이터 오브젝트로 생성하여 반환합니다.**

> Array.entries()

*   반환값 iterator

```js entries
let values = [10, 20, 30];  
// Array 오브젝트로 이터레이터 오브젝트를 생성해 반환합니다.  
1. let iterator = values.entries();  
console.log(iterator.next());  
  
2. for (var [key, value] of iterator){  
 console.log(key, ":", value);  
};  
/* Object   
{value: Array(2)  
 [ {0: 0},  
 {1: 10} ],  
 done: false}  
1: 20  
2: 30  
*/  
```

1.  `iterator` 오브젝트의 `next()`를 호출 하면 `{value: (2),done: false}` 형태를 반환합니다. **배열의 인덱스와 엘리먼트가 프로퍼티 형태로 되기 때문입니다.**  // [{0: 0}, {1: 10}]


2.  **이터레이터 오브젝트는 `for-of`문에 `[key: value]`형태로 키와 값을 동시에 작성할 수 있습니다.** 실행 결과에 “0: 10”이 출력되지 않는 것은 바로 앞의 `next()`에서 이터레이션 처리를 하였기 때문입니다. 따라서 `for-of`문은 두 번째 인덱스부터 처리되었습니다.

* * *

<h2 id="Array_keys">keys(): key 이터레이터 오브젝트 생성</h2>

**key만 갖는 이터레이터 오브젝트를 생성하여 반환합니다.**

배열의 인덱스를 `key` 값으로 사용하여 이터레이터 오브젝트를 생성합니다.  
**배열의 엘리먼트 값은 이터레이터 오브젝트에 포함되지 않습니다.**

```js
let iterator = [10, 20, 30].keys();  
for (var key of iterator){  
 console.log(key, ":", iterator[key]);  
};  
/*  
0 ":" undefined  
1 ":" undefined  
2 ":" undefined  
*/  
```

`[10, 20, 30].keys()`로 이터레이터 오브젝트를 생성하면  
**인덱스 0, 1, 2만 설정되고 엘리먼트 값 [10, 20, 30]은 설정되지 않습니다.**  
`value` 값에 `undefined`가 출력됩니다.

`for(var[key, value] of iterator){}`와 같이 `[key, value]`를 작성하면 `TypeError`가 발생하므로 `key`만 작성해야 합니다.

* * *

<h2 id="Array_values">values(): value 이터레이터 오브젝트 생성</h2>

**value만 갖는 이터레이터 오브젝트를 생성하여 반환합니다.**

배열 엘리먼트 값으로 이터레이터 오브젝트를 생성합니다.  
**배열 인덱스는 이터레이터 오브젝트에 포함되지 않습니다.**  
`Symbol.iterator()`와 같습니다.

```js
// 크롬 52~54, 파이어폭스 47~49 지원하지 않음  
let iterator = [10, 20, 30].values();  
1. console.log(iterator.next());  
2. console.log(iterator.next());  
3. console.log(iterator.next());  
4. console.log(iterator.next());  
/*  
1.Object  
value: 10  
done: false  
  
2.Object  
value: 20  
done: false  
  
3.Object  
value: 30  
done: false  
  
4.Object  
value: undefined  
done: true  
*/  
```

`for-of` 루프 반복을 사용 하려면, 브라우저가 `for-of` 루프와 `for` 루프안에 `let` 스코프 변수를 지원해야 합니다.

* * *

<h2 id="Array_find">find(): 엘리먼트 값 비교, 반환</h2>

`find()` **메서드는 주어진 콜백 함수를 만족하는(`true`값) 첫 번째 엘리먼트의 값을 반환합니다.** 그런 요소가 없다면 `undefined`를 반환합니다.

> Array.find(callback[, thisArg])

*   callback  
    배열의 각 값에 대해 실행할 함수. 아래의 세 인자를 받습니다.
    
    *   element  
        콜백함수에서 처리할 현재 엘리먼트.
        
    *   index  
        콜백함수에서 처리할 현재 엘리먼트의 인덱스.
        
    *   array  
        find 함수를 호출한 배열.
        

*   thisArg (선택적 파라미터)  
    콜백이 호출될 때 this로 사용할 객체.

*   반환 값  
    주어진 판별 함수를 만족하는 첫 번째 요소의 값. 그 외에는 undefined.

### 중요 포인트

*   `find` 메서드는 `callback` 함수가 참을 반환 할 때까지 해당 배열의 각 요소에 대해서 `callback` 함수를 실행합니다. 만약 어느 요소를 찾았다면 `find` 메서드는 해당 요소의 값을 즉시 반환하고, 그렇지 않았다면 `undefined를` 반환합니다.


*   `callback은` 0 부터 length - 1 까지 배열의 모든 인덱스에 대해 호출되며, **값이 지정되지 않은 요소도 포함하여 모든 인덱스에 대해 호출됩니다. 따라서, 희소 배열 (sparse arrays)의 경우에는 값이 지정된 요소만 탐색하는 다른 메소드에 비해 더 비효율적입니다.**


*   `thisArg` 파라미터가 주어진 경우에는 제공되었다면 `thisArg`가 `callback`안에서 `this`로 사용되고, 그렇지 않은 경우 `undefined` 가 `this`로 사용됩니다.


*   `find`는 호출의 대상이 된 배열을 변경(`mutate`)하지 않습니다.


*   `find`가 처리할 배열 요소의 범위는 첫 `callback`이 호출되기 전에 먼저 결정됩니다. 
`find`메서드가 실행 된 이후에 배열에 추가된 요소들에 대해서는 `callback`이 호출되지 않습니다. 
아직 `callback`이 호출되지 않았던 배열 요소가 `callback`에 의해서 변경된 경우, `find`가 해당 요소의 인덱스를 방문할 때의 값으로 `callback`함수에 전달될 것입니다. 즉, 삭제된 요소에도 `callback`이 호출됩니다.

```js
1. let result = [1, 2, 3].find((value, index, allData) => value === 2);  
console.log(result);  
  
2. result = [1, 2, 1].find(function(value, index, allData){  
 return value === 1 && value === this.key;  
}, {key: 1});  
console.log(result);  
//2  
//1  
```

1.  [1, 2, 3]에서 1을 읽으면 콜백 함수가 호출됩니다. 콜백 함수의 value 파라미터에 1이 설정되고 index에 0이 설정되며, `allData`에 배열 전체가 설정됩니다.  
    `value === 2`에서 `false`를 반환하므로 배열의 다음 엘리먼트로 넘어가 콜백 함수를 호출합니다. 엘리먼트 값이 2이므로 `true`가 반환됩니다.  
    **이때 find()를 종료하면서 처리 중인 엘리먼트 값 2가 반환됩니다**.


2.  `find()`의 두 번째 파라미터에 `{key: 1}`을 작성했으며 콜백 함수에서 `this`로 참조할 수 있습니다. 배열의 첫 번째 엘리먼트 값이 1이므로 콜백 함수가 `true`를 반환하고 `find()`가 종료됩니다. 엘리먼트 값 1을 반환합니다.  
    남은 엘리먼트 값 [2, 1]은 처리되지 않습니다.

* * *

<h2 id="Array_findeIndex">findIndex(): 배열 인덱스 반환</h2>

**콜백 함수에서 `true`를 반환하는 첫 번째 엘리먼트의 배열 인덱스를 반환합니다. 만족하는 요소가 없으면 -1을 반환합니다.**

> Array.findIndex(callback(element[, index[, array]])[, thisArg])

*   callback  
    3개의 인수를 취하여 배열의 각 값에 대해 실행할 함수입니다.
    
    *   element  
        배열에서 처리중인 현재 요소입니다.
        
    *   index  
        배열에서 처리중인 현재 요소의 인덱스입니다.
        
    *   array  
        findIndex 함수가 호출된 배열입니다.
        
    *   thisArg (선택적 파라미터)  
        콜백을 실행할 때 this로 사용할 객체입니다.
        

*   반환 값  
    엘리먼트가 함수에 true값을 반환하면 그 배열의 인덱스 반환.  
    그렇지 않으면 -1을 반환합니다.

```js
1. let result = [10, 20, 30].findIndex(  
 (value, index, allData) => value === 20);  
console.log(result);  
  
2. result = [10, 20, 30].findIndex((value, index, allData) => value === 77);  
console.log(result);  
  
3. result = [10, 20, 30].findIndex(function(value, index, allData){  
 return value === 30 && value === this.check;  
}, {check: 30});  
console.log(result);  
// 1  
// -1  
// 2  
```

1.  [10, 20, 30]에서 처음의 10을 읽으면 콜백 함수가 호출됩니다.  
    콜백 함수의 `value` 파라미터에 10이 설정되고 `index`에 0이 설정되며,  
    `allData`에 배열 전체가 설정됩니다. `value === 20` 에서 `false`를 반환하므로 배열의 다음 엘리먼트로 넘어가 콜백 함수를 호출 합니다.  
    엘리먼트 값이 20이므로 `true`를 반환하며 `findIndex()`를 종료하며 처리 중인 인덱스 1을 반환합니다.


2.  배열에 `true`값 77이 없으므로 배열의 엘리먼트 마지막까지 콜백 함수에서 `false`를 반환하게 되면 `findIndex()`를 종료하면서 -1을 반환합니다.


3.  `findIndex()`의 두 번째 파라미터에 `{check: 30}`을 작성했으며 콜백 함수에서 `this`로 참조할 수 있습니다.  
    배열 엘리먼트 값이 30일 때 콜백 함수에서 `true`를 반환하며 인덱스 값이 2이므로  
    최종적으로 2를 반환합니다.
