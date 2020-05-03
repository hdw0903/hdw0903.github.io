---
title: 클로저 -Core JavaScript
disqusId: tunas-blog-1
tags:
  - Core JavaScript
  - JavaScript
date: 2020-05-03 15:56:17
categories: Core JavaScript
---

* 클로저 (Closure)
  * [클로저의 의미 및 원리 이해](/2020/05/03/클로저-Core-JavaScript/#closure)
  * [클로저와 메모리 관리](/2020/05/03/클로저-Core-JavaScript/#closure_memory)
  * [클로저 활용 사례](/2020/05/03/클로저-Core-JavaScript/#closure_ex)
    * 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때
    * 접근 권한 제어 (정보 은닉)
    * 부분 적용 함수
    * 커링 함수
  * [정리](/2020/05/03/클로저-Core-JavaScript/#)

<!-- more -->

------
<h2 id="closure">클로저의 의미 및 원리 이해</h2>

클로저(`Closure`)는 여러 함수형 프로그래밍 언어에서 등장하는 보편적인 특성입니다.
자바스크립트 고유의 개념이 아니라서 ECMAScript 명세에서도 클로저의 정의를 다루지 않고 있고, 다양한 문헌에서 제각각 클로저를 다르게 정의 또는 설명하고 있습니다.

다양한 서적에서 클로저를 한 문장으로 요약해서 설명하는 부분들을 소개하면 다음과 같습니다.
    * 자신을 내포하는 함수의 컨텍스트에 접근할 수 있는 함수
    
    * 함수가 특정 스코프에 접근할 수 있도록 의도적으로 그 스코프에서 정의하는 것
    
    * 함수를 선언할 때 만들어지는 유효범위가 사라진 후에도 호출할 수 있는 함수
    
    * 이미 생명 주기상 끝난 외부 함수의 변수를 참조하는 함수
    
    * 자유변수가 있는 함수와 자유변수를 알 수 있는 환경의 결합
    
    * 로컬 변수를 참조하고 있는 함수 내의 함수
    
    * 자신이 생성될 때의 스코프에서 알 수 있었던 변수들 중 언젠가 자신이 실행될 때 사용할 변수들만을 기억하여 유지시키는 함수


**MDN** 에서는 클로저를 함수와 그 함수가 선언될 당시의 `LexicalEnvironment`의 조합이라고 소개하고,
다른 말로 클로저는 내부 함수에서 외부 함수의 범위로 접근할 수 있게 해주는 함수라고 합니다.


선언될 당시의 `LexicalEnvironment`는 실행 컨텍스트의 구성 요소 중 하나인 `outerEnvironmentReference`에 해당합니다. 
`LexicalEnvironment`의 `environmentRecord`와 `outerEnvironmentReference`에 의해 변수의 유효범위인 스코프가 결정되고 스코프 체인이 가능해집니다.


* `컨텍스트 A`에서 선언한 `내부 함수B`의 실행 컨텍스트가 활성화된 시점에서는 B의 `outerEnvironmentReference`가 참조하는 대상인 A의 `LexicalEnvironment`에도 접근이 가능해 집니다. A에서는 B에서 선언한 변수에 접근할 수 없지만 B에서는 A에 선언한 변수에 접근이 가능해집니다.


* 이런 내부함수에서 외부 변수를 참조하게 되는 경우가, 선언될 당시의 `LexicalEnvironment`와의 상호관계(조합)의 의미가 됩니다.

```js 외부 함수의 변수를 참조하는 내부 함수 -1
var outer = function() {
  var a = 1;
  var inner = function() {
    console.log(++a);
  };
  inner();
};
outer();
```

1. `outer` 함수에 변수 a를 선언했고 1을 할당했습니다.


2. `outer`의 내부함수인 `inner` 함수에서 a의 값을 1 증가시키고 출력합니다.


* `inner`함수 내부에서는 a를 선언하지 않았기 때문에 `environmentRecord`에서 값을 찾지 못하므로 `outerEnvironmentReference`에 지정된 상위 컨텍스트인 `outer`의 `LexicalEnvironment`에 접근하여 다시 a를 찾습니다.


* `outer` 함수의 실행 컨텍스트가 종료되면 `LexicalEnvironment`에 저장된 식별자들(a,inner)에 대한 참조를 지웁니다. 그러면 각 주소에 저장돼 있던 값들은 자신을 참조하는 변수가 하나도 없게 되므로 `가비지컬렉터(GC)`의 수집 대상이 됩니다.

```js 외부 함수의 변수를 참조하는 내부 함수 -2
var outer = function() {
  var a = 1;
  var inner = function() {
    return ++a;
  };
  return inner();
};
var outer2 = outer();
console.log(outer2); // 2
```

* 이 예제 역시 `inner`함수 내부에서 외부변수인 a를 사용했습니다.
`inner` 함수를 실행한 결과를 `return` 하고 나면 `outer`함수의 실행 컨텍스트가 종료된 시점에는 a변수를 참조하는 대상이 없어집니다. 그러므로 예제1과 마찬가지로 식별자들의(a,inner) 값들은 `가비지컬렉터(GC)`에 의해 소멸됩니다.


* 예제1과 예제2는 `outer`함수의 실행 컨텍스트가 종료되기 이전에 `inner` 함수의 실행 컨텍스트가 종료돼 있으며, 이후 별도로 `inner` 함수를 호출할 수 없다는 공통점을 가지고 있습니다.

<mark>그렇다면 outer의 실행 컨텍스트가 종료된 후에도 inner 함수를 호출할 수 있게 만들면 어떨까요?</mark>

```js 외부 함수의 변수를 참조하는 내부 함수 -3
var outer = function() {
  var a = 1;
  var inner = function() {
    return ++a;
  };
  return inner;
};
var outer2 = outer();
console.log(outer2()); // 2
console.log(outer2()); // 3
```

1. `return inner()` 함수의 실행 결과가 아닌 `return inner` 함수 자체를 반환했습니다.


2. `outer2` 변수는 `outer`함수의 실행 결과인 `inner`함수 자체를 참조하게 됩니다.
`outer2` 호출시 `inner` 함수가 실행됨.


3. `inner` 함수의 실행 컨텍스트의 `environmentRecord`에는 수집할 정보가 없습니다. `outerEnvironmentReference`에는 `inner` 함수가 선언된 위치의 `LexicalEnvironment`가 참조복사 됩니다. `inner`함수는 `outer` 함수 내부에서 선언됐으므로, `outer` 함수의 `LexicalEnvironment`가 담깁니다.


4. 스코프체이닝에 따라 `outer`에서 선언한 변수 a에 접근해 1만큼 증가시킨후 결과 값인 2를 반환하고, `inner`함수의 실행 컨텍스트가 종료됩니다.


5. `outer2`<u>를 다시 호출하면 같은 방식으로 a의 값을 2에서 1만큼 증가시켜 그 결과 값인 3을 반환합니다.</u>
 

**중요 포인트**

* `outer`함수의 실행 컨텍스트는 종료된 상태인데 어떻게 외부 함수의 변수(`outer` 함수의 `LexicalEnvironment`)에 접근할 수 있는 걸까?


* 이는 가비지컬렉터의 동작 방식 때문입니다.


* <mark>어떤 값을 참조하는 변수가 하나라도 있다면 그 값은 가비지컬렉터(GC)의 대상이 되지 않습니다.</mark>


* `outer` 함수는 실행 종료 시점에 `inner` 함수를 반환했습니다. `outer` 함수는 `inner`함수를 참조하게 되고 외부함수인 `outer`의 실행이 종료되었지만 내부함수인 `inner`함수는 언젠가 `outer()`형식 (변수 outer2와 같은)으로 호출될 수 있습니다.


* `inner` 함수 역시 `outer`의 변수를 참조하므로 실행 컨텍스트가 활성화 되면 `outerEnvironmentReference`가 `outer` 함수의 `LexicalEnvironment`를 필요로 하게되므로 `가비지컬렉터(GC)`의 대상에서 제외됩니다. 그 덕에 `inner`함수가 외부함수의 변수에 접근할 수 있는 것입니다.


* 클로저란 :
  <mark>외부 함수에서 선언한 변수를 참조하는 내부 함수를 외부로 전달할 경우 외부 함수의 실행 컨텍스트가 종료된 이후에도 변수가 사라지지 않는 현상</mark>


* "내부함수를 외부로 전달"이 return만을 의미하는 것은 아님. 다른 경우도 존재

```js return 없이 클로저가 발생하는 경우
// (1) setInterval/setTimeout
(function() {
  var a = 0; // 외부 함수의 변수를 내부 함수에서
  var intervalId = null; // 참조하고 있으므로 GC의 대상이 되지 않음. 
  var inner = function() {
    if (++a >= 10) { // 외부 함수의 변수a 참조
      clearInterval(intervalId); // 외부 함수의 변수 intervalId 참조
    }
    console.log(a);
  };
  intervalId = setInterval(inner, 1000);
})();
```

별도의 외부객체인 window의 메서드(setTimeout 또는 setInterval)에 전달할 콜백 함수 내부에서 지역변수를 참조합니다.

```js return 없이 클로저가 발생하는 경우
// (2) eventListener
(function() {
  var count = 0;
  var button = document.createElement('button');
  button.innerText = 'click';
  button.addEventListener('click', function() {
    console.log(++count, 'times clicked');
  });
  document.body.appendChild(button);
})();
```

별도의 외부 객체인 DOM의 메서드 (addEventListener)에 등록할 handler 함수 내부에서 지역변수를 참조합니다.

<mark>두 상황 모두 두 지역변수를 참조하는 내부함수를 외부에 전달했기 때문에 클로저(closure)입니다.</mark>

------
<h2 id="closure_memory">클로저와 메모리 관리</h2>






------
<h2 id="closure_ex">클로저 활용 사례</h2>





------
<h2 id="closure">정리</h2>