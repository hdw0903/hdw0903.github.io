---
title: 'ref : DOM에 이름 달기'
tags:
  - ref
  - createRef
  - scrollHeight
  - scrollTop
  - clientHeight

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
date: 2020-05-28 19:13:10
categories: React
---

HTML에서 id를 사용하여 DOM에 이름을 다는 것처럼 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법이 있습니다. 바로 `ref`(reference의 줄임말) 개념입니다.

* [ref는 어떤 상황에서 사용해야 할까?](/2020/05/28/ref-DOM에-이름-달기/#ref)


* [ref 사용](/2020/05/28/ref-DOM에-이름-달기/#ref_use)


* [컴포넌트에 ref 달기](/2020/05/28/ref-DOM에-이름-달기/#ref_component)


* 참고 :
 리액트 컴포넌트 안에서도 id를 사용할 수 있지만, 특수한 경우가 아니라면 사용을 권장하지 않음.
 예를 들어 같은 컴포넌트를 여러 번 사용한다면 중복 id를 가진 DOM이 여러 개 생기니 잘못된 사용이 됨.

 **ref는 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동하기 때문에 이런 문제가 생기지 않음.**

<!-- more -->

------
<h2 id="ref">ref는 어떤 상황에서 사용해야 할까?</h2>

**ref는 DOM을 꼭 직접적으로 건드려야 할 때 사용합니다.**

 1. 특정 `<input>`태그에 포커스를 주는 경우

 2. 스크롤 박스를 조작하는 경우

 3. `<Canvas>`태그에 그림을 그리는 경우


------
<h2 id="ref_use">ref 사용</h2>

프로젝트에서 ref를 사용해 봅시다. **ref를 사용하는 방법은 두 가지가 있습니다.**

------
### 콜백 함수를 통한 ref 설정

**ref를 만드는 가장 기본적인 방법은 콜백 함수를 사용하는 것입니다.**

ref를 달고자 하는 요소에 **ref라는 콜백 함수를 props로 전달해 주면 됩니다**.

이 콜백 함수는 ref 값을 파라미터로 전달받습니다.
그리고 함수 내부에서 파라미터로 받은 ref를 컴포넌트의 멤버 변수로 설정해 줍니다.

```jsx ref 값으로 콜백 함수 전달
<input ref={ (ref) => { this.input=ref }} />
```

이렇게 하면 앞으로 this.input은 input 요소의 DOM을 가리킵니다.
ref의 이름은 원하는 것으로 자유롭게 지정할 수 있습니다.

------
### createRef를 통한 ref 설정

ref를 만드는 또 다른 방법은 리액트에 내장되어 있는 `createRef` 함수를 사용하는 것입니다.

이 기능은 리액트 v16.3부터 도입되었으며 이전 버전에서는 작동하지 않습니다.

```jsx createRef 사용 예시
import React, { Component } from 'react';

class RefSample extends Component {
  input = React.createRef();

  handleFocus = () => {
    this.input.current.focus();
  };

  render() {
    return (
      <div>
        <input ref={this.input} />
      </div>
    );
  }
}
export default RefSample;
```

`createRef`를 사용하여 ref를 만들려면 우선 컴포넌트 내부에서 멤버 변수로 React.createRef()를 담아 줘야 합니다.

해당 멤버 변수를 ref를 달고자 하는 요소에 ref props로 넣어 주면 ref 설정이 완료됩니다.

설정한 뒤 나중에 ref를 설정해 준 DOM에 접근하려면 this.input.current를 조회하면 됩니다.

**콜백 함수를 사용할 때와 다른 점은 이렇게 뒷부분에 .current를 넣어 줘야 한다는 것입니다.**

------
<h2 id="ref_component">컴포넌트에 ref 달기</h2>

리액트에서는 컴포넌트에도 ref를 달 수 있습니다.

이 방법은 주로 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 씁니다.

------
### 사용법

```jsx
<MyComponent
  ref= {(ref) => {this.myComponent=ref}} />
```

이렇게 하면 MyComponent 내부의 메서드 및 맴버 변수에 접근할 수 있습니다.
즉 내부의 ref에도 접근할 수 있습니다.

------
### 예제

**부모 컴포넌트에서 스크롤바 내리기**

ScrollBox.js 컴포넌트 파일을 만들고 스크롤 박스를 만들어 보겠습니다.

```jsx ScrollBox.js
import React, { Component } from 'react';

class ScrollBox extends Component {
  render() {
    const style = {
      border: '1px solid black',
      height: '300px',
      width: '300px',
      overflow: 'auto',
      position: 'relative',
    };
    const innerStyle = {
      width: '100%',
      height: '650px',
      background: 'linear-gradient(white, black)',
    };
    return (
      <div
        style={style}
        ref={(ref) => {
          this.box = ref;
        }}
      >
        <div style={innerStyle} />
      </div>
    );
  }
}
export default ScrollBox;
```

------
#### 컴포넌트에 메서드 생성

컴포넌트에 스크롤바를 맨 아래쪽으로 내리는 메서드를 만들겠습니다.

자바스크립트로 스크롤바를 내릴 때는 DOM 노드가 가진 다음 값들을 사용합니다.

* scrollTop: 세로 스크롤바 위치 (0~350)

* scrollHeight: 스크롤이 있는 박스 안의 div 높이 (650)

* clientHeight: 스크롤이 있는 박스의 높이 (300)

```jsx ScrollBox.js
import React, { Component } from 'react';
class ScrollBox extends Component {
  scrollToBottom = () => {
    const { scrollHeight, clientHeight } = this.box;
    /* 앞 코드에는 비구조화 할당 문법을 사용했습니다.
    다음 코드와 같은 의미입니다.
    const scrollHeight = this.box.scrollHeight;
    const clientHeight = this.box.cliengHeight;
    */
    this.box.scrollTop = scrollHeight - clientHeight;
  };
(...)
```

이렇게 만든 메서드는 부모 컴포넌트인 App 컴포넌트에서 ScrollBox에 ref를 달면 사용할 수 있습니다.

------
#### 컴포넌트에 ref 달고 내부 메서드 사용

```jsx App.js
import React, { Component } from 'react';
import ScrollBox from './ScrollBox';

class App extends Component {
  render() {
    return (
      <div>
        <ScrollBox ref={(ref) => (this.scrollBox = ref)} />
        <button onClick={() => this.scrollBox.scrollToBottom()}>
          맨 밑으로
        </button>
      </div>
    );
  }
}
export default App;
```

App 컴포넌트에서 ScrollBox에 ref를 달고 버튼을 만들어 누르면, ScrollBox 컴포넌트의 scrollToBottom 메서드가 실행됩니다.