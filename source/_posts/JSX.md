---
title: JSX
tags:
  - JSX
  - JSX 규칙
  - ReactDOM.render
  - Fragment
  - 주석

disqusId: tunas-blog-1
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
date: 2020-05-22 18:28:20
categories: React
---

리액트 컴포넌트에서 사용하는 JSX 문법

  * [JSX란?](/2020/05/22/JSX/#JSX)

  * [JSX의 장점](/2020/05/22/JSX/#JSX_장점)

  * [JSX 문법](/2020/05/22/JSX/#JSX_문법)

    * 감싸인 요소
    * 자바스크립트 표현
    * if 문 대신 조건부 연산자(삼항 연산자)
    * AND 연산자(&&)를 사용한 조건부 렌더링
    * undefined를 렌더링하지 않기
    * 인라인 스타일링
    * class 대신 className
    * 꼭 닫아야 하는 태그
    * 주석

<!-- more -->

------
<h2 id="JSX">JSX란?</h2>

JSX는 자바스크립트의 확장 문법이며 XML과 매우 비슷하게 생겼습니다.
이런 형식으로 작성한 코드는 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환됩니다.

```jsx 형태
function App() {
  return (
    <div>
      Hello <b>react</b>
    </div>
  );
}
```
이렇게 작성된 코드는 다음과 같이 변환됩니다.

```js 일반(ES5) 자바스크립트 형태의 코드로 변환
function APP() {
  return React.creatElement("div", null, "Hello ", React.creatElement("b", null, "react"));
}
```

이 처럼 매번 `React.creatElement` 함수를 사용해야 한다면 불편할 것입니다.
JSX를 사용하여 더욱 편하게 UI를 렌더링할 수 있습니다.

------
<h2 id="JSX_장점">JSX의 장점</h2>

* 보기 쉽고 익숙함.
위의 자바스크립트를 사용한 코드와 JSX로 작성한 코드를 비교해 보면
JSX로 작성한 코드는 HTML과 비슷하므로 가독성이 높고 작성하기도 쉽습니다.


* 더욱 높은 활용도
JSX에서는 HTML 태그를 사용할 수 있을 뿐만 아니라, 앞으로 만들 컴포넌트도 JSX 안에서 작성할 수 있습니다.
컴포넌트를 마치 HTML 태그 쓰듯 그냥 작성합니다.

```jsx 예시
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
serviceWorker.unregister();
```

* 참고: ReactDOM.render
이 코드는 컴포넌트를 페이지에 렌더링하는 역활을 하며, render-dom 모듈을 불러와 사용할 수 있습니다.
이 함수의 **첫 번째 파라미터에는 페이지에 렌더링할 내용을 JSX형태로 작성**하고, **두 번째 파라미터에는 해당 JSX를 렌더링할 document 내부 요소를 설정합니다.**

 여기서는 id가 root인 요소 안에 렌더링하게 설정된 것입니다.


------
<h2 id="JSX_문법">JSX 문법</h2>

JSX를 올바르게 사용하려면 몇 가지 규칙을 준수해야 합니다.

------
### 감싸인 요소

**컴포넌트에 여러 요소가 있다면 반드시 부모 요소 하나로 감싸야 합니다.**

```JSX 요소 여러 개가 부모요소로 감싸져 있지 않다면 오류 발생
import React from "react";

function App() {
  return (
      <h1>리액트 안녕</h1>
      <h2>잘 작동하니?</h2>
  );
}

export default App;
```

**요소 여러 개가 부모 요소 하나에 의하여 감싸져 있지 않으면 오류가 발생합니다.**

* 이런 오류가 나는 이유는 **Virtual DOM에서 컴포넌트 변화를 감지할 때 효율적으로 비교할 수 있도록 리액트 컴포넌트 내부는 하나의 DOM 트리 구조로 이루어져야 한다는 규칙이 있기 때문입니다.**  

```jsx div로 감싼 해결법
import React from "react";

function App() {
  return (
    <div>
      <h1>리액트 안녕</h1>
      <h2>잘 작동하니?</h2>
    </div>
  );
}

export default App;
```

이렇듯 요소를 `div`태그로 감싸주면 에러를 해결할 수 있습니다.
하지만 `div`태그를 사용하고 싶지 않은 경우에는 리액트 v16 이상 부터 도입된 `Fragment` 기능을 사용하면 됩니다.

```jsx Fragment 사용
import React, { Fragment } from "react";

function App() {
  return (
    <Fragment>
      <h1>리액트 안녕</h1>
      <h2>잘 작동하니?</h2>
    </Fragment>
  );
}

export default App;
```

코드 상단 `import`구문에서 `react` 모듈에 들어 있는 `Fragment`라는 컴포넌트를 추가로 불러옵니다.
`Fragment`는 다음과 같은 형태로도 표현할 수 있습니다.

```jsx Fragment 생략
import React from "react";
function App() {
  return (
    <>
      <h1>리액트 안녕</h1>
      <h2>잘 작동하니?</h2>
    </>
  );
}
export default App;
```

위 예시 모두 정상적으로 작동하는 경우는 다음과 같은 화면이 브라우저에 출력될 것입니다.

![정상 작동 결과물](/images/React_app.png)

------
### 자바스크립트 표현

JSX 안에서는 자바스크립트 표현식을 쓸 수 있습니다.
자바스크립트 표현식을 작성하려면 JSX 내부에서 코드를 중괄호{}로 감싸면 됩니다.

```jsx
import React from "react";

function App() {
  const name = "리액트";
  return (
    <>
      <h1>{name} 안녕</h1>
      <h2>잘 작동하니?</h2>
    </>
  );
}

export default App;
```

`const name= "리액트";`로 선언해주고 중괄호{}안에 `name`을 작성하여 표현식으로 사용했습니다.

------
### if 문 대신 조건부 연산자(삼항 연산자)

**JSX 내부의 자바스크립트 표현식에서 if 문을 사용할 수 없습니다.**

따라서 조건에 따라 다른 내용을 렌더링해야 할 때는 
* JSX 밖에서 if 문을 사용하여 사전에 값을 설정하거나

* { } 안에 조건부 연산자를 사용하면 됩니다. (조건부 연산자 === 삼항 연산자)

```jsx
import React from "react";

function App() {
  const name = "리액트";
  return (
    <div>
      {name === "리액트" ?(
        <h1>리액트입니다.</h1>
      ):(
        <h2>리액트가 아닙니다.</h2>
      )}
    </div>
  );
}

export default App;
```

이렇게 코드를 작성한 후 저장하면 브라우저에서 '리액트입니다.'라는 문구를 볼 수 있습니다.

여기서 `name` 값을 바꾸면(!===리액트) '리액트가 아닙니다.' 문구가 나타날 것입니다.

------
### AND 연산자(&&)를 사용한 조건부 렌더링

특정 조건을 만족할 때 내용을 보여주고, 만족하지 않을 때는 아무것도 렌더링 하지 않고자 할 때 사용하는 법

```jsx
import React from "react";

function App() {
  const name = "리액트";
  return <div>{name === "리액트" && <h1>리액트입니다.</h1>}</div>;
}

export default App;
```

물론 삼항 연산자로 `{name === "리액트" ? <h1>리액트입니다.</h1> : null}` 형태로 작성할 수도 있지만
&& 연산자로는 예제 처럼 더 짧은 코드로 작업할 수 있습니다.

* && 연산자로 조건부 렌더링을 할 수 있는 이유는 **리액트에서 false를 렌더링할 때는 null과 마찬가지로 아무것도 나타나지 않기 때문입니다.**


* 한가지 주의할 점은 0값은 예외적으로 화면에 나타난다는 것입니다.

------
### undefined를 렌더링하지 않기

리액트 컴포넌트에서는 함수에서 undefined만 반환하여 렌더링 하는 상황을 만들면 안 됩니다.

다음과 같은 코드는 오류를 발생시킵니다.

```jsx
import React from "react";
import './App.css';

function App() {
  const name = undefined;
  return name;
}
export default App;
// App(...): Nothing was returned from render. ......
```

값이 undefined일 수도 있다면 OR(||)연산자를 사용하면 해당 값이 undefined일 때 사용할 값을 지정해 줄 수 있으므로 간단히 오류를 방지할 수 있습니다.

`return name;` > `return name || "값이 undefined입니다";`

* **반면 JSX 내부에서 undefined를 렌더링하는 것은 괜찮습니다.**

```jsx
import React from "react";
import './App.css';

function App() {
  const name = undefined;
  return <div>{name}</div>;
}
export default App;
```

------
### 인라인 스타일링

리액트에서 DOM 요소에 스타일을 적용할 때는 객체 형태로 넣어 주어야 합니다.
스타일 이름 중에 background-color 처럼 하이픈(-) 문자가 포함되어 있는 경우
카멜 표기법(camelCase)으로 작성합니다. backgroundColor

```jsx
import React from "react";

function App() {
  const name = "리액트";
  const style = {
    backgroundColor: "black", // background-color -> backgroundColor
    color: "aqua",
    fontSize: "48px", // font-size -> fontSize
    fontWeight: "bold", // font-weight -> fontWeight
    padding: 16 // 단위 생략시 px로 지정됨
  };
  return <div style={style}>{name}</div>;
}
export default App;
```

style 객체를 미리 선언하고 div의 style 값으로 지정해 주었습니다.
만약 미리 선언하지 않고 div에 style 값을 바로 지정하고 싶다면 아래와 같이 작성합니다.

```jsx
import React from "react";

function App() {
  const name = "리액트";
  return (
    <div
      style={{
        backgroundColor: "black", // background-color -> backgroundColor
        color: "aqua",
        fontSize: "48px", // font-size -> fontSize
        fontWeight: "bold", // font-weight -> fontWeight
        padding: 16, // 단위 생략시 px로 지정됨
      }}
    >
      {name}
    </div>
  );
}

export default App;
```
![결과물](/images/divStyle.png)

------
### class 대신 className

일반 HTML에서 CSS 클래스를 사용할 때는 

```html
<div class="myclass"></div>
```
와 같이 class라는 속성을 설정합니다.

**하지만 JSX에서는 class가 아닌 className으로 설정해 줘야합니다.**

* 우선 App.css 파일을 열어 새 css 클래스를 작성해줬습니다.

```css App.css
.react {
  background: aqua;
  color: black;
  font-size: 48px;
  font-weight: bold;
  padding: 16px;
}
```

* 이제 App.js 파일에서 상단에 App.css를 불러온 뒤 div 요소에 className값을 지정해줍니다.

```jsx
import React from "react";
import "./App.css"; // CSS 불러옴

function App() {
  const name = "리액트";
  return <div className="react">{name}</div>; // class가 아닌 className
}

export default App;
```
* 결과물
  ![결과물](/images/JSX4.png)

* JSX를 작성할 때 CSS 클래스를 설정하는 과정에서 **className이 아닌 class 값을 설정해도 스타일이 적용되지만 브라우저 개발자 도구에서 경고가 나타납니다.**
 ![className이 아닌 class값 설정시](/images/Warningclass.png)

~~이전에는 오류만 발생하고 CSS 클래스가 적용되지 않았지만,~~
<u>리액트 v16 이상부터는 class를 className으로 변환시켜 주고 경고를 띄웁니다.</u>

------
### 꼭 닫아야 하는 태그 

HTML 코드를 작성할 때는 닫지 않는 상태로 코드를 작성하기도 합니다.
ex) `<input></input>`으로 입력하지 않고 `<input>`

**JSX에서는 태그를 닫지 않으면 오류가 발생합니다.**

```jsx
import React from "react";
import "./App.css";

function App() {
  const name = "리액트";
  return (
    <>
      <div className="react">{name}</div>
      <input> 
    </>
  );
}

export default App;
```

이러한 닫혀있지 않은 태그(input)는 `<input></input>` 이나 `<input />` 형태로 닫아줘야 합니다.


------
### 주석

JSX 안에서 주석을 작성하는 방법은 일반 자바스크립트에서 주석을 작성하는 방법과 조금 다릅니다.

```jsx
import React from 'react';
import './App.css';

function App() {
  const name = '리액트';
  return (
    <>
      {/* 주석은 이렇게 작성합니다. */}
      <div
        className="react" // 시작 태그를 여러 줄로 작성하게 된다면 여기에 주석을 작성 할 수 있습니다.
      >
        {name}
      </div>
      // 하지만 이런 주석이나 
      /* 이런 주석은 페이지에 그대로 나타나게 됩니다. */
      <input />
    </>
  );
}

export default App;
```

* JSX 내부에서 주석을 작성할 때는 {/* ... */}와 같은 형식으로 작성합니다.


* 시작 태그를 여러 줄로 작성할 때는 그 내부에서 //...과 같은 형태의 주석도 작성할 수 있습니다.


* 일반 자바스크립트에서 주석을 작성할 때처럼 아무 데나 주석을 작성하면 페이지에 고스란히 나타납니다.


* 브라우저 창에 출력시
 ![React 주석](/images/React_주석.png)