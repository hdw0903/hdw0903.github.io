---
title: Class 오브젝트 -ECMAScript
date: 2020-04-01 09:45:41
categories: ECMAScript6
disqusId: tunas-blog-1
tag: 
- ECMAScript6
- JavaScript
- Class
- OOP
- strict 모드에서 실행
- 클래스에 메서드 작성 방법
- prototype property
- constructor
- constructor
- getter
- setter
- extends 
- super 
- method Overriding
- static
- Class 호이스팅
- computed name
- this
- generator
- name
- Image
- Abstraction
- Class bult-in Object
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

Class(클래스)를 완전하게 이해하려면 객체지항 프로그래밍(OOP:Object Oriented Programming)에 대한 이해가 필요합니다.

OOP만 다루는 책이 있을 정도로 범위가 넓고 깊으므로  
OOP는 나중에 자세히 다루고 ES6기준으로 살펴봅니다.

*   Class 오브젝트
    *   [Class 선언문](/2020/04/01/Class%20오브젝트%20-ECMAScript/#Class_선언문)
    *   [Class 표현식](/2020/04/01/Class%20오브젝트%20-ECMAScript/#Class_표현식)
    *   [Class 특징](/2020/04/01/Class%20오브젝트%20-ECMAScript/#클래스_특징)
        *   strict 모드에서 실행
        *   클래스에 메서드 작성 방법
        *   prototype에 프로퍼티 연결
        *   prototype에 프로퍼티 추가
    *   [constructor](/2020/04/01/Class%20오브젝트%20-ECMAScript/#constructor)
    *   [constructor 반환 값 변경](/2020/04/01/Class%20오브젝트%20-ECMAScript/#constructor_return)
    *   [getter, setter](/2020/04/01/Class%20오브젝트%20-ECMAScript/#getter_and_setter)
    *   [상속](/2020/04/01/Class%20오브젝트%20-ECMAScript/#상속)
    *   [extends 키워드](/2020/04/01/Class%20오브젝트%20-ECMAScript/#extends)
    *   [super 키워드](/2020/04/01/Class%20오브젝트%20-ECMAScript/#super)
        *   메서드 오버라이딩
    *   [빌트인 오브젝트 상속](/2020/04/01/Class%20오브젝트%20-ECMAScript/#builtin)
    *   [Object에서 super 사용](/2020/04/01/Class%20오브젝트%20-ECMAScript/#Object_super)
    *   [static 키워드](/2020/04/01/Class%20오브젝트%20-ECMAScript/#static)
    *   [Class 호이스팅](/2020/04/01/Class%20오브젝트%20-ECMAScript/#Class_Hoisting)
    *   [computed name](/2020/04/01/Class%20오브젝트%20-ECMAScript/#computed_name)
    *   [this](/2020/04/01/Class%20오브젝트%20-ECMAScript/#this)
    *   [제너레이터](/2020/04/01/Class%20오브젝트%20-ECMAScript/#generator)
    *   [new.target](/2020/04/01/Class%20오브젝트%20-ECMAScript/#new_target)
        *   name 프로퍼티
    *   [오브젝트 상속](/2020/04/01/Class%20오브젝트%20-ECMAScript/#Image_Object)

<!-- more -->

* * *

<h2 id="Class_선언문">Class 선언문</h2>

> Class name {}  
> Class name extends super_name {}

엔진이 `function` 키워드를 만나면 `Function` 오브젝트를 생성하듯이  
`class` 키워드를 만나면 `Class` 오브젝트를 생성합니다.  
`Class` 오브젝트는 `Function` 오브젝트, `String` 오브젝트와 같이 **하나의 오트젝트 타입입니다.**

*   `name`에 클래스 이름을 작성합니다.  
    `name` 다음의 `extends`는 키워드로 `super_name`(슈퍼 클래스)를 상속받을 때 사용합니다. 
    이 형태를 클래스 선언문 이라고 합니다.

`class`는 클래스를 선언하는 키워드이고  
엔진이 `class` 키워드로 생성한 오브젝트는 `Class` 오브젝트 입니다.  
문맥에 따라 `Class` 오브젝트를 `class`라고 말하기도 합니다.

```js
class Member {  
 getName() {  
 return "이름";  
 }  
};  
1. let obj = new Member();  
  
2. console.log(obj.getName());  
// 이름  
```

*   `class` 키워드를 작성하고 이어서 클래스 이름을 작성합니다. 그리고 블록{}을 작성하고 블록에 클래스 코드를 작성합니다. 엔진이 `class` 키워드를 만나면 클래스(`Class`) 오브젝트를 생성합니다.

1.  `new` 연산자로 `Member()`를 호출하면 인스턴스를 생성하여 반환합니다.
    
    **`function name(){}`는 `new` 연산자로 인스턴스를 생성하지 않고 `name()`형태로 호출할 수 있지만,** 
    **클래스는 `new` 연산자로 인스턴스 생성없이 `name()`형태로 호출할 수 없으며 `TypeError`가 발생합니다.**


2.  `obj`는 인스턴스이고 `getName()`은 `class Member{}`안에 작성한 메서드 입니다.  
    이와 같이 `new` 연산자로 생성한 인스턴스를 사용하여 클래스에 작성한 메서드를 호출할 수 있습니다. 실행 결과 “이름”이 출력됩니다.
    

*   참고
    
    > new 연산자로 인스턴스를 생성하는 함수는 함수 이름의 첫 문자를 대문자로 작성합니다. 
    클래스 또한 new 연산자로 인스턴스를 생성하므로 클래스 이름의 첫 문자를 대문자로 사용합니다. 
    이는 스펙에 정의된 것은 아니며 자바스크립트 개발자들 사이의 관례입니다. 
    개발자가 코드의 대문자를 보고 인스턴스를 생성한다는 것을 알 수 있으므로 지키는 것이 좋습니다. 빌트인 Number 오브젝트, String 오브젝트도 이와 같은 맥락입니다.
    

*   같은 클래스를 두 번 선언하려고 시도할 때  
    클래스 선언문으로 같은 클래스를 두 번 선언하면 오류가 발생합니다.
    
    > class Foo {};  
    > class Foo {}; // Uncaught SyntaxError: Identifier ‘Foo’ has already been declared
    

*   이전에 표현식으로 정의한 경우에도 오류가 발생합니다.
    
    > var Foo = class {};  
    > class Foo {}; // Uncaught TypeError: Identifier ‘Foo’ has already been declared
    

* * *

<h2 id="Class_표현식">Class 표현식</h2>

클래스 표현식으로 클래스를 선언합니다.

> let name = class {}  
> let name = class inner_name {}  
> let name = class extends super_name {}  
> let name = class inner_name extends super_name {}

할당 연산자 (`=`) 왼쪽에 클래스 이름을 작성하고 오른쪽에 `class` 키워드를 작성하고 블록{}안에 클래스 코드를 작성합니다. 이를 클래스 표현식이라고 합니다.

`Class` 표현식은 이름을 가질 수도 있고, 갖지 않을 수도 있습니다. 이름을 가진 `class` 표현식의 이름은 클래스의 `body`에 대해 `local scope`에 한하여 유효합니다.

4번째 구문 `class` 와 `extends` 키워드 사이의 `inner_name`은 클래스 안에서 자신을 호출할 때 사용합니다. ~~function 키워드 함수에서도 inner_name을 작성할 수 있지만 사용하지 않듯 클래스도 사용하지 않습니다.~~

```js
let Member = class {  
 getName() {  
 return "이름";  
 }  
};  
let obj = new Member();  
console.log(obj.getName());  
```

엔진이 클래스 키워드를 만나면 클래스 오브젝트를 생성하여 `Member` 변수에 할당합니다.

* * *

<h2 id="클래스_특징">클래스 특징</h2>

`Class`는 사실 함수입니다. 함수를 함수 표현식과 함수 선언으로 정의할 수 있듯이 `class` 문법도 `class` 표현식과 `class` 선언 두 가지 방법을 제공합니다.

------
### strict 모드에서 실행
    
**“use strict”을 선언하지 않아도 클래스의 코드는 strict 모드에서 실행됩니다.**
    
------
###  클래스에 메서드 작성 방법
   
```js
class Member{  
 1. setName(name) {  
 this.name = name;  
 }  
 2. getName() {  
 return this.name;  
 }  
};  
4. Member.prototype.getTitle = function(){};  
3. console.log(typeof Member);  
// function  
```

*   `setName(){ }` 과 같이 `function` 키워드와 `콜론(;)`을 작성하지 않고 메서드 이름만 작성합니다.
    

*   `setName()`과 `getName()` 메서드 사이에 콤마를 작성하지 않습니다.
    

*   클래스의 `typeof`는 `function` 입니다. 이는 `class`가 `function` 구조라는 것을 의미합니다.
    

*   function name(){ }은 글로벌 오브젝트(window Object)에 설정되지만 class name(){ }은 글로벌오브젝트에 설정되지 않습니다.
따라서 `window.Member`로 클래스에 접근하면 `undefined`가 반환됩니다.  
    **Class 오브젝트의 프로퍼티는 for()문 등으로 열거할 수 없습니다.**
        
------
### prototype에 프로퍼티 연결

```js
class Member{  
 setName(name) {  
 this.name = name;  
 }  
};  
```

~~prototype에 메서드를 연결하여 prototype.setName과 같이 작성하지 않고~~  
`setName`을 작성합니다. 자바스크립트는 기본적으로 `prototype`에 메서드를 연결하는 구조이므로 **클래스 안에 작성된 메서드를 엔진이 자동으로 prototype에 연결합니다.**  
즉. 엔진이 `Member.prototype.setName` 형태로 연결해줍니다.

이는 중요한 의미를 갖습니다. 자바스크립트의 기본 아키텍처(구성 방식 혹은 컴퓨터 소프트웨어의 호환성)를 유지하면서 객체지향 언어의 특징을 반영하려는 접근입니다.

-----
### prototype에 프로퍼티 추가


> Member.prototype.getTitle = function(  ){ };  

**클래스 밖에서 Member 클래스에 메서드를 추가하려면** 위와 같이  
`Member.prototype`에 메서드를 연결하여 작성합니다.  
`Member` 클래스를 선언할 때는 클래스 블록{}안에 작성하겠지만,  
인스턴스를 생성한 후 상황에 따라 추가할 때 이 형태로 작성합니다.

*   참고  

    이렇게 메서드를 추가하면 **이미 생성된 인스턴스에서 추가한 메서드를 공유할 수 있도록 엔진이 처리하게 되므로 부하 혹은 자원낭비**가 됩니다.  
    좋은 사용 예시로는 사용자의 행동이나 서버 데이터에 따라 메서드를 따로 추가할 수 있는 점이 있습니다.(역동성이 높다.)

* * *

<h2 id="constructor">constructor</h2>

*   `constructor` 메소드는 `class` 인스턴스를 생성하고 생성된 인스턴스를 초기화하는 역활을 합니다.


*   `“constructor”` 라는 이름을 가진 메소드는 클래스 안에 한 개만 존재할 수 있습니다. 만약 클래스에 여러 개의 `constructor` 메소드가 존재하면 `SyntaxError` 가 발생할 것입니다.


*   `constructor`는 부모 클래스의 `constructor` 를 호출하기 위해 `super` 키워드를 사용할 수 있습니다.


*   참고  

    클래스에 `constructor`를 작성하지 않으면 `prototype`의 `constructor`가 호출됩니다.  
    이를 디폴트 `constructor`라고 하고 `constructor`가 없으면 인스턴스를 생성할 수 없습니다.  
    `ES5` 에서 클래스 오브젝트를 실행하면 엔진이 디폴트 `constructor`를 호출해서 이를 활용할 수 없었습니다.  
    `ES6` 에서는 개발자가 `constructor`를 정의할 수 있어서 `Class` 오브젝트 뿐 아니라  
    `Proxy` 오브젝트, `Reflect` 오브젝트에서 활용할 수 있습니다.

```js
1. class Member {  
 constructor(name){  
 this.name = name;  
 }  
 getName() {  
 return this.name;  
 }  
}  
2. let memberObj = new Member("스포츠");  
3. console.log(memberObj.getName());  
// 스포츠  
```

1.  클래스 안에 `constructor`를 작성했으며 `constructor` 안에 생성한 인스턴스에 초기값을 설정하는 코드를 작성했습니다.  
    `constructor` 안의 `this`는 생성하는 인스턴스를 참조합니다.

2.  `new Member(“스포츠”)`를 실행하면 `Member` 클래스에 작성한 `constructor`가 자동으로 호출되며 파라미터 값으로 `“스포츠”`를 넘겨 줍니다.
    
    *   `new` 연산자는 `constructor`를 호출하면서 파라미터를 넘겨주는 역활
    *   호출된 `constructor`가 인스턴스를 생성하여 반환하면 `new` 연산자가 받아 `new`를 실행한 곳으로 반환합니다.

------
* ### <mark>클래스 와 인스턴스 생성 과정 이해를 위한 개념적인 순서</mark>

    1. new Member("스포츠")를 실행합니다.

    2. new 연산자가 constructor를 호출하면서 파라미터 값을 넘겨줍니다.

    3. constructor의 블록 코드를 실행하기 전에 빈 Object 오브젝트를생성합니다.

    4. 이 것이 인스턴스입니다. 인스턴스가 생성되면 빈 오브젝트를 채웁니다.

    5. 인스턴스 구성에 필요한 프로퍼티를 Object 오브젝트에 설정합니다.

    6. constructor 블록에 있는 코드를 실행합니다.

    7. 인스턴스를 먼저 생성하므로 constructor에서 this로 인스턴스를 참조할수    있습니다.

    8. 생성된 인스턴스를 반환합니다.


3.  `console.log(memberObj.getName());`를 호출하면 다음 코드가 실행됩니다.

>getName() {
 return this.name;
}

`constructor`에서 `this`에 “스포츠”를 설정했으므로 “스포츠”가 반환됩니다.

아래는 생성된 `memberObj` 인스턴스 구조입니다.

<img src="/images/constructorInstance.JPG">

`constructor`에서 파라미터로 받은 “스포츠”를 `this.name`에 할당했으며,  
이때 `this`가 생성하는 인스턴스를 참조하므로 인스턴스 프로퍼티로 설정됩니다.

*   &#95;&#95;proto&#95;&#95;는 인스턴스를 생성하면 엔진이 자동으로 첨부합니다.


*   &#95;&#95;proto&#95;&#95;에 `Member.prototype`에 연결된 프로퍼티를 첨부하므로 `getName`도 첨부됩니다.


*   &#95;&#95;proto&#95;&#95;에 `Object` 오브젝트의 `prototype`에 연결된 프로퍼티가 첨부됩니다.

* * *

<h2 id="constructor_return">constructor 반환 값 변경</h2>

`constructor`에는 일반적으로 `return` 문을 작성하지 않으며, 생성한 인스턴스를 반환합니다.  
`return`을 사용하면 인스턴스 이외의 값을 반환할 수 있습니다.

```js
class Member {  
 constructor(){  
 return 1;  
 }  
 getName(){  
 return "이름";  
 }  
};  
let memberObj = new Member();  
console.log(memberObj.getName());  
```

`constructor(){ }`안에 `return 1;`을 작성하였습니다.  
~~일반적으로 숫자 값을 반환하지 않습니다, 엔진 처리 방법 예시를 위해 작성되었습니다.~~  
<mark>constructor에서 Number 또는 String 값을 반환하면 이를 무시하고 생성한 인스턴스를 반환합니다.</mark>

`console.log(memberObj.getName();)`을 호출하면 `constructor`에서 1을 반환하여  
`memberObj`에 1이 설정됩니다. 이후에 `getName()`을 호출하면 인스턴스 1에 `getName()`이 존재하지 않으므로 에러가 발생합니다. 하지만 `Member` 인스턴스를 반환하므로 `getName()`이 호출됩니다.

```js
class Member {  
 constructor(){  
 return {name: "홍길동"};  
 }  
 getName(){  
 return "이름";  
 }  
}  
let memberObj = new Member();  
  
1. console.log(memberObj.name);  
2. console.log(memberObj.getName);  
// 홍길동  
// undefined  
```

`constructor`에서 `Object`오브젝트를 `return`하면 이를 반환합니다. 즉, `{name: “홍길동”}`이 반환됩니다.

1.  `memberObj`에 반환된 `{name: “홍길동”}`이 설정되어 있으므로 `memberObj.name` 값이 홍길동 으로 출력됩니다.


2.  클래스에 `getName` 메서드를 작성했지만, 인스턴스를 반환하지 않고 `{name: “홍길동”}`을 반환하므로  
    `MemberObj`에 `getName`이 존재하지 않습니다. `undefined`가 출력됩니다.

* * *

<h2 id="getter_and_setter">getter, setter</h2>

클래스에도 `getter`와 `setter`를 선언할 수 있습니다.  
메서드 이름 앞에 `“get”`을 작성하면 `getter`, `“set”`을 작성하면 `setter`가 됩니다.

```js getter
class Member {  
 get getName() {  
 return "이름";  
 }  
};  
let memberObj = new Member();  
console.log(memberObj.getName);  
// 이름  
```

`get getName()`과 같이 메서드 이름 앞에 `get`을 작성하여 `getter`로 선언합니다.

`getName`이 `getter`이므로 메서드로 호출됩니다.

```js setter
class Member {  
 set setName(name) {  
 this.name = name;  
 }  
 get getName() {  
 return this.name;  
 }  
};  
let memberObj = new Member();  
memberObj.setName = "이름";  
  
console.log(memberObj.getName);  
// 이름  
```

`memberObj.setName = “이름”`과 같이 `setter`로 선언된 메서드 이름에 값을 할당하면  
`setName`이 메서드로 호출됩니다. 이때 할당하는 값인 “이름”을 파라미터 값으로 넘겨줍니다.

* * *

<h2 id="상속">상속</h2>

객체지향 프로그래밍에서 상속은 주요한 기능 중 하나입니다.  
**클래스를 상속받으면 상속받은 클래스의 메서드와 프로퍼티를 사용할 수 있습니다.**

*   참고  

    상속해 주는 클래스를 일반적으로 부모 클래스라고 부릅니다만 앞으로는 `“슈퍼 클래스”`라고 표기해줍시다.  
    슈퍼 클래스라고 표기하는 이유는 ES6에서 super키워드가 있으며 슈퍼 클래스를 지칭하므로 직관적이기 때문입니다.  
    상속받는 클래스도 일반적으로 자식 클래스라고 부릅니다만, 슈퍼 클래스와 운을 맞추기 위해 `“서브 클레스”`로 표기해줍시다.


<mark>자바스크립트 (객채지향 프로그래밍)의 상속 형태는 상속이 아니다?</mark>

일반적으로 상속이라고 하면 부모의 재산을 자식에게 물려주면 자식이 부모의 능력을 고스란히 물려받습니다.  

`자바스크립트`에서는 객체도 생성자도 모두 프로토타입에 접근할 수 있고 심지어 변경까지할 수 있습니다.  
이는 상속받을 것들을 자기 마음대로 선택,변경할 수 있게 되고 다른 언어의 상속과는 다른 형태를 갖습니다. 

**상속이라고 부르지만, 프로토타입의 (자원)공유로 이해하는 것이 적절해 보입니다.**

------
### ES5에서의 상속 구현 형태

```js ES5 상속 형태
1. function Sports(member){  
 2. this.member = member;  
};  
3. Sports.prototype.setItem = function(item){  
 this.item = item;  
};  
  
4. function Soccer(member){  
 Sports.call(this, member);  
};  
5. Soccer.prototype = Object.create(Sports.prototype, {  
 setGround: {  
 value: function(ground){  
 this.ground = ground;  
 }  
 }  
});  
6. Soccer.prototype.constructor = Soccer;  
  
7. var obj = new Soccer(11);  
8. obj.setItem("축구");  
obj.setGround("상암");  
  
9. console.log(obj.member); // 11  
console.log(obj.item); // 축구  
console.log(obj.ground); // 상암  
```

1.  `Sports` 첫 문자를 대문자로 작성한 것은 `new` 연산자로 인스턴스를 생성하려는 의도입니다. `new Sports()`를 실행하면 `Sports()`가 호출되고, 다시 디폴트 `constructor`를 호출합니다. 그래서 `Sports()`를 생성자(`constructor`)함수라고 부릅니다.


2.  `this.member`에서 `this`가 생성하는 인스턴스를 참조하므로 `member`는 인스턴스 프로퍼티가 됩니다. `Sports`(생성자 함수)함수에서 `this.member = member` 형태가 인스턴스에 초기값으로 설정됩니다. ~~이렇게 설정된 값은 생성된 모든 인스턴스에서 공유하지 않고 인스턴스마다 값을 각각 유지합니다. 이것이 인스턴스를 만드는 목적 중의 하나입니다.~~


3.  생성자 함수가 있으면 `Sports.prototype.setItem`과 같이 `prototype`에 메서드를 연결한 코드가 작성되어 있습니다. 이를 작성하지 않으면 생성자 함수가 아닌 일반 함수 입니다. **이 형태가 ES5에서 인스턴스를 구현하는 기본 형태입니다.**


4.  `Soccer()`가 호출되면 `Sports()`를 호출합니다. `Soccer`의 첫 문자가 대문자이므로 인스턴스를 사용한다는 것을 알 수 있습니다. 그런데 `new Sports()`가 아닌 `Sports.call()`형태로 함수를 호출한 것은, 바로 다음 코드에서 `Sports.prototype`을 사용하여 인스턴스를 생성하므로 인스턴스에 초기값만 성정하면 되기 때문입니다.


5.  `Object.create()`의 두 번째 파라미터 `setGround`를 첫 번째 파라미터인 `Soccer.prototype`에 첨부합니다. 그리고 `Sports.prototype`에 연결된 메서드를 `Soccer.prototype.__proto__`에 첨부합니다.  
    이렇게 연결된 후에 `new Soccer()`로 인스턴스를 생성하면 `Soccer.prototype`과 `Sports.prototype`에 연결된 메서드를 인스턴스 메서드로 호출할 수 있습니다.**ES5에서는 이와 같은 방법으로 상속을 구현합니다.**


6.  `Soccer.prototype`에 `constructor`가 연결되어 있는데, 앞 코드에서  
    `Soccer.prototype`에 프로퍼티를 연결하므로 `constructor`가 지워집니다.  
    `Soccer`를 설정하지 않아도 인스턴스가 생성되지만, `constructor`에서 `Soccer`전체를 참조하는 것이 정상입니다.


7.  `new` 연산자로 `Soccer()` 생성자 함수를 호출하여 인스턴스를 생성합니다.  
    `Sports.prototype`에 연결된 메서드는 인스턴스에 포함되지만, `Sports()` 생성자 함수는 포함되지 않으므로 `Sports.call(this.member);` 코드를 수행하여 인스턴스에 초기값을 설정합니다. `this`는 생성한 인스턴스를 참조하게 됩니다.


8.  `setItem()`은 상속받은 `Sports.prototype`에 연결된 메서드 입니다.  
    상속을 받으면 인스턴스에서 직접 상속받은 메서드를 호출할 수 있습니다.


9.  생성자 함수를 모두 호출하여 인스턴스에 초기값을 설정했으므로 인스턴스 프로퍼티로 프로퍼티 값을 구할 수 있습니다.  
    `ES5`에서는 이와 같이 `prototype`에 연결해야 하며 직관적이지 않은 점도 있습니다.

* * *

<h2 id="extends">extends 키워드</h2>

**`ES6`에서는 `extends` 키워드로 상속을 구현합니다.**

> class subClass extends superClass { }

*   subClass  
    상속 받는 자식 클래스(서브 클래스).
    
*   superClass  
    상속 해주는 부모 클래스(슈퍼 클래스).
    
*   `new subClass()`로 인스턴스를 생성하면 인스턴스에서 `subClass` 클래스와 `super` 클래스의 메서드를 호출할 수 있습니다.
    

```js extends ES6
1. class Sports {  
 constructor(member){  
 this.member = member;  
 }  
 getMember(){  
 return this.member;  
 }  
};  
2. class Soccer extends Sports {  
 setGround(ground){  
 this.ground = ground;  
 }  
};  
3. let obj = new Soccer(11);  
  
4. console.log(obj.getMember()); // 11  
```

1.  `Sports` 클래스를 상속받으므로 `Sports` 클래스는 `슈퍼 클래스`입니다.  
    `new Soccer()`로 인스턴스를 생성하면 `constructor`가 호출되며, `this`가 생성하는 인스턴스를 참조하므로 파라미터로 받은 값을 인스턴스의 `member` 프로퍼티에 설정할 수 있습니다.


2.  `extends` 키워드 기준으로 왼쪽의 `Soccer` 클래스가 `서브클래스` 오른쪽 `Sports` 클래스가 `슈퍼클래스`입니다. 즉, `Soccer` 클래스에서 `Sports` 클래스를 상속받습니다. `this.ground`에서 `this`는 생성한 인스턴스를 참조합니다.
    
    *   이 시점의 Soccer 클래스 구조입니다.

    <img src="/images/extendsSoccer.JPG">
    
    1.  `Soccer.prototype`에 `setGround`가 연결되어 있으며

    2.  Soccer.prototype.&#95;&#95;proto&#95;&#95;에 Sports.prototype에 연결되어 있는 `getMember`가 첨부되어 있습니다.
    
    3.  Soccer.&#95;&#95;proto&#95;&#95;에 Sports.prototype에 연결된 프로퍼티도 첨부되어 있습니다.
    
    *   이와 같이 `extends` 키워드는 서브클래스의 `prototype`에 &#95;&#95;proto&#95;&#95;를 만들고 여기에 슈퍼클래스의 `prototype`에 연결된 프로퍼티를 연결합니다.  
        ~~슈퍼클래스의 prototype에 연결된 메서드를 복사하는 것이 아니라 공유합니다.~~  
        `new Soccer()`로 인스턴스를 생성할 때 `Soccer.prototype`에 연결된 프로퍼티를 사용하므로 서브클래스와 슈퍼클래스의 메서드가 인스턴스에 포함됩니다.

3.  new Soccer(11)을 실행하면 다음의 순서와 방법으로 실행합니다.
    
    1.  `Soccer` 클래스의 `constructor`가 호출됩니다.
    2.  `Soccer` 클래스에는 `constructor`를 작성하지 않았습니다.
    3.  슈퍼 클래스의 `constructor`가 호출되면서 11을 파라미터 값으로 넘겨줍니다.
    4.  슈퍼 클래스의 `constructor`에서 this는 현재의 인스턴스를 참조하므로 인스턴스의 `member` 프로퍼티에 파라미터로 받은 11을 설정한 후 돌아오게 되며,
    5.  생성한 인스턴스를 `obj`에 할당합니다.
    
    *   다음은 obj의 인스턴스 구조입니다.
        <img src="/images/extendsObj.JPG">
        
    *   인스턴스를 생성하는 주체는 서브 클래스입니다. `new Sports()`가 아닌 `new Soccer()`로 인스턴스를 생성합니다.
        
    *   슈퍼 클래스의 `constructor`에서 `this.member`에 설정한 값이 인스턴스 프로퍼티로 설정됩니다.
        
    *   &#95;&#95;proto&#95;&#95;에 서브 클래스의 prototype에 연결된 메서드가 첨부되었으며
        
    *   &#95;&#95;proto&#95;&#95;에 슈퍼 클래스의 prototype에 연결된 메서드가 첨부됩니다.
        
    *   메서드를 호출할 때 &#95;&#95;proto&#95;&#95;를 작성하지 않아도 되므로  
        `setGround()`와 `getMember()`를 인스턴스에서 직접 호출할 수 있습니다.
        

4.  `obj.getMember()`를 호출하면 우선 obj.&#95;&#95;proto&#95;&#95;에서 메서드를 찾습니다. 존재하지 않으면 obj.&#95;&#95;proto&#95;&#95;.&#95;&#95;proto&#95;&#95;에서 찾습니다. 메서드가 존재하면 호출됩니다.  


**이것이 자바스크립트의 상속 메커니즘 입니다.**

* * *

<h2 id="super">super 키워드</h2>

**서브 클래스와 슈퍼 클래스에 같은 이름의 메서드가 존재하면 슈퍼 클래스의 메서드는 호출되지 않습니다.**  
이때 `super` **키워드를 사용하여 슈퍼 클래스의 메서드(혹은 함수)를 호출할 수 있습니다.**

서브 클래스 `constructor`에 `super()`를 작성하면 슈퍼 클래스의 `constructor`가 호출됩니다.  
`super.name()`과 같이 `super` 키워드에 이어서 호출하려는 메서드 이름을 작성합니다.

### 메서드 오버라이딩

서브 클래스와 슈퍼 클래스에 같은 이름의 메서드가 있을 때 서브 클래스의 메서드가 호출 되는 것을  
메서드 오버라이딩(`Overriding`)이라고 합니다. **메서드 오버라이딩은 의도적인 접근 방식입니다.**

서브 클래스와 슈퍼 클래스의 같은 이름의 메서드가 같은 목적을 가진 것을 나타내면서  
서브 클래스의 목적에 맞도록 보완할 때 사용합니다.(슈퍼 클래스의 메서드 기능을 사용하면서 서브 클래스에서 기능을 추가,변경할 때 사용합니다.)

**슈퍼 클래스와 서브 클래스의 메서드 기능/목적이 다른 경우에는 같은 이름을 사용하지 않습니다.**

```js super-1
class Sports {  
 setGround(ground){  
 this.ground = ground;  
 }  
};  
1. class Soccer extends Sports {  
 setGround(ground){  
 2. super.setGround();  
 this.ground = ground;  
 }  
};  
3. let obj = new Soccer();  
obj.setGround("상암구장");  
console.log(obj.ground);  
// 상암구장  
```

~~Sports 클래스는 슈퍼 클래스 입니다. 상속을 해주지만 단독으로 인스턴스를 생성할 수도 있습니다.~~

*   `Class Sports{}`에는 그라운드(`setground`)가 필요합니다.  
    `Sports`의 종목이 여러개라면 종목마다 그라운드(`setGround`)형태도 달라져야 할 수 있습니다.  
    그런 의미에서 `setGround`()메서드는 의미 없어 보일 수 있습니다.


*   하지만 `Sports` 클래스를 상속받는 서브 클래스에서 `setGround`를 오버라이딩 하면 일관성 있게  
    그라운드를 정의할 수 있습니다. 메서드 이름을 유지하면서 서브클래스 마다  
    적합한 그라운드(`setGround`)를 설정해 주는 것입니다.  
    **이를 객체 지향에서 추상화(`Abstraction`)라고 합니다.**


1.  `Soccer` 클래스에서 `Sports` 클래스를 상속받습니다. `Sports` 클래스에 `setGround()`가 있으며  
    `Soccer` **클래스에도 있으므로 메서드가 오버라이딩 됩니다.**  
    `new Soccer()`로 인스턴스를 생성한 후 `setGround()`를 호출하게 되면 서브클래스(`Soccer`)의  
    메서드가 호출됩니다. 이때 `super.setGround()`는 슈퍼클래스(`Sports`)의 `setGround()`를 호출합니다.


2.  `super.setGround()`를 호출하면서 파라미터 값은 정해주지 않았습니다.  
    슈퍼클래스(`Soccer`)의 `setGround()`에서 `this.ground`에 `undefined`가 설정됩니다.  
    `super.setGround()`를 실행한 후 돌아오면 바로 다음 줄의 `this.ground`에서 파라미터로 받은 값을 설정합니다.


3.  `new Soccer()`로 인스턴스를 생성하고 `setGround()`를 호출하면 인스턴스의 `ground` 프로퍼티에 파라미터 값이 설정됩니다. `ground`가 인스턴스 프로퍼티이므로 `obj.ground`로 값을 출력할 수 있습니다.

```js super-2
class Sports {  
    constructor(member){  
        this.member = member;  
        console.log(this.member);  
    }  
};  
2. class Soccer extends Sports {  
 3. constructor(member){  
        super(member);  
        this.member = 456;  
        console.log(this.member);  
    }  
};  
1. let obj = new Soccer(123);  
// 123  
// 456  
```

1.  `new Soccer(123)`을 실행하면 `Soccer` 클래스의 `constructor`가 호출됩니다.


2.  `constructor` 첫째 줄의 `super(member)`를 실행하면 슈퍼 클래스의 `constructor`를  
    호출 하면서 파라미터로 받은 값을 넘겨줍니다. 슈퍼 클래스의 `constructor`를  
    호출하려면 서브클래스 `constructor`의 첫째 줄에 `super()`를 작성해야 합니다.

**super() 앞에 변수를 선언하거나 변수에 값을 할당하는 코드는 작성해도 되지만,**  
**this 키워드는 super() 앞에 사용할 수 없습니다.**

3.  `super()`로 인해 `constructor`가 호출되면 `this`로 인스턴스를 참조할 수 있습니다.  
    따라서 파라미터로 받은 `member` 값을 인스턴스의 `member` 프로퍼티에 할당할 수 있습니다.

* * *

<h2 id="builtin">빌트인 오브젝트 상속</h2>

`extends` 키워드로 `Array` 오브젝트 등의 빌트인 오브젝트를 상속받을 수 있습니다.  
상속을 받지 않고 서브 클래스에서 빌트인 오브젝트의 메서드를 호출해도 되지만 상속 받는 것과 차이가 있습니다. 

**서브 클래스에서 빌트인 오브젝트를 상속받으면 빌트인 오브젝트의 메서드를 마치 서브 클래스에서 선언한 것처럼 사용할 수 있게 됩니다.**

```js builtin
2. class ExtendArray extends Array {  
 constructor(){  
 super();  
 }  
 getTotal(){  
 let total = 0;  
 for (var value of this){  
 total += value;  
 };  
 return total;  
 }  
};  
  
1. let obj = new ExtendArray();  
3. obj.push(10, 20);  
  
4. console.log(obj.getTotal());  
// 30  
```

1.  클래스 `ExtendArray` 에 빌트인 오브젝트인 `Array` 오브젝트를 상속받았습니다.  
    `new` 연산자로 인스턴스를 생성하면 인스턴스는 `Array` 오브젝트의 특징을 갖게됩니다.  
    따라서 인스턴스에서 `push()`와 같은 `Array` 메서드를 직접 호출할 수 있습니다.  
    `“[].push()”`형태가 아닌 `“인스턴스.push()”`형태로 호출할 수 있습니다.  
    이 형태의 차이에 상속 받는 목적이 담겨있다고 할 수 있습니다.


2.  `new ExtendArray()`를 실행하면 아래에 작성한 `constructor`가 호출됩니다.
    
    > constructor(){  
    > super();  
    > }
    
    `super()`가 슈퍼 클래스의 `constructor`를 호출하므로 `Array` 오브젝트 `constructor`가 호출됩니다.
    

3.  `obj` 인스턴스에 `push()`메서드가 상속되어 있으므로 `obj.push(10,20)`형태로 호출할 수 있습니다.  
    `obj.push(10, 20)` 과 `[].push(10, 20)`에서 `obj` 인스턴스가 `Array 리터럴[]`에 해당됩니다.  
    따라서 `obj` 인스턴스에 10 과 20를 설정하는 것은 `Array` 인스턴스에 설정하는 것과 같습니다.  
    **이것이 빌트인 오브젝트를 상속받는 목적 중의 하나입니다.**


4.  `obj.getTotal()`을 호출하면 `for-of` 문으로 [10, 20]을 합산하여 반환합니다.  
    ~~값을 합산해주는 빌트인 함수도 있지만 for(var value of this)문에서  
    this를 다루기 위해 의도적으로 for-of 문을 사용했습니다.~~  
    `this`가 `obj` 인스턴스를 참조합니다. `obj` 인스턴스는 [10, 20]값이 설정되어 있으며  
    `Array` 오브젝트를 상속받은 상태 이므로 `length` 프로퍼티를 갖고있습니다.  
    **즉, 이터레이션을 수행할 수 있는 조건을 충족합니다.**
    
    > for(var value of [10, 20]){  
    > total += value;  
    > } return total
    

위 헝태가 되어 엘리먼트(10, 20)를 하나씩 읽어가면서 `for-of` 문을 반복합니다.

* * *

<h2 id="Object_super">Object에서 super 사용</h2>

**두 개의 `Object` 오브젝트가 연결된 구조에서 `super.name()` 형태로 슈퍼 오브젝트의 메서드를 호출할 수 있습니다.**

```js
let Sports = {  
 getTitle(){  
 console.log("Sports");  
 }  
};  
let Soccer = {  
 getTitle(){  
 super.getTitle();  
 console.log("Soccer");  
 }  
};  
1. Object.setPrototypeOf(Soccer, Sports);  
2. Soccer.getTitle();  
//Sports  
//Soccer  
```

1.  `Object.setPrototypeOf()`을 실행하면 Soccer.&#95;&#95;proto&#95;&#95;에 `Sports` 오브젝트의 프로퍼티가 첨부됩니다.  
    `Object` 오브젝트가 대상이므로 생성자 함수가 없지만,&#95;&#95;proto&#95;&#95;에 프로퍼티가 첨부되는 것이 상속 구조(형태)입니다.
    

2.  `Soccer.getTitle();`이 호출되면, 첫째 줄에서 `super.getTitle()`을 호출합니다.  
    상속을 하면 &#95;&#95;proto&#95;&#95;로 계층을 만들고,&#95;&#95;proto&#95;&#95;에 상속받을 오브젝트의 프로퍼티를 첨부하므로, `super`는 한 단계 아래의 &#95;&#95;proto&#95;&#95;를 참조하는 것과 같습니다.
    

현재 Soccer.&#95;&#95;proto&#95;&#95;에 `Sports` 오브젝트의 `getTitle()`이 첨부되어 있습니다.  
따라서 `super.getTitle()`을 호출하면, `super`가 Soccer.&#95;&#95;proto&#95;&#95;를 참조하므로 Sports` 오브젝트의 `getTitle()`이 호출됩니다.  

**이와 같이 Object.setPrototypeOf()으로 &#95;&#95;proto&#95;&#95;구조를 만들고 상속받을 오브젝트의 프로퍼티를 첨부하면, super키워드로 상속 계층 구조에 있는 메서드를 호출할 수 있습니다.**

* * *

<h2 id="static">static 키워드</h2>

**클래스에 `static`(정적) 메서드를 정의합니다.**

> static methodName() { … }

`prototype`에 연결된 메서드는 인스턴스를 생성하여 호출 하지만,  
`static` 메서드는 인스턴스를 생성하지않고 클래스에 직접 연결하여 호출합니다.

`static` 메서드는 `prototype`에 연결되지 않으므로 인스턴스에서 호출할 수 없습니다.  
클래스 이름을 지정하여 `static` 메서드를 호출해야 합니다.

*   중요 포인트

    엔진이 `class` 키워드를 만나면 클래스 안에 `static` 메서드 작성 여부를 체크합니다.  
    `static` 메서드가 존재한다면 이를 `Function` 오브젝트로 생성합니다.  
    (메서드를 호출하기 위해서는 메서드가 `Function` 오브젝트여야 하고,  
    이렇게 생성함으로써 클래스 아래의 `static`메서드를 호출할 수 있게 합니다.)

```js 자바스크립트 Function 오브젝트 생성 형태
function(){ // Function 오브젝트입니다.  
 function(){ // Function 오브젝트가 아닙니다.  
 }  
}  
```

`function` 안에 `function`은 `Function` 오브젝트로 생성하지 않고, `function`이 호출되어 안에 있는 `function`으로 들어갔을 때 `Function` 오브젝트를 생성합니다. 

**따라서 `function`을 호출하지 않으면 안에 있는 `function`은 아무것도 아닙니다. 그저 작성돼있는 함수 입니다. 이 점이 `static` 키워드를 사용한 메서드와 차이점 입니다.**

```js
class Sports {  
 static getGround() {  
 return "상암구장";  
 }  
};  
1. console.log(Sports.getGround());  
```

`getGround(){}` 앞에 `static`을 작성하여 `static`메서드로 선언했습니다.

1.  `Sports`는 클래스 이름이고 `getGround()`는 `static`메서드 입니다.  
    이와 같이 앞에 클래스 이름을 작성하고 이어서 `static`메서드를 작성하여 호출합니다.

* * *

<h2 id="Class_Hoisting">Class 호이스팅</h2>

**클래스는 호이스팅(`Hoisting`)이 되지 않습니다.**

```js
// Class  
let result = Member;  
class Member {  
 static getMember(){  
 return "member";  
 }  
};  
console.log(Member.getMember()); // member  
console.log(result.getMember()); // TypeError  
  
// function과 비교  
let result2 = Member2;  
function Member2() {  
 return "member2";  
};  
console.log(Member2());// member2  
console.log(result2());// member2  
```

호이스팅 된다면 `Member` 클래스가 `result` 변수에 할당됩니다.  
호이스팅 되지않으면 `Member`를 인식하지 못합니다. //Error

*   클래스는 호이스팅 되지않아 `result`에 할당할 `Member`를 인식하지 못합니다.  
    `result`에 `Member` 값을 넣으려면 클래스 문이 완전히 끝난 뒤에 작성해야합니다.
    
*   `function`은 호이스팅되므로 `result2`에 `Member2`를 인식하여 할당할 수 있습니다.
    

* * *

<h2 id="computed_name">computed name</h2>

**클래스의 메서드 이름을 조합(`computed name`)하여 작성할 수 있습니다.**

```js computed name
let type = "Type";  
class Sports {  
 static ["get" + type](kind){  
 return kind ? "스포츠" : "음악";  
 }  
}  
1. console.log(Sports["get" + type](1)); // 스포츠  
console.log(Sports.getType(1)); // 스포츠  
```

`변수 type` 에 문자열 값 `“Type”`을 할당해 줬습니다.  
`static` 메서드 `[]`안에 문자열 `“get”` 과 클래스 밖에 작성된 `type` 변수 값을  
작성해줬습니다. `static`메서드의 이름이 `getType`이 됩니다.  
이와 같이 `static` 메서드 `[]`안에 조합할 이름을 작성합니다.

1.  문자열 `“get”`과 변수인 `type`을 조합하여 호출합니다.  
    파라미터 값이 1 (true)이므로 호출된 `getType()`에서 “스포츠”를 반환합니다.  
    물론 `console.log(Sports.getType(1));` 형태로 호출할 수 도 있습니다.

* * *

<h2 id="this">this</h2>

**`static` 메서드에서 `this`는 클래스 오브젝트를 참조합니다.**
`constructor` 안에서 `this.constructor.name()`형태로 `static` 메서드를 호출할 수 있습니다.

```js this 예제
class Sports {  
 static setGround(ground){  
 this.ground = ground;  
 }  
 static getGround(){  
 return this.ground;  
 }  
};  
Sports.setGround("상암구장");  
console.log(Sports.getGround());  
// 상암구장  
```

`Sports.setGround(“상암구장”)`를 실행하면 `static`메서드인 `setGround()`가 호출됩니다.  
`this.ground`에서 `this`가 `Sports` 클래스를 참조하므로 `Sports` 클래스에  
`{ground: “상암구장”}` 형태로 설정됩니다.

`Sports.getGround()`를 실행하면 `getGround()` 정적 메서드가 호출됩니다.  
`this.Ground`에서 `this`가 `Sports` 클래스를 참조하므로 `setGround()`에서  
`Sports` 클래스에 설정한 `ground` 프로퍼티 값을 구할 수 있습니다. //상암구장

```js this 호출 예제
class Sports{  
 constructor(){  
 1. console.log(Sports.getGround());  
 2. console.log(this.constructor.getGround());  
 }  
 static getGround(){  
 return "상암구장";  
 }  
};  
let obj = new Sports();  
```

`new Sports()`를 실행하면 `constructor`가 호출됩니다.  
`constructor` 블록{}안에서 `static`메서드인 `getGround`를 호출하며, `this.constructor` 형태도 사용하고 있습니다.

1.  `Sports` 클래스에 `static` 메서드로 `getGround()`를 작성했으므로 `Sports.getGround()` 형태로 호출할 수 있습니다.  
    `constructor` 블록{} 아래에 `getGround()`가 작성되어 있습니다만 이미 `Function` 오브젝트로 생성된 상태이므로 호출이 됩니다.


2.  `constructor`가 `Sports` 클래스를 참조하며 인스턴스의 &#95;&#95;proto&#95;&#95;에    `constructor`가 첨부되어 있으므로 `this.constructor.getGround()` 형태로 `static`메서드를 호출할 수 있습니다. 구조는 다음 사진과 같습니다.
    <img src="/images/ClassThisInstance.JPG">


*   **this.getGround()형태로는 호출할 수 없습니다.** (//undefined)  
    `this`가 `new Sport()`로 생성한 인스턴스를 참조하고,  
    `getGround`는 `static`메서드 이므로 인스턴스에 존재하지 않기 때문입니다.

* * *

<h2 id="generator">제너레이터</h2>

**클래스 안에 제너레이터 함수를 작성할 수 있습니다.**  
클래스 안에 작성한 제너레이터 함수는 prototype에 연결됩니다.

**따라서 static 메서드로 호출할 수 없고 인스턴스를 생성하여 호출해야 합니다.**

```js
class Member{  
 *gen() {  
 yield 10;  
 yield 20;  
 }  
};  
let obj = new Member();  
let genObj = obj.gen();  
  
console.log(genObj.next());  
console.log(genObj.next());  
// Object {value: 10, done: false}  
// Object {value: 20, done: false}
```  

`new Member()`로 클래스 인스턴스를 생성 합니다. 변수 `obj`에 할당됩니다.  
`genObj` 변수에 `obj.gen` 메서드를 할당합니다. `genObj.next()`로 호출할 수 있게됩니다.

* * *

<h2 id="new_target">new.target</h2>

`new.target`은 메타(meta) 프로퍼티로 생성자 함수와 클래스에서 `constructor`를 참조합니다. `new` 연산자로 인스턴스를 생성하지 않으면 `new.target` 값은 `undefined` 입니다.

```js
let sports = function(){  
 console.log(new.target);  
}  
sports();  
new sports();  
// undefined  
/* function() {  
 console.log(new.target);  
}  
*/  
```

`sports();` 와 같이 `new` 연산자를 사용하지 않고 호출하면 `new.target`값은 `undefined` 입니다.

`new sports();`로 호출하면 `new.target`은 `constructor`를 참조합니다.  
`sports` 함수에 `constructor`를 작성하지 않았으므로 디폴트 `constructor`가 호출됩니다.  
디폴트 `constructor = (function: Object() {native code})`  
`constructor`가 `sports` 전체를 참조하므로 sports 함수의 코드가 출력됩니다.

------
### name 프로퍼티

`ES6`는 클래스, 함수 오브젝트에 `name` 프로퍼티가 존재하며 이름이 설정됩니다.

```js
class Sports {  
 constructor(){  
 console.log("Sports:", new.target.name);  
 }  
};  
class Soccer extends Sports {  
 constructor(){  
 super();  
 console.log("Soccer:", new.target.name);  
 }  
};  
let sportsObj = new Sports();  
let soccerObj = new Soccer();  
// Sports: Sports  
// Sports: Soccer  
// Soccer: Soccer  
```

*   `new Sports()`를 호출하면 `Sports` 클래스이므로 클래스 `name` 프로퍼티에 `“Sports”`가  
    설정되어 있습니다. `constructor`에서 `new.target`은 `constructor`를 참조하므로  
    `Sports` 클래스의 `name` 프로퍼티 값 `“Sports”`가 출력됩니다.
    

*   `new Soccer()`를 호출하면 `super()`로 인해 `Sports`의 `constructor`가 호출됩니다.  
    `Sports`의 `constructor`에서 `new.target`은 `super()`로 호출한 `Soccer`의 `constructor`를 참조합니다. 따라서 `new.target.name` 값으로 `Sports`가 아닌 `Soccer`가 출력됩니다.
    

* * *

<h2 id="Image_Object">Image 오브젝트 상속</h2>

*   `DOM`(Document Object Model)에서 제공하는 `Image` 인터페이스, `Audio` 인터페이스 등을 상속받을 수 있습니다.  
    빌트인 `Array` 오브젝트를 상속받으면 `Array` 오브젝트 특징을 갖듯이 상속받은 인터페이스 특징을 갖습니다.
    
*   `DOM`의 `Image` 인터페이스는 웹 페이지에 `png` 파일과 같은 이미지 파일을 표현하기 위한 속성을 제공합니다.  
    인터페이스는 객체지향 용어로 이 자체를 그대로 사용할 수 없고 오브젝트로 변환하여 사용해야 합니다.  
    `DOM`은 오브젝트를 제공하지 않고 인터페이스를 제공하는데,이는 자바스크립트뿐만 아니라 Java등의 다른 언어에서도 사용하기 때문입니다. 각 언어에서 DOM인터페이스를 언어에 맞게 변환하여 사용합니다.
    

인터페이스를 오브젝트로 변환하려면 `extends` 키워드를 사용합니다.

```js extends-image
1. class ExtendsImage extends Image{  
 constructor() {  
 super();  
 }  
 2. setProperty(image){  
 this.src = image.src;  
 this.alt = image.alt;  
 this.title = image.title;  
 }  
};  
let imageObj = new ExtendsImage();  
  
let properties = {  
 src: "file/rainbow.png",  
 alt: "나무와 집이 있고 그 위에 무지개가 있는 모습",  
 title: "무지개"  
};  
  
imageObj.setProperty(properties);  
3. document.querySelector("body").appendChild(imageObj);  
```

1.  `Image` 오브젝트를 `extends` 키워드로 상속받습니다. `ExtendsImage` 클래스는 `Image` 특성을 갖게됩니다.  
    즉, < img > 엘리먼트의 속성을 직접 사용할 수 있습니다.  
    또한 this로 엘리먼트 속성에 접근할 수 있습니다.
    
2.  파라미터로 받은 < img > 속성 값을 this가 참조하는 `imageObj` 인스턴스에 설정합니다.
    
3.  < body> 엘리먼트에 자식 요소로 `imageObj`를 첨부합니다.  
    body 엘리먼트에 `imageObj` 안에 있는 `img` 엘리먼트 속성도 첨부 횝니다.
    

**웹페이지에 이미지가 표시되고 그 속성은 다음과 같습니다.**

> < img src = “file/rainbow.png”  
> alt= “나무와 집이 있고 그 위에 무지개가 있는 모습”  
> title= “무지개” >
