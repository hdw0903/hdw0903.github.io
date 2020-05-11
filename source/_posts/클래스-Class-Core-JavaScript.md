---
title: 클래스(Class) -Core JavaScript
disqusId: tunas-blog-1
tags:
  - JavaScript
  - Core JavaScript
  - Class
  - JavaScript 상속
  - Class 상속
  - Class 와 instance
  - super class
  - sup class
  - prototype chain
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
date: 2020-05-11 19:13:14
categories: Core JavaScript
---

다른 언어의 상속 개념을 흉내 내기위해 자바스크립트 ES6에서 추가된 `Class` 문법
(**내부적으로는 프로토타입을 따름**)

* [클래스와 인스턴스의 개념 이해](2020/05/11/클래스(Class)-Core-JavaScript/#Class)
* [자바스크립트의 클래스](2020/05/11/클래스(Class)-Core-JavaScript/#JavaScript_Class)
* [클래스 상속](2020/05/11/클래스(Class)-Core-JavaScript/#Class_상속)
  * 기본 구현
  * 클래스가 구체적인 데이터를 지니지 않게 하는법
  * constructor 복구하기
  * 상위 클래스에 접근 수단
* [ES6의 클래스 및 클래스 상속](2020/05/11/클래스(Class)-Core-JavaScript/#ES6_Class)
* [정리](2020/05/11/클래스(Class)-Core-JavaScript/#point)

<!-- more -->

------
<h2 id="Class">클래스와 인스턴스의 개념 이해</h2>

상위(`superior`), 하위(`subordinate`) 개념의 앞글자를 따서
상위 클래스(`superclass`), 하위 클래스(`subclass`)로 표현합니다.

자바스크립트를 기준으로 하위 클래스(`subclass`)를 `Array`로 생각해 본다면 상위 클래스(`superclass`)는 `__proto__`와 `Array.prototype`에 따라 `Object`가 되겠습니다.

![superClass 와 subClass](/images/Class.png)

클래스의 속성을 지니는 실존하는 개체를 `instance`라고 합니다.
`instance`는 **"해당 클래스의 조건을 만족하는 구체적인 예시"**라고 해석 할 수도 있습니다.

* `Class`를 바탕으로 `instance`를 만들 때 생성된 개체가 `Class`의 속성을 지니게 됩니다.

* 또한 한 `instance`는 **하나의 클래스만을 바탕으로 만들어 집니다.**
인스턴스가 다양한 클래스에 속할 수는 있지만 인스턴스 입장에서는 모두 '직계존속'클래스들 입니다.
결국 인스턴스를 생성할 때 호출할 수 있는 클래스는 오직 하나뿐 이기 떄문입니다.
![superClass subClass](/images/superClass_subClass.jpg)
[https://coding-restaurant.tistory.com/239](https://coding-restaurant.tistory.com/239)

------
<h2 id="JavaScript_Class">자바스크립트의 클래스</h2>

생성자 함수 `Array`를 `new` 연산자와 함께 호출하면 `instance`가 생성됩니다. 이 때 `Array`를 일종의 `클래스`라고 하면 `Array`의 `prototype` 객체 내부 요소들이 `instance`에 **'상속'**된다고 볼수 있습니다. (내부적으로는 상속이 아닌 프로토 타입 체이닝에 의한 참조 입니다.)

* 한편 `Array` 내부 프로퍼티들 중 `prototype` 프로퍼티를 제외한 나머지는 `instance`에 상속되지 않습니다.

* `instance`가 참조하는지 여부에 따라 
  * 스태틱 맴버(`static member`) 와
  * 인스턴스 맴버(`instance member`)로 나뉩니다.
![static member, instance member](/images/static_member.png)

* `prototype`에 있는 내부 메서드는 `instance`가 직접 호출할 수 있습니다.

* 반대로 `prototype`에 없는 메서드는 `instance`가 참조하지 않으므로 호출할 수 없습니다.

* 이렇게 `instance`**에서 직접 접근할 수 없는 메서드를 스태틱 메서드라고 합니다.**

------
<h2 id="Class_상속">클래스 상속</h2>

------
### 기본 구현

클래스 상속은 객체지향에서 가장 중요한 요소 중 하나입니다, 하지만 자바스크립트는 ES5까지 클래스가 없었기 때문에 프로토 타입 체인을 활용해 클래스 상속을 흉내내었었습니다.

이에 대해 가볍게 알아보겠습니다.

------
#### length 프로퍼티 삭제 가능

```js length 삭제
var Grade = function() {
  var args = Array.prototype.slice.call(arguments);
  for (var i = 0; i < args.length; i++) {
    this[i] = args[i];
  }
  this.length = args.length;
};
Grade.prototype = [];
var g = new Grade(100, 80);

g.push(90);
console.log(g); // Grade { 0: 100, 1: 80, 2: 90, length: 3 }

delete g.length;
g.push(70);
console.log(g); // Grade { 0: 70, 1: 80, 2: 90, length: 1 }
```

* `length`프로퍼티를 삭제하고 다시 `push` 했더니, `push`한 값이 0번째 `index`에 들어가고, `length`가 1이 됐습니다.


* 내장객체인 `배열 instance`의 `length` 프로퍼티는 `{ configurable : false }`라서 삭제가 불가능하지만,
`Grade` 클래스의 `instance`는 **배열 메서드를 상속(참조)하지만 기본적으로는 일반 객체의 성질을 그대로 지니므로 삭제가 가능**해서 문제가 됩니다.

------
#### 빈 배열

* `push`했을 때 0번째 `index`에 70이 들어가고 `length`가 1이 된 이유:
  `g.__proto__`, 즉 `Grade.prototype`이 **빈 배열을 가리키고 있기 때문**


* `push` 명령에 의해 엔진이 `g.length`를 읽으려 하는데 `g.length`가 **존재하지 않으므로 프로토 타입 체이닝을 타고** `g.__proto__.length`을 읽어옴.


* 빈 배열의 `length`는 0 이므로 여기에 값을 할당하고 `length`는 1 만큼 증가합니다.

------
#### 요소가 있는 배열을 prototype에 매칭한 경우

```js
var Grade = function() {
  var args = Array.prototype.slice.call(arguments);
  for (var i = 0; i < args.length; i++) {
    this[i] = args[i];
  }
  this.length = args.length;
};
Grade.prototype = ['a', 'b', 'c', 'd']; // 빈 배열 아님
var g = new Grade(100, 80);

g.push(90);
console.log(g); // Grade { 0: 100, 1: 80, 2: 90, length: 3 }
// length 삭제 후
delete g.length;
g.push(70);
console.log(g); // Grade { 0: 100, 1: 80, 2: 90, ___ 4: 70, length: 5 }
```

* `Grade.prototype`에 빈 배열이 아닌 `length`가 4인 배열을 할당했습니다.


* `length`를 삭제 후 `push`한 값이 `Grade.prototype`에 빈 배열을 할당했을 때와는 다르게 동작합니다.


* `push`명령에 의해 엔진이 `g.length`를 읽으려 하는데 존재하지 않으므로 `g.__proto__.length`를 읽어오는데 `length`값이 4인 배열입니다.


* 그러므로 여기에 (`index : 4`) 70값을 할당하고 `length` 값을 1증가시켜 5가 되는것 입니다.


**이처럼 `class`에 있는 값이 `instance`의 동작에 영향을 줘서는 안됩니다.**(이런 영향을 줄 수 있다는 것 자체가 이미 클래스의 추상성을 해치는 것입니다.)

`class`는 `instance`와의 관계에서는 **구체적인 데이터를 지니지 않고** 오직 `instance`가 **사용할 메서드만을 지니는 추상적인 틀**로만 작용해야 합니다.

------
#### 사용자가 정의한 두 클래스 사이에서의 상속관계 구현

직사각형 클래스와 정사각형 클래스를 만듭니다.

* 직사각형: 두 쌍의 마주 보는 변이 평행이고 그 길이가 같습니다.

* 정사각형: 직사각형이며(직사각형의 조건을 충족) 네 변의 길이가 모두 같습니다.

```js
var Rectangle = function(width, height) {
  this.width = width;
  this.height = height;
};
Rectangle.prototype.getArea = function() {
  return this.width * this.height;
};
var rect = new Rectangle(3, 4);
console.log(rect.getArea()); // 12

var Square = function(width) {
  this.width = width;
};
Square.prototype.getArea = function() {
  return this.width * this.width;
};
var sq = new Square(5);
console.log(sq.getArea()); // 25
```



------
<h2 id="ES6_Class">ES6의 클래스 및 클래스 상속</h2>





------
<h2 id="point">정리</h2>