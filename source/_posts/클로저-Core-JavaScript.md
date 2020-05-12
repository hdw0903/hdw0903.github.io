---
title: 클로저 -Core JavaScript
disqusId: tunas-blog-1
tags:
  - Core JavaScript
  - 코어 자바스크립트
  - JavaScript
  - Object. freeze
  - 동결 객체
  - 부분 동결 객체
  - partially applied function
  - 부분 적용 함수
  - 커링 함수
  - 접근 권한 제어
  - 고차 함수
  - bind
  - forEach
  - 클로저
  - Closure
  - GC
  - LexicalEnvironment
  - outerEnvironmentReference
  - 가비지 컬렉터
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
자바스크립트 고유의 개념이 아니라서 `ECMAScript` 명세에서도 클로저의 정의를 다루지 않고 있고, 다양한 문헌에서 제각각 클로저를 다르게 정의 또는 설명하고 있습니다.

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

* <mark>클로저는 객체지향과 함수형 모두를 아우르는 매우 중요한 개념입니다.</mark>

* 메모리 누수:
개발자의 의도와 달리 어떤 값의 참조 카운트가 0이 되지 않아 GC의 수거 대상이 되지 않는 경우 발생할 수 있습니다.
(개발자가 의도적으로 참조 카운트가 0이 되지 않게 설계한 경우는 '누수'라는 표현은 맞지 않습니다.)

* <mark>클로저는 의도대로 설계한 "메모리 소모"에 대한 관리법을 잘 파악해서 적용하는 것이 중요합니다.</mark>

------
### 메모리 관리 방법

클로저는 필요에 의해 의도적으로 함수의 지역변수를 메모리를 소모하도록 함으로써 발생합니다.
그렇다면 <u>필요성이 사라진 시점</u>에는 더는 메모리를 소모하지 않게 해주면 됩니다.

참조 카운트를 0으로 만들면(GC의 작동원리) GC가 수거해 갈것이고, 이때 소모됐던 메모리가 회수됩니다.

* 참조 카운트를 0으로 만드는 방법 ?
식별자에 참조형이 아닌 기본형 데이터(보통 `null`이나 `undefined`)를 할당하면 됩니다.

```js 클로저의 메모리 관리 - return
// (1) return에 의한 클로저의 메모리 해제
var outer = (function() {
  var a = 1;
  var inner = function() {
    return ++a;
  };
  return inner;
})();
console.log(outer());
console.log(outer());
outer = null; // outer 식별자의 inner 함수 참조를 끊음
```

```js 클로저의 메모리 관리 - setInterval
// (2) setInterval에 의한 클로저의 메모리 해제
(function() {
  var a = 0;
  var intervalId = null;
  var inner = function() {
    if (++a >= 10) {
      clearInterval(intervalId);
      inner = null; // inner 식별자의 함수 참조를 끊음
    }
    console.log(a);
  };
  intervalId = setInterval(inner, 1000);
})();
```

```js 클로저의 메모리 관리 - eventListener
// (3) eventListener에 의한 클로저의 메모리 해제
(function() {
  var count = 0;
  var button = document.createElement('button');
  button.innerText = 'click';

  var clickHandler = function() {
    console.log(++count, 'times clicked');
    if (count >= 10) {
      button.removeEventListener('click', clickHandler);
      clickHandler = null; // clickHandler 식별자의 함수 참조를 끊음
    }
  };
  button.addEventListener('click', clickHandler);
  document.body.appendChild(button);
})();
```

------
<h2 id="closure_ex">클로저 활용 사례</h2>

클로저가 실제로 등장하는 활용 사례

### 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때

대표적인 콜백 함수 중 하나인 이벤트 리스너에 관한 예시

