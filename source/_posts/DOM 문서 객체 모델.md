---
title: DOM (문서 객체 모델)
date: 2020-03-08 06:06:24
disqusId: tunas-blog-1
categories: JavaScript
tag: 
- JavaScript
- DOM
- node type
- selector
- 선택자
- getAttribute
- setAttribute
- removeAttribute
- hasAtrribute
- innerHTML
- event
- form
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

`HTML` 문서의 구조를 가리켜 문서 객체 모델(`DOM`:Document Object Model)이라고 합니다.

`DOM`을 배우는 주된 목적은 자바스크립트를 사용하여 문서 객체를 선택하고 속성 또는 스타일(css)을 적용하기 위해서 입니다.

<!-- more -->

* * *

### 선택자


| 구분            | 종류                          | 설명                                                                      |
|-----------------|-------------------------------|---------------------------------------------------------------------------|
| 직접 선택자     | document.getElementByid       | 아이디를 이용해 요소를 선택해 옵니다.                                     |
|                 | document.getElementsByTagName | 요소의 이름을 이용해 요소를 선택해 옵니다.                                |
|                 | document.formName.inputName   | 폼 요소의 name속성을 이용해 요소를 선택합니다.                            |
| 인접 관계선택자 | parentNode                    | 선택한 요소의 부모 요소를 선택해 옵니다.                                  |
|                 | childNodes                    | 선택한 요소의 모든 자식요소를 선택해 옵니다**배열 객체로 저장됩니다.**    |
|                 | children                      | 선택한 요소의 자식요소인 태그만 선택해 옵니다.**배열 객체로 저장됩니다.** |
|                 | firstChild                    | 선택한 요소의 첫 번째 자식 요소만 선택해 옵니다.                          |
|                 | previousSibling               | 선택한 요소의 이전에 오는 형제 요소만 선택해 옵니다.                      |
|                 | nextSibling                   | 선택한 요소의 다음에 오는 형제 요소만 선택해 옵니다.                      |
|                 |                               |                                                                           |
|                 |                               |                                                                           |
|                 |                               |                                                                           |
|                 |                               |                                                                           |


```js
//h1 태그를 불러와 style속성 color "green" 줍니다.  
document.getElementsByTagName("h1")[0].style.color="green";  
// title id속성을 가지고 있는 요소를 불러와 red로 적용합니다.  
document.getElementById("title").style.color="red";  
//id 값이 food_1인 요소의 하위 li 요소중 2번째 요소를 불러와 red로 적용합니다.  
var myList=document.getElementById("food_1").getElementsByTagName("li")[1];  
 myList.style.backgroundColor="red";  
  
/*id 값이 wrap인 요소의 첫번째 요소가 선택 되야하지만  
ie를 제외한 브라우저에서는 요소가 아닌 공백 문자가 선택되어 표시되지 않습니다.  
*/  
document.getElementById("wrap").firstChild.style.color="red";  
    
var p=document.getElementsByTagName("p")[1]; // 공백 선택  
 p.nextSibling.style.backgroundColor="yellow"; // 공백 선택  
```

*   **파이어폭스,크롬,사파리 등의 브라우저들은 HTML코드에 공백이 있거나 줄바꿈이 있으면 그것을 한칸의 공백문자로 인식합니다.**
    하지만 IE 8 이하에서는 정상적으로 공백이나 줄바꿈이 있어도 문자로 인식하지 않습니다.

* * *

### CSS 적용법


자바스크립트에서 `css` 속성을 작성할 때 주의해야 할 점은  
**자바스크립트가 -(하이픈) 문자를 산술 연산자로 인식한다는 것입니다.**

따라서

| HTML 에서 CSS사용 시 | 자바스크립트에서 CSS 사용시 |
|----------------------|-----------------------------|
| background-color     | backgroundColor             |
| font-size            | fontSize                    |

* * *

### Node 타입

노드타입(`nodeType`)을 이용하면 인접 관계 선택자의 호환성 문제를 해결할 수 있습니다.  
`HTML` 노드에는 `HTML`태그를 연결하는 요소(`Element`)노드와 텍스트를 연결하는 텍스트(`Text`)노드,  
HTML태그의 속성을 연결해 주는 속성(`Attribute`)노드가 있습니다.

선택자로 선택한 요소가 어떤 노드인가에 따라 노드 타입 값이 다음과 같이 표기 됩니다.

| 종류                 | 타입 값 |
|----------------------|---------|
| 요소(Element) 노드   | 1       |
| 속성(Attribute) 노드 | 2       |
| 텍스트(Text) 노드    | 3       |


```js 예시
<a href="http://w3.org" id="m">W3C</a>;  

document.getElementByid("m").nodeType; // 1 "<a>"  
document.getElementByid("m").firstChild.nodeType; // 3 "W3C"  
document.getElementByid("m").getAttributNode("href").nodeType; // 2 "href"  
```

```js 비표준 인접 관계 선택자 해결법
document.getElementById("wrap").children[0].style.color="red";  
    
var p=document.getElementsByTagName("p")[1];  
var nextObj=p.nextSibling;  
  
while(nextObj.nodeType !=1){ //다음 요소 태그를 찾을때까지 반복합니다.  
 nextObj=nextObj.nextSibling;  
 }  
 nextObj.style.backgroundColor="yellow";  
```

* * *

#### getAttribute

`getAttribute ()` 메소드는 지정된 이름을 가진 속성 값을 요소의 값으로 반한합니다.

>기본형
element.getAttribute(attributename)

* * *

#### 1.1 .getAttribute

.`getAttribute`(“속성”);  
요소의 지정한 속성값을 불러옵니다.

> var x = document.getElementsByTagName(“H1”)[0].getAttribute(“class”);

