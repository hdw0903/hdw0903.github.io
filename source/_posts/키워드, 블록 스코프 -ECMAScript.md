---
title: 키워드, 블록 스코프 -ECMAScript
date: 2020-03-16 13:13:42
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
  - ECMAScript6
  - JavaScript
  - global
  - local
  - block Scope
  - let
  - function
  - try -catch
  - switch -case
  - const
  - var

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

## 글로벌 변수 오해

글로벌 오브젝트에 작성한 변수는 **글로벌 오브젝트가 스코프**입니다.  
글로벌 오브젝트에 작성하여 글로벌 변수라고 부르는 것이지,  
**글로벌 오브젝트에서 보면 로컬 변수입니다.**

`var` 키워드를 작성하지 않으면 글로벌 변수로 간주한다는 점으로 인해  
`var`키워드를 작성하지 않을 뿐이지 글로벌 변수는 `var` 키워드를 사용하지 않는다는것이 아닙니다.

글로벌 변수도 var 키워드를 사용하여

> var global = “”;

형식으로 작성하는 것이 정확한 작성법입니다.

글로벌 변수는 객체지향 관점에서 보면 단점이라고 할 수 있습니다.  
`function` 안에서 글로벌 오브젝트에 작성된 글로벌 변수를 사용할 수는 있지만,  
다른 프로그램에서 글로벌 변수 값을 변경 하거나 재사용 할 수도 있는 위험이 있습니다.  
이러한 경우는 자칫 오류를 만들게 되어 객체 지향 기본에서 어긋나는 행동입니다.

<!-- more -->

* * *

## let 키워드

`let` 키워드 변수 선언 형태

> let sports = “축구”;

`let` 키워드는 `var` 키워드의 문제점을 해결하기 위한 것으로  
**다음과 같은 특징이 있습니다.**

1.  함수 안에 작성한 `let` 변수는 함수가 스코프 입니다.
2.  함수 안에 `if(a=b)` `{let sports = “축구”}` 형태의 코드를 작성했을 때,  
    `sports` 변수는 함수가 스코프가 아니라 `if문의 블록{}`이 스코프입니다.
3.  블록{} 밖에 같은 이름의 변수가 있어도 스코프가 다르므로 변수 각각에 값을 설정할 수 있고 그 변수 값이 유지됩니다.
4.  블록{} 안에 블록{}을 계층적으로 작성하면 각각의 블록이 스코프입니다.
5.  같은 스코프 안에서 같은 이름의 `let` 변수는 허용되지 않습니다.

* * *

## 블록 스코프

`let` 변수를 선언하는 가장 큰 목적은 스코프이며 그중에서도 블록 스코프가 돋보입니다.  
블록{} 안과 밖에 변수 이름이 같더라도 스코프가 다르므로 변수가 선언되고 각 변수에 할당된 값이 대체되지 않고 유지됩니다.

```js
let sports = "축구";  
if (sports){  
 let sports = "농구";  
 1. console.log("블록: ", sports); // 농구  
}  
2. console.log("글로벌: ", sports); // 축구  
```

*   `if`문 앞에 같은 이름의 `sports` 변수가 있지만 블록{}을 기준으로  
    스코프가 다르므로 각 `sports` 변수에 값이 할당되어  
    `“축구”`가 `“농구”`로 대체되지 않고 각 값이 유지됩니다.

* * *

## let과 this 키워드

```js
1. var music = "음악"; //var (this)  
console.log(this.music);  
  
2. let sports = "축구"; //let (this)  
console.log(this.sports);  
```

1.  `var` 키워드는 현재 글로벌 오브젝트의 상태이고  
    `this`는 글로벌 오브젝트를 참조하게 되어  
    `music` 변수 값인 “음악”이 출력됩니다.
    

2.  `let` 키워드로 선언,할당한 후  
    `this`로 `sports`값을 출력하면 `undefined`가 출력됩니다.  
    `this`가 글로벌 오브젝트를 의미하여 `window` 오브젝트를 참조하는데  
    `window` 오브젝트에 `let` 변수가 없다는 것은  
    `window` 오브젝트에 `let` 변수가 설정되지 않는다는 의미 입니다.  
    이점이 `var`변수와 `let`변수의 차이입니다.


* * *

## function

`function`도 스코프를 가지므로 **하나의 블록 스코프입니다.**
`function` 안에 선언된 모든 변수가 `function`내의 스코프에 속하고  
`function`안에 `if 블록{}`은 **스코프 안에 스코프를 가지는 계층 구조**의 형성입니다.

```js
let sports = "축구", music = "재즈";  
// get 함수 밖에 sports 와 music을 선언하고 값을 설정했습니다.  
function get(){  
 let music = "클래식"; //get 함수안에 music을 선언,할당했습니다.  
 1. console.log(music);  
 2. console.log(sports);  
}  
get();  
```

1.  `var` 변수와 마찬가지로 함수안에서 `music` 변수를 검색하고,없으면  
    함수 밖으로 나가 검색합니다. 함수안에 `music`변수가 있으므로  
    “클래식”이 출력됩니다.
    

2.  함수안에 `sports`변수가 없으므로 함수 밖의 `sports`값인 “축구”를 출력합니다.
    

<mark>이와 같이 let 변수도 가장 가까운 스코프에 있는 변수를 먼저 사용합니다.</mark>

