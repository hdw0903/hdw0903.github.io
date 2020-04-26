---
title: Strict mode -ECMAScript
date: 2020-03-16 10:04:46
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
---

* * *

### Strict mode

strict mode는  
JavaScript 코드가 “엄격 모드”에서 실행되도록 정의합니다.

“strict mode”지시문은 ECMAScript 버전 5에서 새로 추가되었습니다.

IE9 이하를 제외한 모든 최신 브라우저는 strict mode를 지원합니다.

가끔 엄격하지 않은 기본값을  
“느슨한 모드(sloppy mode)”라고 부르기도 합니다.  
공식적인 용어는 아니지만 혹시 모르니 알아두세요.

`"strict mode"지시문은 script 나 function의 시작 부분에서만 인식됩니다.`

<!-- more -->

*   전체 스크립트 적용 구문

```js
//스크립트 파일 첫번째 줄  
"use strict";  
```

*   function에 적용하는 구문
```js
function strict() {  
 // 함수-레벨 strict mode 문법  
 "use strict";  
```

* * *

#### 1.  strict mode를 사용하면 선언되지 않은 변수를 사용할 수 없습니다.

```js
"use strict";  
myFunction();  
  
function myFunction() {  
 x = 3.14; // x를 선언해 주지 않았기 때문에 오류가 발생합니다.  
}  
y= 3; // y역시 선언되지 않았기에 오류가 발생합니다.  
```

strict mode는 이전에 허용 된 “잘못된 구문”을 실제 오류로 변경합니다.

또한 선언되지 않은 변수를 사용하지 못하게하는 것과 같이  
더 깨끗한 코드를 작성하는 데 도움이됩니다.  
“strict mode”는 문자열이므로 IE 9는 이해하지 않아도 오류를 발생시키지 않습니다.

예를 들어,  
일반적인 JavaScript에서 변수 이름을 잘못 입력하면 새로운 전역 변수가 만들어집니다.  
`strict mode에서는 실수로 전역 변수를 만들 수 없습니다.`

* * *

#### 2.  NaN 은 쓸 수 없는 전역 변수입니다.  
NaN 에 할당하는 일반적인 코드는 아무 것도 하지 않습니다.  
개발자도 아무런 실패 피드백을 받지 않습니다.

엄격 모드에서 NaN 에 할당하는 것은 예외를 발생시킵니다.  
일반 코드에서 조용히 넘어가는 모든 실패에 대해 (쓸 수 없는 전역 또는 프로퍼티에 할당, getter-only 프로퍼티에 할당, 확장 불가 객체에 새 프로퍼티 할당) 엄격 모드에서는 예외를 발생시킵니다.

```js
"use strict";  
  
// 쓸 수 없는 프로퍼티에 할당  
var undefined = 5; // TypeError 발생  
var Infinity = 5; // TypeError 발생  
  
// 쓸 수 없는 프로퍼티에 할당  
var obj1 = {};  
Object.defineProperty(obj1, "x", { value: 42, writable: false });  
obj1.x = 9; // TypeError 발생  
  
// getter-only 프로퍼티에 할당  
var obj2 = { get x() { return 17; } };  
obj2.x = 5; // TypeError 발생  
  
// 확장 불가 객체에 새 프로퍼티 할당  
var fixed = {};  
Object.preventExtensions(fixed);  
fixed.newProp = "ohai"; // TypeError 발생  
```

* * *

#### 3. 엄격 모드는 삭제할 수 없는 프로퍼티를 삭제하려할 때 예외를 발생시킵니다.  (시도가 어떤 효과도 없을 때).

```js
"use strict";  
delete Object.prototype; // TypeError 발생  
```

* * *

#### 4. 또한 function 삭제도 허용하지 않습니다.

```js
"use strict";  
function x(p1, p2) {};  
delete x;        // error  
```

* * *

#### 5.  엄격모드는 유니크한 함수 파라미터 이름을 요구합니다. 일반 코드에서는 마지막으로 중복된 인수가 이전에 지정된 인수를 숨깁니다. 이러한 이전의 인수들은 arguments[i] 를 통해 여전히 남아 있을 수 있으므로, 완전히 접근 불가한 것이 아닙니다. 여전히, 이런 숨김 처리는 이치에 맞지 않으며 원했던 것이 아닐 수 있습니다(예를 들면 오타를 숨길 수도 있습니다). 따라서 엄격 모드에서는 종복 인수명은 구문 에러입니다.

```js 예시1
"use strict";  
function x(p1, p1) {};   // !!! 구문 에러  
```

```js 예시2
function sum(a, a, c){ // !!! 구문 에러  
 "use strict";  
 return a + b + c; // 코드가 실행되면 잘못된 것임  
}  
```

* * *

#### 6.  ECMAScript 5 에서의 엄격 모드는 8진수 구문을 금지합니다.

8진수 구문은 ES5의 문법이 아니지만,  
모든 브라우저에서 앞에 0을 붙여 지원됩니다(0644 === 420 와 “045” === “%”).

`ECMAScript 2015 에서는 접두사 "0o"를 붙여 8진수를 지원합니다.`

```js
var a = 0o10; // ES6: 8진수  
```

