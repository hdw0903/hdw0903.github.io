---
title: 효과 및 애니메이션 -jQuery
date: 2020-03-12 13:21:33
disqusId: tunas-blog-1
categories: jQuery
tag: 
- jQuery
- JavaScript
- hide()
- show()
- toggle()
- fadeIn()
- fadeOut()
- fadeTo()
- fadeToggle()
- slideDown()
- slideUp()
- slideToggle()
- animate
- stop()
- delay()
- queue()
- clearQueue()
- dequeue()
- finish()
- Callback
- chaining
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

*   jQuery Effects
    *   [jQuery Hide/Show](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_Hide_Show)
    *   [jQuery Fade](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_Fade)
    *   [jQuery Slide](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_Slide)
    *   [jQuery Animate](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_Animate)
    *   [jQuery stop()](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_stop)
    *   [jQuery delay()](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_delay)
    *   [jQuery queue()](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_queue)
    *   [jQuery finish()](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_finish)
    *   [jQuery Callback](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_Callback)
    *   [jQuery Chaining](/2020/03/12/효과%20및%20애니메이션%20-jQuery/#jQuery_Chaining)

<!-- more -->

* * *

<h2 id="jQuery_Hide_Show">hide() / show() / toggle()</h2>

> 1.  $(selector).hide(speed,callback);
> 2.  $(selector).show(speed,callback);

*   speed : 효과속도 vlaue:[ “fast” | “normarl” | “slow” | (milliseconds) ]
*   callback : 콜백 함수 실행문 (콜백 매개 변수는 hide () 또는 show () 메소드가 완료된 후 실행될 함수입니다)

1.  선택 요소를 효과 속도에 맞춰 숨긴 후 콜백 실행
2.  선택 요소가 숨겨져 있는 경우 속도에 맞춰 노출시키고 콜백 실행

```js
$("#hide").click(function(){  
 $("p").hide();  
});  
  
$("#show").click(function(){  
 $("p").show();  
});  
```

**toggle() 메서드로 요소를 숨기거나 보여지게 할 수 있습니다.**

```js
$("button").click(function(){  
 $("p").toggle();  
});  
```

*   speed : normarl 값을 제외한 “fast”, “slow” ,milliseconds 값을 사용할 수 있습니다.

* * *

<h2 id="jQuery_Fade">fadeIn() / fadeOut() / fadeToggle() / fadeTo()</h2>

* * *

*   `fadeIn()` 메소드는 숨겨진 요소를 서서히 보여지게 합니다.

```js
$("button").click(function(){  
 $("#div1").fadeIn();  
 $("#div2").fadeIn("slow");  
 $("#div3").fadeIn(3000);  
});  
```

* * *

*   `fadeOut()` 메소드는 보여지는 요소를 서서히 감춥니다.

* * *

*   `fadeToggle()` 메소드는  
    요소가 숨겨져 있다면 서서히 보여지게,  
    보여지고 있는 요소라면 서서히 감추게 해줍니다.

* * *

*   `fadeTo()` 메소드를 사용하면  
    `opacity` (0~1사이) 값을 지정해서 요소를 사라지게 할 수 있습니다.

```js
$("button").click(function(){  
 $("#div1").fadeTo("slow", 0.15);  
 $("#div2").fadeTo("slow", 0.4);  
 $("#div3").fadeTo("slow", 0.7);  
});  
```

* * *

<h2 id="jQuery_Slide">slideDown() / slideUp() / slideToggle()</h2>


>$(selector).slide메소드(speed,callback);

speed: “slow”, “fast”, or milliseconds  
callback: 콜백 함수는 슬라이딩이 완료된 후 실행될 함수 입니다.

* * *

*   `slideDown()` 메소드는 요소를 슬라이드-다운(펼침) 하게 해줍니다.

* * *

*   `slideUp()` 메소드는  
    `slideDown`되어 펼쳐져 있는 요소를 슬라이드-업(닫음) 하게 해줍니다.

* * *

*   `slideToggle()` 메소드는 요소가 슬라이드 닫혀 있다면 펼쳐주고, 펼쳐 있다면 닫아줍니다.

```js
$("#flip").click(function(){ // 클릭 했을 때 실행됩니다.  
 $("#panel").slideToggle();   
 // 클릭 이벤트가 발생한 요소 아래에 slide이벤트를 발생 시킵니다.  
});  
```

* * *

<h2 id="jQuery_Animate">animate</h2>

`animate()` 메소드를 사용하여 animation을 커스텀 할 수 있습니다.

> 기본형  
$(selector).animate({params},speed,callback);

*   params 매개 변수는 애니메이션효과를 줄 CSS 특성을 말합니다.

```js 사용 예시
$("button").click(function(){  
 $("div").animate({left: '250px'});  
});  
```

**기본적으로 HTML 요소들은 정적위치를 가지기에 이동할 수 없습니다.**  
**위치를 이동시키려면 우선 이동시킬 요소의 CSS 속성 position 값을** 
**position: relative, fixed, or absolute으로 설정해 줘야 합니다.**

* * *

**`animate()` 메소드를 이용하여 여러CSS속성을 한번에 조작 할 수도 있습니다.**

```js
$("button").click(function(){  
 $("div").animate({  
 left: '250px',  
 opacity: '0.5',  
 height: '150px',  
 width: '150px'  
 });  
});  
```

**animate() 메소드를 사용할 때 camel 네이밍 문법으로 사용했는지 유의하며 사용해야합니다.**

> padding-left (x) : paddingLeft (o)  
> margin-right (x) : marginRight (o)

또한 색상 애니메이션은 `jQuery`핵심 파일에 포함되어 있지 않습니다.  
색상에 애니메이션을 주고 싶다면 Color Animations plugin from [https://jQuery.com](https://jQuery.com)다운받아야 합니다.

* * *

**애니메이션 효과를 현재 값을 기준으로 상대적으로 줄 수 있습니다.**

단지 값앞에 `+=` 또는 `-=` 을 넣어주는 것 만으로 됩니다.

```js
$("button").click(function(){  
 $("div").animate({  
 left: '250px',  
 height: '+=150px',  
 width: '+=150px'   
 });  
});  
```

* * *

**사전 정의된 값(Pre-defined Values)을 사용 할 수 있습니다.**

예를 들어 `animate()`값을 `hide`, `show`, `toggle` 으로 줄 수 있습니다.

```js toggle
$("button").click(function(){  
 $("div").animate({  
 height: 'toggle'  
 //버튼이 클릭될 때 마다 height 속성이  
 //사라지거나 보여집니다.  
 });  
});  
```

* * *

**Uses Queue Functionality**

기본적으로 `jQuery`에는 애니메이션 대기열 기능이 포함되어있습니다.

이 말은 `jQuery animate()`메소드를 여러 줄로 작성했을 때  
`jQuery` 자체적으로 애니메이션들의 대기열을 만들어 `animate()`메소드 들을 한번에 실행시키는 것이 아니라  
**한개씩 불러와 순차적으로 실행 시키는 기능을 말합니다.**

따라서, `animate()`끼리의 순차적으로 다른 애니메이션효과를 원할때 사용합니다.

```js
$("button").click(function(){  
 var div = $("div");  
 1. div.animate({height: '300px', opacity: '0.4'}, "slow");  
 2. div.animate({width: '300px', opacity: '0.8'}, "slow");  
 3. div.animate({height: '100px', opacity: '0.4'}, "slow");  
 4 .div.animate({width: '100px', opacity: '0.8'}, "slow");  
});  
```

`animate`가 동시에 실행 되는 것이 아니라

1.`animate` 효과가 끝난 후  
2.`animate`가 실행 / 3,4번까지  
순차적으로 실행됩니다.

* * *

<h2 id="jQuery_stop">stop()</h2>

`jQuery stop ()` 메서드는 애니메이션이나 효과가 끝나기 전에 중지하는 데 사용됩니다.

`stop ()` 메소드는 슬라이딩, 페이딩 및 사용자 정의 애니메이션을 포함한 모든 `jQuery` 효과 함수에 작동합니다.

> $(selector).stop(stopAll,goToEnd);

*   stopAll 선택적 매개 변수입니다.  
    애니메이션 큐도 지워야하는지 여부를 지정합니다.  
    기본값은 false입니다. 즉, 활성 애니메이션 만 중지되어 대기중인 애니메이션을 나중에 수행 할 수 있습니다.
    
*   goToEnd 선택적 매개 변수입니다.  
    현재 애니메이션을 즉시 완료할지 여부를 지정합니다.  
    기본값은 false입니다.
    

따라서 기본적으로 `stop()` 메서드는  
**선택한 요소에서 수행중인 현재 애니메이션만 종료합니다.**

```js
$("#stop").click(function(){  
 $("#panel").stop();  
});  
```

* * *

<h2 id="jQuery_delay">delay()</h2>

`delay()` 메소드는 `queue`에서 다음 항목의 실행을 지연시킵니다.

대기중인 `effect`가 지정된 시간만큼 지연된 후 발생합니다.

> $(selector).delay(speed,queueName)

*   queueName : queue이름을 직접 지정해 줄 수있습니다.  
    기본값은 effect queue의 표준인 “fx”입니다.

```js
$("button").click(function(){  
 $("#div1").delay("slow").fadeIn();  
 $("#div2").delay("fast").fadeIn();  
 $("#div3").delay(1000).fadeIn();  
});  
```

* * *

<h2 id="jQuery_queue">queue() / clearQueue() / dequeue()</h2>

`queue`란 특정 요소에 실행 대기 중인  
`method` 또는 `function`의 저장소를 말합니다.

>1. $("요소 선택").queue();
2. $("요소 선택").queue(function(){...});

1.  선택한 요소의 `queue`에 대기중인 메서드를 반환합니다.
2.  선택한 요소의 `queue`에 `function`을 저장합니다.  
    저장된 이후에 대기중인 메서드는 모두 제거합니다.

*   `clearQueue()` 메서드는  
    요소의 `queue`에서 가장 앞에 있는 메서드만 뺴고  
    나머지 `queue`에서 대기중인 모든 메서드를 제거합니다.
    
*   `dequeue()` 메서드는  
    선택한 요소 큐에 대기중인 모든 메서드를 제거합니다.
    

* * *

<h2 id="jQuery_finish">finish()</h2>

`finish()`메소드는 `animation`의 현재 동작상태를 모두 무시하고  
완료 시켜버립니다. (최종 완료 시점만 보여줌)

```js
$("#complete").click(function(){  
 $("div").finish();  
});  
```

구동 원리: 현재 실행중인 애니메이션을 중지하고 `queue`에 대기중인 모든 애니메이션을 제거하고  
선택한 요소에 대한 모든 애니메이션을 완료합니다.

* * *

<h2 id="jQuery_Callback">Callback</h2>

`callback() function`은 현재 실행중인 효과가 완전히 끝난후 실행될 `function`을 의미합니다.

`Javascript`문은 기본적으로 한줄씩 실행됩니다. 그러나 코드 내에 `effect` 들이 있다면  
첫 줄 `effect`가 완료되기 전에 다음 코드 줄에 있는 `effect`가 실행되어 오류가 발생할 수 있습니다.

이를 방지하기 위해 `callback function`을 만드는 것 입니다.

callback function 없이 사용했을 때

```js callback function 없이 사용했을 때
$("button").click(function(){  
 $("p").hide(1000);  
 alert("The paragraph is now hidden");  
});  
```

위에 코드는 `button`요소를 클릭 했을 때 실행되지만 `p`요소가 완전히 숨겨지고 나서 경고창이 나오지 않습니다. **숨겨지기전에 경고창이 나오게 될 것입니다.**

```js callback function 이용한 바른 예
$("button").click(function(){  
 $("p").hide(1000, function(){  
 alert("The paragraph is now hidden");  
 });  
});  
```

먼저 실행될 `hide`메소드에 `callback function`을 넣어줘 정상적으로 `p` 요소가 완전히 `hide` 된 후 경고창이 표시됩니다.

* * *

<h2 id="jQuery_Chaining">chaining</h2>

`jQuery`를 사용하면 액션 / 메소드를 함께 연결할 수 있습니다.  
체인을 사용하면 단일 명령문 내에서 **동일한 요소를** 여러 `jQuery` 메소드로 실행할 수 있습니다.

**이 방법으로 브라우저는 동일한 요소를 두 번 이상 찾을 필요가 없습니다.**

```js
$("#p1").css("color", "red").slideUp(2000).slideDown(2000);  
===  
$("#p1").css("color", "red")  
 .slideUp(2000)  
 .slideDown(2000);  
```

첫번째로 `p`요소의 색상이 변하고 두번째로 슬라이드가 닫히고, 세번째로 슬라이드가 펼쳐집니다.

체인 방식으로 작성하다보면 코드줄이 길어질 수있습니다.

`jQuery`는 구문에 엄격하지 않으므로  
가독성을 위해 줄 바꿈 및 들여쓰기를 사용하여 써도 잘 작동 합니다.
