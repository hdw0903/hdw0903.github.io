---
title: Set 오브젝트 -ECMAScript
date: 2020-04-13 10:32:44
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - Set Object
  - Set method
  - new Set
  - Set. add
  - Set. has
  - Set. entries
  - Set. values
  - Set. keys
  - Set. forEach
  - Set. delete
  - Set. clear
  - Symbol.iterator
  - key
  - value
  
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


`Set` 오브젝트는 `Map` 오브젝트와 비슷하지만 `[key, value]`가 아닌 `[value]`만 작성하는 점이 다릅니다. `Map` 오브젝트에 `Array` 기능을 추가한 오브젝트 입니다.

*   Set 오브젝트
    *   [개요](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set)
    *   [new Set(): Set 인스턴스 생성](/2020/04/13/Set%20오브젝트%20-ECMAScript/#newSet)
    *   [add(): value 추가](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_add)
    *   [has(): value 존재 여부](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_has)
    *   [entries(): 이터레이터 오브젝트 생성](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_entries)
    *   [values(): value 반환 이터레이터 오브젝트 생성](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_values)
    *   [keys(): key 반환 이터레이터 오브젝트 생성](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_keys)
    *   [forEach(): 엘리먼트마다 콜백 함수 호출](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_forEach)
    *   [delete(): 엘리먼트 삭제](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_delete)
    *   [clear(): 모든 value 지움](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_clear)
    *   [Symbol.iterator: 이터레이터 오브젝트 생성](/2020/04/13/Set%20오브젝트%20-ECMAScript/#Set_Symbol_iterator)

<!-- more -->

* * *

<h2 id="Set">개요</h2>

`Set` 오브젝트는 `Array` 오브젝트와 비슷하지만, `Array` 오브젝트에 없는 특성이 있습니다.  
`Set` 오브젝트는 [value1, value2, ···]와 같이 값을 배열로 작성합니다.  
`Set` 오브젝트에 추가한 순서로 인덱스를 부여하여 저장합니다. **따라서 추가한 순서대로 읽히는 것을 보장해 줍니다.**

`Set` 오브젝트는 `key` 개념을 갖고 있으며 `value1, value2`가 **값이면서 키 역활도 합니다.**  
**역활을 하는 것이지 `key`가 별도로 존재하는 것은 아닙니다**.  

**이런 특징으로 인해 `value` 값이 같으면 나중에 추가한 값이 추가되지 않습니다.**  

이 점이 `Array` 오브젝트와 다르며 `Map` 오브젝트와 같습니다.

`[value1, value2]`에 `string`, `number`, `symbol` 등의 원시값(프리미티브) 데이터 타입을 작성할 수 있으며, `Object`, `Function`과 같은 오브젝트도 작성할 수 있습니다. `null` 값은 `undefined`로 취급됩니다.

`Array` 오브젝트에서 엘리먼트를 삭제하려면 배열을 반복하면서 값의 일치 여부를 비교해야 합니다.  
같은 값이 여러 개 있을 수 있으므로 모두 삭제하려면 계속해서 비교하며 삭제가 진행됩니다. 

반면 `Set` 오브젝트는 `value` 값이 같은 것이 없으므로 삭제를 한 번만 실행하면 됩니다.

* * *

<h2 id="newSet">new Set(): Set 인스턴스 생성</h2>

**`Set` 인스턴스를 생성하여 반환합니다.**

> new Set()

*   선택적 파라미터  
    파라미터는 선택으로 이터러블 오브젝트를 작성하고, 그 안에 value를 0개 이상 작성합니다.

```js Set()
const setObj = new Set();  
  
1. const newSet = new Set([1, 2, 1, 2, "스포츠"]);  
2. console.log(newSet.size);  
  
3. for (let element of newSet){  
 console.log(element);  
};  
// 3  
// 1  
// 2  
// 스포츠  
```

*   `Set( )` 파라미터에 `value`를 지정하지 않고 인스턴스를 생성할 수 있습니다. 추후에 `Set` 오브젝트의 메서드를 이용해서 `value`를 추가해줄 수 있습니다.


1.  `Set( )` 파라미터에 이터러블 오브젝트를 작성하고, 그 안에 `value`를 작성한 형태입니다.  
    `Set` 인스턴스에 저장할 때 파라미터 값이 `key` 역활을 하면서 `value`로 저장됩니다.  
    `Set` 인스턴스에서 `value`의 존재 여부를 체크하고 존재하지 않으면 추가, 존재하면 추가하지 않습니다.


2.  `size` 프로퍼티는 `Set` 인스턴스의 `value` 수를 반환합니다. `Set()` 파라미터에 5개의 값을 작성했는데, 실행 결과 3이 출력됩니다. 이는 같은 파라미터값은 **뒤에 작성한 값이 추가되지 않기 때문입니다.**


3.  `for-of` 문으로 `newSet` 인스턴스를 반복할 수 있습니다. `newSet` 인스턴스의 `value`가 `for-of` 문의 `element`에 설정됩니다.

* * *

<h2 id="Set_add">add(): value 추가</h2>

**`Set` 인스턴스 끝에 `value` 값을 추가합니다.**

> Set.prototype.add()

파라미터에 `Set` 인스턴스에 추가할 `String`, 오브젝트 등의 `value`를 지정합니다. 값을 추가 한 후 `Set` 인스턴스를 반환합니다. 따라서 메서드 체인 방법으로 `Set` 인스턴스의 메서드를 호출할 수 있습니다.

```js add()
1. const newSet = new Set();  
newSet.add("축구").add("농구");  
  
2. newSet.add("축구");  
  
3. for (let element of newSet) {  
 console.log(element);  
};  
// 축구  
// 농구  
```

1.  `Set` 인스턴스를 생성하여 `newSet`에 할당합니다. `add()` 파라미터에 `“축구”`를 지정하여 실행하면, `Set` 인스턴스에 `“축구”`가 추가되며 인덱스 값은 `0` 입니다. `add()`를 실행한 후 `newSet` 인스턴스를 반환하므로 `add()`를 연결하여 호출할 수 있습니다. `“농구”`가 추가되고 인덱스 값은 `1` 입니다.


2.  `add()` 파라미터에 지정한 `“축구”`가 `Set` 인스턴스에 존재하므로 `“축구”`가 추가되지 않습니다.


3.  `for-of` 문으로 `newSet` 인스턴스를 반복하면, `newSet` 인스턴스의 `value`가 `for-of` 문의 `element`에 설정됩니다.  
    `newSet` 인스턴스에 추가된 순서대로 전개됩니다.

* * *

<h2 id="Set_has">has(): value 존재 여부</h2>

``Set` 인스턴스에서 `value`의 존재 여부를 반환합니다.`

> Set.prototype.has()

파라미터에 존재 여부를 체크할 값을 지정합니다. `Set` 인스턴스에 값이 존재하면 `true`, 아니면 `false`를 반환합니다.

```js has()
const newSet = new Set();  
newSet.add("sports");  
  
console.log(newSet.has("sports"));  
// true  
```

*   `Set` 인스턴스를 생성하여 `newSet` 변수에 할당했습니다. `add()`를 실행하면 파라미터에 지정한 `“sports”`가 `newSet` 인스턴스에 추가됩니다. `newSet` 인스턴스에 `has()` 파라미터에 지정한 `“sports”`가 존재합니다. `true`를 반환합니다.

* * *

<h2 id="Set_entries">entries(): 이터레이터 오브젝트 생성</h2>

**이터레이터 오브젝트를 생성하여 반환합니다.**

> Set.prototype.entries()

생성한 이터레이터 오브젝트의 `next()`를 호출하면 `Set` 인스턴스에 작성된 순서로 `[key, value]`를 반환합니다.  

`Set` 인스턴스에 `key`를 저장하지 않지만 `value`를 `key`에 설정하여 반환합니다.

```js entries()
1. const newSet = new Set(["one", () => {}]);  
let iteratorObj = newSet.entries();  
  
2. console.log(iteratorObj.next());  
3. console.log(iteratorObj.next());  
// Object {value: Array[2], done: false}  
// Object {value: Array[2], done: false}  
```

1.  두 개의 `value`를 가진 `Set` 인스턴스를 생성합니다. 하나는 `“one”`이고 또 하나는 `function(){}`입니다. `entries()`를 호출하면 이터레이터 오브젝트를 생성하여 반환합니다.


2.  첫 번째 `value`인 `“one”`을 반환하며 실행 결과에 `{value: Array[2], done: false}`가 출력됩니다.  
    `value`를 반환하므로 `{value: “one”, done: false}` 형태로 반환되어야 하지만, `value`를 `key`에 설정하여 반환하므로 `{value: Array[2]}` 형태가 됩니다. `Array[2]`를 펼치면 `0:”one”, 1:”one”`이 표시됩니다.


3.  마찬가지로 `value`인 `function(){}`을 반환하며 `{value: Array[2], done: false}` 형태입니다.  
    `Array[2]`를 펼치면 `0: function(){}, 1: function(){}`이 표시됩니다.

* * *

<h2 id="Set_values">values(): value 반환 이터레이터 오브젝트 생성</h2>

**`value` 값을 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.**

> Set.prototype.value

생성한 이터레이터 오브젝트의 `next()`를 호출하면 `Set` 인스턴스에 작성된 순서로 `value`를 반환합니다.

```js values()
1. const newSet = new Set(["one", () => {}]);  
let iteratorObj = newSet.values();  
  
2. console.log(iteratorObj.next());  
3. console.log(iteratorObj.next());  
// Object {value: "one", done: false}  
// Object {value: function, done: false}  
```

1.  두 개의 `value`를 가진 `Set` 인스턴스를 생성합니다. 하나는 `“one”` 또 하나는 `function(){}`입니다.  
    인스턴스의 `values()`를 호출하면 `value`를 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.


2.  첫 번째 `value`인 `“one”`이 대상이며 `{value: “one”, done: false}`를 반환합니다.  
    `etntries()` 메서드가 `{value: Array[2]}`를 반환하는 것과 차이점 입니다.


3.  두 번째 `value`인 `function(){}`이 대상이며 `{value: function(){}, done: false}`를 반환합니다.

* * *

<h2 id="Set_keys">keys(): key 반환 이터레이터 오브젝트 생성</h2>

**`key` 값을 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.**

> Set.prototype.keys()

생성한 이터레이터 오브젝트의 `next()`를 호출하면 `Set` 인스턴스에 작성된 순서로 `key` 값을 반환합니다.  
`Set` 인스턴스에 `value`만 설정되므로 `value`를 `key`로 하여 반환합니다.  
그다지 의미가 없지만 같은 이름의 `Map` 인스턴스 메서드와 반환 구조를 맞추기 위한 것으로 생각됩니다.

```js keys()
const newSet = new Set(["one", () => {}]);  
let iteratorObj = newSet.keys();  
  
console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
// Object {value: "one", done: false}  
// Object {value: function(){}, done: false}  
```

*   앞의 예제들과 주어진 값은 같습니다.  
    `keys()`를 호출하면 `value`를 `key`로 하여 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.  
    <mark>반환되는 프로퍼티 이름이 key가 아닌 value입니다.</mark>

* * *

<h2 id="Set_forEach">forEach(): 엘리먼트마다 콜백 함수 호출</h2>

**`Set` 인스턴스에 작성된 순서로 반복하면서 콜백 함수를 호출합니다.**

> Set.prototype.forEach()

*   파라미터  
    Function (반복할 때 마다 호출할 callback 함수)  
    Object (선택) callback 함수에서 this로 참조할 오브젝트

`forEach()`를 호출할 때마다 세 개의 파라미터를 넘겨줍니다.  
첫 번째 파라미터가 `value`이고 두 번째 파라미터가 `key`입니다. 세 번째 파라미터는 실행 중인 `Set` 인스턴스 입니다.  

<mark>value 값과 key 값이 같습니다.</mark>

```js forEach()
const newSet = new Set(["one", "two"]);  
  
newSet.forEach(function(value, key, obj) {  
 console.log(value, this.member);  
}, {member: 10});  
// one : 10  
// two : 10  
```

*   `forEach()`를 처음 호출하면 콜백 함수의 `value` 와 `key` 파라미터에 `“one”`이 설정되고 `obj` 파라미터에 `newSet` 인스턴스가 설정됩니다. 콜백 함수에서 `this`로 `forEach()` 두 번째 파라미터에 지정한 `Object` 오브젝트를 참조합니다.

<mark>화살표 함수로 콜백 함수를 작성하면 콜백 함수 블록에서 this가 window 오브젝트를 참조하므로 이 코드와 같이 function 키워드로 작성해야 합니다.</mark>

* * *

<h2 id="Set_delete">delete(): 엘리먼트 삭제</h2>

**`Set` 인스턴스에서 `value` 값이 같은 엘리먼트를 삭제합니다.**

> Set.prototype.delete()

*   파라미터  
    삭제할 value
    
*   반환 값  
    삭제 성공시 true, 아니면 false 반환
    

```js delete()
const newSet = new Set(["one"]);  
  
console.log(newSet.delete("one"));  
// true  
```

*   `newSet` 인스턴스에서 `delete()` 파라미터에 지정한 `“one”` 과 같은 `value`가 있으면 엘리먼트를 삭제합니다.  
    삭제되므로 `true`를 반환합니다.

* * *

<h2 id="Set_clear">clear(): 모든 value 지움</h2>

**`Set` 인스턴스의 모든 `value()`를 지웁니다.**

> Set.prototype.clear()

```js clear()
const newSet = new Set(["one", "two"]);  
newSet.clear();  
console.log(newSet.size);  
// 0  
```

*   `clear()`를 실행하면 `newSet` 인스턴스에서 엘리먼트를 모두 지우므로 `size` 프로퍼티 값이 `0`으로 출력됩니다. 
인스턴스 자체를 지우는 것이 아니라 엘리먼트를 모두 지우는 것 이므로 다시 `value`를 추가해줄 수 있습니다.

* * *

<h2 id="Set_Symbol_iterator">Symbol.iterator: 이터레이터 오브젝트 생성</h2>

**이터레이터 오브젝트를 생성하여 반환합니다.**

> Set.prototype[Symbol.iterator]

생성한 이터레이터 오브젝트의 `next()`를 호출하면, `Set` 인스턴스에 작성된 순서로 `value`를 반환합니다.

```js Symbol.iterator
1. const newSet = new Set([1, "스포츠"]);  
let iteratorObj = newSet[Symbol.iterator]();  
  
2. console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
// Object {value: 1, done: false}  
// Object {value: "스포츠", done: false}  
```

1.  `newSet` 인스턴스를 생성하고 [1, “스포츠”]를 할당합니다.  

    `newSet` 인스턴스의 `[Symbol.iterator] ()`를 호출하면 이터레이터 오브젝트를 생성하여 반환합니다.


2.  `next()`를 호출할 때마다 `newSet` 인스턴스의 `value`값을 `{value: 1, done: false}` 형태의 `value`에 설정하여 반환합니다.
