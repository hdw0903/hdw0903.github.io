---
title: Symbol 프로퍼티 -ECMAScript
date: 2020-04-06 12:55:12
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - Symbol
  - Symbol property
  - Well-Known Symbol
  - isConcatSpreadable
  - toStringTag
  - unscopable
  - Array-like
  - species
  - toPrimitive
  - Symbol.iterator
  - Generator
  - asyncIterator
  - match
  - matchAll
  - getter
  - instanceof
  - RegExp
  - filter
  
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


`Symbol` 오브젝트의 프로퍼티들을 살펴봅니다.  
이 프로퍼티들은 메서드로도 사용할 수 있습니다

*   Symbol 프로퍼티
    *   [Well-Known Symbol](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#Well-Known_Symbol)
    *   [toStringTag](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#toStringTag)
        *   클래스의 메서드로 사용
    *   [isConcatSpreadable](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#isConcatSpreadable)
        *   Array-like 오브젝트에서 사용
    *   [unscopable](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#unscopable)
    *   [species 개념](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#species_개념)
    *   [species](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#species)
    *   [다른 Class 반환](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#return_other_Class)
    *   [null 반환](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#return_null)
    *   [toPrimitive](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#toPrimitive)
        *   toPrimitive() 파라미터의 세가지 모드(mode)
    *   [이터레이터](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#Symbol_iterator)
        *   Array.prototype[Symbol.iterator]
        *   String.prototype[Symbol.iterator]
        *   Object 이터레이션
    *   [제너레이터](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#Symbol_generator)
    *   [asyncIterator(): 비동기 반복](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#Symbol_asyncIterator)
    *   [match(): match 결과 반환](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#Symbol_match)
    *   [matchAll()](/2020/04/06/Symbol%20프로퍼티%20-ECMAScript/#Symbol_matchAll)

<!-- more -->

* * *

<h2 id="Well-Known_Symbol">Well-Known Symbol</h2>


스펙에서 @@iterator 형태로 작성된 것을 볼 수 있으며  
@@는 Symbol 대신 사용한 것입니다.  
따라서 @@iterator는 `Symbol.iterator`와 같습니다.  
@@iterator 형태는 스펙에서 사용하며 `Symbol.iterator` 형태는 내부 프로퍼티인  
[[Description]]에 저장되는 형태입니다.  

**개발자 코드에서는 `Symbol.iterator` 형태를 사용합니다.**


 | Spec name            | [[Description]]              |
|----------------------|------------------------------|
| @@asyncIterator      | Symbol.asyncIterator         |
| @@hasInstance        | Symbol.hasInstance           |
| @@isConcatSpreadable | Symbol.isConcatSpreadable    |
| @@iterator           | Symbol.iterator              |
| @@match              | Symbol.match                 |
| @@matchAll           | Symbol.matchAll              |
| 참고용 프로퍼티      | Symbol.prototype.description |
| @@replace            | Symbol.replace               |
| @@search             | Symbol.search                |
| @@species            | Symbol.species               |
| @@split              | Symbol.split                 |
| @@toPrimitive        | Symbol.toPrimitive           |
| @@toStringTag        | Symbol.toStringTag           |
| @@unscopables        | Symbol.unscopables           |



* `Well-Known Symbol`은 스펙에서 처리 알고리즘(`Algorism`)을 구분하기 위해 부여한 이름입니다. 즉, 자바스크립트 엔진이 디폴트로 처리하는 알고리즘 유형 이름입니다.
    

* <mark>자바스크립트 프로그램에 같은 이름의 Well-Known.Symbol을 작성하면  
엔진의 디폴트 처리를 실행하지 않고 프로그램에 작성한 코드를 실행합니다(오버라이딩)  
Well-Known Symbol이 오버라이드되는 것과 같으므로 프로그램에 같은 이름을작성하여  
Well-Known Symbol기능을 대체할 수 있습니다.</makr>
    

**이런 가변성과 유용성을 제공하는 것이 Well-Known Symbol의 목적입니다.**

* * *

<h2 id="toStringTag">toStringTag</h2>

[object Object] 형태에서 `Object`를 `Symbol.toStringTag` 값으로 표시합니다.  
객체의 기본 문자열 설명을 만드는 데 사용되는 문자열 값 속성입니다.  

[Object.prototype.toString()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) 메소드에 의해 내부적으로 액세스됩니다.

------
### 설명

**많은 자바스크립트 타입들은 기본적으로 tag를 가지고 있습니다.**

```js tag default
Object.prototype.toString.call('foo');     
// "[object String]"  
Object.prototype.toString.call([1, 2]);    
// "[object Array]"  
Object.prototype.toString.call(3);         
// "[object Number]"  
Object.prototype.toString.call(true);      
// "[object Boolean]"  
Object.prototype.toString.call(undefined); 
// "[object Undefined]"  
Object.prototype.toString.call(null);      
// "[object Null]"  

// ... and more  
```

**기본적으로 toStringTag symbol이 정의되어 있는 것도 있습니다.**

```js
Object.prototype.toString.call(new Map());       
// "[object Map]"  
Object.prototype.toString.call(function* () {}); 
// "[object GeneratorFunction]"  
Object.prototype.toString.call(Promise.resolve()); 
// "[object Promise]" 

// ... and more  
```

**class를 생성하면 자바스크립트는 기본값으로 Object tag를 설정합니다.**

```js
class ValidatorClass {}  
  
Object.prototype.toString.call(new ValidatorClass()); 
// "[object Object]"  
```

**toStringTag를 이용하여 자신만의 맞춤 태그를 설정할 수 있습니다.**

```js
class ValidatorClass {  
 get [Symbol.toStringTag]() {  
 return 'Validator';  
 }  
}  
Object.prototype.toString.call(new ValidatorClass()); 
// "[object Validator]"  
```

```js toStringTag 예제
let Sports = function(){};  
let sportsObj = new Sports;  
1. console.log(sportsObj.toString());  
  
2. Sports.prototype[Symbol.toStringTag] = "Sports-Function";  
3. console.log(sportsObj.toString());  
// [object Object]  
// [object Sports-Function]  
```

1.  `new Sports`로 생성한 인스턴스로 `toString()`을 실행하면 `[object Object]`가 반환됩니다.  
    `Object` 오브젝트도 `[object Object]`를 반환하므로 구분이 어렵습니다.  
    한편 `new Sports()`의 파라미터 값이 없을때 `new sports;` 로 사용할 수 있습니다.


2.  `Sports.prototype[Symbol.toStringTag] = “Sports-Function”;`  
    Sports.prototype[Symbol.toStringTag]에 [object Object] 의 Object에 표시할 문자열을 지정합니다.


3.  `sportsObj.toString()`을 실행하면 엔진에서 디폴트 `toStringTag` 값을 반환하기 전에  
    `sportsObj`에서 `Symbol.toStringTag`의 작성 여부를 체크합니다. 

    값이 작성되어 있다면 디폴트 값이 아닌 작성된 값을 반환합니다.  
    따라서 `[object Sports-Function]` 이 출력됩니다.  
    [object Sports-Function]을 정규표현식으로 분리하면 Sports-Function을 구할 수 있습니다.

------
### 클래스의 메서드로 사용

**`Symbol.toStringTag`를 클래스의 `getter`로 작성할 수 있습니다.**

```js
class Book {};  
let bookObj = new Book();  
1. console.log(bookObj.toString());  
  
2. class Sports {  
 get [Symbol.toStringTag]() {  
 return "Sports-class";  
 }  
};  
3. let sportsObj = new Sports();  
console.log(sportsObj.toString());  
  
4. console.log(Map.prototype[Symbol.toStringTag]);  
//[object Object]  
//[object Sports-class]  
//Map  
```

1.  `toString()`을 실행하면 `[object Object]`가 반환됩니다.  
    따라서 `Function` 오브젝트, `Object` 오브젝트와 구분할 수 없습니다.


2.  클래스에 `Symbol.toStringTag`를 `getter`로 선언했습니다.  
    `return` 문에 `[object Object]`에서 `Object`에 표시될 문자열을 작성했습니다.
    `Sports-class`가 반환됩니다.  

    `getter`로 작성하지 않고 메서드로 작성하면 `[object Object]`가 반환되므로 `getter`로 작성해야 합니다.


3.  인스턴스로 `toString()`을 호출하면 `sportsObj` 인스턴스에 작성된 `getSymbol.toStringTag`가 호출됩니다.  
    호출된 메서드에서 `“Sports-class”`를 반환하므로 `[object Sports-class]`가 출력됩니다.


4.  Map 같이 Symbol.toStringTag를 기본값(빌트인)으로 가지고 있는 것들이 있습니다.  
    Map.prototype[Symbol.toStringTag]를 실행하면 “Map”이 출력됩니다.  
    개발자 코드로 Symbol.toStringTag에 표시될 문자열을 작성하거나 getter를 사용하지 않아도 됩니다.

* * *

<h2 id="isConcatSpreadable">isConcatSpreadable</h2>

`Symbol.isConcatSpreadable`는  
[Array.prototype.concat()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) **메서드를 사용할 때 객체를 배열 요소에 병합해야하는지 구성하는 데 사용됩니다.**

> [Symbol.isConcatSpreadable] = true / false

*   프로퍼티의 기본값은 true 입니다.  
    false로 설정시 배열을 펼처서 합치지 않고 배열의 끝에 배열을 추가합니다.  
    사용시 true/false 에 따라 생성되는 배열의 length값이 달라집니다.

```js
1. let one = [11, 12], two = [21, 22];  
let result = one.concat(two);  
console.log(result, result.length);  
/*  
Array(4)  
 0: 11  
 1: 12  
 2: 21  
 3: 22  
 length: 4  
 __proto__: Array(0)  
4   
*/  
  
2. two[Symbol.isConcatSpreadable] = false;  
result = one.concat(two);  
console.log(result, result.length);  
/*  
Array(3)  
 0: 11  
 1: 12  
 2: Array(2)  
 0: 21  
 1: 22  
 length: 2  
 Symbol(Symbol.isConcatSpreadable): true  
 __proto__: Array(0)  
 length: 3  
 __proto__: Array(0)  
3  
*/  
  
3. two[Symbol.isConcatSpreadable] = true;  
result = one.concat(two);  
console.log(result, result.length);  
/*  
Array(4)  
 0: 11  
 1: 12  
 2: 21  
 3: 22  
 length: 4  
 __proto__: Array(0)  
4  
*/  
```

1.  one 배열의 [11, 12]에 `concat()`의 파라미터에 지정한 two 배열 [21, 22]를 결합하면,  
    21 과 22가 각각 분리되어 one 배열 끝에 추가됩니다.  
    [11, 12, 21, 22] length: 4 가 반환됩니다.  

    `Array.prototype`에 `[Symbol.isConcatSpreadable]`을 작성하지 않았으므로 디폴트 true값 입니다.  
    일반적인 `concat()`메서드 사용입니다.


2.  `two[Symbol.isConcatSpreadable]`에 `false` 값을 할당했습니다.

    `concat()` 메서드로 `one` 배열과 `two` 배열을 결합하면 `two` 배열의 `[21, 22]`를 **분리하지 않고  배열 자체를 첨부합니다.** 
    [11, 12, Array[2]] 형태로 결합되어 length 값은 3을 갖습니다.  
    
    개발자 도구에서 Array[2]를 살펴보면 [21, 22] length: 2 가 할당되어 있고  
    `Symbol(Symbol.isConcatSpreadable)`: `true`로 `isConcatSpreadable` = `false` 가 적용된 것을 볼 수 있습니다. 기본값 isConcatSpreadable = true 인 경우 프로퍼티에 나타나지 않습니다.


3.  two[Symbol.isConcatSpreadable]에 `true`를 할당하였으며 디폴트 값 true와 같습니다.  
    one 배열에 two 배열의 [21, 22]가 분리되어 끝에 첨부됩니다.  
    [11, 12, 21, 22] 형태가 됩니다.

------
### Array-like 오브젝트에서 사용

<mark>유사 배열(Array-like) 오브젝트도 concat()을 사용할 수 있습니다. 하지만 유사 배열 오브젝트는 isConcatSpreadable = true 값을 가지지 않습니다.</mark> 

**concat()으로 배열을 하나씩 분리하여 추가 병합하고 싶다면 isConcatSpreadable를 사용하여 true로 설정해줍니다.**

```js isConcatSpreadable > Array-like
let one = [11, 12];  
1. let fiveSix = {  
 0: "five",  
 1: "six",  
 length: 2  
};  
let result = one.concat(fiveSix);  
console.log(result, result.length);  
/*  
Array(3)  
 0: 11  
 1: 12  
 2: Object{  
 0: "five",   
 1: "six",   
 length: 2}   
 length: 3  
 __proto__: Array(0)  
3  
*/  
  
2. let arrayLike = {  
 [Symbol.isConcatSpreadable]: true,  
 0: "five",  
 1: "six",  
 length: 2  
};  
result = one.concat(arrayLike);  
console.log(result, result.length);  
/*  
Array(4)  
 0: 11  
 1: 12  
 2: "five"  
 3: "six"  
 length: 4  
 __proto__: Array(0)  
4  
*/  
```

1.  one은 배열 [11, 12]이고 fiveSix는 Array-like {0: “five”, 1” “six”, length:2}입니다.  
    이 상태에서 `concat()`을 실행하면 [11, 12, Object] 형태가 됩니다.  
    **오브젝트 형태가 분리되어 첨부되지 않습니다.**


2.  `Array-like` 오브젝트에 `[Symbol.isConcatSpreadable]`: `true`를 작성하여,  
    `concat()`을 실행하면 `Array-like` 오브젝트의 각 프로퍼티를 분리하여 배열 끝에 첨부합니다.

* * *

<h2 id="unscopable">unscopable</h2>

[with](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/with)문에서 사용하며 값이 true 이면 프로퍼티를 전개 하지 않습니다.

> [Symbol.unscopables] = true/false

`Symbol.unscopables` 값이 `true`일 때, `with` 문에서 프로퍼티를 전개하지 않으므로  
`Object` **오브젝트의 프로퍼티 키를 사용하면 에러가 발생합니다.**

ES5 에서 “use strict” 범위에 with 문을 사용하면 에러가 발생합니다.
ES6도 마찬가지 입니다. ES6에서 stirct모드는 보편적인 환경이므로 Symbol.unscopables를 사용할 빈도가 그다지 높지 않습니다.

```js Symbol.unscopables
// "use strict" 를 선언하면 with 에서 에러 발생  
let sports = {  
 soccer: "축구",  
 baseball: "야구"  
};  
1. with(sports){  
 console.log(soccer, baseball);  
};  
  
2. sports[Symbol.unscopables] = {  
 baseball: true  
};  
  
try {  
 3. with (sports) {  
 console.log(soccer);  
 let value = baseball;  
 }  
} catch (e) {  
 console.log(e.message);  
};  
// 축구 야구  
// 축구  
// baseball is not defined  
```

1.  `with` 문을 실행하면 파라미터에 작성한 `sports` 오브젝트의 프로퍼티가 펼쳐진(`spread`)형태가 됩니다.  

    따라서 프로퍼티 값을 구할 때 sports[“soccer”]형태로 작성하지 않고 soccer만 작성합니다.  
    `soccer`의 프로퍼티 값인 “축구”가 반환되고 `baseball`의 프로퍼티 값도 마찬가지로 반환되어  
    “축구” “야구”가 출력됩니다.


2.  `with(sports)`를 실행할 때 `sports` 오브젝트에서 전개하지 않을 프로퍼티를 작성합니다.  
    `baseball: true`를 설정해줬으므로 `with` 문에서 `baseball` **프로퍼티가 전개되지 않습니다.**  
    그러므로 `soccer`의 프로퍼티 값인 “축구”만 출력됩니다.


3.  `with(sports)` 문에서 `baseball` 프로퍼티가 전개되지 않으므로 `baseball`을 변수에 할당하면  
    에러가 발생합니다.

* * *

<h2 id="species_개념">species 개념</h2>

**`Symbol.species`는 `constructor`를 반환합니다.**  

`constructor`를 반환한다는 것은 **`constructor`로 인스턴스를 생성하여 반환하는 것과 같습니다.**  
`Symbol.species`를 오버라이드 할 수 있으며, 개발자 코드로 반환되는 인스턴스를 변경할 수 있습니다.

`species`의 개념을 살펴 봅니다.

```js species 개념
1. let arrayObj = [1, 2, 3];  
2. let sliceOne = arrayObj.slice(1, 3);  
3. let sliceTwo = sliceOne.slice(1, 2);  
```

1.  let arrayObj = [1, 2, 3]을 실행하면 `Array` 오브젝트(인스턴스)를 생성하고 엘리먼트 값으로  
    1, 2, 3을 설정합니다. 아래는 [1, 2, 3]이 할당된 `arrayObj` 인스턴스 구조입니다.
    <img src="/images/speciesArrayObj.JPG">    
    
    arrayObj 구성을 보면 prototype이 없으며 &#95;&#95;proto&#95;&#95;만 있습니다.  
    &#95;&#95;proto&#95;&#95;에는 Array 오브젝트의 prototype에 연결된 프로퍼티가 첨부되어 있습니다.  
    Array의 오브젝트 프로퍼티가 연결되어 있지만 arrayObj에는 연결된 프로퍼티가 없습니다.  
    따라서 arrayObj는 Array 오브젝트가 아닌 Array 인스턴스입니다.
    

2.  `arrayObj.slice(1, 3)` 코드는 자바스크립트에서 일반적으로 사용하는 `[1, 2, 3].slice(1, 3)`형태와 다릅니다.
`[1, 2, 3]`으로 `Array` 인스턴스를 생성하여 `arrayObj`에 할당하고, `arrayObj`에 있는 `slice()`를 호출하는 형태 입니다. 

실행하면 `[1, 2, 3]`에서 `2`와 `3`이 반환되어 `sliceOne` 변수에 할당될 것으로 생각할 수 있습니다.  

**하지만, `sliceOne`변수에 할당되는 것은 `Array` 인스턴스 입니다.** 2 와 3이 인스턴스 값으로 설정됩니다.

`arrayObj` 와 `sliceOne` 둘다 `Array` 오브젝트 인스턴스 입니다. 배열의 엘리먼트 값이 달라집니다.


**여기서 중요한 점은 Array 인스턴스(arrayObj)의 slice()를 호출하면 slice()실행 결과가 반영된**  
**Array 인스턴스가 반환된다는 점입니다. Array 인스턴스가 반환되므로 sliceOne 인스턴스를 지정하여 slice()를 호출할 수 있습니다.**


3.  `sliceOne` 인스턴스를 지정하여 `slice()`를 호출하고 마찬가지로 `Array` 인스턴스가 반환됩니다.  
    `Symbol.species`를 이해하려면 먼저 이에 대한 이해가 필요합니다.

* * *

<h2 id="species">species</h2>

**`Symbol.species`는 `constructor`를 반환합니다.**

`Symbol.species`는 static 액세서 프로퍼티로 `getter`만 있고 `setter`는 없습니다.  

`Array`, `Map`, `Set`, `Promise`, `RegExp`, `ArrayBuffer`, `TypedArray` 오브젝트에 `Symbol.species`가 빌트인으로 포함되어 있습니다.

위에 거론된 빌트인 오브젝트를 상속받는 클래스에 `Symbol.species`를 작성하면, 빌트인 오브젝트의 `Symbol.species`가 오버라이드됩니다. 이를 통해 클래스의 `Symbol.species`에서 다른 오브젝트를 반환할 수 있습니다.


*   예를 들어 인스턴스의 slice()를 호출하면 slice()를 호출한 인스턴스를 반환하지만, 클래스에 Symbol.species를 작성하여 인스턴스가 아닌 Array 오브젝트를 반환할 수 있습니다. **(인스턴스 대신 인스턴스의 부모 오브젝트 반환)**

```js
1. class ExtendArray extends Array {  
 static get [Symbol.species]() {  
 return Array;  
 }  
};  
2. let oneInstance = new ExtendArray(1, 2, 3);  
  
3. let twoInstance = oneInstance.slice(1, 2);  
4. console.log(oneInstance instanceof ExtendArray);  
  
5. console.log(twoInstance instanceof Array);  
  
6. console.log(twoInstance instanceof ExtendArray);  
//true  
//true  
//false  
```

[instanceof 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/instanceof)는 생성자의 프로토 타입 속성이 객체의 프로토 타입 체인에 나타나는지 테스트하여 true/false값을 반환합니다.


1.  `ExtendArray` 클래스에서 `Array` 오브젝트를 상속받습니다. 
클래스에 `static` 메서드이면서 `getter`를 사용하여 `Symbol.species`를 작성하였습니다. 
`Array` 오브젝트의 `Symbol.species`가 오버라이드 됩니다.

 인스턴스의 `Array` 오브젝트 메서드를 호출하면 오버라이드된 `Symbol.species`가 호출됩니다.  
 그리고 `return Array;`를 하므로 메서드를 호출한 인스턴스가 반환되지 않고 `Array` 인스턴스가 반환됩니다.
 * `return Array`가 `Array` 오브젝트의 `constructor`를 반환하지만, `return` 문은 오른쪽의 표현식을 평가하고 평가 결과를 반환하므로 `Array` 오브젝트의 `constructor`를 호출하고 그 결과인 `Array` 인스턴스를 반환합니다.


2.  `new` 연산자로 `ExtendArray()`룰 호출하면서 파라미터로 1, 2, 3을 넘겨줬습니다.  
    `ExtendArray` 클래스에서 `Array` 오브젝트를 상속받았으므로 생성한 인스턴스는 `Array` 오브젝트 특성을 갖습니다.


3.  `oneInstance` 인스턴스에 상속받은 `Array` 오브젝트가 존재하므로 `slice()`를 호출할 수 있습니다.  
    `slice()`가 호출되면 `ExtendArray` 클래스에 `getter`로 작성된 `Symbol.species()`가 호출되며,  
    `oneInstance`가 반환되지 않고 `Array` 인스턴스를 생성하여 반환합니다.


4.  new ExtendArray(1, 2, 3)으로 생성한 인스턴스를 oneInstance에 할당했으므로 true가 출력됩니다.


5.  oneInstance.sliceOne(1, 2)를 실행하면 클래스에 작성한 Symbol.species()가 호출됩니다.  
    return Array로 생성한 인스턴스를 twoInstance에 할당했으므로 true가 출력됩니다.


6.  `new ExtendArray()`로 생성한 인스턴스를 `twoInstance`에 할당한 것이 아니라, 
    `Array` 오브젝트로 생성한 인스턴스(`oneInstance`)를 할당했으므로 `false`가 출력됩니다.  
    **이처럼 Symbol.species로 반환되는 인스턴스를 변경할 수 있습니다.**

* * *

<h2 id="return_other_Class">다른 Class 반환</h2>

위 에서 return 문에서 상속받은 Array 오브젝트를 반환하는 형태를 살펴봤습니다.  
여기서는 다른 클래스를 반환하는 형태를 살펴봅니다.

```js other-Class
1. class ExtendOne extends Array{  
 showOne(){  
 console.log("ExtendOne");  
    
 }  
};  
2. class ExtendTwo extends Array{  
 static get [Symbol.species]() {  
 return ExtendOne;  
 }  
 showTwo(){  
 console.log("ExtendTwo");  
 }  
};  
3. let twoInst = new ExtendTwo(10, 20, 30);  
let threeInst = twoInst.filter(value => value > 10);  
console.log(threeInst);  
  
4. threeInst.showOne();  
console.log(threeInst.showTwo);  
/*  
ExtendOne(2)  
 0: 20  
 1: 30  
 length: 2  
 __proto__: Array  
*/  
// ExtendOne  
// undefined  
```

1.  `ExtendOne` 클래스에서 `Array` 오브젝트를 상속받습니다.  
    이 클래스를 아래에 작성한 2. `ExtendTwo` 클래스의 [Symbol.species]에서 반환합니다.


2.  `[Symbol.species]()`에서 `ExtendOne`을 `return` 하므로 클래스를 반환하는 것처럼 보입니다만,  
    클래스의 `constructor`를 반환하므로 인스턴스로 생성하여 반환하게 됩니다.  
    이와 같이 `[Symbol.species]()`를 호출한 인스턴스가 아닌 다른 인스턴스로 반환할 수 있습니다.


3.  `twoInst.filter()`가 호출되면, 우선 `twoInst` 인스턴스를 생성한 `ExtendTwo` 클래스에서 `[Symbol.species]` 작성 여부를 체크합니다. 존재하면 클래스의 `[Symbol.species]()`를 호출하고 존재하지 않으면 상속받은 `Array` 오브젝트의 `[Symbol.species]()`를 호출합니다.


4.  호출된 `[Symbol.species]()`에서 `ExtendOne` 인스턴스를 반환하여 `ExtendTwo` 클래스에서 `Array` 오브젝트를 상속받았으므로 [filter()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 메서드를 호출할 수 있습니다.  
    ExtendOne 클래스에서 filter() 메서드를 지원하지 않는 오브젝트를 상속받으면 Error가 발생합니다.

 `filter()` 실행 결과 `twoInst`에 설정하지 않고 반환할 인스턴스에 할당합니다. 즉 `threeInst` 인스턴스에 할당됩니다. 따라서 `twoInst` 인스턴스 값 [10, 20, 30]이고 `threeInst` 인스턴스 값은 [20, 30]입니다.


5.  `return ExtendOne`으로 인해 `ExtendOne` 인스턴스가 `threeInst`에 설정되므로,  
    `threeInst` 인스턴스의 `showOne()`을 호출할 수 있습니다. `showTwo`는 존재하지 않으므로 `undefined`가 출력됩니다.

* * *

<h2 id="return_null">null 반환</h2>

**`[Symbol.species]()`에서 `null`을 반환하면 디폴트[Symbol.species] ()가 호출됩니다.**

```js
class ExtendOne extends Array{  
 static get [Symbol.species]() {  
 return null;  
 }  
};  
1. let oneInst = new ExtendOne(10, 20, 30);  
let arrayInst = oneInst.filter(value => value > 10);  
  
2. console.log(arrayInst instanceof Array);  
3. console.log(arrayInst instanceof ExtendOne);  
// true  
// false  
```

1.  `oneInst.filter()`를 호출하면 `ExtendOne` 클래스의 `[Symbol.species]()`가 호출되며 `null`를 반환합니다.  

 null이 반환되면 디폴트 [Symbol.species] ()가 호출됩니다.  
 `ExtendOne` 클래스에 작성하지 않았지만 상속받은 `Array` 오브젝트에 있으므로 [Symbol.species] ()가 호출되며  
 `Array` 인스턴스를 생성하여 반환합니다. 따라서 `filter()`를 실행할 수 있습니다.


2.  `oneInst.filter()`를 실행하면 `[Symbol.species]()`에서 `Array` 인스턴스를 반환하여 `arrayInst`에 할당하므로 `true`값이 출력됩니다.


3.  `[Symbol.species]()`에서 `Array` 인스턴스를 반환하므로 `arrayInst`는 `ExtendOne` 클래스의 인스턴스가 아닙니다. //false

* * *

<h2 id="toPrimitive">toPrimitive</h2>

**오브젝트를 프리미티브(원시값) 타입으로 변환합니다.**

자바스크립트의 프리미티브 값은 `number`, `string`, `boolean`, `undefined`, `null` 그리고 `symbol`입니다.

+연산자는 앞뒤의 값 타입에 따라 값을 더하거나 연결합니다. (1+2)는 3이되지만 (1+”2”)는 12가 됩니다.  
반면 곱하기(*), 나누기(/), 빼기(-)는 연산만 하고 연결은 하지 않습니다.

<mark>연산 대상이 Number 타입이 아닐 경우 엔진의 ToPrimitive 모듈을 기준으로 값을 변환합니다. 예를 들어, 숫자에 true를 더하면 1로 변환하여 더하고, 문자열에 true를 더하면 "true"로 변환하여 연결합니다. 이때 Symbol.toPrimitive()로 ToPrimitive를 오버라이드하여 엔진의 변환 기준을 변경할 수 있습니다.</mark>(엔진의 디폴트 변환 값을 개발자 코드로 변경)

------
### 세 가지 모드

`Symbol.toPrimitive()` 에서 값을 변환하는 기준은 이를 호출하는 형태에 따라 결정됩니다.

엔진은 호출한 곳의 형태에 따라 Symbol.toPrimitive(hint) 파라미터에 세 가지 모드(Mode)를 설정합니다.  
~~개발자가 작성하는 것이 아니라 엔진이 다음 기준으로 설정하는 것입니다.~~

1.  `Number` 환경이면 `“number”`를 toPrimitive(hint) 파라미터에 설정합니다.
    

2.  `String` 환경이면 `“string”`을 toPrimitive(hint) 파라미터에 설정합니다.
    

3.  `Number` 와 `String` 환경이 아니면 toPrimitive(hint) 파라미터에 `“default”`를 설정합니다.
    
```js
let obj = {  
 [Symbol.toPrimitive](hint){  
 if (hint === "number"){  
 return 30;  
 };  
 if (hint === "string"){  
 return "문자열";  
 };  
 return "디폴트";  
 }  
};  
1. console.log("1:", 20 + obj); // 1: 20디폴트  
2. console.log("2:", 20 * obj); // 2: 600  
  
3. console.log("3:", obj + 50); // 3: 디폴트50  
4. console.log("4:", +obj + 50);// 4: 80  
5. console.log("5:", `${obj}` + 123); // 5: 문자열123  
```

1.  (20 + obj)와 같이 오브젝트가 연산 대상이면, 자동으로 obj 오브젝트의 [Symbol.toPrimitive] ()가 호출됩니다. 파라미터에 “default”가 설정됩니다. 함수에서 “디폴트”를 반환하여 문자열을 연결하는 형태가 되어 “20디폴트”가 출력됩니다.


2.  곱하기(*)를 사용하였으므로 파라미터에 “number”가 설정됩니다. 함수에서 30을 반환하며 20을곱해 600이 출력됩니다.


3.  (20 + obj)에서 파라미터에 “default”가 설정되듯이 마찬가지로 “default”가 설정됩니다.  
    함수에서 “디폴트”를 반환하여 50을 연결하므로 “디폴트50”이 출력됩니다.


4.  +obj 에서 +는 단항+연산자로 피연산자를 Number 타입으로 변환합니다.  
    함수에서 30을 반환하며 50을더해 80이 출력됩니다.


5.  템플릿을 사용한 형태로 파라미터에 “string”이 설정됩니다. 함수에서 “문자열”을 반환하여  
    ${obj}가 “문자열”로 변환됩니다. 123을 연결하여 “문자열123”이 출력됩니다.

* * *

<h2 id="Symbol_iterator">이터레이터</h2>

**`Symbol.iterator()`는 이터레이터 오브젝트를 생성하여 반환합니다.**


`Symbol.iterator`는 `String`, `Array`, `Map`, `Set`, `TypedArray` 오브젝트의 `prototype`에 연결되어 있습니다.  
오브젝트의 [Symbol.iterator]를 호출하면 이터레이터 오브젝트를 생성하여 반환합니다.

Object 오브젝트에는 Symbol.iterator가 없습니다만, 개발자 코드로 구현할 수 있습니다.

------
### Array.prototype[Symbol.iterator]

배열 처리를 위한 이터레이터 오브젝트를 생성하여 반환합니다. 배열 엘리먼트를 하나씩 처리할 수 있습니다.

```js iterator-Array
let numberArray = [10, 20];  
for (let value of numberArray){  
 console.log(value);  
};  
let iteratorObj = numberArray[Symbol.iterator]();  
  
console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
//10  
//20  
/*Object  
 value: 10  
 done: false  
 __proto__: Object*/  
/*Object  
 value: 20  
 done: false  
 __proto__: Object*/  
/*Object  
 value: undefined  
 done: true  
 __proto__: Object*/  
```

*   numberArray는 Array 오브젝트 인스턴스 구조입니다.  
    Symbol.iterator()를 호출할 수 있습니다.
    
*   numberArray[Symbol.iterator] ()는 numberArray 인스턴스의 Symbol.iterator()를 호출하는 것으로 이터레이터 오브젝트를 생성하여 반환합니다. 이터레이터 오브젝트를 사용하여 배열 엘리먼트를 하나씩 처리할 수 있습니다.
    
*   next()를 호출할 때마다 배열 엘리먼트 값을 {value: 10, done:false} 형태로 반환합니다.
    
------
### String.prototype[Symbol.iterator]

문자열 처리를 위한 이터레이터 오브젝트를 반환합니다.

문자열을 문자 단위로 하나씩 처리할 수 있습니다.

```js iterator-string
let stringValue = "1A";  
for (let value of stringValue) {  
 console.log(value);  
}  
  
let iterObj = stringValue[Symbol.iterator]();  
  
console.log(iterObj.next());  
console.log(iterObj.next());  
console.log(iterObj.next());  
// 1  
// A  
  
// Object {value: "1", done: false}  
// Object {value: "A", done: false}  
// Object {value: undefined, done: true}  
```

*   “1A”를 1과 A로 분리하여 for-of문을 실행합니다. 문자 단위로 반복할 수 있는 것은  
    String.prototype에 Symbol.iterator가 있기 때문입니다.  
    for-of문에서 stringValue를 String 오브젝트로 생성하고 그 안에 Symbol.iterator()를 사용합니다.
    

*   stringValue[Symbol.iterator] ();형태로 호출하면 이터레이터 오브젝트를 반환합니다.  
    이터레이터 오브젝트를 사용하여 문자열을 문자 단위로 하나씩 처리할 수 있습니다.
    

*   next()를 호출할 때마다 문자열의 문자를 value 프로퍼티에 설정하고 {value: “1”, done: false}형태로 반환합니다.
    

------
### Object 이터레이션

Object 오브젝트에는 기본적으로 Symbol.iterator를 갖고 있지 않습니다. (for-of 등 반복처리불가.)

<mark>Object 오브젝트에 Symbol.iterator를 작성하면 반복 처리를 할 수 있습니다.</mark>

```js iterator-Object
1. let obj = {  
 [Symbol.iterator](){  
    2. return {  
        maxCount: 2,  
        count: 0,  
        next(){   
            if (this.count < this.maxCount){  
            return {value: this.count++, done: false};  
            }  
            return {value: undefined, done: true};  
            }  
        }  
    }  
};  
  
3. let iteratorObj = obj[Symbol.iterator]();  
console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
console.log(iteratorObj.next());  
```

1.  Object 오브젝트를 반복처리 하기 위해서 [Symbol.iterator] ()를 obj prototype에 작성해줬습니다.


2.  호출된 obj[Symbol.iterator] ()의 return으로 반환된 오브젝트를 iteratorObj에 할당합니다.


3.  iteratorObj에 next()가 있으므로 iteratorObj.next() 형태로 호출할 수 있습니다.

* * *

<h2 id="Symbol_generator">제너레이터</h2>

`Object` 오브젝트에 `Symbol.iterator`를 제너레이터 함수로 작성하면, 이터레이터로 반복할 때 마다 `yield`를 수행합니다.

```js
1. obj[Symbol.iterator] = function*(){  
 yield 10;  
 yield 20;  
 yield 30;  
};  
2. let result = [...obj];  
console.log(result);  
//[10, 20, 30]  
```

1.  obj 오브젝트의 [Symbol.iterator]를 제너레이터 함수로 작성하였습니다.  
    이터레이터로 반복할 때 마다 `yield` 표현식을 평가하여 값을 반환합니다.


2.  대괄호 [] 안에 `spread` 연산자로 `obj` 오브젝트를 작성했습니다. [...obj]를 시작하면  
    엔진에서 `obj`에 [Symbol.iterator] 작성 여부를 체크합니다. 작성되있으므로  
    [Symbol.iterator] ()가 호출되며 이터레이터 오브젝트를 생성하여 반환합니다.  
    이터레이터가 반복될 때마다 `yield`에서 반환한 값을 배열에 첨부합니다.  
    반복이 끝나면 생성된 배열을 반환합니다.

* * *

<h2 id="Symbol_asyncIterator">asyncIterator(): 비동기 반복</h2>

`Symbol.asyncIterator`는 오브젝트의 `asyncIterator` 기본 값을 지정합니다.  
`asyncIterator` 프로퍼티가 오브젝트에 설정되면 `await for of` 루프(`loop`)에서 비동기 반복할 수 있습니다.

오브젝트가 비동기 반복이 가능하려면 `Symbol.asyncIterator` 프로퍼티 키가 존재해야합니다.

```js
const myAsyncIterable = {  
    async* [Symbol.asyncIterator]() {  
        yield "hello";  
        yield "async";  
        yield "iteration!";  
    }  
};  
  
(async () => {  
    for await (const x of myAsyncIterable) {  
    console.log(x);  
        // "hello"  
        // "async"  
        // "iteration!"  
 }  
})();  
```

*   **오브젝트의 [Symbol.asyncIterator] 속성을 설정해줌으로써 비동기 이터러블을 커스텀하여 정의할 수 있습니다.**


**현재 기본적으로 `[Symbol.asyncIterator]` 프로퍼티가 `built-in` 되어 있는 `JavaScript` 오브젝트는 없습니다.**

하지만 `WHATWG`(Web Hypertext Application Technology Working Group, WHATWG)에서는  
최근에 [Symbol.asyncIterator]가 스펙에 기준에 도달함으로써 비동기 반복이 가능한 첫 번째 built-in 오브젝트로 설정되었습니다.

[WHATWG 위키](https://ko.wikipedia.org/wiki/WHATWG)  
[WHATWG 공식홈페이지](https://whatwg.org/)

* * *

<h2 id="Symbol_match">match(): match 결과 반환</h2>

`String` 오브젝트에서 정규 표현식을 사용할 수 있는 메서드  
`match()`, `replace()`, `search()`, `split()` 에 **대응하는**

`Symbol` 오브젝트 `Symbol.match()`, `Symbol.replace()`, `Symbol.search()`, `Symbol.split()`이 있습니다.

*   `String.prototype.match()`가 호출되면 먼저 오브젝트에서 `Symbol.match` 작성 여부를 체크합니다.  
    작성돼있다면 오브젝트의 `Symbol.match()`를 호출합니다. `String.prototype.match()`는 호출되지 않습니다.

```js
1. console.log("1", "Sports".match(/s/));  
// 1: [0: "5", index: 5, input: "Sports"]  
  
class MatchCheck {  
 constructor(base) {  
 this.base = base;  
 }  
 2. [Symbol.match](target) {  
 return this.base.indexOf(target) >= 0;  
 }  
}  
3. let instMatch = new MatchCheck("sports");  
4. console.log("2:", "po".match(instMatch));  
// 2: true  
```

1.  “Sports”에 패턴/s/를 매치하면 매치 결과를 배열로 반환합니다. “Sports”가 문자열이므로  
    엔진이 String.prototype에 연결된 프로퍼티로 String 인스턴스를 생성합니다.  
    생성한 인스턴스에 “Sports”를 설정한 후 match()를 호출하면서 /s/를 파라미터로 넘겨줍니다.


2.  Symbol.match()의 target 파라미터에 설정된 문자열이 this.base에 포함되어 있으면 true를 반환하고, 아니면 false를 반환합니다.


3.  new MatchCheck(“sports”);를 실행하면 constructor가 호출되고 파라미터로 넘겨 준 “sports”가 this.base에 설정됩니다.


4.  “po”.match(instMatch)가 호출되면 파라미터에 작성한 instMatch 인스턴스에서 Symbol.match의 작성 여부를 체크합니다. 존재하므로 Symbol.match(target)을 호출하면서 “po”를 파라미터 값으로 넘겨줍니다.  
    Symbol.match()에서 “sports” 와 “po”로 match를 행하며, “po”가 있으므로 true를 반환합니다.


<h2 id="Symbol_matchAll">matchAll()</h2>

`Symbol.matchAll` 은 `String` 오브젝트를 정규표현식으로 평가하여  
반환결과를 이터레이터 오브젝트로 반환합니다.

Symbol.matchAll은 [String.prototype.matchAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)메서드에 의해 호출 됩니다.

+참고 [RegExp.prototype[@@matchAll] ( )](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/@@matchAll)

```js
let re = /[0-9]+/g;  
let str = '2016-01-02|2019-03-07';  
let result = re[Symbol.matchAll](str);  
  
console.log(Array.from(result, x => x[0]));  
// expected output: Array ["2016", "01", "02", "2019", "03", "07"]  
```