---
title: prototype -Core JavaScript
disqusId: tunas-blog-1
tags:
  - Core JavaScript
  - JavaScript
date: 2020-05-05 17:57:53
categories: Core JavaScript
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

* 프로토타입 (prototype)
  * [프로토타입의 개념 이해](/2020/05/05/prototype-Core-JavaScript/#프로토타입의-개념-이해)
    * constructor, prototype, instance
    * constructor 프로퍼티
  * [프로토타입 체인](/2020/05/05/prototype-Core-JavaScript/#prototype_chain)
    * 메서드 오버라이드
    * 프로토타입 체인
    * 객체 전용 메서드의 예외사항
    * 다중 프로토타입 체인
  * [정리](/2020/05/05/prototype-Core-JavaScript/#)

<!-- more -->

------
<h2 id="프로포타입의-개념-이해">프로토타입의 개념 이해</h2>

### constructor, prototype, instance

> var instance = new Constructor();

위 코드를 추상화 하여 나타내면 다음과 같습니다.

![prototype schematic](/images/prototype_schematic.png)

윗변(실선)의 왼쪽 꼭지점에는 `Constructor`(생성자 함수)를, 오른쪽 꼭짓점에는 `Constructor.prototype`이라는 프로퍼티를 위치시켰습니다.
왼쪽 꼭짓점부터 아래를 향한 화살표 중간에 `new`가 있고, 화살표의 종점에는 `instance`가 있습니다.
오른쪽 꼭짓점으로부터 대각선 아래로 향하는 화살표의 종점에는 `instance.__proto__`이라는 프로퍼티를 위치시켰습니다. 

------

* 어떤 생성자 함수(`Constructor`)를 `new` 연산자와 함께 호출하면

* `Constructor`에서 정의된 내용을 바탕으로 새로운 인스턴스(`instance`)가 생성됩니다.

* 이떄 `instance`에는 `__proto__`라는 프로퍼티가 자동으로 부여되는데,

* 이 프로퍼티는 `Constructor`의 `prototype`이라는 프로퍼티를 참조합니다.

`prototype`이라는 프로퍼티와 `__proto__` <mark>이 둘의 관계가 프로토타입 개념의 핵심입니다.</mark>

`prototype`은 객체입니다. 이를 참조하는 `__proto__` 역시 객체입니다.

`prototype` 객체 내부에는 `instance`가 사용할 메서드를 저장합니다. 그러면 `instance`에서도 숨겨진 프로퍼티인 `__proto__`를 통해 이 메서드들에 접근할 수 있게 됩니다.

------

```js Person이라는 생성자 함수의 prototype에 getName이라는 메서드를 지정한 예제
var Person = function(name) {
  this._name = name;
};
Person.prototype.getName = function() {
  return this._name;
};
```
이제 `Person`의 `instance`는 `__proto__`프로퍼티를 통해 `getName`을 호출할 수 있습니다.
`instance`의 `__proto__`가 `Constructor`의 `prototype`프로퍼티를 참조하므로 결국 둘은 같은 객체를 바라보기 때문입니다.

```js this 바인딩 값
var suzi = new Person('Suzi');
suzi.__proto__.getName(); //undefined

Person.prototype === suzi.__proto__ // true
```

* `suzi.__proto__.getName();`를 실행해 `undefined`가 나왔다는 것은 이 변수가 "호출할 수 있는 함수"에 해당한다는 것을 의미합니다. 


* 만약 함수가 아닌 다른 데이터 값이었다면 `TypeError`가 발생했을 것입니다. 에러가 아닌 `undefined`를 반환했으므로 `getName`이 실제로 실행됐고 `getName`이 함수라는 것이 입증됐습니다.


* `undefined`를 반환한 이유는 `this`의 바인딩 값이 잘못됐음을 의미합니다.


* `suzi.__proto__.getName();`에서 `getName` 함수 내부에서의 `this`는 `suzi`가 아니라 메서드명 바로앞의 객체 즉, `suzi.__proto__`를 참조하게 되는 것입니다. 


* `suzi.__proto__` 내부에 `name`프로퍼티가 없으므로 엔진이 "데이터 영역에 지정되지 않은 식별자에 접근할 때"를 뜻하는 `undefined`를 반환하게 됩니다. 

------

`__proto__` 객체에 `name` 프로퍼티가 있다면 `undefined`가 아니라 프로퍼티 값이 출력 되겠죠?

```js
var suzi = new Person('Suzi');
suzi.__proto__._name = 'SUZI__proto__';
suzi.__proto__.getName(); // SUZI__proto__
```

위 예제 코드들의 관건은 `this`가 어떤 값을 참조하게 되는가 였습니다.
`this`가 `instance`를 참조하게 하는 방법은 간단합니다.
`__proto__`를 생략하고 `instance`뒤에 바로 메서드를 작성하면 됩니다.

```js __proto__ 생략
var suzi = new Person('Suzi', 28);
suzi.getName(); // Suzi
```

이런 코드가 실행되는 이유는 `__proto__`가 생략 가능한 프로퍼티이기 때문입니다.

```js
suzi.__proto__.getName
> suzi(.__proto__).getName
== suzi.getName
```

* 정리하면 
  * `__proto__`를 생략하지 않으면 `this`는 `suzi.__proto__`를 참조 (내부에 name프로퍼티 존재하지 않음)
  * 생략하면 `suzi`를 참조 가능해짐. (suzi.getName 형태, name 프로퍼티 존재)

  ![prototype schematic2](/images/prototype_schematic2.png)

  * new 연산자로 `Constructor` 호출시 `instance` 생성되고 
  `instance`의 생략가능한 프로퍼티인 `__proto__`는 
  `Constructor`의 `prototype`을 참조

```js prototype과 __proto__
var Constructor = function(name) {
  this.name = name;
};
Constructor.prototype.method1 = function() {};
Constructor.prototype.property1 = 'Constructor Prototype Property';

var instance = new Constructor('Instance');
console.dir(Constructor); //Constructor의 디렉터리 구조 출력
console.dir(instance); //instance의 디렉터리 구조 출력
```
* 위 예제를 크롬 개발자도구에서 실행한 결과

![prototype Constructor](/images/prototype_Constructor.png)

* `Constructor`의 디렉터리 구조를 출력한
  * 첫 번째 줄에 함수라는 의미의 `f` 와 함수이름 `Constructor`, 인자 `name`이 출력되었습니다.
  * 그 내부에는 <mark>옅은 색</mark>의 argument, caller, length, name, `prototype`, &#95;&#95;proto&#95;&#95;등의 프로퍼티들이 나타납니다.
  * 내부 프로퍼티중 `prototype`을 열면 개발자가 직접 추가한 `metod1`, `property1`등의 값은 <mark>짙은 색</mark>으로 보이고, constructor, &#95;&#95;proto&#95;&#95; 등은 <mark>옅은 색</mark>으로 보입니다.
        이런 색상의 차이는 { enumerable: false } 속성이 부여된 프로퍼티인지 여부에 따릅니다.
        짙은 색은 enumerable, 즉 열거 가능한 프로퍼티임을 의미하고, 
        옅은 색은 innumerable, 즉 열거할 수 없는 프로퍼티입니다.
        for in 문 등으로 객체의 프로퍼티 전체에 접근할 때 접근 가능 여부를 
        색상으로 구분지어 표기하는 것입니다.

<div align="center">

![prototype Instance](/images/prototype_Instance.png)
</div>

* `instance`의 디렉터리 구조를 출력한
  * 첫 번째 줄에 `Constructor`가 출력됩니다.
  생성자 함수의 `instance`는 해당 생성자 함수의 이름을 표기함으로 
  해당 함수의 `instance`임을 나타냅니다.
  * `Constructor`를 열어보면 `name`프로퍼티가 짙은 색으로 표기되고, `__proto__`프로퍼티가 옅은 색으로 표기됩니다.
  * `__proto__`를 열어보면 `method1`, `property1`, `constructor`, `__proto__` 등이 있으므로,
  `Constructor`의 `prototype`과 동일한 내용으로 구성돼 있음을 확인할 수 있습니다.

------
#### 내장(built-in) 생성자 함수 Array 구조

```js Array
var arr = [1, 2];
console.dir(arr);
console.dir(Array);
```

![prototype arr and Array](/images/prototype_arr_Array.png)

* arr
  * 첫 줄에 `Array(2)`가 표기됩니다.
  * `Array` 생성자 함수를 원본으로 생성됐고, `length` 값 2를 알 수 있습니다.
  * `index 0, 1`은 짙은 색으로 length와 &#95;&#95;proto&#95;&#95;는 옅은 색으로 표기됩니다.
  * &#95;&#95;proto&#95;&#95; 에는 Array 메서드 들이 포함되어 있습니다.

* Array
  * 첫 줄에 함수를 뜻하는 `f`가 표시됩니다.
  * 함수의 프로퍼티인 `argument`, `caller`, `length`, `name`등이 표기됩니다.
  * 또한 `Array` 함수의 정적 메서드 `from`, `isArray` `of` 등도 있습니다.
  * `prototype`을 열어보면 왼쪽(arr)의 &#95;&#95;proto&#95;&#95;와 동일한 구성임을 확인할 수 있습니다.


* 위 결과를 도식으로 나타면 다음과 같습니다.
![prototype Array schematic](/images/prototype_Array_schematic.png)

* Array를 new 연산자와 함께 호출하든, 배열 리터럴을 생성하든 `instance`인 [1, 2]가 만들어집니다.


* `instance`의 `__proto__`은 Array.prototype을 참조함으로 `instance`가 push, pop, forEach 등 Array 메서드를 자신의 것처럼 호출할 수 있습니다.(`__proto__`가 생략 가능하도록 설계돼 있기 때문에)


* **한편 `Array의 prototype 프로퍼티 내부`에 있지 않은 `from, isArray` 등의 메서드들은 `instance`가 직접 호출할 수 없습니다. 이들은 `Array 생성자 함수`에서 직접 접근해야 실행 가능합니다.**

```js
// Array 생성자 함수를 원본으로하는 instance인 arr
var arr = [1, 2];

// __proto__ 생략 가능으로 인한 Array 메서드 직접 호출 
arr.forEach(function (){}); // (o)

// Array의 prototype 프로퍼티 내부에 없는 메서드는
// instance가 직접 호출 불가능
arr.isArray(); // (x) TypeError: arr.isArray is not a function

// Array 생성자 함수에서 직접 접근하여 실행해야 됨
Array.isArray(arr); // (o) true
```

------
### constructor 프로퍼티

`생성자 함수`의 프로퍼티인 `prototype` 객체 내부에는 `constructor` 프로퍼티가 있습니다.
`instance`의 `__proto__` 객체 내부에도 마찬가지로 존재합니다.
`constructor` 프로퍼티는 원래의 생성자 함수(자기 자신)를 참조하고, 
`instance`로부터 그 원형을 알 수 있는 수단으로 `instance`와의 관계에 있어 필요한 정보입니다.

```js constructor 프로퍼티 
var arr = [1, 2];
Array.prototype.constructor === Array; // true
arr.__proto__.constructor === Array; // true
arr.constructor === Array; // true

var arr2 = new arr.constructor(3, 4);
console.log(arr2); // [3, 4]
```

------
#### 다양한 constructor 접근 방법

```js
var Person = function(name) {
  this.name = name;
};
var p1 = new Person('사람1'); // Person { name: "사람1" } true
var p1Proto = Object.getPrototypeOf(p1);
var p2 = new Person.prototype.constructor('사람2'); // Person { name: "사람2" } true
var p3 = new p1Proto.constructor('사람3'); // Person { name: "사람3" } true
var p4 = new p1.__proto__.constructor('사람4'); // Person { name: "사람4" } true
var p5 = new p1.constructor('사람5'); // Person { name: "사람5" } true

[p1, p2, p3, p4, p5].forEach(function(p) {
  console.log(p, p instanceof Person);
});
```

* **다음은 모두 동일한 대상을 가리키게 됩니다.**

        
        1. [Constructor]

        2. [instance].__proto__.constructor
        
        3. [instance].constructor
        
        4. Object.getPrototypeOf([instance]).constructor
        
        5. [Constructor].prototype.constructor


* **다음은 모두 동일한 객체에 접근할 수 있습니다.**
    

      1. [Constructor].prototype
    
      2. [instance].__proto__
    
      3. [instance]
    
      4. Object.getPrototypeOf([instance])


* 따라서 p1 부터 p5까지 모두 Person의 instance입니다.

------
<h2 id="prototype_chain">프로토타입 체인</h2>

------
### 메서드 오버라이드

`instance`가 동일한 이름의 프로퍼티 또는 메서드를 가지고 있는 상황이라면

`instance.method()` 형태로 호출했을 때
`instance.__proto__.method`가 아닌 `instance`객체에 있는 해당 `method`가 호출됩니다.
여기서 일어난 현상을 메서드 위에 메서드를 덮어씌웠다고 하여 <mark>메서드 오버라이드</mark>라고합니다.

* 자바스크립트 엔진은 프로퍼티(혹은 메서드)를 찾을 때 가장 가까운 대상인 자신의 프로퍼티를 먼저 검색하고, 없으면 그다음으로 가까운 대상인 &#95;&#95;proto&#95;&#95;를 검색합니다.
그러므로 메서드 오버라이드 됐을 때 &#95;&#95;proto&#95;&#95;에 있는 메서드는 우선 순위에서 밀려 호출되지 않는 것입니다.

------
#### 메서드 오버라이드된 상태에서 prototype에 있는 메서드에 접근법

> instance.&#95;&#95;proto&#95;&#95;.method()

형태로 호출하면 정상적으로 `prototype`에 있는 `method`에 접근할 수 있습니다.
하지만 `this`가 `instance`를 바라보지 않고 있습니다.

`call`이나 `apply`를 사용하면
> instance.&#95;&#95;proto&#95;&#95;.method.call(thisArg) 형태로 작성하여 `this` 대상을 지정합니다.

------
### 프로토타입 체인

<mark>자바스크립트의 모든 객체의 최상위 객체에는 Object 객체가 존재합니다.</mark>

따라서 모든 객체의 `__proto__`에는 `Object.prototype`이 연결됩니다.

**Array 객체를 예시로 든 최상위 객체 Object와의 구조 도식(~~prototype 역시 객체입니다.~~)**

![객체의 최상위 객체 Object](/images/객체의_최상위_객체_Object.png)


* 앞에서 `__proto__`는 생략가능한 프로퍼티이므로 `배열[]`에서 `Array.prototype` 내부의 메서드를 직접 호출할 수 있었습니다. 

* 마찬가지로 `배열[]`의 `__proto__`를 계속 따라가다 보면 `Object.prototype`이 있으므로 `Object.prototype`의 내부 메서드도 직접 호출할 수 있습니다.

------
이러한 `__proto__` 프로퍼티 내부에 다시 `__proto__`프로퍼티가 연쇄적으로 이어진 것을 
**프로토타입 체인**(`prototype chain`)이라 하고, 

이 체인을 따라 검색하는 것을 **프로토타입 체이닝**(`prototype chaining`)이라고 합니다.

------
### 객체 전용 메서드의 예외사항

어떤 생성자 함수이든 `prototype`은 객체이기 때문에 `Object.prototype`이 언제나 프로토타입 체인의 최상단에 존재하게 됩니다.

따라서 **객체에서만 사용할 메서드는 다른 데이터 타입처럼 프로토타입 객체 안에 정의할 수 없습니다.**

**객체에서만 사용할 메서드를 `Object.prototype`내부에 정의한다면 다른 데이터 타입도 해당 메서드를 사용할 수 있게 되기 때문입니다.**