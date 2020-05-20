---
title: 'Array.prototype. map, filter, reduce 구현하기'
disqusId: tunas-blog-1
tags:
  - Array.prototype.map
  - Array.prototype.filter
  - Array.prototype.reduce
  - 메소드 만들기
  - 메소드 구현
  - JavaScript
  - ECMAScript6

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
date: 2020-05-19 21:53:01
categories: ECMAScript6
---

`React` 입문전에 `Array` 메소드 `map`, `filter`, `reduce`를 확실히 알고 
구현해 만들어보는 것이 도움이 된다길래 한번 만들어 봤습니다.

* [Array.prototype.map](/2020/05/19/Array-prototype-map-filter-reduce-구현하기/#map)
  * map 원리
  * map 메소드 만들어보기

* [Array.prototype.filter](/2020/05/19/Array-prototype-map-filter-reduce-구현하기/#filter)
  * filter 원리
  * filter 메소드 만들어보기

* [Array.prototype.reduce](/2020/05/19/Array-prototype-map-filter-reduce-구현하기/#reduce)
  * reduce 원리
  * reduce 메소드 만들어보기

<!-- more -->

------
<h2 id="map">Array.prototype.map</h2>

------
### map 원리

* `map()` 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 실행합니다.


* 결과를 모아 생성된 새로운 배열을 반환합니다. (**원본값 수정X**)


> Array.prototype.map(callback,thisArg)
callback : currentValue, index, array


* `map` 메서드는 `callback` 함수와 `thisArg` 두가지 인자를 받고
  (thisArg는 callback을 실행할 때 this로 사용하는 값입니다.) **지정해 주지 않을시 `this`는 전역객체**


* `callback` 함수는 아래 3가지 인자를 받습니다.
  
  * **currentValue**
    처리할 현재 요소.
  * **index** (Optional)
    처리할 현재 요소의 인덱스.
  * **array** (Optional)
    map()을 호출한 배열.


* **반환 값**
  각 요소에 함수를 호출하여 반환된 새로운 배열

------
### map 만들어보기

```js map 메서드 구현
"use strict"
debugger;

const a = ["a", "b", "c"];

/// 기존 map
const result = a.map(x => x + 1);
console.log(result); // [ 'a1', 'b1', 'c1' ]

/// map1 메소드 생성
Array.prototype.map1 = function(callback,thisArg){
  let mapping = [];
  for (var i = 0; i < this.length; i ++) {
    var mappedValue = callback.call(thisArg || Window, this[i], i, this);
    mapping[i] = mappedValue;
  }
  return mapping;
};

const result1 = a.map1(x => x + 1);
console.log(result1); // [ 'a1', 'b1', 'c1' ]
```

------
<h2 id="filter">Array.prototype.filter</h2>

------
### filter 원리

* `filter()` 메서드는 주어진 함수의 테스트를 통과(`true`)하는 모든 요소를 모아 **새로운 배열로 반환합니다**.


* `false`를 반환하는 요소는 버립니다.


>Array.prototype.filter(callback, thisArg)
callback : currentValue, index, array


* `filter` 메서드는 `callback`, `thisArg` 두가지 인자를 받고
  (thisArg는 callback을 실행할 때 this로 사용하는 값입니다.) **지정해 주지 않을시 `this`는 전역객체**


* `callback` 함수는 아래 3가지 인자를 받습니다.
  
  * **currentValue**
  처리할 현재 요소.

  * **index** (Optional)
  처리할 현재 요소의 인덱스.
  
  * **array** (Optional)
  filter()를 호출한 배열.


* **반환 값**
  각 요소에 함수를 테스트하여 true를 반환한 요소만 모은 새 배열

------
### filter 만들어보기


```js filter 메서드 구현
"use strict"
debugger;

const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

// 기본 filter 사용
const result = words.filter(word => word.length > 6);
console.log(result); //[ 'exuberant', 'destruction', 'present' ]

// filter1 메서드 생성하여 사용
Array.prototype.filter1 = function(callback, thisArg){
  let filtermapping = [];
  for (var i = 0; i < this.length; i++){
    let filterValue = callback.call(thisArg || Window, this[i], i, this);
      if (filterValue === true){
        filtermapping.push(this[i])
      }      
  }
  return filtermapping;
};

const result1 = words.filter1(word => word.length > 6);
console.log(result1);//[ 'exuberant', 'destruction', 'present' ]
```

------
<h2 id="reduce">Array.prototype.reduce</h2>

------
### reduce 원리

* `reduce()` 메서드는 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 하나의 결과값을 반환합니다.


>Array.prototype.reduce(callback, initialValue)
callback : accumulator, currentValue, currentIndex, array

* `callback`
  배열의 각 요소에 대해 실행할 함수. 다음 네 가지 인수를 받습니다.
  
  * **accumulator**
    누산기accmulator는 **콜백의 반환값을 누적합니다**. 콜백의 이전 반환값 또는, 콜백의 첫 번째 호출이면서 `initialValue`를 제공한 경우에는  `initialValue`의 값입니다.
  
  * **currentValue**
    처리할 현재 요소.
  
  * **currentIndex** (Optional)
    처리할 현재 요소의 인덱스. `initialValue`를 제공한 경우 0, 아니면 1부터 시작합니다.
  
  * **array** (Optional)
    reduce()를 호출한 배열.


* `initialValue`
  callback의 최초 호출에서 첫 번째 인수에 제공하는 값. **초기값을 제공하지 않으면 배열의 첫 번째 요소**를 사용합니다. 
  **빈 배열에서 초기값 없이 reduce()를 호출하면 오류가 발생합니다.**


* **반환 값**
  누적 계산의 결과 값.

------
### reduce 만들어보기

```js reduce 메서드 구현
"use strict"
debugger;

const array1 = [1, 2, 3, 4, 7];
const emptyArray = [];

const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 기존 reduce
console.log(array1.reduce(reducer, 5)); //22
console.log(array1.reduce(reducer)); //17
// 빈 배열에서 초기값 없이 reduce()를 호출하면 오류
console.log(emptyArray.reduce(reducer));
//TypeError: Reduce of empty array with no initial value


// reduce1 메서드 생성하여 사용
Array.prototype.reduce1 = function(callback,initialValue){
  let reduceValue = null;

  if(initialValue === undefined){
    if(this.length === 0){
      throw new TypeError("Reduce of empty array with no initial value")
    }
    reduceValue += callback(this[0], this[1], this)
    for (var i = 2; i < this.length; i ++){
      reduceValue =+ callback(reduceValue, this[i], this)
    }
  }else{
    reduceValue += callback(initialValue, this[0], this)
    for (var i = 1; i < this.length; i ++){
      reduceValue =+ callback(reduceValue, this[i], this)
    }
  }
  return reduceValue;
}

console.log(array1.reduce1(reducer, 5)); //22
console.log(array1.reduce1(reducer)); //17
console.log(emptyArray.reduce1(reducer));
//TypeError: Reduce of empty array with no initial value
```