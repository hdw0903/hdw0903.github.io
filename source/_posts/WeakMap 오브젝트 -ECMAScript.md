---
title: WeakMap 오브젝트 -ECMAScript
date: 2020-04-13 08:22:25
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - WeakMap
  - GC
  - Garbage Collection
  - new WeakMap
  - WeakMap. set
  - WeakMap. get
  - WeakMap. has
  - WeakMap. delete

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

*   WeakMap 오브젝트
    
    *   [개요](/2020/04/13/WeakMap%20오브젝트%20-ECMAScript/#WeakMap)
        *   GC(Garbage Collection)
    *   [new WeakMap 인스턴스 생성](/2020/04/13/WeakMap%20오브젝트%20-ECMAScript/#newWeakMap)
    *   [set(): key, value 설정](/2020/04/13/WeakMap%20오브젝트%20-ECMAScript/#WeakMap_set)
    *   [get(): key가 같은 value 반환](/2020/04/13/WeakMap%20오브젝트%20-ECMAScript/#WeakMap_get)
    *   [has(): key 존재 여부](/2020/04/13/WeakMap%20오브젝트%20-ECMAScript/#WeakMap_has)
    *   [delete(): 엘리먼트 삭제](/2020/04/13/WeakMap%20오브젝트%20-ECMAScript/#WeakMap_delete)

<!-- more -->

* * *

<h2 id="WeakMap">개요</h2>

`WeakMap`는 약한(weak) Map 오브젝트 입니다.  
`Map` 오브젝트와 작성 방법과 형태는 같지만, 약한 점이 있습니다.

*   `WeakMap` 오브젝트는 `key`에 `Object`만 작성할 수 있으며 `string`, `numbers`, `symbol` 등의 (원시 데이터형)은 허용되지 않습니다.


*   `value`는 타입에 제한이 없습니다.


*   `key`에 오브젝트만 지정할 수 있는 것이 중요하며, 그 이유를 이해하는 것이 `WeakMap` 오브젝트의 사용 핵심입니다.
    
    *   ex) let music = {title: “음악”} 코드를 실행하면 music 변수에 Object 오브젝트를 생성하여 할당합니다.  
        다시 music 변수에 {singer: “가수”}를 할당하면 {title: “음악”}을 사용할 수 없게 됩니다.  
        당연한 예시지만, 이로 인해 발생하는 문제를 해결하기 위한 것이 WeakMap 오브젝트의 목적입니다.

------
### GC(Garbage Collection)

```js GC
1. let music = {title: "음악"};  
2. let music = {singer: "가수"};  
```

1.  생성된 `Object 오브젝트`가 메모리에 설정되며, 메모리 주소가 `music 변수`에 할당됩니다.  
    `music`을 전개하면 `{title: “음악”}`이 전개됩니다.  
    `music`을 전개하다는 것은 `music` 변수에 `설정된 메모리 주소의 오브젝트`를 전개하는 것입니다.


2.  `music = {singer: “가수”}`도 `Object 오브젝트`를 생성하여 메모리에 설정합니다. 생성한 Object 오브젝트를 `music 변수`에 할당하므로 `music 변수의 메모리 주소`가 바뀌게 됩니다.  
    즉, `{title: “음악”}`을 사용할 수 없게 됩니다.


**이때 엔진은 사용하지 않는 {title: “음악”}을 메모리에서 지웁니다.**  

<mark>이를 GC(Garbage Collection)라고 하고, GC를 수행하는 것을 Garbage Collector라고 합니다.  
GC를 하지 않으면 사용하지 않는 메모리가 쌓이게 됩니다. 즉 메모리 릭(Memory Leak)이 발생합니다.</mark>

`GC`가 사용하지 않게 된 오브젝트를 메모리에서 지우면, `WeakMap` 인스턴스의 `key`에서 오브젝트를 참조할 수 없게됩니다. 메모리에 존재하지 않는 오브젝트를 참조하는 것은 문제가 됩니다. 이때, 엔진이 `WeakMap` 인스턴스의 `key` 와 `value`를 삭제합니다. 개발자 코드로 삭제하지 않아도 됩니다. 이런 점을 반영한 것이 `WeakMap` 오브젝트입니다.

`WeakMap` 오브젝트에서 제공하는 메서드는 간단합니다.  
`set()`, `get()`, `has()`, `delete()`만 있습니다. `WeakMap` 인스턴스에 `[key, value]`를 `CRUD`(Create, Read, Update, Delete)하기 위한 기본적인 메서드만 있습니다.  
`WeakMap` 인스턴스는 열거할 수 없습니다. 그러므로 `forEach(), entries()` 메서드가 없습니다.

`Map` 오브젝트는 `size` 프로퍼티가 있어 현재의 `[key, value]` 수를 알 수 있으나, `WeakMap` 오브젝트는 `size` 프로퍼티가 없어서 현재의 `[key, value]` 수를 알 수 없습니다. `length` 프로퍼티도 ~~개발자 도구에서는 표시되나~~ 실제로 사용하면 `undefined`를 반환합니다.

* * *

<h2 id="newWeakMap">new WeakMap 인스턴스 생성</h2>

**`WeakMap` 인스턴스를 생성하여 반환합니다.**

> new WeakMap()

*   선택적 파라미터  
    Object, 이터러블 오브젝트안에 작성 [key, value] 형태
    
*   반환  
    생성한 WeakMap 인스턴스
    
```js
const emptyWeakMap = new WeakMap();  
  
let obj = {};  
const newWeakMap = new WeakMap([  
 [obj, "오브젝트"]  
]);  
```

*   `WeakMap()`에 파라미터를 작성하지 않아도 `WeakMap` 인스턴스를 생성할 수 있습니다.  
    이는 상황에 따라 엘리먼트를 추가하려는 의도입니다.


*   `WeakMap()` 파라미터에 이터러블 오브젝트를 작성하고 그 안에 `[key, value]` 형태로 작성합니다.  
    `key`에 오브젝트 이름을 지정하고 `value`에 값을 작성합니다. `key`인 오브젝트 자체에 프로퍼티가 있으므로 `value`는 오브젝트에 대한 `주석(설명)`이라고 볼 수 있습니다.

* * *

<h2 id="WeakMap_set">set(): key, value 설정</h2>

**`WeakMap` 인스턴스에 `key` 와 `value`를 설정합니다.**

> WeakMap.prototype.set()

*   파라미터
    
    *   Object (key, Object/Function 등의 오브젝트)
    *   any (value)
*   반환  
    key, value가 추가된 WeakMap 인스턴스
    

첫 번째 파라미터에 `key`로 설정될 `Object, Function`과 같은 오브젝트를 지정합니다.  
`string`, `numbers`, `boolean`, `null`, `undefined`, `symbol`을 지정하면 에러가 발생합니다.  
두 번째 파라미터에 임의의 값을 작성합니다.

```js set-1
"use strict";  
debugger;  
  
const newWeakMap = new WeakMap();  
1. (function(){  
 var obj = {item: "weakmap"};  
 newWeakMap.set(obj, "GC");  
}());  
  
const newMap = new Map();  
2. (function(){  
 var obj = {item: "map"};  
 newMap.set(obj, "Keep");  
}());  
  
  
3. setTimeout(function() {  
 4. console.log("1:", newWeakMap);  
 5 .console.log("2:", newMap);  
}, 1000);  
// 1: WeakMap {Object {item: "weakmap"} => "GC"}  
// 2: Map {Object {item: "map"} => "keep"}  
```

*   위 코드는 상황에 따라 실행 결과가 다르게 출력될 수 있습니다.  
    개발자 도구에서 `Sources`로 들어가 `.js파일`을 선택해 소스 코드를 화면에 표시하고  
    F5를 눌러 새로고침 합니다. 소스 코드 두 번째 줄에 `debugger`를 작성했으므로 이 앞에서 멈추게 됩니다.  
    `debugger`로 멈춘 시점에서 한 줄씩 소스 코드를 이동하면, 실행 결과의 1번 형태로 출력되지 않고, `“WeakMap {}”` 형태가 출력될 수 있습니다. 이는 소스 코드를 한 줄씩 따라 가는 동안 `GC`가 실행되어 `WeakMap` 인스턴스의 `[key, value]`를 삭제하기 때문입니다.


1.  `(function(){코드});` 형태는 자동으로 함수가 실행됩니다. 
 함수를 저장하지 않고 한 번만 사용하려는 것이 목적입니다. 함수 실행이 끝나 함수를 빠져나오면 함수에서 선언한 변수가 저장되지 않으므로 다시 사용할 수 없습니다. 이렇게 사용하지 않게 된 것을 `Garbage Collector`가 메모리에서 삭제합니다.  
    
 `newWeakMap.set(obj, “GC”)`을 실행하면, `obj` 오브젝트를 참조하는 메모리 주소가 `newWeakMap` 인스턴스의 `key`에 설정됩니다. 함수 실행이 끝나 함수를 빠져나오면 함수블록의 `obj` 변수가 메모리에서 삭제됩니다. 
    
 따라서 `newWeakMap` 인스턴스의 `key`에서 `obj` 오브젝트를 참조할 수 없게 됩니다.  
 이때, `obj` 변수에서 참조하는 `{item: “weakmap”}`이 메모리에서 지워지면 `newWeakMap` 인스턴스의 `[key, value]`를 삭제합니다. 이것이 `WeakMap` 오브젝트의 목적입니다.


2.  위의 1번 코드와 같은 형식이며 `WeakMap`이 아닌 `Map`을 사용한 것이 다릅니다.  
    함수를 빠져나오면 `obj` 오브젝트를 참조하는 `Map` 인스턴스 `[key, value]`를 삭제하지 않고 유지합니다.  
    이점이 `WeakMap` 과 `Map`의 차이입니다.


3.  1초 후에 `setTimeout` 콜백 함수를 실행하게 됩니다. 1초 간격을 둔 것은 `GC`의 실행 여부에 따른 상태를 출력하기 위해서 입니다.


4.  `console.log()`로 `newWeakMap`을 찍으면 두 가지 형태로 출력될 수 있습니다.  
    `newWeakMap` 인스턴스의 `key`로 등록한 오브젝트가 메모리에서 삭제되면 `newWeakMap` 인스턴스의 `[key, value]`가 삭제되므로 빈 Object 오브젝트 {}가 출력됩니다. 메모리에서 삭제되지 않은 상태라면, `newWeakMap` 인스턴스에 `[key, value]`가 남아 있으므로 `1: WeakMap {Object {item: “weakmap”} => “GC”}` 형태로 출력됩니다.


5.  `newMap` 인스턴스는 `GC`의 영향을 받지 않으므로 항상 `Map {Object {item: “map”} => “keep”}` 형태로 출력됩니다.

```js set-2
const newWeakMap = new WeakMap();  
  
1. let sportsObj = {};  
newWeakMap.set(sportsObj, "Object-1");  
  
2. sportsObj = {};  
3. newWeakMap.set(sportsObj, "Object-2");  
  
4. setTimeout(function() {  
 console.log(newWeakMap);  
}, 1000);  
// WeakMap {Object { } => "Object-2"} 또는  
// WeakMap {Object { } => "Object-2"}, WeakMap {Object { } => "Object-1"}  
```

1.  `sportsObj` 변수에 `Object` 오브젝트를 생성하여 할당하고, `set( )`의 첫 번째 파라미터에 `sportsObj`를 지정하여 추가합니다.


2.  새로운 `Object` 오브젝트를 생성하여 `sportsObj` 변수에 할당하므로 `sportsObj`가 참조하는 `메모리 주소`가 변경됩니다. `newWeakMap` 인스턴스에 `key`로 설정된 `sportsObj`가 참조하는 `Object` 오브젝트는 `GC` 대상이 됩니다.


3.  `set( )`의 첫 번째 파라미터에 `sportsObj`를 지정하여 설정합니다. `newWeakMap` 인스턴스에 `sportsObj`가 있지만, `sportsObj`가 참조하는 메모리 주소가 다르므로 추가됩니다. 따라서 `newWeakMap` 인스턴스에는 `sportsObj`가 참조하는 `Object` 오브젝트가 두 개 존재하게 됩니다. 개발자 도구에서 소스 코드에 중단점(`Break Point`)를 걸어 `newWeakMap` 인스턴스를 펼치면 두 개의 `Object` 오브젝트가 표시됩니다.

```js set-3
const newWeakMap = new WeakMap();  
  
let fn = function(){};  
newWeakMap.set(fn, "함수");  
  
newWeakMap.set(fn, "value 변경");  
console.log(newWeakMap);  
// WeakMap {function => "value 변경"}  
```

*   `function` 오브젝트를 생성하여 `변수 fn`에 할당한 후, `set()`에서 `key`로 지정하여 `newWeakMap`에 추가합니다.  
    `function` 오브젝트도 오브젝트이므로 `WeakMap` 인스턴스에 설정할 수 있습니다.
    
    
*   `set( )`의 첫 번째 파라미터에 `fn`을 지정하여 `newWeakMap` 인스턴스에 설정합니다. `fn`이 참조하는 `메모리 주소`가 같으므로 추가하지 않고 두 번째 파라미터 값으로 `value`를 변경합니다. `newWeakMap` 인스턴스에 `[key, value]`가 하나만 존재합니다.
    

* * *

<h2 id="WeakMap_get">get(): key가 같은 value 반환</h2>

**`WeakMap` 인스턴스에서 지정한 `key`의 `value`를 반환합니다.**

> WeakMap.prototype.get()

*   파라미터  
    WeakMap 인스턴스의 key와 비교할 오브젝트를 지정합니다.
    
*   반환 값  
    key가 존재하면 value를 반환하고, 존재하지 않으면 undefined를 반환합니다.
    

```js get()
const newWeakMap = new WeakMap();  
  
let obj = {};  
newWeakMap.set(obj, "오브젝트");  
  
console.log(newWeakMap.get(obj));  
// 오브젝트  
```

*   `obj` 변수에 Object 오브젝트를 생성하여 할당하고, 이를 `key`로 하여 `newWeakMap` 인스턴스에 추가합니다. `value` 값은 “오브젝트”입니다.  
    `get()` 파라미터에 오브젝트를 지정합니다. `newWeakMap` 인스턴스에 파라미터로 지정한 `obj` 오브젝트가 존재하므로 `[key, value]`에서 `value`를 반환합니다.

* * *

<h2 id="WeakMap_has">has(): key 존재 여부</h2>

**`WeakMap` 인스턴스에서 `key` 존재 여부를 반환합니다. `true/false`**

> WeakMap.prototype.has()

파라미터에 WeakMap 인스턴스의 key와 비교할 오브젝트를 지정합니다. 오브젝트가 존재하면 `true`, 아니면 `false`를 반환합니다.

```js has()
const newWeakMap = new WeakMap();  
  
let obj = {};  
newWeakMap.set(obj, "오브젝트");  
  
console.log(newWeakMap.has(obj));  
// true  
```

*   Object 오브젝트를 생성하여 obj 변수에 할당하고, 이를 key로 하여 newWeakMap 인스턴스에 설정합니다. value값은 “오브젝트”입니다.  
    newWeakMap 인스턴스에 has()파라미터에 지정한 obj 오브젝트가 존재하므로 true가 출력됩니다.

* * *

<h2 id="WeakMap_delete">delete(): 엘리먼트 삭제</h2>

**`WeakMap` 인스턴스에서 `key`가 같은 엘리먼트를 삭제합니다.**

> WeakMap.prototype.delete()

파라미터의 WeakMap 인스턴스의 key와 비교할 오브젝트를 지정합니다. `key`가 존재하면 `[key, value]`를 삭제하고 `true`를 반환합니다. 존재하지 않으면 `false`를 반환합니다.

```js delete()
const newWeakMap = new WeakMap();  
  
let obj = {};  
newWeakMap.set(obj, "오브젝트");  
  
console.log(newWeakMap.delete(obj));  
// true  
```

`Object` 오브젝트를 생성하여 `obj` 변수에 할당하고, 이를 `key`로 하여 `newWeakMap`에 추가한 상태에서 위 코드를 실행합니다. `delete()` 파라미터에 `key`로 등록된 `obj` 오브젝트를 지정했으므로 `[key, value]`가 삭제됩니다. 또한, 삭제에 성공했으므로 true를 반환합니다. `newWeakMap` 인스턴스에서 삭제하는 것이지 `obj`에서 참조하는 메모리의 `Object 오브젝트{}`가 삭제되는 것은 아닙니다.
