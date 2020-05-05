---
title: prototype -Core JavaScript
disqusId: tunas-blog-1
tags:
  - Core JavaScript
  - JavaScript
date: 2020-05-05 17:57:53
categories: Core JavaScript
---

* 프로토타입 (prototype)
  * [프로토타입의 개념 이해](/2020/05/05/prototype-Core-JavaScript/#프로토타입의-개념-이해)
    * constructor, prototype, instance
    * constructor 프로퍼티
  * [프로토타입 체인](/2020/05/05/prototype-Core-JavaScript/#)
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

<img src="/images/prototype_schematic.png">

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

  <img src="/images/prototype_schematic2.png">

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
console.dir(Constructor);
console.dir(instance);
```