초보 개발자들은 가끔 앞에 붙은 0 이 무의미하다고 생각하여, 이를 정렬용으로 사용합니다 — 하지만 이는 숫자의 의미를 바꿔버립니다.  
이 8진수 문법은 거의 무용하며 잘못 사용될 수 있으므로 엄격모드에서 이 구문은 에러입니다.

```js
"use strict";  
var sum = 015 + // !!! 구문 에러  
 197 +  
 142;  
```
  
------------------  
#### 7. ECMAScript 6 의 엄격모드는 primitive 값에 프로퍼티를 설정하는 것을 금지합니다. 엄격모드가 아닐 때에는 프로퍼티 설정이 간단하게 무시되지만(no-op), 엄격모드에서는 TypeError 를 발생시킵니다.  
  
```javascript  
(function() {  
"use strict";  
  
false.true = "";         // TypeError  
(14).sailing = "home";   // TypeError  
"with".you = "far away"; // TypeError  
})();  
```

`primitive`: 원시값 또는 원시 자료형  
객체도 아니고 메서드도 아닌 데이터입니다.  
string, number, bigint, boolean, null, undefined, symbol  
7가지 원시 자료형이 존재 합니다.

<mark>모든 원시 값은 불변합니다. 즉, 변형할 수 없습니다. 원시값 자체와, 원시값을 할당한 변수를 혼동하지 않는 것이 중요합니다. 변수는 새로운 값을 다시 할당할 수 있지만, 이미 생성한 원시값은 객체, 배열, 함수와는 달리 변형할 수 없습니다.</mark>

```js 원시값 예시
// 문자열 메서드는 문자열을 변형하지 않음  
var bar = "baz";  
console.log(bar);        // baz  
bar.toUpperCase();  
console.log(bar);        // baz  
  
// 배열 메소드는 배열을 변형함  
var foo = [];  
console.log(foo);        // []  
foo.push("plugh");  
console.log(foo);        // ["plugh"]  
  
// 할당은 원시 값에 새로운 값을 부여 (변형이 아님)  
bar = bar.toUpperCase(); // BAZ  
```

`원시 값을 교체할 수는 있지만, 직접 변형할 수는 없습니다.`

``` js 원시형 코드 실행 과정
// 원시값  
let foo = 5;  
  
// 원시값을 변경해야 하는 함수 정의  
function addTwo(num) {  
 num += 2;  
}  
// 같은 작업을 시도하는 다른 함수  
function addTwo_v2(foo) {  
 foo += 2;  
}  
  
// 원시값을 인수로 전달해 첫 번째 함수를 호출  
addTwo(foo);  
// 현재 원시값 반환  
console.log(foo);   // 5  
  
// 두 번째 함수로 다시 시도  
addTwo_v2(foo);  
console.log(foo);   // 5  
```

5 대신 7 일 것이라고 예상하였나요?  
그렇다면, 이 코드의 실행 과정을 살펴보세요.

1.  addTwo 와 addTwo_v2 함수 호출을 위해, JavaScript는 식별자 foo 의 값을 찾습니다. 이는 인스턴스화된 첫 번째 구문의 변수를 올바르게 찾습니다.
    
2.  찾은 다음, JavaScript는 인수를 함수의 매개변수로서 전달합니다.
    
3.  함수의 본문 내 구문들을 실행하기 전에, JavaScript는 원래 전달된 인수(원시 값)를 복사해 로컬 복사본을 생성합니다. 이러한 복사본은 함수의 스코프 내에서만 존재하며, 함수 정의 내에 지정한 식별자를 통해 접근가능합니다(addTwo 의 num, addTwo_v2 의 foo).
    
4.  그 후, 함수의 구문들이 실행됩니다.  
    4-1. 첫 번째 함수내에서, 로컬 num 인수가 생성되었습니다. 이 값을 2 증가시키는 것이며, 원래 foo 의 값이 아닙니다!
    
    4-2. 두 번째 함수내에서, 로컬 foo 인수가 생성되었습니다. 이 값을 2 증가시키는 것이며, 원래(외부) foo 의 값이 아닙니다! 또한, 이 경우에서, 외부 foo 변수에는 어떤 방법으로든 접근할 수 없습니다. 이는 자바스크립트의 어휘적 유효 범위(lexical scoping)와 결과 변수 섀도잉 때문입니다. 로컬 foo 는 외부 foo 를 숨깁니다.
    

<mark>결과적으로, 우리 함수들 내부의 모든 변경은 그 복사본으로 작업하였기 때문에, 원본 foo 에 전혀 영향을 주지 않았습니다.</mark>

이것이 원시값이 변하지 않는 이유입니다. 원시값에 직접 작업하지 않으므로, 원본을 건드리지 않고 복사본 가져와 계속 작업을 합니다.

* * *

#### 8. function의 this키워드는 엄격모드에서 다르게 작동합니다.

this 키워드는 함수를 호출 한 객체를 나타냅니다.  
객체를 지정하지 않으면 엄격 모드의 function은 undefined로 반환됩니다.

```js
"use strict";  
function myFunction() {  
 alert(this); // will alert "undefined"  
}  
myFunction();  
```