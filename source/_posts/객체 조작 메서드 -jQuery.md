---
title: 객체 조작 메서드 -jQuery
date: 2020-03-10 13:52:04

categories: jQuery
tag: 
- JavaScript
- jQuery
- jQuery method
- jQuery 선택자

toc: true

widgets:
  - type: toc
    position: right
  - type: categories
    position: right

sidebar:
  right:
    sticky: true
---

1. 속성 조작 메서드
2. 수치 조작 메서드
3. 객체 편집 메서드
4. jQuery 선택자 정리

<!-- more -->


* * *

### 속성 조작 메서드

-------
*   `html()` 메서드

    >1. $("요소 선택").html();
    2. $("요소 선택").html("새 요소");

1.  선택한 요소에 하위 요소들을 불러옵니다.
2.  선택한 요소에 하위 요소를 모두 지우고 “새 요소”로 바꿉니다

-------
*   `text()` 메서드

    >1. $("요소 선택").text();
    2. $("요소 선택").text("새 택스트");

1.  선택한 요소에 포함하는 모든 텍스트을 불러옵니다.
2.  선택한 요소에 있는 텍스트를 제거하고 “새 텍스트”로 바꿉니다

-------
*   `css()` 메서드

    >1. $("요소 선택").css("속성");
    2. $("요소 선택").css("속성","값");

1.  선택한 요소에 스타일(css)속성을 불러옵니다.
2.  선택한 요소에 스타일(css)속성을 바꾸거나 새 스타일(css)을 추가합니다.

-------
*   `attr()` 메서드

    >1. $("요소 선택").attr("속성");
    2. $("요소 선택").attr("속성","새 값");

1.  선택한 요소에 지정한 속성 값을 불러옵니다.
2.  선택한 요소에 지정한 속성 값을 새로 생성하거나 변경합니다.

-------
*   `removeAttr()` 메서드

    >1. $("요소 선택").revomeAttr("속성");

1.  선택한 요소에 지정한 속성을 제거합니다.

-------
*   `addClass()` 메서드

    >1. $("요소 선택").addClass("클래스 값");

1.  선택한 요소에 새 클래스(class)를 생성합니다.

-------
*   `removeClass()` 메서드

    >1. $("요소 선택").removeClass("클래스 값");

1.  선택한 요소에 지정한 클래스를 삭제합니다.

-------
*   `toggleClass()` 메서드

    >1. $("요소 선택").toggleClass("클래스 값");

1.  선택한 요소에 지정한 클래스의 존재 여부에 따라  
    존재 하지않으면 생성하고, 존재 하면 삭제합니다.

-------
*   `hasClass()` 메서드

    >1. $("요소 선택").hasClass("클래스 값");

1.  선택한 요소에 지정한 클래스 존재 여부의 따라  
    존재할 시 true 아닐시 false를 반환합니다.

-------
*   `val()` 메서드

    >1. $("입력 요소 선택").val();
    2. $("입력 요소 선택").val("새 값");

1.  선택한 입력 요소의 value 속성 값을 가져옵니다.
2.  선택한 입력 요소의 value 속성에 새 값을 입력하거나 변경합니다.

-------
*   `prop()` 메서드

    >1. $("요소 선택").prop("속성");
    2. $("요소 선택").prop("속성","새 값");
    3. $("요소 선택").prop("[tagname | nodeType | selectedIndex]");

1.  선택한 요소의 속성 값을 반환합니다.
2.  선택한 요소에 새 속성과 새 값을 생성하거나 기존 속성을 변경합니다.
3.  선택한 요소에 태그명, 노드타입, 선택상자의 선택된 옵션의 인덱스를 반환합니다.

* * *

### 수치 조작 메서드


| 종류          | 설명                                                                                     |
|---------------|------------------------------------------------------------------------------------------|
| height()      | **순수 요소**의 높이 값 (margin,padding,border 제외)                                     |
| width()       | **순수 요소의** 너비 값 (margin,padding,border 제외)                                     |
| innerHeight() | **여백(padding)을 포함**한 높이 값 (margin,border 제외)                                  |
| innerWidth()  | **여백(padding)을 포함**한 너비 값 (margin,border 제외)                                  |
| outerHeight() | **여백(padding)과 선(border) 포함**한 높이 값 (margin제외)                               |
| outerWidth()  | **여백(padding)과 선(border) 포함**한 너비 값 (margin제외)                               |
| position()    | 선택한 기준 요소(position:relative)위치로 부터 (position:absolute) 위치 값을 반환합니다. |
| offset()      | 선택한 요소가 문서(document)에서 수평/수직으로 얼마나 떨어져 있는지 떨어져있는 값을 반환 |
| scrollLeft()  | 브라우저 수평 스크롤 이동 높이 값 반환                                                   |
| scrollTop()   | 브라우저 수직 스크롤 이동 너비 값 반환                                                   |


* * *

### 객체 편집 메서드

-------
*   **before() / after()** 메서드

    >1. $("요소 선택").before(새 요소);
    2. $("요소 선택").after(새 요소);

1.  선택한 요소의 이전 위치에 새 요소 생성
2.  선택한 요소의 다음 위치에 새 요소 생성

