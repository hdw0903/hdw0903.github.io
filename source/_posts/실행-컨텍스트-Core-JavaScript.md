---
title: 실행 컨텍스트 -Core JavaScript
date: 2020-04-26 17:50:25
disqusId: tunas-blog-1
categories: Core JavaScript
tag:
  - Core JavaScript
  - JavaScript
  - execution context
  - hoisting
  - this
  - VariableEnvironment
  - LexicalEnvironment
  - environmentRecord
  - stack
  - queue
  - function expression
  - scope
  - scope chain
  - outerEnvironmentReference
  - Global variable
  - Local variable
  - 코어 자바스크립트
  
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

실행 컨텍스트(`execution context`)는 실행할 코드에 제공할 환경 정보를 모아놓은 객체로,
자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념입니다.
자바스크립트는 실행 컨텍스트가 활성화되는 시점에 선언된 변수를 위로 끌어올리고(`호이스팅`), 외부 환경 정보를 구성하고, `this` 값을 설정하는 등의 동작을 수행하는데, 이로 인해 다른 언어에서는 발견할 수 없는 특이한 현상들이 발생합니다.

* 실행 컨텍스트
  * [실행 컨텍스트란?](/2020/04/26/실행-컨텍스트-Core-JavaScript/#execution_context)
    * 스택과 큐
  * [VariableEnvironment](/2020/04/26/실행-컨텍스트-Core-JavaScript/#VariableEnvironment)
  * [LexicalEnvironment](/2020/04/26/실행-컨텍스트-Core-JavaScript/#LexicalEnvironment)
    * environmentRecord와 호이스팅
      * 호이스팅 규칙
      * 함수 선언문과 함수 표현식
    * 스코프, 스코프 체인, outerEnvironmentReference
      * 스코프 체인
      * 전역변수 와 지역변수
  * [this](/2020/04/26/실행-컨텍스트-Core-JavaScript/#thisBinding)

<!-- more -->

------
<h2 id="execution_context">실행 컨텍스트란?</h2>

------
### 스택(stack)과 큐(queue)

* 스택 : 출입구가 하나뿐인 데이터 구조
비어있는 스택에 순서대로 데이터 a,b,c,d를 저정했다면 꺼낼 때는 반대로 d,c,b,a의 순서대로 꺼낸다.
저장할 수 있는 데이터 스택이 넘치면 엔진에서 `RangeError: Maximum call stack size exceeded` 에러를 던집니다.


* 큐 : 한쪽은 입구, 한쪽은 출구를 담당하는 양쪽 데이터 구조
비어있는 큐에 순서대로 데이터 a,b,c,d를 저정했다면 꺼낼 때도 a,b,c,d의 순서대로 꺼낸다.

------

#### 엔진이 동일한 환경에 있는 코드들을 실행할 때

필요한 환경정보들을 모아 컨텍스트를 구성하고, 이를 콜 스택에 쌓아올렸다가, 가장 위에 쌓여있는 컨텍스트와 관련된 코드들을 실행하므로 코드의 환경과 순서를 보장받을 수 있습니다.

흔히 실행 컨텍스트를 구성하는 방법은 함수를 실행하는 것입니다.
```js 실행 컨텍스트와 콜 스택
1. var a = 1;
function outer() {
  function inner() {
    console.log(a); // undefined
    var a = 3;
  }
  3. inner(); 
  console.log(a); // 1
}
2. outer(); 
console.log(a); // 1
```

1. 처음 자바스크립트 코드를 실행하는 순간 전역 컨텍스트가 콜 스택에 담깁니다.
(~~코드 내부에서 별도의 실행 명령 없이 브라우저에서 자동으로 실행하므로 자바스크립트 파일이 열리는 순간 전역 컨텍스트가 활성화 된다고 이해~~)

2. 전역 컨텍스트와 관련된 코드들을 순차적으로 진행하다가 `outer()`를 호출하면 엔진은 `outer`에 대한 환경 정보를 수집해 `outer` 실행 컨텍스트를 생성한 후 콜 스택에 담습니다.
이때, 콜 스택 내부에서 전역 컨텍스트 위에 `outer` 실행 컨텍스트가 놓인 상태가 되어 `outer` 실행 컨텍스트 즉, `outer` 함수 내부 코드들을 순차적으로 실행합니다. (전역 컨텍스트 일시정지)

3. 마찬가지로 `outer` 실행 컨텍스트를 진행하다 `inner()`함수의 실행 컨텍스트가 콜 스택의 가장 위에 담기면 `outer` 컨텍스트는 일시정지되고 `inner` 함수 내부 코드를 실행합니다.

* 실행 컨텍스트와 콜 스택
![실행 컨텍스트와 콜 스택](/images/context_callStack.png)

<u>실행 컨텍스트가 콜 스택의 맨위에 쌓이는 순간이 곧 현재 실행할 코드에 관여하는 시점.
기존의 컨텍스트는 새로 쌓인 컨텍스트보다 아래에 위치할 수 밖에 없음</u>

<mark>이렇게 어떤 컨텍스트가 활성화될 때 엔진은 해당 컨텍스트에 관련된 코드들을 실행하는데 필요한 환경 정보들을 수집하여 실행 컨텍스트 객체에 저장함.</mark>

이 객체는 엔진이 활용 목적으로 생성할 뿐 개발자 코드로 확인할 수는 없음
이에 담기는 정보들은 다음과 같습니다.

  * `VariableEnvironment` : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보.
  선언 시점의 `LexicalEnvironment`의 스냅샷(`snapshot`)으로, 변경 사항은 반영되지 않음.

  * `LexicalEnvironment` : 처음에는 `VariableEnvironment`와 같지만 변경 사항이 실시간으로 반영되는 점이 다름.

  * `ThisBinding` : `this` 식별자가 바라봐야 할 대상 객체

------
<h2 id="VariableEnvironment">VariableEnvironment</h2>

`VariableEnvironment`에 담기는 내용은 `LexicalEnvironment`와 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다릅니다.

실행 컨텍스트를 생성할 때 `VariableEnvironment`에 정보를 먼저 담은 후
이를 그대로 복사하여 `LexicalEnvironment`를 만들고, 생성된 `LexicalEnvironment`를 활용합니다.

`VariableEnvironment`와 `LexicalEnvironment` 는 초기화 과정 중에는 사실상 완전히 동일하고 둘 다 내부에 `environmentRecord`와 `outer-EnvironmentReference`로 구성되어 있습니다. 

다음의 `LexicalEnvironment`를 살펴보며 자세한 내용과 둘의 코드 진행과정 차이를 알아봅시다.

------
<h2 id="LexicalEnvironment">LexicalEnvironment</h2>

`LexicalEnvironment`은 개발자 용어로 `정적 환경`으로 통하나 <u>실제 의미가 동일 하지는 않습니다.</u>
커뮤니케이션을 위해 정적 환경으로 불리지만 `LexicalEnvironment`를 현재 컨텍스트의 내부에는 식별자들이 있고 그 외부 정보를 다른 객체가 참조하도록 구성되어 있다는 식의 <u>컨텍스트 구성 환경 정보들을 모아놓은</u> 느낌으로 이해합시다.

------
### environmentRecord와 호이스팅

`environmentRecord`에는 현재 컨텍스트와 관련된 코드의 <mark>식별자 정보</mark>들이 저장됩니다.

* 컨텍스트를 구성하는 함수에 지정된 <mark>매개변수 식별자</mark>

* 선언한 함수가 있을 경우 <mark>그 함수 자체</mark>

* var등으로 선언된 <mark>변수의 식별자 등</mark>

**컨텍스트 내부 전체를 처음부터 끝까지 순서대로 수집합니다.**

* 참고 :
<mark>전역 실행 컨텍스트는 변수 객체를 생성하지 않습니다.</mark>
자바스크립트 구동 환경이 별도로 제공하는 객체 즉, <mark>전역 객체를 활용합니다</mark>. (브라우저의 window, Node.js의 global 객체 등)
이들은 자바스크립트 내장 객체가 아닌 호스트 객체로 분류됩니다.

------
#### 호이스팅 규칙

`environmentRecord`가 현재 컨텍스트와 관련된 변수 정보를 수집한 상태는
컨텍스트의 코드가 실행되기 전의 상태입니다. 코드가 실행되기 전에 엔진이 이미 해당 환경에 속한 코드의 변수명을 모두 알고 있는 상태가 되는 것 입니다.

이 상태를 엔진이 식별자 정보들을 최상단으로 끌어올려놓고 순차적으로 코드를 실행하는 것처럼 보여
<mark>변수 정보를 수집하는 과정을 더욱 이해하기 쉬운 방법으로 대체한 가상의 개념으로 등장한 개념이 호이스팅<mark> 입니다.

엔진이 실제로 끌어올리지는 않지만 편의상 끌어올린 것으로 간주하는 개념입니다.

```js 함수 선언의 호이스팅
function a() {
  1. console.log(b); // (1)
  var b = 'bbb'; // 수집 대상 1(변수 선언)
  2. console.log(b); // (2)
  function b() {} // 수집 대상 2(함수 선언)
  3. console.log(b); // (3)
}
a();
// function b(){}
// bbb
// bbb
```

* `environmentRecord`에는 매개변수의 이름, 함수 선언, 변수명 들이 담깁니다.
엔진은 이러한 호이스팅으로 인해 코드 실행전 부터 컨텍스트 구성 정보를 가지고 있습니다.


1. `environmentRecord`에 의해 `var b;` 와 `var b = function b (){}`가 최상단에 위치한다고 볼 수 있습니다. 
`function b(){}` 를 참조하여 출력됩니다.


2. 3. `var b = 'bbb'` 으로 할당된 후 이므로 `bbb`가 출력됩니다.

------
#### 함수 선언문과 함수 표현식

* 함수 선언문 : `function` 정의부만 존재하고 별도의 할당 명령이 없는 것
<mark>반드시 함수명이 정의되어 있어야 함.</mark>

* 함수 표현식 : 정의한 `function`을 별도의 변수에 할당하는 것
<mark>함수명이 없어도 됨.</mark>

```js 함수를 정의하는 세 가지 방식
function a() {
  /* ... */
} // 함수 선언문. 함수명 a가 곧 변수명.
a(); // 실행 OK.

var b = function() {
  /* ... */
}; // (익명) 함수 표현식. 변수명 b가 곧 함수명.
b(); // 실행 OK.

var c = function d() {
  /* ... */
}; // 기명 함수 표현식. 변수명은 c, 함수명은 d.
c(); // 실행 OK.
d(); // 에러!
```

* 함수 선언문과 함수 표현식 호이스팅 시 차이.

```js 호이스팅 전 원본 코드
console.log(sum(1, 2));
console.log(multiply(3, 4));

function sum(a, b) {
  // 함수 선언문 sum
  return a + b;
}

var multiply = function(a, b) {
  // 함수 표현식 multiply
  return a * b;
};
```

```js 호이스팅 후 함수 표현식과 선언문의 차이
var sum = function sum(a, b) {
  // 함수 선언문은 전체를 호이스팅합니다.
  return a + b;
};
var multiply; // 변수는 선언부만 끌어올립니다.
console.log(sum(1, 2));
console.log(multiply(3, 4));

multiply = function(a, b) {
  // 변수의 할당부는 원래 자리에 남겨둡니다.
  return a * b;
};
```

* 위에서 볼 수 있듯 함수 선언문은 함수 전체를 호이스팅

* 함수 표현식은 변수 부분만 호이스팅, 할당하는 부분은 호이스팅 하지 않음.

* `console.log(multiply(3,4))` 시점의 `multiply`에는 값이 할당돼 있지 않습니다.
따라서 `multiply is not a function` 에러 메시지가 출력되고 , 에러로 인해 아래 할당부 코드는 실행되지 않고 종료됩니다.

------
### 스코프, 스코프 체인, outerEnvironmentReference

* 스코프(scope) : 식별자에 대한 유효범위

* 스코프 체인 : 식별자의 유효범위를 안에서부터 바깥으로 차례로 검색해나가는 것

* outerEnvironmentReference : 스코프 체인을 가능하게 하는 `LexicalEnvironment`의 두 번째 수집자료 `outerEnvironmentReference`

------
#### 스코프 체인

`outerEnvironmentReference`는 현재 호출된 함수가 <mark>선언될 당시의 LexicalEnvironment</mark>를 참조합니다. (== 콜 스택 상에서 실행 컨텍스트가 활성화된 상태)

```js outerEnvironmentReference 동작 예시
function a (){
  //(...) 
  function b (){
    //(...)
    function c (){
      //(...)
    }
  }
}
```

a 함수 내부에 b 함수를 선언하고 다시 b 함수 내부에 c 함수를 선언했습니다.

* 이 경우 `함수 c`의 `outerEnvironmentReference`는 `함수 b`의 `LexicalEnvironment`를 참조합니다.
`함수 b`의 `LexicalEnvironment`에 있는 `outerEnvironmentReference`는 다시 `함수 a`의 `LexicalEnvironment`를 참조합니다.

* 이처럼 `outerEnvironmentReference`는 연결리스트 형태를 가지고 있고 선언 시점의 `LexicalEnvironment`를 계속 찾아 올라가면 마지막에는 전역 컨텍스트의 `LexicalEnvironment`가 있을 것입니다.

* `outerEnvironmentReference`는 <mark>자신이 선언된 시점의 LexicalEnvironment만 참조</mark>하고 있으므로 가장 가까운 요소부터 차례로 접근할 수 밖에 없습니다.

이런 구조적 특성 덕분에 여러 스코프에서 동일한 스코프를 선언한 경우에는 <mark>무조건 스코프 체인 상에서 가장 먼저 발견된 식별자만 접근 가능</mark> 하게 됩니다.

```js 개발자 도구로 스코프 체인 확인법 -크롬
var a = 1;
var outer = function() {
  var b = 2;
  var inner = function() {
    console.dir(inner);
  };
  inner();
};
outer();
```

* 함수 내부에 함수를 출력하는 방법으로 현재 실행 컨텍스트를 제외한 상위 스코프 정보들을 개발자 도구에서 확인할 수 있습니다. (`debugger` 사용시 더욱 자세한 정보 확인 가능)

------
#### 전역변수와 지역변수

* 전역변수 : 전역 스코프에서 선언한 변수

* 지역변수 : 전역 스코프가 아닌 함수 스코프 또는 블록 스코프 내부에서 선언한 변수 
지역 변수는 선언된 스코프 외부에서 접근할 수 없음.

------
<h2 id="thisBinding">this</h2>

실행 컨텍스트의 `thisBinding`에는 `this`로 지정된 객체가 저장됩니다.
실행 컨텍스트 활성화 당시에 `this`가 지정되지 않으면 `this`는 전역 객체를 참조하게 됩니다.

함수를 호출하는 방식 등 사용되는 방법에 따라 `this`가 참조하는 대상이 변하게 되므로
`this`에 자세한 내용은 따로 다루도록 하겠습니다.