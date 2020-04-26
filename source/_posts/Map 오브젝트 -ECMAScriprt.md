---
title: Map 오브젝트 -ECMAScript
date: 2020-04-10 09:48:25
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
---

Map 오브젝트는 Object 오브젝트와 비슷하지만,  
다양한 타입을 프로퍼티 키로 사용할 수 있는 점이 다릅니다.

*   Map 오브젝트
    *   [개요](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#MapObjcet)
    *   [new Map(): Map 인스턴스 생성](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#new_Map)
    *   [set(): key와 value 설정](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_set)
    *   [get(): key가 같은 value 반환](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_get)
    *   [has(): key 존재 여부](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_has)
    *   [entries(): 이터레이터 오브젝트 생성](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_entries)
    *   [keys(): key 반환 이터레이터 오브젝트 생성](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_keys)
    *   [values(): value 반환 이터레이터 오브젝트 생성](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_values)
    *   [forEach(): 엘리먼트마다 콜백 함수 호출](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_forEach)
    *   [delete(): 엘리먼트 삭제](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_delete)
    *   [clear(): 모든 key,value 지움](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_clear)
    *   [Symbol.iterator(): 이터레이터 오브젝트 생성](/2020/04/10/Map%20오브젝트%20-ECMAScriprt/#Map_Symbol_iterator)

<!-- more -->

* * *

<h2 id="MapObjcet">개요</h2>

Map 오브젝트는 key와 value로 구성됩니다. key, value  
Object 오브젝트의 key 타입은 String 또는 Symbol이지만  
Map 오브젝트는 아무 객체와 원시값(Object,Function 등등)이라도 key 와 value로 사용할 수 있습니다.  
key 와 value 값을 저장하며 각 쌍의 삽입 순서도 기억합니다.

Map 오브젝트는 key, value로 구성되지만 {key: value}형태로 작성하지 않고,  
[“key”, “value”]와 같이 이터러블 형태로 작성합니다.

<mark>key에 다양한 타입을 작성할 수 있는 것과 이러터블 형태로 작성하는 것이  
Map 오브젝트와 Object 오브젝트의 차이입니다.</mark>

Map 오브젝트는 key 값이 같으면 추가하지 않고 value 값이 대체되며 추가한 순서대로 읽습니다.

*   참고 용도 : Map 과 Object 차이 정리



| (●’◡’●)         | Map                                                                     | Object                                                                                                                                                                                                                                    |
|-----------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 의도치 않은 key | Map은 명시적으로 제공한 키 외에는 어떤 키도 가지지 않습니다.            | Object는 프로토타입을 가지므로 기본 키가 존재할 수 있습니다. 주의하지 않으면 직접 제공한 키와 충돌할 수도 있습니다. * 참고: ES5부터, 프로토타입으로 인한 키 충돌은 Object.create(null)로 해결할 수 있지만, 실제로 쓰이는 경우는 적습니다. |
| key 타입        | Map의 key는 함수, 객체 등을 포함한 모든 값이 가능합니다.                | Object의 키는 반드시 String 또는 Symbol이어야 합니다.                                                                                                                                                                                     |
| key 정렬        | Map의 키는 정렬됩니다. 따라서 Map의 이터레이션은 삽입순으로 이뤄집니다. | Object의 키는 정렬되지 않습니다. * 참고: ECMAScript 2015 이후로, 객체도 문자열과 Symbol 키의 생성 순서를 유지합니다. ECMEScript 2015 명세를 준수하는 JavaScript 엔진에서 문자열 키만 가진 객체를 순회하면 삽입 순을 따라갑니다.           |
| 크기            | Map의 항목 수는 size 속성을 통해 쉽게 알아낼 수 있습니다.               | Object의 항목 수는 직접 알아내야 합니다.                                                                                                                                                                                                  |
| 이터러블        | Map은 이터러블이므로, 바로 이터레이션할 수 있습니다.                    | Object를 순회하려면 먼저 모든 키를 알아낸 후, 그 키의 배열을 순회해야 합니다.                                                                                                                                                             |
| 성능            | 잦은 키-값 쌍의 추가와 제거에서 더 좋은 성능을 보입니다.                | 키-값 쌍의 빈번한 추가 및 제거에 최적화되지 않았습니다.                                                                                                                                                                                   |

*   Map 오브젝트가 편리하고 유용성이 높습니다. 그러나 모든 상황에 해당하지는 않습니다.  
    Map 오브젝트는 컬렉션에서 효율이 높습니다. key, value 형태이고 이터러블일 때는 Map 오브젝트를 사용하고  
    값과 함수가 혼합된 형태면 Object 오브젝트를 사용하는 것이 좋습니다.

* * *

<h2 id="new_Map">new Map(): Map 인스턴스 생성</h2>

Map 인스턴스를 생성하여 반환합니다.

> new Map([iterable])

*   iterable 선택적 파라미터  
    요소가 키-값 쌍인 Array 또는 다른 순회 가능한 객체(예: [[1, ‘one’], [2, ‘two’]]). 각 키-값 쌍은 새로운 Map에 포함됩니다.

```js for-of
let emptyMap = new Map();  
  
1. let newMap = new Map([  
 ["key1", "value1"],  
 ["key2", "value2"],  
 ["key1", "sports"]  
]);  
  
2. for (var element of newMap){  
 console.log(element);  
};  
// ["key1", "sports"]  
// ["key2", "value2"]  
```

*   emptyMap = new Map()과 같이 파라미터를 작성하지 않고 Map 인스턴스를 생성할 수 있습니다. [key, value]가 없는 형태로 생성됩니다. 이는 Map 오브젝트 메서드를 사용하여 [key, value]를 추가해줄 수 있습니다.

1.  Map() 파라미터는 이터러블 오브젝트이어야 하므로 대괄호[]를 작성하였으며, 그 안에 [“key1”, “value1”]형태로 작성하였습니다. key1이 key가 되고 value이 value가 됩니다. 다음은 newMap 인스턴스 구조입니다.
    <img src="/images/newMapInstance.JPG">
    
    1.  &#95;&#95;proto&#95;&#95;: Map은 new Map()을 실행하면 Map.prototype에 연결된 프로퍼티로 인스턴스를 생성한다는 것을 암시합니다.
        
    2.  0: {“key1” => “sports”}에서 0은 인덱스이며 Map() 파라미터에 작성한 값이 아닙니다. Map은 엔진이 파라미터의 이터러블 오브젝트에 작성한 순서로 인덱스를 부여합니다. 따라서 작성한 순서대로 읽을 수 있습니다.
        

2.  for-of 문 출력 결과가 [“key1”, “sports”], [“key2”, “value2”]  
    두 개만 표시된 이유는 첫 번째의 “key1”과 세 번째 “key1”이 같기 때문입니다.  
    이와 같이 key 값이 같으면 추가되지 않고, value 값을 나중에 작성한 값으로 대체합니다.

~~인덱스를 부여하여 작성한 순서로 저장하더라도 key 값의 일치 여부를 체크 하기 때문에 첫 번째의 value값을 세 번째의 value 값으로 대체하고 세 번째를 추가하지 않습니다.~~

```js for.Each()
let newMap = new Map([  
 ["key1", "value1"],  
 ["key2", "value2"]  
]);  
  
for (var element of newMap){  
 element.forEach((keyValue, index) => {  
 console.log(index, keyValue);  
 });  
};  
  
for (var [key, value] of newMap){  
 console.log(key, value);  
};  
/*  
0 "key1"  
1 "value1"  
0 "key2"  
1 "value2"  
key1 value1  
key2 value2  
*/  
```

*   Map() 파라미터에 이터러블 오브젝트를 작성하고, 그 안에 두 개의 [key, value]를 작성했습니다. for-of 문을 반복하면 element 변수에 [“key1”, “value1”]형태로 설정됩니다. 배열 형태이므로 for.Each() 메서드를 사용할 수 있습니다. for.Each()가 [“key1”, “value1”]형태를 첫 번째로 읽으면 for.Each() 의 keyValue 파라미터에 “key1”이 설정되고 index에 0이 설정됩니다. 두 번째를 읽으면 keyValue 파라미터에 “value1”이 설정되고 index에 1이 설정됩니다.

*   for-of 문을 처음 반복하면 [“key1”, “value1”]이 읽힙니다. 이때 “key1”이 for-of 문의 [key, value]에서 key에 설정되고 “value1”이 value에 설정됩니다. key, value가 모두 설정되므로 앞 코드와 같이 forEach()로 배열을 전개하지 않아도 됩니다.

```js Error
1. try {  
 new Map(["one", 1]);  
}catch(e){  
 console.log("[one, 1]");  
};  
  
2. try {  
 new Map({one: 1});  
}catch(e){  
 console.log("{one: 1}");  
};  
  
3. let newMap = new Map([{one: 1}]);  
console.log(newMap);  
// [one, 1]  
// {one: 1}  
// {undefined => undefined}  
```

1.  Map() 파라미터에 이터러블 오브젝트를 작성하고 그 안에 배열로 엘리먼트를 작성합니다.  
    [“one”, 1]에서 대괄호[]가 이터러블이므로 이를 제외하면 “one”, 1 형태가 되어 에러가 발생합니다.  
    [[“one”, 1]]형태로 작성해야 합니다.

2.  Map() 파라미터에 key,value를 작성하지만, {key: value} 형태로 작성할 수 없으며 TypeError가 발생합니다.  
    [[“one”, 1]]형태로 작성해야 합니다.

3.  Map() 파라미터에 대괄호[]를 작성하고, 그 안에 {one: 1}형태로 작성하면 에러가 발생하지 않고  
    Map 인스턴스가 생성됩니다. 하지만 실행 결과에서 볼 수 있듯이 key,value에 값이 설정되지 않아 undefined가 출력됩니다.

* * *

<h2 id="Map_set">set(): key와 value 설정</h2>

set() 메서드는 Map 오브젝트에서 주어진 key를 가진 엘리먼트를 추가하고, key에 엘리먼트가 이미 있다면 대체합니다.

> Map.prototype.set(key, value)

첫 번째 파라미터에 key가 될 String 또는 오브젝트를 작성하고 두 번째 파라미터에 value를 작성합니다.  
set()을 실행한 후 Map인스턴스를 반환하므로 메서드 체인(method chain)형태로 계속해서 Map 인스턴스의 메서드를 호출할 수 있습니다.

```js set
1. const newMap = new Map();  
2. newMap.set("one", 100);  
3. console.log(newMap.size);  
  
4. newMap.set({}, "오브젝트");  
5. newMap.set(function(){}, "Function");  
  
6. newMap.set(new Number("123"), "인스턴스");  
7. newMap.set(NaN, "Not a Number");  
  
8. for (var [key, value] of newMap) {  
 console.log(key, value);  
};  
/*  
1  
one 100  
Object {} : "오브젝트"  
function (){} : "Function"  
Number {[[PrimitiveValue]]: 123} "인스턴스"  
NaN "Not a Number"  
*/  
```

1.  new Map() 파라미터를 작성하지 않고 인스턴스를 생성했습니다.  
    인스턴스에 key, value를 추가할 수는 있으나 인스턴스를 삭제할 수는 없습니다.

2.  newMap.set()으로 newMap 인스턴스에 [key, value]값을 추가합니다. “one”이 키가 되고 100이 value가 됩니다.

3.  size 프로퍼티는 Map 인스턴스의 엘리먼트 수를 반환합니다. 이 값은 바꿀 수 없습니다. 바꾸면 TypeError가 발생합니다.

4.  set()의 첫 번째 파라미터에 Object {}가 key가 되고, 두 번째 파라미터인 “오브젝트”가 value가 됩니다.

5.  set()의 첫 번째 파라미터인 function 오브젝트가 key가 되고 두 번째 파라미터인 “Function”이 value가 됩니다.

6.  첫 번째 파라미터인 Number 인스턴스가 key가 되고, 두 번째 파라미터인 “인스턴스”가 value가 됩니다.

7.  첫 번째 파라미터인 NaN이 key가 되고, 두 번째 파라미터인 “Not a Number”가 value가 됩니다. NaN을 key로 사용할 수 있습니다.
    
8.  newMap 인스턴스를 for-of 문으로 반복하면 추가한 순서대로 전개됩니다.
    
    *   newMap의 인스턴스 구조
        
            Map(5)
                [[Entries]]
                0: {"one" => 100}
                1: {Object => "오브젝트"}
                2: {function(){} => "Function"}
                3: {Number => "인스턴스"}
                4: {NaN => "Not a Number"}
        

```js
const newMap = new Map();  
newMap.set("one", 100);  
newMap.set("one", 123);  
  
let sportsObj = {sports: "스포츠"};  
1. newMap.set(sportsObj, "Sports Object");  
2. newMap.set(sportsObj, "Sports Object-변경");  
  
3. newMap.set({}, "Object-1");  
4. newMap.set({}, "Object-2");  
```

*   newMap 인스턴스에 “one”을 key로 하여 100을 설정한 후, 다시 “one”을 key로 하여 123을 설정하면  
    key 값이 같으므로 100 이 123으로 대체됩니다.

1.  set()의 첫 번째 파라미터에 Object 오브젝트를 직접 작성하지 않고 별도로 작성한 sportsObj 오브젝트를 지정했습니다. sportsObj가 key가 되고 “Sports Object”가 value가 됩니다.

2.  sportsObj key가 존재하므로 value가 대체됩니다.

3.  set()의 첫 번째 파라미터에 Object 리터럴 {}을 지정했습니다. Object 오브젝트를 생성하여 메모리에 저장하고 newMap 인스턴스에 key로 사용하여 “Object-1”을 추가합니다.

4.  set()의 첫 번째 파라미터에 Object 리터럴을 지정해도 앞의 key값과 중복되지 않습니다.  
    왜냐하면 첫 번째 파라미터의 Object 리터럴이 새로운 Object 오브젝트를 생성하므로 앞의 코드와 다른 메모리 주소에 저장되기 때문입니다. 앞의 Object 오브젝트가 저장된 메모리 주소와 생성한 Object 오브젝트의 메모리 주소가 다르므로 value가 대체되지 않고 추가됩니다.

* * *

<h2 id="Map_get">get(): key가 같은 value 반환</h2>

Map 인스턴스에서 key 값이 같은 value를 반환합니다.

> Map.prototype.get(key)

파라미터에 검색할 key 값을 작성합니다. 파라미터의 key가 Map 인스턴스에 존재하면 value를 반환하고, 존재하지 않으면 undefined를 반환합니다. key의 값 타입까지 체크합니다 (123 와 “123”은 같지 않습니다.)

```js get()
const newMap = new Map();  
1. newMap.set("one", 100);  
console.log(newMap.get("one"));  
  
2. console.log(newMap.get("two"));  
  
3. let sportsObj = {sports: "스포츠"};  
newMap.set(sportsObj, "Sports Object");  
console.log(newMap.get(sportsObj));  
//100  
//undefined  
//Sports Object  
```

1.  set()을 실행하면 [“one”, 100]형태로 등록됩니다. get() 파라미터에 검색할 key 값을 작성합니다. “one”이 newMap 인스턴스에 존재하므로 value인 100을 반환합니다.

2.  get()의 파라미터 값인 “two”가 newMap 인스턴스 key에 존재하지 않으므로 undefined 입니다.

3.  set()의 첫 번째 파라미터에 sportsObj의 메모리 주소를 key로 지정하여 newMap 인스턴스에 추가합니다.  
    다시 sportsObj의 메모리 주소를 파라미터 값으로 지정하여 get()을 수행하므로 “Sports Object”가 반환됩니다.

```js
const newMap = new Map();  
  
1. newMap.set({}, "Object");  
console.log(newMap.get({}));  
  
2. newMap.set(123, "값 123");  
console.log(newMap.get(123));  
console.log(newMap.get("123"));  
  
3. newMap.set(NaN, "Not a Number");  
console.log(newMap.get(NaN));  
// undefined  
// 값 123  
// undefined  
// Not a Number  
```

1.  set() 파라미터인 Object 오브젝트와 get() 파라미터인 Object 오브젝트의 메모리 주소가 다름으로 undefined를 반환합니다.

2.  get(123)으로 검색하면 key 의 타입까지 체크하여 value를 반환합니다.  
    get(“123”)으로 값을 구하면 “123”이 String 타입이므로 타입이 같지않습니다. undefined가 반환됩니다.

3.  set()의 파라미터에 NaN을 지정하고 get()의 파라미터에 NaN을 지정하면 key 값이 같으므로 value 값을 반환합니다. ~~ES5에서 (NaN === NaN) 비교 결과가 true가 아닌 문제가 있었지만 Map 오브젝트에 반영되었습니다.~~

* * *

<h2 id="Map_has">has(): key 존재 여부</h2>

Map 인스턴스에서 key 값의 존재 여부를 반환합니다.

> Map.prototype.has(key)

key 값이 존재하면 true, 아니라면 false를 반환합니다.

```js
const newMap = new Map();  
newMap.set("one", 100);  
  
console.log(newMap.has("one"));  
// true  
```

*   key 를 비교한다는 점은 get()과 같습니다.  
    get()은 해당 key의 value를 반환.  
    has()는 key값 존재 여부를 체크, true/false 값을 반환합니다.

* * *

<h2 id="Map_entries">entries(): 이터레이터 오브젝트 생성</h2>

[key, value]를 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.

> Map.prototype.entries()

생성한 이터레이터 오브젝트에 next()를 호출하면 [key, value]를 반환합니다.  
next()를 호출할 때마다 Map 인스턴스에 추가한 순서대로 읽힙니다.

```js
const newMap = new Map([  
 ["key1", "value1"],  
 ["key2", "value2"]  
]);  
  
let iteratorObj = newMap.entries();  
  
let result = iteratorObj.next();  
console.log(result);  
  
console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
// Object {value: Array[2], done: false}  
// Object {value: Array[2], done: false}  
// Object {value: undefined, done: true}  
```

1.  new Map()으로 두 개의 엘리먼트를 가진 Map 인스턴스를 생성합니다.  
    생성한 Map 인스턴스의 entries()를 호출하여 이터러블 오브젝트를 생성하고 반환합니다.

2.  next()를 호출하면 첫 번째의 [“key1”, “value1”]를 {value: Array[2], done: false} 형태의 value에 설정하여 반환합니다. Array[2]로 표시된 이유는 [“key1”, “value1”] 형태이기 때문입니다.

* * *

<h2 id="Map_keys">keys(): key 반환 이터레이터 오브젝트 생성</h2>

key 값을 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.

> Map.prototype.keys()

생성한 이터레이터 오브젝트의 next()를 호출하면 [key, value]형태에서 key 값만 반환합니다.  
Map 인스턴스에 추가한 순서대로 읽힙니다.

```js
const newMap = new Map([  
 ["key1", "value1"]  
]);  
newMap.set({}, "오브젝트");  
  
1. let iteratorObj = newMap.keys();  
2. console.log(iteratorObj.next());  
  
3. console.log(iteratorObj.next());  
// Object {value: "key1", done:false}  
// Object {value: Object, done:false}  
```

1.  newMap.keys()로 key 값만 반환하는 이터레이터 오브젝트를 생성해 iteratorObj에 할당합니다.

2.  next()를 호출하면 [“key1”, “value1”]에서 “key1”을 {value: “key1”, done:false} 형태로  
    value에 설정하여 반환합니다.

3.  ({}, “오브젝트”) 형태로 추가했으므로 Object 오브젝트가 key가 됩니다.  
    {value: Object, done: false} 형태가 반환됩니다.

* * *

<h2 id="Map_values">values(): value 반환 이터레이터 오브젝트 생성</h2>

value 값을 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.

> Map.prototype.values()

생성한 이터레이터 오브젝트의 next()를 호출하면 value 값 만 반환합니다.

```js
const newMap = new Map([  
 ["key1", "value1"]  
]);  
newMap.set({}, "오브젝트");  
  
1. let iteratorObj = newMap.values();  
2. console.log(iteratorObj.next());  
  
3. console.log(iteratorObj.next());  
// Object {value: "value1", done: false}  
// Object {value: "오브젝트", done: false}  
```

1.  newMap.values()로 value 값을 반환하는 이터레이터 오브젝트를 생성하여 반환합니다.

2.  next()를 호출하면 [“key1”, “value1”]에서 “value1”을 {value: “value1”, done: false}의 value에 설정하여 반환합니다.

3.  “오브젝트”가 value가 됩니다. {value: “오브젝트”, done:false} 형태로 반환됩니다.

* * *

<h2 id="Map_forEach">forEach(): 엘리먼트마다 콜백 함수 호출</h2>

Map 인스턴스를 반복할 때 마다 callback 함수를 호출합니다.

> Map.prototype.forEach(function)

첫 번째 파라미터에 반복할 때마다 호출할 콜백 함수를 작성합니다.

두 번째 파라미터는 선택적 파라미터로 콜백 함수에서 this로 참조할 오브젝트를 지정합니다.

### 중요 포인트

forEach()는 호출할 때마다 세 개의 파라미터를 넘겨줍니다.

1.  [key, value]에서 value
2.  [key, value]에서 key
3.  실행 중인 Map 인스턴스

`key, value 순서가 아닌 value, key 순서입니다.`

```js
const newMap = new Map([  
 ["key1", "value1"],  
 [{}, "오브젝트"]  
]);  
  
newMap.forEach((value, key, map) => {  
 console.log(key, value);  
});  
// key1 value1  
// Object { } "오브젝트"  
```

*   forEach()가 처음 호출되면 [“key1”, “value1”]을 읽으며 파라미터 value에 “value1”이 설정되고 파라미터 key에 “key1”이 설정됩니다. 세 번째의 map 파라미터에 newMap 인스턴스가 설정됩니다.  
    두 번째 호출 역시 마찬가지로 설정됩니다.

* * *

<h2 id="Map_delete">delete(): 엘리먼트 삭제</h2>

Map 인스턴스에서 key 값이 같은 엘리먼트를 삭제합니다.

> Map.prototype.delete(key)

파라미터에 삭제할 key 값을 지정합니다. Map 인스턴스에 같은 key가 존재하면 엘리먼트를 삭제하고 true를 반환합니다. key값이 존재하지 않으면 false를 반환합니다.

```js
const newMap = new Map([  
 ["key1", "value1"],  
 [{}, "오브젝트"]  
]);  
let sportsObj = {};  
newMap.set(sportsObj, "추가");  
  
1. console.log(newMap.delete("key1"));  
2. console.log(newMap.delete({}));  
3. console.log(newMap.delete(sportsObj));  
//true  
//false  
//true  
```

1.  delete() 파라미터에 작성한 “key1”이 newMap 인스턴스에 존재하므로 [“key1”, “value1”] 엘리먼트를 삭제하고 true를 반환합니다.

2.  delete() 파라미터에 작성한 Object 오브젝트{}가 newMap 인스턴스에 존재하는 것처럼 보이지만,  
    새로운 Object 오브젝트를 생성하여 메모리에 저장하므로 newMap 인스턴스에 저장된 Object 오브젝트와 메모리 주소가 달라서 삭제하지 못합니다. false가 반환됩니다.

3.  delete() 파라미터의 sportsObj와 newMap 인스턴스의 sportsObj가 같은 메모리 주소를 참조하므로 삭제가 되며 true를 반환합니다.

* * *

<h2 id="Map_clear">clear(): 모든 key, value 지움</h2>

Map 인스턴스의 모든 [key, value]를 지웁니다.

> Map.prototype.clear()

Map 인스턴스를 삭제하는 것이 아니라 [key, value]만 지웁니다.  
나중에 Map 인스턴스에 [key, value]를 추가해 줄 수 있습니다.

```js
const newMap = new Map([  
 ["key1", "value1"],  
 [{}, "오브젝트"]  
]);  
  
1. console.log(newMap.size);  
  
newMap.clear();  
2. console.log(newMap.size);  
//0  
//2  
```

1.  size 프로퍼티는 newMap 인스턴스의 [key, value]엘리먼트 수를 반환합니다. 2가 출력됩니다.

2.  clear()는 newMap 인스턴스의 모든 [key, value]를 지웁니다. 따라서 size 값으로 0이 출력됩니다.

* * *

<h2 id="Map_Symbol_iterator">Symbol.iterator(): 이터레이터 오브젝트 생성</h2>

이터레이터 오브젝트를 생성하여 반환합니다.

> Map.prototype.[Symbol.iterator]

생성한 이터레이터 오브젝트의 next()를 호출하면 Map 인스턴스에 작성된 순서대로 [key, value]를 반환합니다.  
Map.prototype.entries()와 같습니다.

```js
let newMap = new Map([  
 ["1", "music"],  
 ["2", "sports"]  
]);  
  
1. let iteratorObj = newMap[Symbol.iterator]();  
  
2. console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
// Object {value: Array[2], done: false}  
// Object {value: Array[2], done: false}  
```

1.  newMap 인스턴스의 Symbol.iterator를 호출하면 이터레이터 오브젝트를 생성하여 반환합니다.

2.  next()를 호출할 때마다 [“1”, “music”] 과 [“2”, “sports”]를 순서대로 읽어  
    {value: Array[2], done: false} 형태로 반환합니다.  
    Array[2]에 읽은 [key, value]가 설정됩니다.
