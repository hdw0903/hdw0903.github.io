---
title: Symbol 메서드 -ECMAScript
date: 2020-04-09 10:34:23
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - Symbol method
  - [[key]]
  - [[symbol]]
  - Symbol.for
  - Well-Known Symbol
  - Symbol 레지스트리
  - Symbol. keyFor
  - Symbol. valueOf
  - Symbol. toString
  - getOwnPropertySymbols
  - JSON.stringify

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


*   Symbol 메서드
    *   [for(): Symbol 값 저장](/2020/04/09/Symbol%20메서드%20-ECMAScript/#Symbol_for)
        *   글로벌(전역) Symbol 레지스트리
        *   Symbol의 대표적 사용 형태 세 가지
    *   [keyFor(): key 값 변환](/2020/04/09/Symbol%20메서드%20-ECMAScript/#Symbol_keyFor)
    *   [toString(): 문자열로 변환](/2020/04/09/Symbol%20메서드%20-ECMAScript/#Symbol_toString)
    *   [valueOf(): Symbol 프리미티브 값](/2020/04/09/Symbol%20메서드%20-ECMAScript/#Symbol_valueOf)
    *   [getOwnPropertySymbols(): Symbol 프로퍼티 반환](/2020/04/09/Symbol%20메서드%20-ECMAScript/#getOwnPropertySymbols)
    *   [JSON.stringify(): JSON 형태로 변환](/2020/04/09/Symbol%20메서드%20-ECMAScript/#JSON_stringify)
        *   주석을 프로퍼티 key 값으로 활용

<!-- more -->

* * *

<h2 id="Symbol_for">for(): Symbol 값 저장</h2>

**글로벌 `Symbol` 레지스트리(`registry`)에 `Symbol` 값을 저장합니다.**

> Symbol.for(key);

*   key  
    String, 필수. 심볼의 키 (심볼의 설명을 위해서도 쓰입니다).
    
*   반환 값  
    해당 키에 해당하는 심볼이 있다면 반환, 없으면 새로운 심볼을 만들고 반환합니다.
    

<mark>Symbol은 두 개의 스코프에 저장될 수 있습니다.  
앞에서 다루었던 Symbol()은 Symbol을 생성한 스코프에 Symbol 값이 설정됩니다.</mark>

반면, `Symbol.for()`은 글로벌 `Symbol` 레지스트리에 {key: value} 형태로 저장됩니다.  
파라미터에 지정한 문자열이 `key`가 되고 생성한 `Symbol` 값이 `value`가 됩니다.

**Symbol.for()는 매 호출마다 새로운 심볼을 만들지 않고 현재 레지스트리에 해당 키를 가진 심볼이 있는지 먼저 검사를 합니다.**

**레지스트리에 key가 있다면 그 심볼(value)을 반환합니다. 만약 key에 해당하는 심볼이 없다면 Symbol.for()는 새로운 전역 심볼을 만들어 반환합니다.**

글로벌 `Symbol` 레지스트리는 `Symbol` 값을 공유하기 위한 영역입니다.  
다른 자바스크립트 프레임워크에서도 공유할 수 있습니다.

------
### 글로벌(전역) Symbol 레지스트리

글로벌(전역) `Symbol` 레지스트리는 다음과 같은 가진 기록 구조를 가진 리스트입니다.  
초기 값은 비어 있습니다.

*   `[[[key]]` : 심볼을 구분하기 위해 사용되는 문자열 키
*   `[[[symbol]]` : 전역으로 저장되는 심볼

------
### Symbol의 대표적 사용 형태 세 가지

1.  `Symbol()` : Symbol 값을 생성하여 스코프 안에서 사용합니다.

2.  `Symbol.for()` : 글로벌 Symbol 레지스트리에 저장되며 전체 프로그램에서 사용합니다.

3.  `Well-Known Symbol` : 빌트인 Symbol 프로퍼티로 오버라이드하여 기능을 추가 및 변경 합니다.

```js Symbol.for()
1. console.log(Symbol.for("sports")); //새로운 전역심볼 생성  
2. console.log(Symbol.for("sports"));// 이미 만들어진 심볼을 검색  
  
3. console.log(Symbol.for("ABC") === Symbol.for("ABC")); //전역심볼  
4. console.log(Symbol.for("DEF") === Symbol("DEF")); //지역 심볼  
5. console.log(Symbol.for(true)); // 문자열이 아님  
// Symbol(sports)  
// Symbol(sports)  
  
// true  
// false  
// Symbol(true)  
```

1.  `Symbol` 값을 생성하여 글로벌 `Symbol` 레지스트리에 {key: value} 형태로 저장됩니다. 

    `key`: 파라미터로 넘겨준값 (“sports”)  
    `value`: 생성한 Symbol 값 ( Symbol.for(“sports”) )  
    생성한 `Symbol`을 출력하면 `for`을 제외하고 `Symbol(“sports”)`로 출력됩니다.


2.  바로 위에서 `sports`를 글로벌 `Symbol` 레지스트리에 저장했으므로  
    저장된 `sports` 키의 `value`를 검색하여 반환합니다.  
    마찬가지로 `for`을 제외한 `Symbol(“sports”)` 형태로 출력됩니다.


3.  `Symbol` 값을 생성하여 `“ABC”`를 프로퍼티 키로 글로벌 `Symbol` 레지스트리에 저장합니다.  
    오른쪽 `Symbol.for(“ABC”)`에서 글로벌 `Symbol` 레지스트리에 프로퍼티 키 `“ABC”`를 검색하여  
    생성되있는 `Symbol` 값을 반환합니다. 왼쪽과 오른쪽 `Symbol` 값이 같으므로 `true`가 반환됩니다.


4.  `Symbol.for()`은 글로벌 `Symbol` 레지스트리에 저장되고  
    `Symbol()`은 스코프에 저장되면서 둘은 공유되지 않습니다. `false`가 반환됩니다.


5.  파라미터에 `true` 와 같이 문자열이 아닌 값을 던져주면 문자열로 변환하여 프로퍼티 키로 사용합니다.  
    
    `key`: “true”  
    `value`: Symbol.for(“true”)  
    출력시 Symbol(true)로 출력됩니다.

* * *

<h2 id="Symbol_keyFor">keyFor(): key 값 변환</h2>

**글로벌 `Symbol` 레지스트리에서 프로퍼티 키 값을 반환합니다.**

> Symbol.keyFor(Symbol)

파라미터에 글로벌 `Symbol` 레지스트리에 저장한 `Symbol`을 지정합니다.  
`Symbol`이 아니면 `TypeError`가 발생합니다.

글로벌 `Symbol` 레지스트리에 `Symbol`이 존재하면 프로퍼티 키를 반환합니다.  
`Symbol`이 존재하지 않으면 `undefinded`를 반환합니다.

```js keyFor()
let globalSym = Symbol.for('foo'); // 글로벌 Symbol 레지스트리에 Symbol 값을 생성합니다.  
console.log(Symbol.keyFor(globalSym)); // "foo"  
  
let localSym = Symbol();  
console.log(Symbol.keyFor(localSym)); // undefined  
```

*   Symbol()은 글로벌 스코프에서 생성하더라도 글로벌 Symbol 레지스트리에는 등록되지 않습니다.  
    undefined가 반환됩니다.

* * *

<h2 id="Symbol_toString">toString(): 문자열로 변환</h2>

**`Symbol`을 문자열로 변환하여 반환합니다.**  
`Symbol` 값이 아닌 `Symbol` 값을 생성한 `Symbol()` 형태를 문자열로 반환합니다.  
`Well-Known Symbol`, 글로벌 `Symbol` 레지스트리도 변환됩니다.

> Symbol.prototype.toString()

생성한 `Symbol` 값을 문자열에 연결하면 `TypeError가` 발생하지만, `toString`으로 변환하여 연결하면 에러가 나지 않고 문자열로 연결됩니다. `toString()` 메서드가 `Symbol.prototype`에 연결되어 있으므로 `Symbol()` 또는 `Symbol.for()`로 생성한 `Symbol`을 사용합니다.

```js toString()
console.log("1:", Symbol("123").toString());  
// 1: Symbol(123)  
  
console.log("2:", Symbol.for("ABC").toString());  
// 2: Symbol(ABC)  
  
console.log("3:", Symbol.iterator.toString());  
// 3: Symbol(Symbol.iterator)  
```

1.  2.  `Symbol` 값이 아닌 `Symbol` 값을 생성할 때의 형태를 문자열로 반환합니다.  
        `Symbol.for()`도 `Symbol()`과 같지만 출력할 때 `for`을 제외시킵니다.


3.  `Well-Known Symbol`을 문자열로 반환합니다. `Symbol` 값을 생성할 때의 형태  
    `Symbol.iterator`가 `Symbol()`의 파라미터에 표시됩니다.

* * *

<h2 id="Symbol_valueOf">valueOf(): Symbol 프리미티브 값</h2>

`valueOf()` 메서드는 `Symbol` 오브젝트의 프리미티브 값을 반환합니다.

> Symbol.prototype.valueOf()

모든 빌트인 오브젝트에 `valueOf()` 메소드가 있으며 프리미티브 값을 반환합니다.  
예시로 New Number(123)으로 생성한 인스턴스를 valueOf()로 실행하면 123이 반환됩니다.

`Symbol`도 빌트인 오브젝트이지만 반환 값 형태가 다릅니다.  
Symbol(123)으로 생성한 Symbol로 valueOf()를 실행하면 Symbol 값이 반환되지 않고,  
Symbol 값을 생성할 때의 “Symbol(123)”이 반환됩니다.

* * *

<h2 id="getOwnPropertySymbols">getOwnPropertySymbols(): Symbol 프로퍼티 반환</h2>

`getOwnPropertySymbols()`는 반환 대상이 `Symbol`인 `Object` 오브젝트 메서드 입니다.  
배열로 반환합니다.

> Object.getOwnPropertySymbols()

*   파라미터  
    추출 대상 Object
    
*   반환  
    지정한 Object 에서 Symbol 이외의 프로퍼티는 반환하지 않고 오직 Symbol만 배열로 반환합니다.
    

```js getOwnPropertySymbols
let bookObj = {book: 123};  
bookObj[Symbol("one")] = 10;  
bookObj[Symbol.for("two")] = 20;  
  
let names = Object.getOwnPropertyNames(bookObj);  
console.log("1:", names);  
// 1: ["book"]  
  
let symbolList = Object.getOwnPropertySymbols(bookObj);  
console.log("2:", symbolList);  
// 2: [Symbol(one), Symbol(two)]  
  
for (let sym of symbolList){  
 console.log(sym.toString(), bookObj[sym]);  
// Symbol(one) 10  
// Symbol(two) 20  
}  
  
let emptyList = Object.getOwnPropertySymbols({});  
console.log("5:", emptyList.length);  
// 5: 0  
```

*   `{book:123}`으로 `Object` 오브젝트를 생성하여 `bookObj`에 할당합니다.

    `Symbol(“one”)`으로 `Symbol`을 생성하고, `bookObj` 오브젝트의 `symbol-keyed` 프로퍼티로 사용하여 10을 할당합니다.  

    이때 `bookObj` 오브젝트는 `{book: 123, Symbol(“one”): 10}` 형태가 됩니다.

    글로벌 `Symbol` 레지스트리에 `“two”`를 프로퍼티 키로 `Symbol` 값을 등록하고, `bookObj` 오브젝트에 `symbol-keyed` 프로퍼티로 사용하여 `20`을 할당합니다.

    이때 `bookObj` 오브젝트는 `{book: 123, Symbol(“one”): 10, Symbol(“two”): 20}` 형태가 됩니다.


1.  `getOwnPropertyNames()` 파라미터에 지정한 `bookObj` 오브젝트에서 프로퍼티 이름을 배열로 반환합니다.  

    이때 `Symbol`은 반환되지 않습니다. 즉 `[“book”]`만 출력됩니다.  
    `for-in`문으로 전개해도 `Symbol`은 열거되지 않습니다.


2.  `getOwnPropertySymbols()` 파라미터에 지정한 `bookObj` 오브젝트에서  
    `Symbol-keyed` 프로퍼티를 배열로 반환합니다. `Symbol`이 아닌 프로퍼티는 반환하지 않습니다.  
    `[Symbol(one), Symbol(two)]`가 반환됩니다.


3.  4.  `getOwnPropertySymbols()`에서 배열로 반환해 줌으로 `for-of`문으로 전개할 수 있습니다.  
        `Symbol`이 `sym` 변수에 설정되어 `String`으로 변환되어 출력됩니다.


5.  `getOwnPropertySymbols()` 파라미터에 빈 `Object` 를 지정하면 빈 배열로 반환합니다.  
    `length` 값이 `0` 이 됩니다.

* * *

<h2 id="JSON_stringify">JSON.stringify(): JSON 형태로 변환</h2>

**자바스크립트 형태를 JSON 형태의 문자열로 변환합니다.**

> JSON.stringify(value[, replacer[, space]])

*   value  
    JSON 문자열로 변환할 값.
    

*   replacer 선택적 파라미터  
    문자열화 동작 방식을 변경하는 함수, 혹은 JSON 문자열에 포함될 값 객체의 속성들을 선택하기 위한 화이트리스트(whitelist)로 쓰이는 String 과 Number 객체들의 배열. 이 값이 null 이거나 제공되지 않으면, 객체의 모든 속성들이 JSON 문자열 결과에 포함된다.
    

*   space 선택적 파라미터  
    가독성을 목적으로 JSON 문자열 출력에 공백을 삽입하는데 사용되는 String 또는 Number 객체.
    
    *   Number 라면, 공백으로 사용되는 스페이스(space)의 수를 나타낸다; 이 수가 10 보다 크면 10 으로 제한된다. 1 보다 작은 값은 스페이스가 사용되지 않는 것을 나타낸다.
    *   String 이라면, 그 문자열(만약 길이가 10 보다 길다면, 첫번째 10 개의 문자)이 공백으로 사용된다. 
    이 매개 변수가 제공되지 않는다면(또는 null 이면), 공백이 사용되지 않는다.


*   반환 값  
    주어진 값과 대응하는 JSON 문자열.
    

`JSON.stringify()`로 자바스크립트 형태의 `{key: value}`를 `JSON` 형태의 문자열로 변환하면,  
`Symbol-keyed` 프로퍼티로 작성한 `Symbol`은 변환에서 제외됩니다.  
`Symbol` 값을 외부에 노출시키지 않으려는 의도이지만, 에러 역시 나지 않으므로 주의해야 합니다.

```js JSON.stringify
let result = JSON.stringify({[Symbol("one")]: "1"});  
console.log(result);  
console.log(typeof result);  
  
console.log(JSON.stringify({[Symbol.for("two")]: "2"}));  
// "{ }"  
// string  
// "{ }"  
```

*   위와 같이 `Symbol(“one”)`으로 생성한 `Symbol`을 `Object` 오브젝트의 `symbol-keyed` 프로퍼티로 사용한 것을  
    
    `JSON.stringify()`로 변환하면, **`Symbol`이 변환에서 제외되어 중괄호 {}만 문자열로 반환됩니다.**

------
### 주석을 프로퍼티 key 값으로 활용

`Symbol` 주석을 프로퍼티 키로 활용하여 `JSON.stringify()`에서 `Symbol`이 제외되는 것을 방지하기 위한  
하나의 코드작성 법입니다.

```js JSON.stringify() Symbol 제외 방지
let bookObj = {};  
bookObj[Symbol("one")] = 10;  
bookObj[Symbol.for("two")] = 20;  
  
1. let symbolList = Object.getOwnPropertySymbols(bookObj);  
console.log(symbolList);  
  
let first, second, key, keyValue = {};  
2. for (let sym of symbolList){  
 3. key = Symbol.keyFor(sym);  
 4. if (key){  
 keyValue[key] = bookObj[sym];  
 } 5. else {  
 //Symbol(one)  
 first = /^Symbol[(]/[Symbol.replace](sym.toString(), "");  
 second = /[)]$/[Symbol.replace](first, "");  
 6. keyValue[second] = bookObj[sym];  
 }  
};  
7. console.log(JSON.stringify(keyValue));  
// [Symbol(one), Symbol(two)]  
// {"one": 10, "two": 20}  
```

*   빈 오브젝트 bookObj에 Symbol(one) 과 Symbol.for(two)로 생성한 Symbol을  
    Symbol-keyed 프로퍼티로 사용하여 프로퍼티 값을 설정해 줬습니다.  
    {Symbol(“one”): 10, Symbol(“two”): 20} 형태가 됩니다.


1.  getOwnPropertySymbols() 파라미터의 bookObj 오브젝트에서 반환받으면  
    [Symbol(one), Symbol(two)]가 출력됩니다. symbolList 변수에 할당합니다.


2.  for-of 문으로 symbolList를 반복하면 배열의 순서대로 sym 변수에 할당됩니다.


3.  Symbol.keyFor(sym)을 실행하면 글로벌 Symbol 레지스트리에 등록한 Symbol 키를 반환받습니다.  
    따라서 Symbol(one)은 undefined를 Symbol.for(two)는 “two”를 반환합니다.


4.  Symbol.for(“two”)가 for-of 문의 sym에 설정될때 실행됩니다.  
    bookObj 오브젝트의 sym으로 값을 구하면 20이 반환되며, 글로벌 Symbol 레지스트리에 “two”가 프로퍼티 키로 등록되어 있으므로 {two: 20}형태가 됩니다.  
    ~~Symbol.for()로 생성한 Symbol은 프로퍼티 키와 값을 갖고 있으므로 {key: value}형태를 만들수 있습니다.~~


5.  Symbol(“one”)이 for-of 문의 sym에 설정될 때 실행됩니다.  
    “one”을 프로퍼티 키로 사용하기 위해 정규 표현식으로 “one”이외의 문자를 빈 문자열로 대체합니다.


6.  bookObj 오브젝트에서 Symbol(“one”)으로 생성한 값으로 프로퍼티 값을 구하면 10이 반환되며,  
    second 변수 값이 “one”이므로 {one: 10}형태가 됩니다. for-of문 반복을 완료하면  
    {one: 10, two: 20} 형태가 됩니다.


7.  파라미터에 keyValue를 지정하여 JSON.stringify()를 실행하면 {“one”: 10, “two”: 20}으로 변환됩니다.  
    Symbol() 주석을 유일하게 지정하면 {key: value} 형태로 Symbol을 변환할 수 있습니다.  
    Symbol()로 생성한 값을 오브젝트의 프로퍼티 키로 사용하고 이를 서버로 전송하려면 계획적인 접근이 필요합니다.
