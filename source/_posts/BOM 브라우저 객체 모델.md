---
title: BOM (브라우저 객체 모델)
date: 2020-03-04 10:18:19
categories: JavaScript
disqusId: tunas-blog-1
tag: 
  - JavaScript
  - BOM
  - window
  - window method
  - window open
  - specs
  - moveTo 
  - moveBy 
  - resizeTo
  - resizeBy
  - senInterval
  - clearInterval
  - setTimeout
  - clearTimeout
  - screen
  - location
  - history
  - navigator
  - 브라우저 객체 모델

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

브라우저에 내장된 객체를

**브라우저 객체(BOM: Browser Object Model)**라고 합니다.

`window`는 브라우저 객체의 최상위 객체 입니다.

`window` 객체는 여러가지 하위 객체를 포함하고 있습니다.


<!-- more -->

* * *

## window 객체 메서드 종류
--------------------------------------------------------

| 종류           | 설명                                             |
|----------------|--------------------------------------------------|
| open()         | 새 창(popup)을 열 때 사용                        |
| alert()        | 경고 창을 띄울때 사용 (window객체 없이 작성가능) |
| prompt()       | 질의응답 창을 띄움 (window객체 없이 작성가능)    |
| confirm()      | 확인/취소 창을 띄움 (window객체 없이 작성가능)   |
| scrollBy(x, y) | 윈도우 스크롤의 위치를 상대적으로 이동           |
| scrollTo(x, y) | 윈도우 스크롤의 위치를 절대적으로 이동           |
| setInterval()  | 일정 간격으로 지속적으로 실행문을 실행시킴       |
| setTimeout()   | 일정 간격으로 한번만 실행문을 실행시킴           |
| focus()        | 윈도우에 초점 맞춤                               |
| blur()         | 윈도우에 맞춘 초점 제거                          |
| close()        | 윈도우 닫음                                      |
| window.onload  | 윈도우 객체 로드 완료시 실행되는 객체            |
| moveTo(x, y)   | 윈도우의 위치를 절대적으로 이동                  |
| moveBy(x, y)   | 윈도우의 위치를 상대적으로 이동                  |
| resizeTo(x, y) | 윈도우의 크기를 절대적으로 지정                  |
| resizeBy(x, y) | 윈도우의 크기를 상대적으로 지정                  |


* * *

### open()

`open()`는 **새 브라우저 창을 띄울 때 사용합니다.**  
사이트에서 팝업 창을 띄울 때 자주 사용되는 메서드 입니다.

>기본형
window.open("url","name","specs,replace");

사용예시

```js
window.open("https://www.google.com","google",  
"width=400, height=500, left=50, top=10,scrollbars=no,toolbars=no,location=no");  
```

* * *

#### 1.1. 반환값(ret)

새로 만들어진 창 객체가 반환됩니다. 창의 생성에 실패하면 `null`을 반환합니다.

이 객체를 통해서 새창을 제어할 수 있습니다. 예로 `ret.close();` 로 창을 닫을 수 있습니다.

#### 1.2. url

새창에 보여질 주소 입니다. 선택적인 값으로 비워두면 빈창(`about:blank`)이 보입니다.

빈 창을 열고 내용을 동적으로 적을 수 도 있습니다.
```js
    var win = window.open("", "PopupWin", "width=500,height=600");
    win.document.write("<p>새창에 표시될 내용 입니다.</p>");
```

#### 1.3. name

새로 열릴 창의 속성 또는 창의 이름을 지정합니다.

선택적인 값으로 기본값은 `“_blank”` 입니다.

사용 가능한 값을 다음과 같습니다.

*   _blank : 새 창에 열립니다. 이것이 기본값입니다.

    
*   _parent : 부모 프레임에 열립니다.


*   _self : 현재 페이지를 대체합니다.


*   _top : 로드된 프레임셋을 대체합니다.


