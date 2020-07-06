---
title: jQuery 이벤트 핸들러 -jQuery
date: 2020-03-12 10:40:59

categories: jQuery
tag:
- jQuery
- JavaScript
- jQuery evnet
- jQuery method
- on()
- bind()
- delegate()
- one()
- off()
- unbind()
- ready()
- load()
- click()
- dbclick()
- <a 태그>
- return false
- preventDefault
- mouseover
- mouseout
- mousemove
- hover
- change()
- index()
- 마우스 좌표값 구하기
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

jQuery에서의 이벤트 사용 기본형

```html
<a href="#" id="btn"
  >버튼<a>
    <script>
      $("#btn").click(function(){...})
    </script>
  </a></a
>
```

- \$(“#btn”)  
  이벤트 대상

* click()  
  이벤트 등록 메서드

- function(){…}  
   이벤트가 발생 했을 때 (이벤트 대상이 클릭되었을 때)  
   실행되는 이벤트 핸들러

<!-- more -->

---

## ready() / load() 메서드

**HTML 문서를 불러올 때 지정된 객체의 로딩이 완료되면 실행되는 이벤트 핸들러.**

> 1.  \$(문서 객체).ready(function)(){...});

2. \$("이미지 또는 프레임").load(function(){...});

- 1.  문서의 로딩이 완료된 후 실행됩니다.

* 2. HTML 문서 로딩이 완료되더라도 이미지나 프레임 소스는 그 이후에 로딩됩니다.
     load() 메서드는 이런 이미지나 프레임에 연동된 소스가 로딩이 완료된 후 실행됩니다.  
     _이미지나 프레임을 제외한 객체에는 사용할 수 없습니다._

---

## click() / dbclick() 메서드

> 1.  \$("요소 선택").click(function(){...});
> 2.  \$("요소 선택").click();

1.  선택한 요소를 클릭할 때 마다 실행문을 실행합니다.

2)  선택한 요소에 강제로 click이벤트가 발생, 유저가 클릭하지 않아도 실행됩니다.

> 3.  \$(“요소 선택”).on(“dbclick”,function(){…});
> 4.  \$(“요소 선택”).dbclick();

3.  선택한 요소를 두번 연속으로 클릭했을 때 실행됩니다.

4)  강제로 더블클릭 이벤트가 발생합니다.

---

## 이벤트 대상이 < a> 태그일 때

**`< a>`태그의 경우 클릭할 때 마다 링크된 주소로 이동됩니다.**

### 링크된 주소로 이동되지 않도록 만드는 방법

---

#### return false

`return false`를 이용하여 `a` 태그를 클릭 했을 때 링크된 주소로 이동되는 것을 막을 수 있습니다.  
단 `return`은 `function` 실행문을 강제로 종료시키므로 함수의 마지막 부분에 작성합니다.

```js
$("a").click(function(){
 ...실행문...
 return false;
});
```

---

#### preventDefault()

`function` 매개변수에 `.preventDefault()` 메서드를 사용하면 링크된 주소로 이동되는 것을 막을 수있습니다.

```js
$("a").click(function (x) {
  x.preventDefalut();
  //...실행문...
});
```

---

## mouseover() / mouseout() / hover() / mousemove()

```js
1. $("요소 선택").mouseover(function(){...});
2. $("요소 선택").mouseover();

3. $("요소 선택").mouseout(function(){...});
4. $("요소 선택").mouseout();

5. $("요소 선택").hover(
 function() {실행문1},
 function() {실행문2}
);

6. $("요소 선택").mousemove(function(){...});
7. $("요소 선택").mousemove();
```

- `mouseover()`

  1.  선택한 요소에 **마우스를 올릴 때 마다** 실행됩니다.
  2.  선택한 요소에 강제로 `mouseover` 이벤트를 발생시킵니다.

* `mouseout()`

  3.  선택한 요소에서 **마우스가 벗어날 때마다** 실행됩니다.
  4.  선택한 요소에 강제로 `mouseout` 이벤트를 발생시킵니다.

- `hover()`

  5.  마우스를 올릴 때 마다 {실행문1}을 실행시키고,  
      마우스가 벗어날 때 마다 {실행문2}를 실행시킵니다.

* `mousemove()`

  6.  선택한 **요소의 영역에서 마우스를 움직일 때 마다** 실행됩니다.
  7.  선택한 요소에 강제로 `mousemove` 이벤트를 발생시킵니다.

---

### mousemove() 마우스 좌표값 구하기

```js mousemove() 마우스 좌표값 구하기
$(function () {
  $(document).mousemove(function (e) {
    // document 영역내에서 마우스를 움직일때 마다 실행
    var x = e.pageX; // X 좌표값 구하기
    var y = e.pageY; // Y 좌표값 구하기
    $("p").text("x좌표값:" + x + "y좌표값:" + y); // x,y 좌표값을 출력
  });
});
```

---

## change() / index()

```js
1. $("요소 선택").change(function(){...});
2. $("요소 선택").change();

3.$("요소 선택").이벤트(function(){
 $("요소 선택").index(this);
});
```

1.  요소의 값이(value) 새 값으로 바뀌고 포커스가 다른 요소로 이동  
    되었을 때 실행됩니다.

2)  강제로 change 이벤트 메서드를 발생시킵니다.

3.  이벤트를 등록한 요소 중에서 이벤트가 발생한 요소의 인덱스 값을 반환합니다.

---

## 그룹 이벤트 등록 및 삭제

| 종류       | 설명                                                                                                             |
| ---------- | ---------------------------------------------------------------------------------------------------------------- |
| on()       | 여러 개의 이벤트를 등록할 때 사용합니다. 이벤트를 등록한 이후에 동일한 태그가 생성되어도 같이 이벤트 실행됩니다. |
| bind()     | 선택 요소에 한 개 이상의 이벤트를 등록.                                                                          |
| delegate() | 선택 요소의 하위 요소에 여러 개의 이벤트 등록. 등록 이후 생성된 동일한 요소에도 같이 이벤트 생성됩니다.          |
| one()      | 한번 이벤트가 발생되면 자동으로 등록된 이벤트가 제거되어 한 번만 실행됩니다.                                     |
| off()      | 선택한 요소에 등록된 이벤트를 제거합니다.                                                                        |
| unbind()   | 선택한 요소에 등록된 이벤트를 제거합니다.                                                                        |

```js 사용 ex)
$(function () {
  $("#btn").on({
    "mouseover focus": function () {
      $(this).css("background-color", "yellow");
    }, // id 값 btn에 마우스가 올라가거나 포커스되면 배경색을 바꿉니다.
    "mouseout blur": function () {
      $(this).css("background-color", "red");
    }, // id 값 btn에 마우스가 벗어나거나 포커스아웃 되면 배경색을 바꿉니다.
  });
  $("#btn").append("<a href='#'>버튼2</a>");
  //id값 btn의 마지막에 새 요소 (a태그로 감싼 버튼2)를 추가했습니다.
  // .on 이벤트로 적용된 이벤트가 그대로 적용됩니다.
});
```
