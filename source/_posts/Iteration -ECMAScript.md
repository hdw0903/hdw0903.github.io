---
title: Iteration -ECMAScript
date: 2020-03-18 12:54:51
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - Iteration
  - iterable protocol
  - iterator protocol
  - Symbol.iterator
  - 이터레이션
  - 이터레이터

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

## 1. 개요

**Iteration은 반복 처리를 나타내며 이를 위한 프로토콜(Protocol)을 갖고 있습니다.**

`protocol` 이라고 하면 통신이 연상되는데  
통신에 있어 프로토콜은 약속된 기준과 방법으로 데이터를 송수신하는 것을 의미합니다. (통신 프로토콜 = 통신규약)

`ES6`에서 프로토콜도 규약입니다.  
`Iteration`**을 위한 규약이 있으며 이를 지켜야 반복 처리가 가능합니다.**

예를 들어 자바스크립트에서 `Array`(배열)를 반복 처리 하기 위해서는  
배열이 반복할 수 있는 `Object`(오브젝트)여야 하며,  
오브젝트에 반복 처리를 할 수 있는 `method`(메서드)가 필요합니다.  
이러한 규약이 `ES6`의 `Iteration Protocol` (반복 처리 규약) 입니다.

<!-- more -->

* * *

## iterable protocol

`iterable protocol`(이터러블 프로토콜)은 **오브젝트의 반복 처리 규약을 정의** 합니다.  
**빌트인 오브젝트** `String`, `Array`, `Map`, `Set`, `TypedArray`, `Argument` 와  
**DOM**의 `NodeList` 는 기본 값으로 이터러블 프로토콜을 갖고 있습니다.

이와 같은 오브젝트는 자바스크립트 엔진이 렌더링될 때 이터러블 프로토콜이 설정되기 때문에 사전처리를 하지 않아도 반복 처리를 할 수 있습니다.  
오브젝트에 이터러블 프로토콜이 설정되면 이터러블 오브젝트라고 합니다.

자바스크립트는 이터러블 오브젝트에 `Symbol.iterator`가 있어야 합니다.(protocol 규약)  
`Symbol.iterator`가 있으면 이터러블 오브젝트 입니다.  

ex) 이터러블 오브젝트가 아닌 오브젝트에 `Symbol.iterator` 코드를 추가하면 이터러블 오브젝트가 됩니다.

자체 오브젝트에는 없지만, 상속받은 `prototype chain`에 있어도 이터러블 오브젝트가 됩니다.

ex) 빌트인 `Array` 오브젝트를 상속받은 오브젝트는 이터러블 오브젝트.

-------
### Array에 Symbol.iterator 존재 여부 체크

```js Array 오브젝트가 할당된 arrayObj에서 Symbol.iterator 존재 여부 체크
let arrayObj = [];  
let result = arrayObj[Symbol.iterator];  
console.log(result); // function values() { [native code] }  
```

오브젝트에 프로퍼티 존재 여부를 체크할 때 

`arrayObj.propertyKey` 또는 `arrayObj[propertyKey]`형태로 작성하지만,

`Symbol`은 2번과 같이 `[]`안에  
`arrayObj[Symbol.iterator]` 형태로 작성해야 합니다.

빌트인 오브젝트 `Array`에 `Symbol.iterator`가 설정되어 있으므로  
`function values() { [native code] }` 함수코드가 출력됩니다.

------
### Object에 Symbol.iterator 존재 여부 체크

```js Object가 할당된 objectObj에서 Symbol.iterator 존재 여부 체크
let objectObj = {};  
let result = objectObj[Symbol.iterator];  
console.log(result); //undefined  
```

`objectObj`는 `Symbol.iterator`가 존재 하지 않으므로 `undefined`가 출력됩니다.  
이는 `objectObj`가 **이터러블 오브젝트가 아니라는 뜻이 됩니다.**

* * *

## iterator protocol

`iterator protocol`(이터레이터 프로토콜)은 `next()`메서드를 사용해  
오브젝트의 값을 차례대로 처리할 수 있는 방법을 제공합니다.

자바스크립트에서 `{key: value}` 형태의 오브젝트는 작성한 순서대로 열거되는 것이 보장되지 않습니다.  
이는 오브젝트에 `next()`가 없다는 의미이기도 합니다.

~~ES6에서는 오브젝트에 추가한 순서대로 key, value가 열거되는  
Map 오브젝트가 있습니다. 자세한 사항은 Map오브젝트에서 다룹니다.~~

```js
let arrayObj = [1, 2];  
let iteratorObj = arrayObj[Symbol.iterator]();  
  
console.log("1:", typeof iteratorObj);  
  
console.log("2:", iteratorObj.next());  
console.log("3:", iteratorObj.next());  
console.log("4:", iteratorObj.next());  
```

1.  `Array` 오브젝트를 생성하여 [1,2]값을 `arrayObj`에 할당합니다.
    

2.  `arrayObj`의 `Symbol.iterator()`을 호출하면 이터레이터 오브젝트를 생성하여 반환합니다.
    

* **"1: object"**  
`Symbol.iterator()`로 반환받은 `iteratorObj` 타입.  
(이터러블 오브젝트에 Symbol.iterator()로 반환받은 오브젝트이므로 이터레이터 오브젝트 입니다.)  
이터레이터 오브젝트 이므로 next()메소드를 사용 할 수있습니다.


* **"2: Object {value: 1, done: false}"**  
`value: 1` = 이터러블 오브젝트의 값 [1,2] 중 1  
done: false = 이터레이터가 끝나지 않은 것을 의미합니다.


* **"3: Object {value: 2, done: false}"**  
`value: 2` = 이터러블 오브젝트의 값 [1,2] 중 2  
done: false = 이터레이터가 끝나지 않은 것을 의미합니다.


* **"4: Object {value: undefined, done: true}"**  
`value: undefined` = 이터러블 오브젝트의 값 [1,2] 중 이터레이터로 읽은 값이 없음  
`done: true` = 이터레이터 종료됨.


이와 같이 이터레이터를 사용하여 이터러블 오브젝트의 값을 작성한 순서대로 읽을 수 있습니다. 
**for()문으로 반복하는 것과는 차이가 있습니다.(목적도 다름)**