*   name(임의의 이름) : 새 창이 열리고 창의 이름을 지정합니다.  
    동일한 이름에 다시 open() 을 하면 기존의 열린창의 내용이 바뀝니다.  
    다른 이름을 사용하면 또다른 새창이 열립니다.
    

#### 1.4 specs


| 속성        | 설명                                                   |
|-------------|--------------------------------------------------------|
| width       | 새 윈도우의 너비                                       |
| height      | 새 윈도우의 높이                                       |
| left        | 왼쪽 기준 팝업 위치 지정 **음수는 사용할 수 없습니다** |
| top         | 상단 기준 팝업 위치 지정 **음수는 사용할 수 없습니다** |
| status      | 상태 표시줄 유무                                       |
| menubar     | 메뉴바 유무                                            |
| resizable   | 화면 크기 조절 가능 여부                               |
| location    | 주소 표시줄 유무 **Opera에서만 작동**                  |
| scrollbars  | 스크롤바 유무 **IE,Firefox,Opera에서 작동**            |
| toolbar     | 툴바 유무 **IE,Firefox에서 작동**                      |
| resizable   | 창의 리사이즈 가능 유무 **IE에서만 작동**              |
| fullscreen  | 전체화면모드. **IE에서만 작동**                        |
| channelmode | 전체화면으로 창이 열립니다. **IE에서만 작동**          |


#### 1.5 replace

**히스토리 목록에 새 항목을 만들지 현재 항목을 대체할지 지정합니다.**

*   true : 현재 히스토리를 대체합니다.
    
*   false : 히스토리에 새 항목을 만듭니다.
    