-------
*   **append() / appendTo() / prepend() / prependTo()** 메서드

    >1. $("요소 선택").append("새 요소");
    // 선택한 요소 내의 마지막 위치에 새 요소를 추가합니다.
    >2. $("새 요소").appendTo("요소 선택");
    // 새 요소를 선택한 요소 내의 마지막 위치에 추가합니다.
    >3. $("요소 선택").prepend("새 요소");
    // 선택한 요소 내의 앞 위치에 새 요소를 추가합니다.
    >4. $("새 요소").prependTo("요소 선택");
    // 새 요소를 선택한 요소 내의 앞 위치에 추가합니다.

to : ~을 ~에게  
영어 뜻을 잘생각하여 사용해야 합니다.

-------
*   **inserBefore() / inserAfter() / clone()** 메서드

    >1. $("새 요소").inserBefore("요소 선택");
    2. $("새 요소").inserAfter("요소 선택");
    3. $("요소 선택").clone(ture);
    4. $("요소 선택").clone(false);

1.  선택한 요소의 이전 위치에 새 요소를 생성합니다.
2.  선택한 요소의 다음 위치에 새 요소를 생성합니다.
3.  선택한 요소의 하위 요소까지 복제합니다. ()값 작성안할시 기본 true값 입니다.
4.  선택한 요소의 하위 요소를 제외하고 복제합니다.

-------
*   **empty() / remove()** 메서드

    >1. $("요소 선택").empty();
    2. $("요소 선택").remove();

1.  선택한 요소에 모든 하위 요소를 비웁니다.
2.  선택한 요소를 삭제 합니다.

-------
*   **replaceAll() / replaceWith()** 메서드

    >1. $("새 요소").replaceAll("요소 선택");
    2. $("요소 선택").replaceWith("새 요소");

1.  replaceAll() 메서드는 선택한 요소를 새 HTML 요소로 바꿉니다.
2.  replaceWith() 메서드는 선택한 요소를 새 내용으로 바꿉니다.

-------
*   **unwrap() / wrap() / wrapAll() / wrapInner()** 메서드

    >1. $("요소 선택").unwrap();
    2. $("요소 선택").wrap("새 요소");
    3. $("요소 선택").wrapAll("새 요소");
    4. $("요소 선택").wrapInner("새 요소");

1.  선택한 요소의 **부모 요소를 삭제**합니다.
2.  선택한 요소를 새 요소로 **각각** 감쌉니다.
3.  선택한 요소를 **새 요소로 한꺼번에** 감쌉니다.
4.  선택한 요소에 **하위 요소를** 새 요소로 감쌉니다.

----------

### jQuery선택자 정리 예제

```js style , body ex)
<style type="text/css">  
 div{background-color:yellow;}  
 .tit{background-color:orange;}  
</style>  
</head>  
<body>  
 <h1 align="center"><strong>내용1</strong></h1>  
 <h2><strong>내용2</strong></h2>  
 <h2>내용3</h2>  
 <h2 class="tit">내용4</h2>  
 <ul class="myList">  
 <li>리스트1</li>  
 <li id="two">리스트2</li>  
 <li class="three">리스트3</li>  
 <li>리스트4</li>  
 </ul>  
</body>  
```

```js JavasSript-jQuery
<script type="text/javascript" src="경로/jQuery-버전.js"></script>  
<script type="text/javascript">  
  
$(function(){  
 $("h1").attr("align","left");   
 // h1 태그의 align 속성값을 left로 적용합니다  
 $("li:first").text("첫 번째 리스트");  
 // li 태그 첫 번째 요소의 텍스트를 "첫 번째 리스트"로 바꿉니다.  
 $("h2 > strong").css("color","red");  
 // h2 태그 안에 있는 strong 태그에 css적용하여 빨간색으로 적용합니다.  
 $("#two").prev().css("color","blue");  
 // id값 two인 요소 이전 요소에 css적용하여 파란색으로 적용합니다.  
 $("#two").next().css("color","purple");  
 // id값 two인 요소 다음 요소에 css적용하여 보라색으로 적용합니다.  
 $("#two").parent().css("border","2px dashed navy");  
 // id값 two인 요소의 부모 요소를 선택하고 css로 border값, 속성을 적용합니다.  
  
 $(".myList").prepend("<li>Front</li>");  
 //class명 myList 요소 내의 가장 앞 위치에 "<li>Front</li>" 요소를 추가합니다.  
 $(".myList").append("<li>Back</li>");  
 //class명 myList 요소 내의 마지막 위치에 "<li>Back</li>" 요소를 추가합니다.  
  
 $("<li>앞에 생성</li>").insertBefore(".three");  
 //"<li>앞에 생성</li>"요소를 class명 "three"인 요소의 이전 위치에 생성합니다.  
 $("<li>뒤에 생성</li>").insertAfter(".three");  
 //"<li>뒤에 생성</li>"요소를 class명 "three"인 요소의 다음 위치에 생성합니다.  
  
 $("h2").eq(1).wrap("<div/>");  
 //h2 태그중 인덱스(1) 즉 2번째 h2요소를 div 태그로 감쌉니다.  
 $("h2:has('strong')").addClass("tit");  
 //strong 태그를 가지고 있는 h2태그에 class "tit"를 추가합니다.  
 $("h2:last").removeClass("tit");  
 //h2 태그중 마지막 요소에 class "tit"속성을 제거합니다.  
  
});  
</script>  
```