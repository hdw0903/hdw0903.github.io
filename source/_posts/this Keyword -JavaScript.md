---
title: this Keyword -JavaScript
date: 2020-03-10 09:41:19
disqusId: tunas-blog-1
categories: JavaScript
tag: 
  - JavaScript
  - this

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

Javascript `this` 키워드는 속한 객체를 나타냅니다.

사용 위치에 따라 다른 값을 갖습니다.

<!-- more -->

*   **method**  
    메소드에서 this는 메소드의 소유자 오브젝트를 나타냅니다.

```js
var person = {  
 firstName: "Han",  
 lastName : "Dongwon",  
 id       : 2020,  
 fullName : function() {  
 return this.firstName + " " + this.lastName;  
 }  
};  
```

`this`는 `person` 객체를 나타냅니다.  
`person` 오브젝트는 `fullName` 메소드의 소유자입니다.

*   **Alone**  
    this 혼자 사용되면 전역 객체를 나타냅니다.  
    브라우저 창에서 전역 개체는 [object Window]입니다.
    

*   **function**  
    함수에서 this는 전역 객체를 나타냅니다.  
    함수에서 엄격 모드에서(in strict mode)는 정의되지 않습니다.(undefined)
    

*   **event**  
    이벤트에서 this는 이벤트를 받은 HTML요소(element)를 나타냅니다.
    

```html
<button onclick="this.style.display='none'">  
 display none  
</button>  
```

this는 이벤트를 받는 HTML요소 button을 나타냅니다.

*   call() 및 apply()와 같은 메소드는 this를 모든 객체에 참조할 수 있습니다.

```js
var person1 = {  
 fullName: function() {  
 return this.firstName + " " + this.lastName;  
 }  
}  
var person2 = {  
 firstName:"Han",  
 lastName: "Dongwon",  
}  
person1.fullName.call(person2);  
```

person2를 인수로 사용하여 person1.fullName을 호출하는 경우  
person1의 메소드 인 경우에도 this가 person2를 참조합니다.


* [this 더 자세히 알아보기](https://hdw0903.github.io/2020/04/27/this-Core-JavaScript/)