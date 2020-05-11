---
title: 이벤트 -JavaScript
date: 2020-03-09 12:33:55
disqusId: tunas-blog-1
categories: JavaScript
tag: 
- JavaScript
- event
- event handler
- mouse event
- keybord event
- etc. event
- addEventListener

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

* * *

브라우저에서 유저가 취하는 모든 동작을 `이벤트`라 합니다.  
이벤트가 발생했을 때 자바스크립트 실행문을 실행하는 것을  
`이벤트 핸들러`라 합니다.

<!-- more -->

* * *

### 마우스 이벤트

*   `onmouseover`  
    마우스가 지정한 요소에 올라갔을 때 발생합니다.
    
*   `onmouseout`
    마우스가 지정한 요소를 벗어났을 때 발생합니다.
    
*   `onmousemove`  
    마우스가 지정한 요소 영역에서 움직일 때 발생합니다.
    
*   `onclick`  
    마우스가 지정한 요소를 클릭했을 때 발생합니다.
    
*   `ondbclick`  
    마우스가 지정한 요소를 더블 클릭 했을 때 발생합니다.
    

* * *

### 키보드 이벤트

*   `onkeypress`  
    지정한 요소에서 키보드가 눌렸을 때 발생합니다.
    
*   `onkeydown`  
    지정한 요소에서 키보드를 눌렀을 때 발생합니다.
    
*   `onkeyup`  
    지정한 요소에서 키보드를 눌렀다 떼었을 때 발생합니다.
    

* * *

### 기타 이벤트

*   `onfocus`  
    지정한 요소에 focus 되면 발생합니다.
    
*   `onblur`  
    지정한 요소에 focus가 다른 요소로 이동되어 focus를 잃으면 발생합니다.
    
*   `onchange`
    지정한 요소에 value 속성값이 바뀌고 focus가 이동되었을 때 발생합니다.
    
*   `onload`  
    지정한 요소의 하위 요소를 모두 로딩 했을 때 발생합니다.
    
*   `onunload`  
    문서를 닫거나 다른 문서로 이동했을 때 발생합니다.
    
*   `onsubmit`  
    폼 요소에 전송 버튼을 눌렀을 때 발생합니다.
    
*   `onreset`  
    폼 요소에 취소 버튼을 눌렀을 때 발생합니다.
    
*   `onresize` 
    지정된 요소의 크기가 변경되었을 때 발생합니다.
    
*   `onerror`  
    문서 객체가 로드되는 동안 문제가 발생되었을 때 발생합니다.
    

더 많은 이벤트 정보:  
[https://www.w3schools.com/jsref/dom_obj_event.asp](https://www.w3schools.com/jsref/dom_obj_event.asp)

* * *

지정한 요소에 이벤트를 적용하는 방법에는  
요소에 직접 이벤트를 등록하는 방법과,  
DOM을 이용하여 지정된 요소에 이벤트를 등록하는 방법이 있습니다.

**직접 요소 이벤트 등록 방식**

html 태그에 직접 등록합니다

```html
<button id= "btn" onclick="alert('Event')">버튼</button>  
```

**DOM을 이용한 이벤트 등록 방식**

스크립트 선언문 영역에 작성하여 등록합니다.

```js
<button id="btn">버튼</button>  
<script>  
 document.getElementByid("btn").onclick=function(){  
 alert('Event');  
 }  
</script>  
```

* * *

### 키보드 접근성

마우스 이벤트를 등록할 경우에는 마우스가 없어도

접근(작동)할 수 있도록 해야 하는데,

이것을 키보드 접근성 이라고 합니다.

마우스 이벤트가 등록되었을 때는 반드시 키보드로 작동할 수 있게  
대응 이벤트를 함께 작성해야 합니다.

| 마우스 이벤트 | 키보드 대응 이벤트 |
|---------------|--------------------|
| onmouseover   | onfocus            |
| onmouseout    | onblur             |


```js 예시
/* 마우스가 없을 경우 키보드 [tab]키를 사용해 이용할  
 수 있도록 키보드 대응하는 이벤트 onfocus를 추가하였습니다.*/  
 var btn=document.getElementById("btn");  
 btn.onmouseover=btn.onfocus=function(){ colorBg(); }  
  
/* 마우스가 없을 경우 키보드 [tab]키를 사용해 이용할  
 수 있도록 키보드 대응하는 이벤트 onfocus를 추가하였습니다.*/  
 <button onmouseover="colorBg();" onfocus="colorBg();">  
 버튼3</button>  
```

* * *

### 한 요소에 이벤트 중복 등록

잘못된 사례:

```html
<button id=”btn” onclick=”alert(‘실행문1’);” onclick=”alert(‘실행문2’)”;>버튼  
//두번째 경고 창인 실행문2만 실행됩니다.

document.getElementById(“btn”).onclick=function(){  
alert(‘실행문1’); //실행 안됨  
}  
document.getElementById(“btn”).onclick=function(){  
alert(‘실행문2’); // 실행 됨  
}
```

* * *

**한 요소에 중복으로 이벤트 등록 하는 방법**

*   IE 8 이하 이외에 브라우저 (파이어폭스,크롬,사파리 등)

**addEventListener**

>document.getElementById("myBtn").addEventListener("click", myFunction);
document.getElementById("myBtn").addEventListener("click", someOtherFunction);

기존 이벤트를 덮어 쓰지 않고  
동일한 요소에 많은 이벤트를 추가 할 수 있습니다.

이 예제는 동일한 `< button>` 요소에  
두 개의 클릭 이벤트를 추가하는 방법을 보여줍니다.

잘못된 예시 처럼 `function`에 `alert 실행문 1,2` 가 있더라도  
경고문(실행문1)이 뜨고 난뒤 두번째 경고문 (실행문2)가 나타나게 합니다.

*   IE 8 이하 브라우저에서는 **attachEvent**를 사용합니다.

조건문을 이용하여 (크로스 브라우징 검사)  
사용자 브라우저에 맞는 실행문을 나눠 사용 할수 있습니다.

```js
var x = document.getElementById("myBtn");  
if (x.addEventListener) {  // IE8 이전버젼을 제외한 브라우저  
 x.addEventListener("click", myFunction);  
} else if (x.attachEvent) { // IE8 혹은 그 이전 버젼의 IE브라우저  
 x.attachEvent("onclick", myFunction);  
}  
```