* * *

#### 1.2 .setAttribute

.`setAttribute`(“속성”,”새 값”);  
`setAttribute ()` 메소드는 지정된 속성을 요소에 추가하고 지정된 값을 제공합니다.  
추가하려는 속성이 이미 존재한다면, 지정된 값만 추가/변경 됩니다.  
IE 8.0 이전 버젼에서 작동하지 않을 수 있습니다.

이 메소드를 사용하여 값에 스타일 속성을 요소에 추가 할 수 있지만 인라인 스타일링 대신 스타일 오브젝트의 속성을 사용하는 것이 좋습니다. 이는 **스타일에 지정된 다른 CSS 속성을 덮어 쓰지 않기 때문입니다.**

Bad:

> element.setAttribute(“style”, “background-color: red;”);

Good:

> element.style.backgroundColor = “red”;

* * *

#### 1.3 .removeAttribute

`removeAttribute ()` 메소드는 요소에서 지정된 속성을 제거합니다.

`removeAttributNode()` 메소드와 다른점은 `removeAttributeNode()` 메소드는 지정된 `Attr` 오브젝트를 제거하고 이 메소드는 지정된 이름의 속성을 제거합니다.  

둘의 결과는 다르지 않지만,  
`removeAttribute()` 메소드는 반환 값을 갖지 않는 반면 `removeAttributeNode()` 메소드는 제거된 속성을 `Attr` 오브젝트로 반환합니다.

> document.getElementById(“myAnchor”).removeAttribute(“href”);

* * *

#### 1.4 .hasAtrribute

`hasAttribute ()` 메소드는 지정된 속성이 존재하면 `true`를, 그렇지 않으면 `false`를 리턴합니다.  
IE 9.0 이전 버젼에서 작동하지 않을 수 있습니다.

> var x = document.getElementById(“myBtn”).hasAttribute(“onclick”);

* * *

### innerHTML

`innerHTML`은 요소(`element`)내에 포함 된 `HTML` 또는 `XML` 마크업을 가져오거나 설정합니다.

*   주의: < div>, < span> 노드가 (&), (<), (>) 문자를 포함하는 텍스트 노드를 자식으로 가지고 있다면,  
    innerHTML은 이러한 문자들을 각각 “&amp ;”, “&lt ;” ,”&gt ;”로 반환합니다.  
    Node.textContent를 사용하여 이러한 텍스트 노드 내용의 원본을 복사할 수 있습니다.

종류|설명
------|------
element.innerHTML | 선택한 요소의 모든 하위 요소를 문자 데이터로 반환합니다.
element.innerHTML= text; | 선택한 요소의 전체 하위 요소를 새 요소로 변경 또는 생성합니다.


*   경고: 프로젝트가 보안 점검을 거치게 되는 프로젝트인 경우,  
    `innerHTML` 을 사용하면 코드가 거부 될 가능성이 높습니다.  
    예를 들어, 브라우저 확장에서 `innerHTML을` 사용하고  
    `addons.mozilla.org`에 확장을 제출하면 자동 검토 프로세스를 통과하지 못합니다.

* * *

### 이벤트

여기서는 이벤트를 적용시키는 방법과 몇 가지 종류만 다루고  
자세한 내용은 뒤에서 다룹니다.

| 이벤트      | 설명                                     |
|-------------|------------------------------------------|
| onclick     | 요소를 마우스 클릭했을 때 이벤트가 발생  |
| onmouseover | 요소를 마우스 오버 했을 때 이벤트 발생.  |
| onmounout   | 요소에 마우스가 벗어났을 때 이벤트 발생. |
| submit      | 폼에 전송이 일어났을 때 이벤트 발생.     |

>기본형
요소선택.이벤트종류=function(){
}

```js 예시
document.getElementByid("btn1").onclick=function(){  
 alert("welcome")  
}  
```

* * *

### form 요소 선택자

유저가 입력한 값이 유효한 값인지 검사하기 위해서는  
폼에 입력 요소를 선택하고 속성을 제어할 수 있어야 합니다.  
아래는 폼 요소를 선택할 때 사용하는 몇 가지 방식입니다.

| 구분                 | 종류                                                                                           | 설명                                       |
|----------------------|------------------------------------------------------------------------------------------------|--------------------------------------------|
| 입력 요소 선택자     | document.getElementByid(“아이디 명)                                                            | 폼 요소를 아이디로 선택합니다.             |
|                      | document.폼이름.입력 요소 이름                                                                 | 폼 요소를 이름으로 선택합니다.             |
| select option 선택자 | document.폼이름.입력 요소 이름.option[ index]                                                  | < select>에< option>을 선택합니다.         |
|                      | var i= document.폼이름.입력요소 이름.selectedIndex; document.폼 이름.입력요소 이름.option[ i]; | < select>에 선택된 < option>을 선택합니다. |


**폼 요소 속성 종류**

| 구분                  | 속성         | 설명                                                                                                                    |
|-----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------|
| 전체                  | value        | 입력 요소의 value 속성을 변경하거나, 값을 가져옵니다.                                                                   |
|                       | disabled     | 입력 요소의 disabled 속성을 변경하거나, 현재 상태의 값을 가져옵니다.                                                    |
|                       | defaultValue | 입력 요소 초기에 입력된 value 값을 가져옵니다.                                                                          |
| 선택박스              | selected     | < select>태그에 < option> 선택된 상태 값을 가져옵니다. 선택되있다면true 아니라면 false                                  |
| 체크 박스 라디오 버튼 | checked      | 체크박스 또는 라디오 버튼태그 체크 상태 값을 반환하거나 체크 여부를 제어 합니다. 체크 되있으면 true 해제되 있다면 false |