*   출처: [https://www.w3schools.com/jsref/met_win_open.asp](https://www.w3schools.com/jsref/met_win_open.asp)

* * *

### moveTo(), moveBy(), resizeTo(), resizeBy()

* * *

* `moveTo()` 메서드는 **브라우저 창의 위치를 이동시킬때 사용합니다.**

>moveTo(100,200);
//브라우저 창을 x100,y200 위치에서 뜨게 합니다.

* * *

* `moveBy()` 메서드는 **현재 브라우저 창의 위치를 기준으로 이동 시킵니다.**

>moveBy(100,200);
//실행될 때 마다 x100,y200만큼 이동합니다.

* * *

* `resizeTo()` 메서드는 **브라우저 창의 너비와 높이를 바꿀때 사용합니다.**

>resizeTo(200,300);
//width 200px, height 300px로 바꿉니다.

* * *

* `resizeBy()` 메서드는 **현재 창을 기준으로 지정된 픽셀 수만큼 창의 오른쪽 아래 모서리를 이동합니다.**  
    **왼쪽 상단 모서리는 이동하지 않습니다 (원래 좌표로 유지됨).**

>resizeBy(200,300);
//오른쪽 아래 모서리를 width 200px, height 300px 만큼 늘립니다.


------
### senInterval(), clearInterval()

------
#### setInterval()


>기본형
var 참조변수=setInterval(function, milliseconds, param1, param2, ...)

*   `setInterval()` 메소드는 `function`을 호출하거나    
    일정한 간격 (밀리 초)으로 실행문을 반복하여 실행시킬 때 사용합니다.
    

*   `setInterval()` 메소드는 `clearInterval()`이 호출되거나    
    `window`창이 닫힐 때까지 계속됩니다.
    

*   `setInterval()`에 의해 리턴 된 `ID` 값은    
    `clearInterval()` 메소드의 매개 변수로 사용됩니다.

    
*   지정된 밀리 초 후에 함수를 한 번만 실행하려면 `setTimeout()` 메소드를 사용합니다.
    

*   1000ms = 1 초
    

*   4.0버젼 이전의 IE,Opera에서 실행되지 않을 수 있습니다.
    

##### 1.1 function

필수 항목입니다. 실행될 기능(스크립트 실행문)항목 입니다.

##### 1.2 milliseconds

필수 할목입니다. 코드 실행 빈도에 대한 간격 (밀리 초)입니다.

값이 10보다 작은 경우 값 10이 사용됩니다.

##### 1.3 param

선택 옵션입니다.

함수에 전달할 추가 매개 변수가 있는 경우 사용합니다.  
**IE9 및 이전 버전에서는 지원되지 않음**

* * *

#### clearInterval()

`clearInterval()` 메소드는 `setInterval()`로 설정된 타이머를 지웁니다.

*   `clearInterval()` 메소드를 사용하려면 `setInterval()` 메소드를 작성할 때  
    변수를 선언 해줘야 합니다.

```js
myVar = setInterval("javascript function", milliseconds);  
// clearInterval에 setInterval 변수를 선언해 줌으로써  
// setInterval을 중지 할 수 있습니다.  
clearInterval(myVar);  
```

* * *

### setTimeout(), clearTimeout()

* * *

`setTimeout()` 메서드는 일정한 간격 후 스크립트 실행문을

**단 한번만 실행시킵니다.**

반복되길 원하면 `setInterval()`을 사용합니다.

`setTimeout()`변수가 실행되지 않길 원하면 `clearTimeout()`을 사용합니다.

>기본형
var(참조변수) = setTimeout(function, milliseconds, param1, param2, ...)

`setTimeout()`의 `milliseconds`는 코드를 실행하기전에 대기할 밀리 초를 뜻합니다.  
`setInterval()`의 `mliiiseconds`과 다르게 생략 할 수 있습니다.  
생략하게 되면 0값을 갖게되어 바로 실행됩니다.

* * *

`clearTimeout()` 메서드는 아직 실행 되지않은 `setTimeout()`을 취소합니다.

>clearTimeout(id_of_settimeout);

------
### screen

`screen`객체는 사용자의 모니터 정보(속성)을 제공하는 객체입니다.

> screen.속성; 으로 사용합니다.


| 속성        | 설명                                              |
|-------------|---------------------------------------------------|
| availHeight | 윈도우 작업표시줄을 제외한 스크린 높이 값을 반환. |
| availWidth  | 윈도우 작업표시줄을 제외한 스크린 너비 값을 반환. |
| height      | 화면의 총 높이값을 반환.                          |
| width       | 화면의 총 너비값을 반환.                          |
| colorDepth  | 사용자 모니터가 표현 가능한 컬러 bit를 반환.      |
| pixelDepth  | 화면의 색상 해상도 (픽셀 당 비트 수)를 반환.      |

* * *

### location

`location` 객체는 사용자 브라우저 주소창에 url정보(속성)와

새로고침 기능(메서드)를 제공하는 객체 입니다.

>location.속성;
또는
location.메서드() 으로 사용합니다.

------
#### location 객체 속성

| 속성     | 설명                                                       |
|----------|------------------------------------------------------------|
| hash     | URL의 **앵커 부분 (#)**을 설정하거나 반환합니다.           |
| host     | URL의 **호스트 이름과 포트 번호**를 설정하거나 반환합니다. |
| hostname | URL의 **호스트 이름**을 설정하거나 반환합니다.             |
| href     | **주소 영역에 참조 주소**를 설정하거나 URL을 반환합니다.   |
| origin   | URL의 **프로토콜, 호스트 이름 및 포트 번호**를 반환합니다. |
| pathname | URL의 **경로 이름(pathname)**을 설정하거나 반환합니다.     |
| port     | URL의 **포트 번호**를 설정하거나 반환합니다.               |
| protocol | URL의 **프로토콜**을 설정하거나 반환합니다.                |
| search   | URL의 **쿼리 값**을 설정하거나 반환합니다.                 |

------
#### location 객체 메서드

| 종류      | 설명                                              |
|-----------|---------------------------------------------------|
| assign()  | 새 문서(페이지)를 로드 합니다.                    |
| reload()  | 현재 문서(페이지)를 새로고침 합니다.              |
| replace() | 현재 문서(페이지)를 새 문서(페이지)로 교체합니다. |


* * *

### history

`history` 객체에는 브라우저 창 내에서 사용자가 방문한 URL이 포함됩니다.


| 속성                      | 설명                                                                                                                             |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| History.length            | 현재 페이지를 포함해, 방문 기록에 저장된 목록의 개수를 나타내는 정수를 반환합니다.                                               |
| History.scrollRestoration | 기록 탐색 시 **스크롤 위치 복원 여부**를 명시할 수 있습니다. 가능한 값은 auto와 manual입니다.                                    |
| History.state             | 기록 스택 최상단의 상태를 나타내는 값을 반환합니다.**popstate 이벤트를 기다리지 않고 현재 기록의 상태를 볼 수 있는 방법입니다.** |

* * *



| 메서드 종류            | 설명                                                                                                                                                                                                                                                                                                                                       |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| History.back()         | 세션 기록의 바로 뒤 페이지로 이동하는 비동기 메서드입니다. 브라우저의 뒤로 가기 버튼을 눌렀을 때, 그리고 history.go(-1)을 사용했을 때와 같습니다.**참고**: 세션 기록의 제일 첫 번째 페이지에서 호출해도 오류는 발생하지 않습니다.                                                                                                          |
| History.forward()      | 세션 기록의 바로 앞 페이지로 이동하는 비동기 메서드입니다. 브라우저의 앞으로 가기 버튼을 눌렀을 때, 그리고 history.go(1)을 사용했을 때와 같습니다.**참고**: 세션 기록의 제일 마지막 페이지에서 호출해도 오류는 발생하지 않습니다.                                                                                                          |
| History.go()           | 현재 페이지를 기준으로, 상대적인 위치에 존재하는 세션 기록 내 페이지로 이동하는 비동기 메서드입니다. 예를 들어, 매개변수로 -1을 제공하면 바로 뒤로, 1을 제공하면 바로 앞으로 이동합니다.**세션 기록의 범위를 벗어나는 값을 제공하면 아무 일도 일어나지 않습니다. 매개변수를 제공하지 않거나, 0을 제공하면 현재 페이지를 다시 불러옵니다.** |
| History.pushState()    | 주어진 데이터를 지정한 제목(제공한 경우 URL도)으로 방문 기록 스택에 넣습니다. 데이터는 DOM이 불투명(opaque)하게 취급하므로, 직렬화 가능한 모든 JavaScript 객체를 사용할 수 있습니다. 참고로, Safari를 제외한 모든 브라우저는 title 매개변수를 무시합니다.                                                                                  |
| History.replaceState() | 세션 기록 스택의 제일 최근 항목을 주어진 데이터, 지정한 제목 및 URL로 대체합니다. 데이터는 DOM이 불투명(opaque)하게 취급하므로, 직렬화 가능한 모든 JavaScript 객체를 사용할 수 있습니다. 참고로, Safari를 제외한 모든 브라우저는 title 매개변수를 무시합니다.                                                                              |

```html
<button id="prevBtn" onclick="history.back();">  
이전 페이지  
</button>  
  
<button id="nextBtn" onclick="history.forward();">  
다음 페이지  
</button>  
  
<button onclick="history.go(-1);">  
1단계 이전 페이지  
</button>  
  
<button onclick="history.go(-2);">  
2단계 이전 페이지  
</button>  
```

* * *

### navigator

`navigator` 객체는 현재 방문자가 사용하는 브라우저 정보와  
운영체제의 정보를 제공하는 객체 입니다.

> 기본형  
> navigator.속성;

| 속성          | 설명                                                                                                         |
|---------------|--------------------------------------------------------------------------------------------------------------|
| appCodeName   | 유저 브라우저의 내부 코드명을 반환합니다.                                                                    |
| appName       | 유저 브라우저의 이름을 반환합니다.                                                                           |
| appVersion    | 브라우저의 버젼 정보를 반환합니다.                                                                           |
| cookieEnabled | 브라우저에서 쿠키가 활성화되어 있는지 확인합니다.쿠키가 무시되면false값 무시되지 않으면 true값을 반환합니다. |
| geolocation   | **사용자의 위치를 ​​찾는 데 사용할 수있는 Geolocation 객체를 반환합니다.**                                     |
| language      | 유저 브라우저의 언어를 반환합니다.                                                                           |
| onLine        | 브라우저가 온라인 상태인지 확인합니다.                                                                       |
| platform      | 브라우저 플랫폼을 나타내는 문자열을 반환합니다.                                                              |
| product       | 유저 브라우저의 엔진 이름은 반환합니다.                                                                      |
| userAgent     | 유저의 브라우저와 운영체제 종합 정보를 반환합니다.


```js 예시
document.write("appCodeName: "+navigator.appCodeName,"<br/>");  
document.write("appName: "+navigator.appName,"<br/>");  
document.write("appVersion: "+navigator.appVersion,"<br/>");  
document.write("language: "+navigator.language,"<br/>");  
document.write("product: "+navigator.product,"<br/>");  
document.write("platform: "+navigator.platform,"<br/>");  
document.write("userAgent: "+navigator.userAgent,"<br/>");  
```

```html 총정리 실습
<h1>운영체제 및 스크린 정보</h1>  
<p id="info_wrap"></p>  
  
<!-- 아래 버튼을 누를 때 마다 info1 함수의 중괄호 사이의   
 자바스크립트 실행문 코드가 실행됩니다. -->  
<button onclick="info1();">운영체제 정보</button> <br />  
<!-- 아래 버튼을 누를 때 마다 info2 함수의 중괄호 사이의   
 자바스크립트 실행문 코드가 실행됩니다. -->  
<button onclick="info2();">스크린 정보</button> <br />  
  
<script type="text/javascript">  
 /*① [운영 체제 정보]버튼을 누를 때마다 실행되는 함수입니다*/  
 function info1(){  
 /* 사용자 브라우저 운영체제 및 종합 정보를 반환하고,   
 모두 소문자로 바꿉니다.*/  
 const os=navigator.userAgent.toLowerCase();  
    
 // 요소값 중 id값이 "info_wrap"인 요소를 가져옵니다.  
 const info_wrap=document.getElementById("info_wrap");  
    
 /* navigator 객체를 이용해 반환받은 정보에   
 다음과 같은 정보가 포함되어 있으면  
 운영체제 명을 지정한 요소 사이에 텍스트로 출력합니다*/  
 if(os.indexOf("windows")>=0){  
 info_wrap.innerHTML="윈도우";  
    
 }else if(os.indexOf("macintosh")>=0){  
 info_wrap.innerHTML="맥킨토시";  
    
 }else if(os.indexOf("iphone")>=0){  
 info_wrap.innerHTML="아이폰";  
    
 }else if(os.indexOf("android")>=0){  
 info_wrap.innerHTML="안드로이드";  
 }  
 //3초 후에  운영체제 정보를 지웁니다.  
 setTimeout("info_wrap.innerHTML=''",3000);  
 }  
    
 /*② [스크린 정보]버튼을 누를 때마다 실행되는 함수입니다*/  
 function info2(){  
 //사용자 스크린 너비와 높이값을 가져와 저장합니다.  
 const sc_w=screen.width;  
 const sc_h=screen.height;  
 // 요소 중 id가 "info_wrap"인 요소 객체를 가져옵니다.  
 const info_wrap=document.getElementById("info_wrap");  
    
 // 스크린 너비와 높이값을 텍스트로 출력합니다.  
 info_wrap.innerHTML=sc_w+"X"+sc_h;  
 //3초 후에 스크린 정보를 지웁니다.  
 setTimeout("info_wrap.innerHTml=''",3000);  
 }  
 </script>  
```