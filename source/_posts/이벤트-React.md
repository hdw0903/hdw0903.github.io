---
title: 이벤트 -React
tags:
  - 이벤트
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
date: 2020-05-27 18:23:01
categories: React
---

* [리액트의 이벤트 시스템](/2020/05/27/이벤트-React/#React_event)
  * 이벤트 사용 시 주의사항


* [이벤트 핸들링 익히기](/2020/05/27/이벤트-React/#event_handling)


* [함수형 컴포넌트로 구현](/2020/05/27/이벤트-React/#3)

<!-- more -->

------
<h2 id="React_event">리액트의 이벤트 시스템</h2>

리액트의 이벤트 시스템은 웹 브라우저의 HTML 이벤트와 인터페이스가 동일하기 때문에 사용법이 비슷한데,

주의해야할 몇가지 사항이 있습니다.

------
### 이벤트 사용 시 주의 사항

1. 이벤트 이름은 카멜 표기법으로 작성합니다.
  ex) onclick -> onClick, onchange -> onChange


2. 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라, **함수 형태의 객체를 전달합니다.**


3. DOM 요소에만 이벤트를 설정할 수 있습니다.

 `<div>`, `<button>`, `<input>`, `<form>`, `<span>`등 DOM요소에만 이벤트를 사용할 수 있습니다.

 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정할 수 없습니다.

 ```jsx 컴포넌트에 이벤트 설정 불가
 <Mycomponent onClick={something} />
 ```
 위 코드는 Mycomponent를 클릭할 때 something 함수를 실행하는 것이 아니라, 그냥 이름이 onClick인 props를 Mycomponent에게 전달해 줍니다. 이벤트가 발생하지 않습니다.

------
### 이벤트 종류

리액트에서 지원하는 이벤트 종류는 다음과 같습니다.

|           |            |             |
|-----------|------------|-------------|
| Clipboard |    Touch   | Composition |
|     UI    |  Keyboard  |    Wheel    |
|   Focus   |    Media   |     Form    |
|   Image   |    Mouse   |  Animation  |
| Selection | Transition |             |

* 더 자세한 이벤트 정보는 [리액트 메뉴얼을 참고합니다.](https://facebook.github.io/react/docs/events.html)

------
<h2 id="event_handling">이벤트 핸들링 익히기</h2>


다음 단계에 맞춰 이벤트 핸들링을 익혀봅니다.
![이벤트 핸들링 예제 순서](/images/이벤트_핸들링_예제_순서.png)


먼저 EventPractice.js 파일을 만들고 클래스형 컴포넌트로 작성 후 App 컴포넌트에서 불러와 렌더링하겠습니다.

```jsx EventPractice.js
import React, { Component } from "react";

class EventPractice extends Component {
  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
      </div>
    );
  }
}
export default EventPractice;
```

```jsx App.js
import React from "react";
import EventPractice from "./EventPractice";

const App = () => {
  return <EventPractice />;
};

export default App;
```

------
### onChange 이벤트 핸들링

#### onChange 이벤트 설정

EventPractice 컴포넌트에 input 요소를 렌더링하는 코드와 해당 요소에 onChange 이벤트를 설정하는 코드를 작성합니다.

```jsx EventPractice.js
import React, { Component } from "react";

class EventPractice extends Component {
  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input
          type="text"
          name="message"
          placeholder="아무거나 입력하세요"
          onChange={(e) => {
            console.log(e);
          }}
        />
      </div>
    );
  }
}
export default EventPractice;
```

웹 브라우저에서 개발자 도구를 열어 인풋에 아무거나 입력해보면 이벤트 객체가 콘솔에 나타납니다.

콘솔에 기록되는 e 객체는 `SyntheticEvent`로 웹 브라우저에서 네이티브 이벤트를 감싸는 객체입니다.

* `SyntheticEvent`는 네이티브 이벤트와 달리 이벤트가 끝나고 나면 이벤트가 초기화되므로 정보를 참조할 수 없습니다. ex) 0.5초뒤 e 객체를 참조하면 e 객체 내부의 모든 값이 비워져있어 참조 불가능.


* 비동기적으로 이벤트 객체를 참조할 일이 있다면 e.persist() 함수를 호출해 줘야합니다.

onChange 이벤트가 발생할 때, 변할 input 값을 콘솔에 기록하려면 `e.target.value`를 콘솔에 넣어주면 됩니다.

```jsx EventPractice.js onChange 코드 수정
onChange={
  (e) => {
    console.log(e.target.value);
  }
}
```

input 값이 변할 때 마다 콘솔에 기록됩니다.

------
#### state에 input 값 담기

* state 초기값을 설정하고

* 이벤트 핸들링 함수 내부에서 this.setState 메서드를 호출하여 state를 업데이트합니다.

* input의 `value` 값을 `state`에 있는 값으로 설정합니다.

```jsx EventPractice.js
import React, { Component } from "react";

class EventPractice extends Component {

  state ={
    message: ''
  }

  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input
          type="text"
          name="message"
          placeholder="아무거나 입력하세요"
          value={this.state.message}
          onChange={(e) => {
            this.setState({
              message: e.target.value
            })
          }}
        />
      </div>
    );
  }
}
export default EventPractice;
```

브라우저에서 인풋에 입력했을 때 오류가 발생하지 않고 재대로 입력할 수 있다면 state에 텍스트를 잘 담은 것입니다.

------
#### 클릭 이벤트시 state 값 출력 후 초기화

`<button>`을 하나 만들고 클릭 이벤트가 발생 시 현재 comment(state로 담은 값)값을 alert 창으로 띄운 후 초기값으로 설정합니다.

```jsx
import React, { Component } from "react";

class EventPractice extends Component {
  state = {
    message: "",
  };

  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input
          type="text"
          name="message"
          placeholder="아무거나 입력하세요"
          value={this.state.message}
          onChange={(e) => {
            this.setState({
              message: e.target.value,
            });
          }}
        />
        <button
          onClick={() => {
            alert(this.state.message);
            this.setState({
              message: "",
            });
          }}
        >
          확인
        </button>
      </div>
    );
  }
}
export default EventPractice;
```

input에 아무거나 입력 후 확인 버튼을 누르면 state에 담은 값이 alert창으로 출력되고 확인버튼을 누르면 input 값이 초기화 됩니다.

------
### 임의 메서드 만들기

앞서 onChange와 onClick에 전달한 함수를 따로 빼내어 컴포넌트 임의 메서드를 만들어 봅니다.

```jsx EventPractice.js
import React, { Component } from "react";

class EventPractice extends Component {
  state = {
    message: "",
  };
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleClick = this.handleClick.bind(this);
  }

  handleChange(e) {
    this.setState({
      message: e.target.value,
    });
  }
  
  handleClick() {
    alert(this.state.message);
    this.setState({
      message: "",
    });
  }
  
  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input
          type="text"
          name="message"
          placeholder="아무거나 입력하세요"
          value={this.state.message}
          onChange={this.handleChange}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}
export default EventPractice;
```

------
#### Property Initializer Syntax를 사용한 메서드 작성

메서드 바인딩은 생성자 메서드에서 바인딩 하는 방법이 정석이지만,
새로운 메서드를 만들 때마다 constructor를 수정해야하는 번거로움이 있습니다.

바벨의 `transform-class-properties` 문법을 사용하여 화살표 함수 형태로 작성하면 
**생성자 함수를 사용하지 않고 동적으로 바인딩이 가능합니다.**

```jsx
class EventPractice extends Component {
  state = {
    message: "",
  };
  handleChange = (e) => {
    this.setState({
      message: e.target.value,
    });
  };
  handleClick = () => {
    alert(this.state.message);
    this.setState({
      message: "",
    });
  };
```

`transform-class-properties`을 사용하여 이처럼 사용할 수 있습니다.

------
### input 여러 개 다루기

**동일한 여러 태그 다뤄보기**

동일한 태그 여러 개를 작업할 때 메서드를 여러개 만드는 방법보다 쉬운 방법이 있습니다.

* **event 객체를 활용하여 처리할 수 있습니다.**

onChange 이벤트 핸들러에서 e.target.name은 해당 인풋의 name을 가리킵니다. (현재 message)

이 값을 사용하여 state를 설정해 보겠습니다.

```jsx e.target.name 사용
import React, { Component } from 'react';
class EventPractice extends Component {
  state = {
    username: '',
    message: ''
  };
  handleChange = (e) => {
    this.setState({
      [e.target.name]: e.target.value
    });
  };
  handleClick = () => {
    alert(this.state.username + ': ' + this.state.message);
    this.setState({
      username: '',
      message: ''
    });
  };
  
  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input
          type="text"
          name="username"
          placeholder="유저명"
          value={this.state.username}
          onChange={this.handleChange}
        />
        <input
          type="text"
          name="message"
          placeholder="아무거나 입력해보세요"
          value={this.state.message}
          onChange={this.handleChange}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}
export default EventPractice;
```

**객체 안에서 key를 []로 감싸면 그 안에 넣은 레퍼런스가 가리키는 실제 값이 key 값으로 사용됩니다.**

------
### onKeyPress 이벤트 핸들링

comment 인풋에서 enter를 눌렀을 때 KeyPress 이벤트를 처리하는 handleClick 메서드를 호출하도록 해봅니다.

```jsx KeyPress
import React, { Component } from "react";
class EventPractice extends Component {
  state = {
    username: "",
    message: "",
  };
  handleChange = (e) => {
    this.setState({
      [e.target.name]: e.target.value,
    });
  };
  handleClick = () => {
    alert(this.state.username + ": " + this.state.message);
    this.setState({
      username: "",
      message: "",
    });
  };
  handleKeyPress = (e) => {
    if (e.key === "Enter") {
      this.handleClick();
    }
  };
  render() {
    return (
      <div>
        <h1>이벤트 연습</h1>
        <input
          type="text"
          name="username"
          placeholder="유저명"
          value={this.state.username}
          onChange={this.handleChange}
        />
        <input
          type="text"
          name="message"
          placeholder="아무거나 입력해보세요"
          value={this.state.message}
          onChange={this.handleChange}
          onKeyPress={this.handleKeyPress}
        />
        <button onClick={this.handleClick}>확인</button>
      </div>
    );
  }
}
export default EventPractice;
```

두 번째 텍스트 인풋에서 텍스트를 입력하고 enter를 누르면 handleClick 메서드가 실행됩니다.

------
<h2 id="3">함수형 컴포넌트로 구현</h2>

여태 한 작업을 함수형 컴포넌트로 똑같이 구현할 수 있습니다.

함수형 컴포넌트로 구현 시 

```jsx 함수형 컴포넌트로 작성
import React, { useState } from 'react';

const EventPractice = () => {
  const [username, setUsername] = useState('');
  const [message, setMessage] = useState('');
  const onChangeUsername = (e) => setUsername(e.target.value);
  const onChangeMessage = (e) => setMessage(e.target.value);
  const onClick = () => {
    alert(username + ': ' + message);
    setUsername('');
    setMessage('');
  };
  const onKeyPress = (e) => {
    if (e.key === 'Enter') {
      onClick();
    }
  };
  return (
    <div>
      <h1>이벤트 연습</h1>
      <input
        type="text"
        name="username"
        placeholder="유저명"
        value={username}
        onChange={onChangeUsername}
      />
      <input
        type="text"
        name="message"
        placeholder="아무거나 입력해보세요"
        value={message}
        onChange={onChangeMessage}
        onKeyPress={onKeyPress}
      />
      <button onClick={onClick}>확인</button>
    </div>
  );
};
export default EventPractice;
```

위 코드에서는 e.target.name을 활용하지 않고 onChange 관련 함수 두 개를 따로 만들어 주었습니다.

이번에는 useState를 통해 사용하는 상태에 문자열이 아닌 객체를 넣어 보겠습니다.

```jsx EventPractice.js 수정
import React, { useState } from 'react';

const EventPractice = () => {
  const [form, setForm] = useState({
    username: '',
    message: '',
  });
  const { username, message } = form;
  const onChange = (e) => {
    setTimeout(() => console.log(e), 500);
    const nextForm = {
      ...form, // 기존의 form 내용을 이 자리에 복사 한 뒤
      [e.target.name]: e.target.value, // 원하는 값을 덮어씌우기
    };
    setForm(nextForm);
  };
  const onClick = () => {
    alert(username + ': ' + message);
    setForm({
      username: '',
      message: '',
    });
  };
  const onKeyPress = (e) => {
    if (e.key === 'Enter') {
      onClick();
    }
  };
  return (
    <div>
      <h1>이벤트 연습</h1>
      <input
        type="text"
        name="username"
        placeholder="유저명"
        value={username}
        onChange={onChange}
      />
      <input
        type="text"
        name="message"
        placeholder="아무거나 입력해보세요"
        value={message}
        onChange={onChange}
        onKeyPress={onKeyPress}
      />
      <button onClick={onClick}>확인</button>
    </div>
  );
};
export default EventPractice;
```

e.target.name 값을 활용하려면, 위와 같이 useState를 쓸 때 인풋 값들이 들어있는 form 객체를 사용해 주면 됩니다.