```js 예시2
var sports = "축구"; // var sports  
let music = "재즈"; // let music  
  
function get(){  
 var sports = "농구";  
 let music = "클래식";  
  
 1. console.log("1:", sports);  
 2. console.log("2:", this.sports);  
 3 .console.log("3:", this.music);  
};  
// 1번째 호출   
window.get();  
// 2번째 호출  
get();  
```

1.  **`strict` 모드에서 `window.get`과 같이 `get()` 앞의 오브젝트 위치에**  
    **`window`를 작성하면 `function get()` 내의 `this`가 `window` 오브젝트를 참조합니다.**

    1.  함수 안에 `sport` 변수가 있으므로 그 값인 “농구”를 출력합니다.
 
    2.  `window.get()`형태로 호출했으므로 `this`가 `window` 오브젝트를 참조하여  
     `get()`함수 밖의 `var` 변수 `sports`의 값 “축구”를 출력합니다.
 
    3.  `undefined`  
     `this`가 `window` 오브젝트를 참조하여 `get()`함수 밖의 `music` 변수를 찾지만  
     `music` 변수가 `let`으로 선언되어 있어 `this`(window 오브젝트)로 참조할 수 없어 `undefined`가 출력됩니다.

2.  **get()과 같이 오브젝트를 지정하지 않고 호출하면 this가 window 오브젝트를 참조하지 않습니다.**

    1.  `sports`는 `var` 변수입니다.  
     `var`변수는 `window` 오브젝트 지정과 관계없이 함수 안의 변수를 참조하여  
     “농구”가 출력됩니다.
     
    2.  ,3. 에러  
     `window` 오브젝트를 작성하지 않고 호출하여 `this`가 `window` 오브젝트를 참조하지못하고 엔진은 참조할 오브젝트 위치에 `undefined`를 설정합니다.  
     `this`는 참조할 오브젝트 위치에 있는 `undefined`를 참조하게 되고 `TypeError`가 발생합니다.
     
* * *

## try-catch

**`try-catch`문에서 `try 블록{}`기준으로 블록 스코프를 갖습니다.**  
**`catch` 블록은 스코프를 가지지 않으며 `try` 블록 스코프에 속합니다.**

```js
let sports = "축구"; //try문 밖의 let sports   
try {  
 let sports = "농구"; //try문 안에 let sports  
 1. console.log(sports);  
} catch (e) {};  
  
2. console.log(sports);  
```

1.  `try`문 `블록{}` 스코프의 `sports` 값 출력 “농구”
2.  `try`문 밖의 `sports` 값 출력 “축구”

* * *

## switch-case

**`switch-case` 문에서 `switch` 블록이 블록 스코프입니다.**  
**`switch` 안에 `case`는 별도의 스코프를 갖지 않으며 `switch` 스코프에 속합니다.**

```js
var count = 1; // switch문의 case1:을 실행하기 위해 count에 1할당  
let sports = "축구"; //switch문 밖의 let sports  
switch (count) {  
 case 1:  
 let sports = "농구"; //switch문 내의 let sports  
 console.log(sports); // "농구"가 출력됩니다.  
};  
console.log(sports); //"축구"가 출력됩니다.  
```

* * *

## for()

**`for()`문에서 `var`변수로 작성하는 것과 `let`변수로 작성하는 것에는 큰 차이가 있습니다.**  
<mark>let 변수는 반복할 때 마다 스코프를 갖는 반면, var 변수는 스코프를 갖지 않습니다.</mark>

```html
<ul>  
 <li>1~10</li>  
 <li>11~20</li>  
 <li>21~30</li>  
</ul>  
```
```js var
var nodes = document.querySelector("ul");  
for (var k = 0; k < nodes.children.length; k++){  
 var el = nodes.children[k];  
 el.onclick = function(event){  
 event.target.style.backgroundColor = "yellow";  
 console.log(k);  
 }  
};  
```

`querySelector(“ul”)`으로 `html`에 작성된 `li` 요소 3개를 `nodes` 변수에 할당합니다.  
`nodes.childern.length`는 `NodeList`의 요소 수로 3입니다.  
`for`문을 반복하면서 각 `li` 요소마다 `onclick` 이벤트를 설정합니다.  
클릭시 배경색을 변경하고 `for`문의 `K` 변수 값을 출력합니다.  
어떤 `li`요소를 클릭하더라도 콘솔에 `K`값 3이 출력되며 3이 K의 최종값 입니다.

클릭한 `li` 요소에 해당하는 `K변수 값`을 출력하고 싶다면  
`let`변수를 사용하면 됩니다.

------
## let

```js let
var nodes = document.querySelector("ul");  
for (let k = 0; k < nodes.children.length; k++){  
 var el = nodes.children[k];  
 el.onclick = function(event){  
 event.target.style.backgroundColor = "yellow";  
 console.log(k);  
 }  
}  
```

`li`요소를 클릭하면 `onclick`이벤트를 설정했을 때 사용한 `K변수 값`을 출력합니다. 0,1,2  
이는 `let`변수가 **스코프를 갖기 때문입니다.**

* * *

## const

`const` 변수에 할당된 값은 상수가 됩니다.  
**상수는 재할당 할 수 없으며 재선언 할 수도 없습니다.**

`const` 변수는 **선언-초기화-할당**을 한번에 합니다.  
즉, `const a;`처럼 선언만 해놓을 수 없습니다. 반드시 `const a = 0;` 처럼 **초기값을 할당**해 줘야 합니다.

상수 선언에는 대소문자 모두 사용할 수 있지만, **일반적인 관습은 모두 대문자를 사용하는 것입니다.**
