---
title: WeakSet 오브젝트 -ECMAScript
date: 2020-04-14 08:55:14
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - WeakSet
  - new WeakSet
  - WeakSet. add
  - WeakSet. has
  - WeakSet. delete
  - GC

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

`value`로 오브젝트만 사용하는 `WeakSet`

*   WeakSet
    
    *   [개요](/2020/04/14/WeakSet%20오브젝트%20-ECMAScript/#WeakSet)
    *   [new WeakSet(): WeakSet 인스턴스 생성](/2020/04/14/WeakSet%20오브젝트%20-ECMAScript/#newWeakSet)
    *   [add(): value 추가](/2020/04/14/WeakSet%20오브젝트%20-ECMAScript/#WeakSet_add)
    *   [has(): value 존재 여부](/2020/04/14/WeakSet%20오브젝트%20-ECMAScript/#WeakSet_has)
    *   [delete(): 엘리먼트 삭제](/2020/04/14/WeakSet%20오브젝트%20-ECMAScript/#WeakSet_delete)

<!-- more -->

* * *

<h2 id="WeakSet">개요</h2>

`WeakSet` 오브젝트는 `value`로 오브젝트만 사용할 수 있으며, `string`, `number`, `symbol`과 같은 값을 사용할 수 없습니다. `key`는 사용하지 않습니다.

`WeakMap` 인스턴스와 마찬가지로 `GC`(Garbage Collection)가 발생하면 `WeakSet` 인스턴스의 `value`가 삭제됩니다.  

<mark>WeakMap 오브젝트는 key가 기준이고 WeakSet 오브젝트는 value가 기준입니다.</mark>

* * *

<h2 id="newWeakSet">new WeakSet(): WeakSet 인스턴스 생성</h2>

**`WeakSet` 인스턴스를 생성하여 반환합니다.**

> new WeakSet()

*   파라미터는 선택으로 이터러블 오브젝트를 작성하고, 그 안에 오브젝트를 지정합니다.  
    지정한 오브젝트가 value에 설정됩니다.

```js new WeakSet
1. let newString = new String("문자열");  
let newNumber = new Number(12345);  
const newWeakSet = new WeakSet([newString, newNumber]);  
  
2. try {  
 new WeakSet(["ABC", 345]);  
} catch (e) {  
 console.log("object 이외 지정 불가");  
};  
// object 이외 지정 불가  
```

1.  `String` 인스턴스와 `Number` 인스턴스를 생성합니다. `WeakSet` 인스턴스를 생성하면서 파라미터에 이터러블 오브젝트를 작성하고, 그 안에 `String` 인스턴스와 `Number` 인스턴스를 지정합니다. 
각 인스턴스의 메모리 주소가 `WeakSet` 인스턴스의 `value`로 설정됩니다.


2.  newWeakSet() 파라미터에 문자열(“ABC”) 또는 숫자(345)를 작성하면 에러가 발생합니다.  
    Object, Function과 같은 오브젝트만 지정할 수 있습니다.

* * *

<h2 id="WeakSet_add">add(): value 추가</h2>

`WeakSet` 인스턴스에 `value` 를 추가합니다.

> WeakSet.prototype.add()

파라미터에 value로 설정될 오브젝트를 지정합니다. Object, Function과 같은 오브젝트를 지정할 수 있습니다.  
~~string, number, boolean, null, undefined, symbol을 작성하면 에러가 발생합니다.~~

```js add()
const newWeakSet = new WeakSet();  
  
1. let newString = new String("문자열");  
newWeakSet.add(newString);  
  
2. let obj = {sports: "스포츠"};  
newWeakSet.add(obj); 
``` 

1.  <mark>new String()으로 인스턴스를 생성하여 변수에 할당한 후, add() 파라미터에 지정하면 newWeakSet 인스턴스에 value로 추가됩니다.</mark> 문자열을 String 인스턴스로 생성하여 지정하였습니다.


2.  Object 오브젝트를 생성하여 변수에 할당하고 add() 파라미터에 지정하면 newWeakSet 인스턴스에 value로 추가됩니다.

* * *

<h2 id="WeakSet_has">has(): value 존재 여부</h2>

**`WeakSet` 인스턴스에서 `value` 존재 여부를 체크하여 반환합니다.**

> WeakSet.prototype.has()

*   파라미터에 WeakSet 인스턴스의 value와 비교할 오브젝트를 지정합니다. 오브젝트가 존재하면 true를 반환하고,  
    존재하지 않으면 false를 반환합니다.

```js has()
let newString = new String("문자열");  
const newWeakSet = new WeakSet([newString]);  
  
console.log(newWeakSet.has(newString));  
//true  
```

*   has() 파라미터에 지정한 newString 인스턴스가 newWeakSet 인스턴스의 value에 있으므로 true를 반환합니다.

* * *

<h2 id="WeakSet_delete">delete(): 엘리먼트 삭제</h2>

**`WeakSet` 인스턴스에서 `value`가 같은 엘리먼트를 삭제합니다.**

> WeakSet.prototype.delete()

*   파라미터에 WeakSet 인스턴스에서 삭제할 오브젝트를 지정합니다. 오브젝트가 존재하면 value를 삭제하고 true를 반환합니다. 존재하지 않으면 false를 반환합니다.

```js delete()
let newString = new String("문자열");  
const newWeakSet = new WeakSet([newString]);  
  
console.log(newWeakSet.delete(newString));  
console.log(newWeakSet.has(newString));  
// true  
// false  
```

*   new String()으로 인스턴스를 생성하여 newWeakSet 인스턴스에 추가한 상태입니다.  
    delete() 파라미터에 newWeakSet 인스턴스에 존재하는 newString 인스턴스를 지정해 줬으므로 삭제하고 true를 반환합니다. 삭제가 완료되었으므로 has()로 존재 여부를 체크해보면 당연히 false를 반환하게 됩니다.
