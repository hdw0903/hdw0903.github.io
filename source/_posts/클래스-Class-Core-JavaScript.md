---
title: 클래스(Class) -Core JavaScript
disqusId: tunas-blog-1
tags:
  - JavaScript
  - Core JavaScript
  - 코어 자바스크립트
  - Class
  - JavaScript 상속
  - Class 상속
  - Class 와 instance
  - super class
  - sup class
  - prototype chain
  - 빈 함수
  - Object.create
  - Object.freeze
  - ES5 ES6 클래스 문법 비교
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

* [클래스와 인스턴스의 개념 이해](/2020/05/11/클래스-Class-Core-JavaScript/#Class)
* [자바스크립트의 클래스](/2020/05/11/클래스-Class-Core-JavaScript/#JavaScript_Class)
* [클래스 상속](/2020/05/11/클래스-Class-Core-JavaScript/#Class_상속)
  * 기본 구현
  * 클래스가 구체적인 데이터를 지니지 않게 하는법
  * constructor 복구하기
  * 상위 클래스에 접근 수단
* [ES6의 클래스 및 클래스 상속](/2020/05/11/클래스-Class-Core-JavaScript/#ES6_Class)
* [정리](/2020/05/11/클래스-Class-Core-JavaScript/#point)

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
[도식 그림 출처: 코딩맛집](https://coding-restaurant.tistory.com/239)

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
* `Rectangle`과 `Square`클래스에 공통 요소가 보입니다. `width` 프로퍼티가 공통이고, `getArea`메서드는 다른 부분이 있으나 비슷합니다.


* `Square`에서 `width`프로퍼티만 쓰지 않고 `height`프로퍼티에 `width`값을 부여하는 형태로 변경한다면 `getArea`도 동일하게 쓸 수 있겠습니다.

```js Square 수정
var Square = function(width) {
  this.width = width;
  this.height = width;
};
Square.prototype.getArea = function() {
  return this.width * this.height;
};
```

* `Square`를 위와같이 수정해 주면 `Square`를 `Rectangle`의 하위 클래스로 삼을 수 있습니다.


* `getArea` 메서드는 동일한 동작을 하므로 **상위 클래스에서만 정의하고, 하위 클래스에서는 해당 메서드를 상속**하면서 `height` 대신 `width`를 넣어주면 되겠습니다.

```js Rectangle을 상속하는 Square 클래스
var Square = function(width) {
  Rectangle.call(this, width, width);
};
Square.prototype = new Rectangle();
```

* `Square` 생성자 함수 내부에서 `Rectangle` 생성자 함수를 함수로 호출하고 **인자 height 자리에 width를 전달합니다.**


* `Square.prototype = new Rectangle();`  : 메서드를 상속하기 위해 프로토타입 객체에 `Rectangle`의 `instance`를 부여했습니다.

하지만 위 코드만으로 **완벽한 클래스 체계가 구축됐다고 볼 수는 없습니다.**

**아직 클래스에 있는 값이 인스턴스에 영향을 줄 수 있는 구조이기 때문입니다.**

* `console.dir(sq);`로 sq `instance`에 대하여 콘솔로 출력해보면
![console.dir(sq);](/images/dir_sq.png)
첫 줄에서 `Square`의 `instance`을 표시하고 있고 `width`와 `height`에 5가 잘 들어있습니다. `__proto__`는 `Rectangle`의 `instance`임을 표시하고 이어서 `width`, `height`에 모두 `undefined`가 할당되어 있습니다. `Square.prototype`에 값이 존재하여 이후에 임의로 `Square.prototype.width (또는 height)`에 값을 부여하고 `sq.width(또는 height)`의 값을 지워버린다면 프로토타입 체이닝에 의해 엉뚱한 결과가 나오는 문제가 생길 수 있습니다.

![Rectangle -> Square 상속 관계 구현 도식](/images/Square도식.jfif)
[도식 그림 출처: 코딩맛집](https://coding-restaurant.tistory.com/239)

나아가 `constructor`가 여전히 `Rectangle`을 바라보고 있는 문제도 있습니다. `sq.constructor`로 접근하면 프로토타입 체이닝을 따라 `sq.__proto__.__proto__,` 즉 `Rectangle.prototype`에서 찾게 되며 이는 `Rectangle`을 가리키고 있기 때문입니다.

```js
var rect2 = new sq.constructor(2, 3);
console.log(rect2); // Rectangle {width: 2, height: 3}
```

이처럼 하위 클래스로 삼을 생성자 함수의 `prototype` 에 상위 클래스의 `instance`를 부여하는 것만으로도 기본적 메서드 상속은 가능하지만 다양한 문제가 발생할 여지가 있어 구조적 안정성이 떨어집니다.

------
### 클래스가 구체적인 데이터를 지니지 않게 하는법

클래스 (prototype)가 구체적인 데이터를 지니지 않게 하는 방법 중 
가장 쉬운 방법은 일단 만들고 나서 프로퍼티들을 일일히 지우고 더는 **새로운 프로퍼티를 추가할 수 없게 하는 것**입니다.

```js
delete Square.prototype.width; 
delete Square.prototype.height; 
Object.freeze(Square.prototype);
```

프로퍼티가 많다면 반복 작업이 될테니 반복을 없애고 좀 더 범용적으로 이런 동작을 하는 함수를 만들면 좋겠습니다.

```js 인스턴스 생성 후 프로퍼티 제거
var extendClass1 = function(SuperClass, SubClass, subMethods) {
  SubClass.prototype = new SuperClass();
  for (var prop in SubClass.prototype) {
    if (SubClass.prototype.hasOwnProperty(prop)) {
      delete SubClass.prototype[prop];
    }
  }
  if (subMethods) {
    for (var method in subMethods) {
      SubClass.prototype[method] = subMethods[method];
    }
  }
  Object.freeze(SubClass.prototype);
  return SubClass;
};

var Rectangle = function(width, height) {
  this.width = width;
  this.height = height;
};
Rectangle.prototype.getArea = function() {
  return this.width * this.height;
};
var Square = extendClass1(Rectangle, function(width) {
  Rectangle.call(this, width, width);
});
var sq = new Square(5);
console.log(sq.getArea()); // 25
```

* `extendClass1` 함수는 `SuperClass`와 `SubClass`, `SubClass`에 추가할 메서드들이 정의된 객체를 받아 `SubClass`의 `prototype` 내용을 정리하고 `freeze`하는 내용으로 구성돼있습니다.

------
#### 두 번째 방법(빈 함수)

더글라스 크락포드가 제시하여 대중적으로 알려진 방법입니다.
`SubClass`의 `prototype`에 직접 `SubClass`의 `instance`를 할당하는 대신

**아무런 프로퍼티를 생성하지 않는 빈 생성자 함수(`Bridge`)를 하나 더 만들어서** 그 `prototype`이 `SubClass`의 `prototype`을 바라보게 한 다음,`SubClass`의 `prototype`에는 `Bridge`의 `instance`를 할당하게 하는 것입니다.
(**빈 함수에 다리 역활을 부여**)

![클래스 상속 및 추상화 방법(2)- 빈 함수 활용](/images/Class2.png)

* `Bridge`라는 빈 함수를 만들고, `Bridge.prototype`이 `Rectangle.prototype`을 참조하게 한 다음, `Square.prototype`에 `new Bridge()`로 할당하면, 우측 그림처럼 `Rectangle` 자리에 `Bridge`가 대체됩니다.


* 이로써 `instance`를 제외한 **프로토타입 체인 경로상에는 더는 구체적인 데이터가 남아있지 않게 됩니다**.

마찬가지로 반복작업을 없애기 위해 범용적으로 이런 동작을 하는 함수를 만들어 보겠습니다.

```js 빈 함수를 활용
var extendClass2 = (function() {
  var Bridge = function() {};
  return function(SuperClass, SubClass, subMethods) {
    Bridge.prototype = SuperClass.prototype;
    SubClass.prototype = new Bridge();
    if (subMethods) {
      for (var method in subMethods) {
        SubClass.prototype[method] = subMethods[method];
      }
    }
    Object.freeze(SubClass.prototype);
    return SubClass;
  };
})();

var Rectangle = function(width, height) {
  this.width = width;
  this.height = height;
};
Rectangle.prototype.getArea = function() {
  return this.width * this.height;
};
var Square = extendClass2(Rectangle, function(width) {
  Rectangle.call(this, width, width);
});
var sq = new Square(5);
console.log(sq.getArea()); // 25
```

* 즉시실행함수 내부에서 `Bridge`를 선언하여 이를 클로저로 활용함으로써 메모리에 불필요한 함수 선언을 줄였습니다.

* `subMethods`에는 `SubClass`의 `prototype`이 담길 메서드들을 객체로 전달하게 했습니다.

------
#### 세 번째 방법(Object.create)

세 번째 방법은 ES5에서 도입된 `Object.create`를 이용한 방법으로 이 방법은

`SubClass`의 `prototype`의 `__proto__`가 `SuperClass`의 `prototype`을 바라보되, `SuperClass`의 `instance`가 되지는 않으므로 앞서 소개한 두 방법보다 간편하면서 안전합니다.

```js Object.create 활용
var Rectangle = function(width, height) {
  this.width = width;
  this.height = height;
};
Rectangle.prototype.getArea = function() {
  return this.width * this.height;
};
var Square = function(width) {
  Rectangle.call(this, width, width);
};
Square.prototype = Object.create(Rectangle.prototype);
Object.freeze(Square.prototype);

var sq = new Square(5);
console.log(sq.getArea()); // 25
```

~~클래스 상속 및 추상화를 흉내 내기 위한 라이브러리가 많이 있지만 기본적인 접근 방법은 위 세가지 아이디어를 크게 벗어나지 않습니다.~~

결론적으로 `SubClass.prototype`의 `__proto__`가 `SuperClass.prototype`을 참조하고, `SubClass.prototype`에는 불필요한 `instance 프로퍼티`가 남아있으면 안되기 때문입니다.

------
### constructor 복구하기

위 세 가지 방법 모두 기본적인 상속에는 성공했지만, `SubClass` `instance`의 `constructor`는 여전히 `Superclass`를 가리키는 상태입니다.

* 엄밀히는 `SubClass`의 `instance`에는 `constructor`가 없고, `SubClass.prototype`에도 없는 상태입니다.


* 프로토타입 체인상에 가장 먼저 등장하는 `SuperClass.prototype`의 `constructor`가 가리키는 대상인 `SuperClass`가 출력되는 것입니다.

따라서 `SubClass.prototype.constructor`가 원래의 `SubClass`를 바라보도록 해주겠습니다.

```js 인스턴스 생성 후 프로퍼티 제거 + constructor 복구
var extendClass1 = function(SuperClass, SubClass, subMethods) {
  SubClass.prototype = new SuperClass();
  for (var prop in SubClass.prototype) {
    if (SubClass.prototype.hasOwnProperty(prop)) {
      delete SubClass.prototype[prop];
    }
  }
  //SubClass.prototype.constructor가 원래의 SubClass를 바라보도록 함
  SubClass.prototype.consturctor = SubClass;
  if (subMethods) {
    for (var method in subMethods) {
      SubClass.prototype[method] = subMethods[method];
    }
  }
  Object.freeze(SubClass.prototype);
  return SubClass;
};
```

```js 빈 함수 활용 + constructor 복구
var extendClass2 = (function() {
  var Bridge = function() {};
  return function(SuperClass, SubClass, subMethods) {
    Bridge.prototype = SuperClass.prototype;
    SubClass.prototype = new Bridge();
    SubClass.prototype.consturctor = SubClass;
    //SuperClass와의 관계를 복구하기 위해
    //Bridge.prototype.constructor가 SuperClass를 바라보게 하는 작업 추가
    Bridge.prototype.constructor = SuperClass;
    if (subMethods) {
      for (var method in subMethods) {
        SubClass.prototype[method] = subMethods[method];
      }
    }
    Object.freeze(SubClass.prototype);
    return SubClass;
  };
})();
```
빈 함수(`Bridge`)를 이용한 두 번째 방법의 경우 `SubClass.prototype`이 `SuperClass` 대신 `Bridge`의 `instance`를 바라보는 상태이므로 `SuperClass`와의 관계를 복구하기 위해 `Bridge.prototype.constructor`가 `SuperClass`를 바라보게 하는 작업이 추가돼야 합니다. 


```js Object.create 활용 + constructor 복구
var extendClass3 = function(SuperClass, SubClass, subMethods) {
  SubClass.prototype = Object.create(SuperClass.prototype);
  //SubClass.prototype.constructor가 원래의 SubClass를 바라보도록 함
  SubClass.prototype.constructor = SubClass;
  if (subMethods) {
    for (var method in subMethods) {
      SubClass.prototype[method] = subMethods[method];
    }
  }
  Object.freeze(SubClass.prototype);
  return SubClass;
};
```

------
### 상위 클래스에 접근 수단

하위 클래스의 메서드에서 **상위 클래스의 메서드 실행 결과를 바탕으로 추가적인 작업을 수행하고 싶을 때**

```js
SuperClass.prototype.method.apply(this, arguments)
```
매번 이런식으로 코드를 추가해서 해결하는 것은 상당히 번거롭고 가독성이 떨어지는 방식입니다.

하위 클래스에서 **상위 클래스의 프로토타입 메서드에 접근하기 위한 별도의 수단**이 있다면 편리할 것 같습니다.

이런 **별도의 수단**인 다른 객체지향 언어들의 클래스 문법 중 하나인 `super`를 흉내 내보겠습니다.

```js 상위 클래스 접근 수단 super 메서드 추가
var extendClass = function(SuperClass, SubClass, subMethods) {
  SubClass.prototype = Object.create(SuperClass.prototype);
  SubClass.prototype.constructor = SubClass;
  SubClass.prototype.super = function(propName) {
    // 추가된 부분 시작
    var self = this;
    if (!propName) return function() { //인자가 비어있을 경우 
        SuperClass.apply(self, arguments);// SuperClass 생성자 함수에 접근하는 것으로 간주
      };
    var prop = SuperClass.prototype[propName]; //SuperClass.prototype 내부의 propName에 해당하는 값이     
    if (typeof prop !== 'function') return prop;//함수가 아닌 경우 해당값을 그대로 반환합니다.
    return function() { //함수인 경우
      return prop.apply(self, arguments); //메서드에 접근하는 것으로 여김
    };
  }; // 추가된 부분 끝
  if (subMethods) {
    for (var method in subMethods) {
      SubClass.prototype[method] = subMethods[method];
    }
  }
  Object.freeze(SubClass.prototype);
  return SubClass;
};

var Rectangle = function(width, height) {
  this.width = width;
  this.height = height;
};
Rectangle.prototype.getArea = function() {
  return this.width * this.height;
};
var Square = extendClass(Rectangle, function(width) {
    this.super()(width, width); // super 사용 (1)
  }, {
    getArea: function() {
      console.log('size is :', this.super('getArea')()); // super 사용 (2)
    }
  }
);
var sq = new Square(10);
sq.getArea(); // size is : 100
console.log(sq.super('getArea')()); // 100
```

* 추가된 부분에서 `super` 메서드의 동작을 정의하고 있습니다.


* 7번째 줄에서 인자가 비어있을 경우 `SuperClass` 생성자 함수에 접근하는 것으로 간주했습니다.


* `this`가 달라지는 것을 막기 위해 클로저를 활용했습니다.


* 11번째 줄은 `SuperClass`의 `prototype` 내부의 `propName`에 해당하는 값이 함수가 아닌 경우 해당값을 그대로 반환합니다.


* 12번째 줄은 함수인 경우이므로 마찬가지로 클로저를 활용해 메서드에 접근하는 것으로 여기도록 했습니다.

이제 `SuperClass`의 **생성자 함수에 접근하고자 할 때는** `this.super()`, `SuperClass`의 **프로토타입 메서드에 접근하고자 할 때는** `this.super(propName)`과 같이 사용할 수 있습니다.

------
<h2 id="ES6_Class">ES6의 클래스 및 클래스 상속</h2>

* `ES6`에서는 본격적으로 클래스 문법이 도입됐습니다.

* `ES5`에서의 생성자 함수 및 프로토타입 과 `ES6`의 클래스 문법을 비교해봅니다.

------
#### ES5 / ES6 클래스 문법 비교

```js ES5와 ES6 클래스 문법 비교
var ES5 = function(name) {
  this.name = name;
};
ES5.staticMethod = function() {
  return this.name + ' staticMethod';
};
ES5.prototype.method = function() {
  return this.name + ' method';
};
var es5Instance = new ES5('es5');
console.log(ES5.staticMethod()); // es5 staticMethod
console.log(es5Instance.method()); // es5 method

/////////////////////////////////

var ES6 = class { //중괄호 {}내부가 클래스 본문 영역입니다.
  constructor(name) { //클래스 본문에서는 'function'키워드를 생략하더라도
    this.name = name; //모두 메서드로 인식합니다.
  } //메서드와 다음 메서드 사이에는 콤마(,)로 구분하지 않습니다.
  static staticMethod() { //static 키워드는 해당 메서드가 static 메서드라는 뜻입니다.
    return this.name + ' staticMethod'; //ES5의 생성자 함수에 바로 할당하는 메서드와 동일하게
  }                                     //생성자 함수(클래스) 자신만이 호출할 수 있습니다.
  method() { //method는 자동으로 prototype 객체 내부에 할당되는 메서드입니다.
    return this.name + ' method';//ES5.prototype.method와 동일하게, 
  } //instance가 프로토타입 체이닝을 통해 자신의 것처럼 직접 호출가능합니다.
};
var es6Instance = new ES6('es6');
console.log(ES6.staticMethod()); // es6 staticMethod
console.log(es6Instance.method()); // es6 method
```

------
#### ES6의 클래스 상속

```js ES6의 클래스 상속
var Rectangle = class {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  getArea() {
    return this.width * this.height;
  }
};
var Square = class extends Rectangle {
  constructor(width) {
    super(width, width);
  }
  getArea() {
    console.log('size is :', super.getArea());
  }
};
```

* `Square`를 `Rectangle` 클래스를 상속받게 하기위해 `class`명렁어 뒤에 `extends Rectangle`을 추가합니다.
이것으로 상속 관계 설정이 완료됩니다.


* `constructor` 내부에서는 `super`라는 키워드를 함수처럼 사용할 수 있습니다. 이 함수는 `SuperClass`의 `constructor`를 실행합니다.


* `constructor` 메서드를 제외한 다른 메서드에서는 `super`키워드를 마치 객체처럼 사용할 수 있고, 이때 객체는 `SuperClass.prototype`을 바라보는데, **호출한 메서드의 `this`는 `super`가 아닌 원래의 `this`를 그대로 따릅니다.**

[ES6의 Class 오브젝트 더 자세히 알아보기](https://hdw0903.github.io/2020/04/01/Class%20%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8A%B8%20-ECMAScript/)

------
<h2 id="point">정리</h2>

* 클래스는 어떤 사물의 공통 속성을 모아 정의한 추상적인 개념, `instance`는 클래스의 속성을 갖는 구체적인 사례
상위 클래스(`SuperClass`)의 조건을 충족하면서 더욱 구체적인 조건이 추가된 것을 하위 클래스(`SubClass`)라고 함.


* 클래스의 `prototype` 내부에 정의된 메서드를 `프로토타입 메서드`라고 하며, 이들은 `instance`가 마치 자신의 것처럼 호출할 수 있습니다.


* 클래스(생성자 함수)에 직접 정의한 메서드를 `스태틱 메서드`라고 하며, 이들은 `instance`가 직접 호출할 수 없고 클래스(생성자 함수)에 의해서만 호출할 수 있음.


* 클래스 상속을 흉내 내기 위한 세 가지 방법
  
  * `SubClass.prototype`에 `Superclass`의 `instance`를 할당하고 프로퍼티를 모두 삭제하는 방법.
  
  * 빈 함수(`Bridge`)를 할용하는 방법

  * Object.create를 이용하는 방법

  **세 방법 모두 `constructor` 프로퍼티가 원래의 생성자 함수를 바라보도록 조정해 줘야함** 