```js
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul'); // (공통 코드)

fruits.forEach(function(fruit) {
  // (A)
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', function() {
    // (B)
    alert('your choice is ' + fruit);
  });
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

1. `fruits` 변수를 순회하며 `li`를 생성하고 각 `li`를 클릭하면 해당 리스너의 콜백 함수가 실행됩니다.


2. `forEach`메서드에 넘겨준 익명의 콜백 함수(A)는 내부에서 외부 변수를 사용하지 않으므로 클로저가 없습니다.


3. `addEventListener`에 넘겨준 콜백 함수(B)에는 함수내의 `fruit`라는 외부 변수를 참조하고 있으므로 클로저가 있습니다.


4. (A)는 `fruits`의 개수만큼 실행되며, 그때마다 새로운 실행 컨텍스트가 생성됩니다.


5. (A)의 실행 종료 여부와 무관하게 클릭 이벤트에 의해 각 컨텍스트의 (B)가 실행될 때는 (B)의 `outerEnvironmentReference`가 (A)의 `LexicalEnvironment`를 참조하게 됩니다.


6. 따라서 (B)함수가 참조할 예정인 변수 `fruit`에 대해서는 (A)함수가 종료된 후에도 `CG` 대상에서 제외되어 계속 참조 가능하게 됩니다.

그런데 (B)함수의 쓰임이 콜백 함수에 국한되지 않는 경우라면 반복을 줄이기 위해 (B)함수를 외부로 분리하는 편이 나을 수 있습니다.

따라서 다음은 `fruit`을 인자로 받아 출력하는 형태입니다.

```js 콜백 함수 외부로꺼내어 공통 함수로 사용
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul');

var alertFruit = function(fruit) {
  alert('your choice is ' + fruit);
};
fruits.forEach(function(fruit) {
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruit);
  $ul.appendChild($li);
});
document.body.appendChild($ul);
alertFruit(fruits[1]);
```

* 공통 함수로 사용하고자 콜백 함수를 외부로 꺼내어 `alertFruit`라는 변수에 담았습니다.
`alertFruit`를 직접 실행할 수 있게 되었습니다.

* 하지만 각 `li`를 클릭하면 클릭한 대상의 과일명이 아닌 `[object MouseEvent]`라는 값이 출력됩니다.
이는 콜백 함수의 인자에 대한 제어권을 `addEventListener`가 가진 상태이며, `addEventListener`는 콜백 함수를 호출할 때 첫 번째 인자에 "이벤트 객체"를 주입하기 때문입니다.

이 문제는 `bind`메서드를 활용하면 해결할 수 있습니다.

```js bind 메서드 사용
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul');

var alertFruit = function(fruit) {
  alert('your choice is ' + fruit);
};
fruits.forEach(function(fruit) {
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruit.bind(null, fruit));
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

* 하지만 `bind`를 활용하면 이벤트 객체가 인자로 넘어오는 순서가 바뀌는 점과,
함수 내부에서 `this`가 참조하는 값이 달라지는점을 감안해야 합니다.

* 이러한 변경사항 마저 발생하지 않게 만들려면 `bind`메서드가 아닌 다른 방식으로 만들어야 합니다.

```js 고차함수를 사용하여 클로저를 적극적으로 활용
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul');

var alertFruitBuilder = function(fruit) {
  return function() {
    alert('your choice is ' + fruit);
  };
};
fruits.forEach(function(fruit) {
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruitBuilder(fruit));
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

* 고차함수란 함수를 인자로 받거나 함수를 리턴하는 함수입니다.

1. `alertFruit` 함수 대신 `alertFruitBuilder`라는 이름의 함수를 작성했습니다.
`alertFruitBuilder` 함수 내부에서는 다시 익명함수를 반환합니다.


2. 이 익명함수 내부의 코드가 기존의 `alertFruit` 함수의 코드입니다. 


3. `alertFruitBuilder` 함수를 실행하면서 `fruit` 값을 인자로 전달하면, 함수의 실행 결과가
다시 함수(`return function`)가 되며, 이렇게 반환된 함수를 리스너의 콜백 함수로써 전달할 것입니다.


4. 클릭 이벤트가 발생하면 이 함수의 실행 컨텍스트가 열리면서 `alertFruitBuilder`의 파라미터로 넘어온 `fruit`를 `outerEnvironmentReference`에 의해 참조할 수 있게됩니다. 
즉, `alertFruitBuilder`의 실행 결과로 반환된 함수에는 클로저가 존재합니다.

------
#### 콜백 함수 내부에서 외부변수를 참조하기 위한 방법 정리.

1. 콜백 함수를 내부함수로 선언하여 외부변수를 직접 참조하는 방법.(`GC의 참조카운트` 이용)


2. `bind`메서드를 활용하여 값을 직접넘겨주는 방법. 클로저는 발생하지 않지만 몇가지 제약이 생김


3. 콜백 함수를 고차함수로 바꿔서 클로저를 적극적으로 활용하는 방법. 

------
### 접근 권한 제어(은닉)

정보은닉(information hiding)은 어떤 모듈의 내부 로직에 대해 외부로의 노출을 최소화해서 모듈간의 결합도를 낮추고 유연성을 높이고자 하는 현대 프로그래밍 언어의 중요한 개념 중 하나입니다.

흔히 접근 권한에는 `public`, `private`, `protected` 세 종류가 있습니다.

* `public` : 외부에서 접근 가능한 것

* `private` : 내부에서만 사용하며 외부에 노출되지 않는 것

자바스크립트는 기본적으로 변수 자체에 이러한 접근 권한을 직접 부여하도록 설계돼 있지 않습니다. 하지만 접근 권한 제어가 불가능한 것은 아닙니다. 클로저를 이용하면 함수 차원에서 `public`한 값과 `private`한 값을 구분하는 것이 가능합니다.

```js public/private -return
var outer = function() {
  var a = 1;
  var inner = function() {
    return ++a;
  };
  return inner;
};
var outer2 = outer();
console.log(outer2());
console.log(outer2());
```

이전에 본 클로저 예제 입니다.

* `outer`함수를 종료할 때 `inner` 함수를 반환함으로써 `outer`함수의 지역변수 a의 값을 외부에서도 읽을 수 있게 되었습니다.


* 이처럼 클로저를 활용하면 외부 스코프에서 함수 내부의 변수들 중 선택적으로 일부 변수에 대한 접근 권환을 부여할 수 있습니다. (`return`을 활용하여)


* `outer`함수는 외부(전역 스코프)로 부터 철저하게 격리된 닫힌 공간입니다.
외부에서는 외부 공간에 노출돼 있는 `outer`라는 변수를 통해 `outer`함수를 실행할 수는 있지만, `outer`함수 내부에는 어떠한 개입도 할 수 없습니다. 
외부에는 오직 `outer`함수가 `return`한 정보에만 접근할 수 있습니다.
`return`값이 외부에 정보를 제공하는 유일한 수단이 됩니다.


* 외부에 제공하고자 하는 정보들을 모아서 `return`하고, 내부에서만 사용할 정보들은 `return`하지 않는 것으로 접근 권한 제어가 가능한 것입니다.


* `return`한 변수들은 공개 맴버(`public member`)가 되고, 그렇지 않은 변수들은 비공개 맴버(`private member`)가 되는 것입니다.


------
#### 접근 권한 제어를 통한 보드 게임 예시

자동차 경주 보드 게임.

규칙

    1. 각 턴마다 주사위를 굴려 나온 숫자(km)만큼 이동.
    
    2. 차량별로 연료량(fuel)과 연비(power)는 무작위로 생성.
    
    3. 남은 연료가 이동할 거리에 필요한 연료보다 부족하면 이동 불가.
    
    4. 모든 유저가 이동할 수 없는 턴에 게임이 종료됨.
    
    5. 게임 종료 시 가장 멀리 이동해 있는 사람이 승리.

```js 규칙에 따른 간단한 자동차 객체
var car = {
  fuel: Math.ceil(Math.random() * 10 + 10), // 연료(L)
  power: Math.ceil(Math.random() * 3 + 2), // 연비(km/L)
  moved: 0, // 총 이동거리
  run: function() {
    var km = Math.ceil(Math.random() * 6);
    var wasteFuel = km / this.power;
    if (this.fuel < wasteFuel) {
      console.log('이동불가');
      return;
    }
    this.fuel -= wasteFuel;
    this.moved += km;
    console.log(km + 'km 이동 (총 ' + this.moved + 'km)');
  },
};
```

위 코드는 `run` 메서드를 실행할 때마다 `car`객체의 `fuel`, `moved` 값이 변합니다.

하지만 자바스크립트를 아는사람이 `car`객체의 `fuel`, `power`, `moved`값을 직접 지정해 버린다면 공평한 게임이 되지 못합니다. 
* 이렇게 값을 바꾸지 못하도록 객체가 아닌 함수로 만들고, 필요한 맴버만을 `return`할 필요가 있습니다.

```js 함수를 실행함으로써 객체 생성
var createCar = function() {
  var fuel = Math.ceil(Math.random() * 10 + 10); // 연료(L)
  var power = Math.ceil(Math.random() * 3 + 2); // 연비(km / L)
  var moved = 0; // 총 이동거리
  return {
    get moved() {
      return moved;
    },
    run: function() {
      var km = Math.ceil(Math.random() * 6);
      var wasteFuel = km / power;
      if (fuel < wasteFuel) {
        console.log('이동불가');
        return;
      }
      fuel -= wasteFuel;
      moved += km;
      console.log(km + 'km 이동 (총 ' + moved + 'km). 남은 연료: ' + fuel);
    },
  };
};
var car = createCar();
```

* `createCar`라는 함수를 실행함으로써 객체를 생성하게 했습니다. `fuel`, `power` 변수는 비공개 맴버로 지정해 외부에서의 접근을 제한했고, `moved`변수는 `getter`만을 부여함으로써 "읽기전용" 속성을 부여했습니다.

* 이제 외부에서는 오직 `run`메서드를 실행하는 것과 현재의 `moved`값을 확인하는 두 가지 동작만 할 수 있습니다.

* `run`메서드를 다른 내용으로 덮어씌우는 어뷰징은 여전히 가능한 상태이긴 하지만 앞서의 코드보다 훨씬 안전한 코드가 됐습니다. 이런 어뷰징까지 막기 위해서는 객체를 `return`하기 전에 미리 변경할 수 없게끔 조치를 취해야 합니다.

```js Object.freeze
var createCar = function() {
  var fuel = Math.ceil(Math.random() * 10 + 10); // 연료(L)
  var power = Math.ceil(Math.random() * 3 + 2); // 연비(km / L)
  var moved = 0; // 총 이동거리
  var publicMembers = {
    get moved() {
      return moved;
    },
    run: function() {
      var km = Math.ceil(Math.random() * 6);
      var wasteFuel = km / power;
      if (fuel < wasteFuel) {
        console.log('이동불가');
        return;
      }
      fuel -= wasteFuel;
      moved += km;
      console.log(km + 'km 이동 (총 ' + moved + 'km). 남은 연료: ' + fuel);
    },
  };
  Object.freeze(publicMembers);
  return publicMembers;
};
var car = createCar();
```

* [Object.freeze](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)를 사용하여 `publicMembers`객체를 동결객체로 만들었습니다.


* `Object.freeze`메서드는 호출된 객체의 직속 속성만 동결하며 내부의 속성 값이 객체라면 그 객체는 동결되지 않아 추가/제거/재할당의 대상이 될 수 있으므로 (얕은동결) 주의하여야 합니다.

------
#### 클로저를 활용해 접근권한 제어 방법 정리

1. 함수에서 지역변수 및 내부함수 등을 생성

2. 외부에 접근권한을 주고자 하는 대상들로 구성된 참조형 데이터(대상이 여럿일 경우 객체 또는 배열, 하나일 경우 함수)를 return 합니다.

3. return한 변수들은 공개 맴버가 되고, 그렇지 않은 변수들은 비공개 맴버가 됩니다.

------
### 부분 적용 함수

부분 적용 함수(`partially applied function`)란 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켰다가. 나중에 나머지 인자를 넘길 때 원래 함수의 실행 결과를 얻을 수 있게 하는 함수입니다.

`this`를 바인딩해야 하는 점을 제외하면 `bind`메서드의 실행 결과가 바로 부분 적용 함수입니다.

```js bind - 부분 적용 함수
var add = function() {
  var result = 0;
  for (var i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
};
var addPartial = add.bind(null, 1, 2, 3, 4, 5);
console.log(addPartial(6, 7, 8, 9, 10)); // 55
```

`addPartial` 함수에 `this`값 `null`과 인자 5개를 미리 적용하고, 대기합니다.
추후에 추가적으로 인자들을 전달하며 호출하면 대기중이던 인자들과 차례대로 적용되어 실행합니다.

`add`함수는 `this`값을 사용하지 않지만, `bind`메서드는 `this`값을 변경할 수 밖에 없기 때문에 `this`에 관여하지 않는 다른 방법의 부분 적용 함수가 필요합니다.

// p 135 ~ 137 보류

디바운스(`debounce`)는 짧은 시간 동안 동일한 이벤트가 많이 발생한 경우 이를 전부 처리하지 않고 처음 또는 마지막에 발생한 이벤트에 대해 한 번만 처리하는 것으로, `scroll`, `wheel`, `mousemove`, `resize`등에 적용하기 좋습니다.

```js 부분 적용 함수 - 디바운스
var debounce = function(eventName, func, wait) {
  var timeoutId = null;
  return function(event) {
    var self = this;
    console.log(eventName, 'event 발생');
    clearTimeout(timeoutId);
    timeoutId = setTimeout(func.bind(self, event), wait);
  };
};
var moveHandler = function(e) {
  console.log('move event 처리'); };
var wheelHandler = function(e) {
  console.log('wheel event 처리'); };

document.body.addEventListener(
  'mousemove',
  debounce('move', moveHandler, 500));
document.body.addEventListener(
  'mousewheel',
  debounce('wheel', wheelHandler, 700));
```

* `debounce` 함수는 출력 용도로 지정한 `eventName`과 실행할 함수(`func`),마지막으로 발생한 이벤트인지 여부를 판단하기 위한 대기시간 (`wait(`(ms))을 받습니다.


* 내부에서는 `timeoutId` 변수를 생성하고, 클로저로 `EventListener`에 의해 호출될 함수를 반환합니다. 반환될 함수 내부에서는 `setTimeout`을 사용하기 위해 `this`를 별도의 변수에 담고 `clearTimeout`으로 대기큐를 초기화하게 했습니다.


* 마지막으로 `setTimeout`으로 `wait` 시간만큼 지연시킨 다음, 원래의 `func`를 호출하는 형태입니다.


* 최초의 event가 발생하면 `timeoutId = setTimeout(func,bind(self,event),wait)`에 의해 timeout의 대기열에 'wait 시간 뒤에 func를 실행 함'이라는 내용이 담깁니다. 그런데 `wait`시간이 경과하기 전에 동일한 event가 발생하게 되면 앞의 `clearTimeout(timeoutId)`에 의해 앞에 저장했던 대기열을 초기화하고, 다시 `timeoutId = setTimeout(func,bind(self,event),wait)`에서 새로운 대기열을 등록합니다.


* 결국 각 동일한 이벤트가 이전 이벤트로 부터 `wait`시간 내에 다시 발생하는 한 마지막에 발생한 이벤트만이 초기화되지 않고 실행됩니다.


* `debounce`함수에서 클로저로 처리되는 변수는 `eventName`, `func`, `wait`, `timeoutId`입니다.

-------
### 커링 함수

------
<h2 id="closure">정리</h2>

* 클로저란 어떤 함수에서 선언한 변수를 참조하는 내부함수를 외부로 전달할 경우, 함수의 실행 컨텍스트가 종료된 후에도 해당 변수가 사라지지 않는 현상입니다.


* 내부 함수를 외부로 전달하는 방법에는 함수를 return하는 경우뿐아니라 콜백으로 전달하는 경우도 포함됩니다.


* 클로저는 그 본질이 메모리를 계속 차지하는 개념이므로 더는 사용하지 않게 된 클로저에 대해서는 메모리를 차지하지 않도록 관리해줄 필요가 있습니다.
