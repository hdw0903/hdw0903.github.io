---
title: 컴포넌트(component)
tags:
  - 컴포넌트
  - component
  - render
  - state
  - props
  - setState
  - PropsTypes
  - isRequired
  - children
  - defaultProps
  - export
  - import
  - useState
  - constructor
  
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
date: 2020-05-24 18:53:57
categories: React
---

컴포넌트를 선언하는 방식은 두 가지입니다.
하나는 함수형 컴포넌트이고 또 다른 하나는 클래스형 컴포넌트입니다.

* [클래스형 컴포넌트](/2020/05/24/컴포넌트-component/#component)
  * 함수형 컴포넌트


* [컴포넌트 생성](/2020/05/24/컴포넌트-component/#New_component)
  * src 디렉터리에 MyComponent.js 파일 생성
  * 코드 작성
  * 모듈 내보내기(export) 및 불러오기(import)


* [props](/2020/05/24/컴포넌트-component/#props)
  * JSX 내부에서 props 렌더링
  * 컴포넌트를 사용할 때 props 값 지정하기
  * props 기본값 설정: defaultProps
  * 컴포넌트 태그 사이의 내용을 보여주는 children
  * 비구조화 할당 문법을 통한 props 내부 값 추출
  * propTypes를 통한 props 검증
  * 클래스형 컴포넌트에서 props 사용하기

* [state](/2020/05/24/컴포넌트-component/#state)
  * 클래스형 컴포넌트의 state
  * 함수형 컴포넌트에서 useState 사용하기


* [state 사용시 주의 사항](/2020/05/24/컴포넌트-component/#state_주의사항)

<!-- more -->

------
<h2 id="component">클래스형 컴포넌트</h2>

아래 App 컴포넌트는 함수형 컴포넌트이며, 코드가 다음과 같은 구조로 이루어져 있습니다.

```jsx App.js
import React from 'react';
import './App.css';

function App() {
  const name = "리액트";
  return <div className="react">{name}</div> 
}

export default App;
```

App.js 코드를 수정하여 클래스형 컴포넌트로 만들면

```jsx 클래스형 컴포넌트
import React, { Component } from 'react';

class App extends Component {
  render() {
    const name = "react";
    return <div className="react">{name}</div>
  }
}

export default App;
```

클래스형 컴포넌트로 작성하였지만 역활은 이전의 함수형 컴포넌트와 똑같습니다.

* 클래스형 컴포넌트와 함수형 컴포넌트의 차이점은 **클래스형 컴포넌트의 경우 `state` 기능 및 `라이프사이클` 기능을 사용할 수 있다는 것과 임의 메서드를 정의할 수 있다는 것입니다.**


* 클래스형 컴포넌트에서는 `render`함수가 꼭 있어야 하고, 그 안에서 보여 주어야 할 JSX를 반환해야 합니다.

------
### 함수형 컴포넌트

* **함수형 컴포넌트 장점**
  
  * 클래스형 컴포넌트보다 선언하기 훨씬 편리하다.
  * 메모리 자원도 클래스형 컴포넌트보다 덜 사용한다.
  * 프로젝트를 완성하여 빌드한 후 배포시에도 함수형 컴포넌트 결과물의 파일 크기가 더 작다.


* **함수형 컴포넌트 주요 단점**

  * 함수형 컴포넌트의 주요 단점은 `state`와 `라이플사이클 API`의 사용이 불가능하다는 점이었으나,
   **이 단점은 리액트 v16.8 업데이트 이후 `Hooks`라는 기능이 도입되면서 해결되었습니다.**
  완전히 클래스형 컴포넌트와 똑같이 사용할 수 있는 것은 아니지만 조금 다른 방식으로 비슷한 작업을 할 수 있게 되었습니다. 


<u>리액트 공식 메뉴얼에서는</u> 컴포넌트를 새로 작성할 때 `함수형 컴포넌트`와 `Hooks`를 사용하도록 권장합니다. 하지만 클래스형 컴포넌트가 없어진 것은 아니므로 클래스형 컴포넌트의 기능도 꼭 알아 두어야합니다.

------
<h2 id="New_component">컴포넌트 생성</h2>

![컴포넌트 생성 과정](/images/컴포넌트생성.png)

### src 디렉터리에 MyComponent.js 파일 생성

컴포넌트를 만들려면 **컴포넌트 코드를 선언할 파일을 만들고 내부에 컴포넌트 코드를 선언해야 합니다**.

파일 목록 중 src 디렉터리 내부에 새 파일을 만들고 파일 이름을 MyComponent.js라고 입력합니다.

------
### 코드 작성하기 

MyComponent.js 파일을 열고 새 컴포넌트의 코드를 작성합니다.

```jsx MyComponent.js
import React from 'react';

const MyComponent = () => {
  return <div>새 컴포넌트</div>
};

export default MyComponent;
```

------
### 모듈 내보내기(export) 및 불러오기(import)

* 모듈 내보내기(export)
```jsx 내보내기 (export)
export default MyComponent;
```

위 코드가 다른파일에서 이 파일을 불러오기(`import`)할 때, 선언한 `MyComponent` 클래스를 불러오도록 설정해줍니다.

* 모듈 불러오기(import)
```jsx 불러오기(import)
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return <MyComponent />;
};

export default App;
```

다른 컴포넌트에서 `MyComponent` 컴포넌트를 불러와서 사용하려면 위와 같이 두 번째줄에 `import` 구문 처럼 사용합니다.

------
<h2 id="props">props</h2>

`props`는 `properties`를 줄인 표현으로 **컴포넌트 속성을 설정할 때 사용하는 요소입니다.**

`props 값`은 해당 컴포넌트를 불러와 사용하는 부모 컴포넌트(App 컴포넌트에서 MyComponent 컴포넌트를 불러와 사용한다면 App 컴포넌트가 부모 컴포넌트입니다.)에서 설정할 수 있습니다.

-----
### JSX 내부에서 props 렌더링

props 값은 컴포넌트 함수의 파라미터로 받아와 사용할 수 있습니다.

props를 렌더링할 때 JSX 내부에서 중괄호{}로 감싸 주면 됩니다.

```jsx MyComponent.js
import React from 'react';

const MyComponent = props => {
  return <div>제 이름은 {props.name}입니다.</div>
};

export default MyComponent;
```

------
### 컴포넌트를 사용할 때 props 값 지정하기

App 컴포넌트(부모 컴포넌트)에서 MyComponent의 props 값을 지정해 보겠습니다.

```jsx App.js
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return <MyComponent name="React" />;
};

export default App;
```

코드를 저장하고 브라우저를 확인해 보면 "제 이름은 React입니다."가 출력됩니다.

------
### props 기본값 설정: defaultProps

설정한 name 값을 지우고 다시 실행하면

```jsx
return <MyComponent name="React" />;
// name 값 삭제
return <MyComponent />;
```

브라우저에 "제 이름은 입니다."가 출력될 것입니다.

`defaultProps`는 이 처럼 props 값을 따로 지정하지 않았을 때 보여줄 기본값을 설정해줍니다.

```jsx defaultProps
import React from 'react';

const MyComponent = props => {
  return <div>제 이름은 {props.name}입니다.</div>
};

MyComponent.defaultProps = {
  name: "기본 이름"
}

export default MyComponent;
```

이 처럼 `MyComponent.defaultProps`에 `name` 값으로 "기본 이름"을 지정해주면
props 값이 지정되있지 않을 때 "제 이름은 기본이름 입니다"라고 출력됩니다.

------
### 태그 사이의 내용을 보여 주는 children

리액트 컴포넌트를 사용할 때 컴포넌트 태그 사이의 내용을 보여 주는 props가 바로 `children`입니다.

```jsx App.js
import React from "react";
import MyComponent from "./MyComponent";

const App = () => {
  return <MyComponent>리액트</MyComponent>;
};

export default App;
```

위 코드에서 MyComponent 태그 사이에 작성한 "리액트"라는 문자열을 MyComponent 내부에서 보여 주려면 `props.children` 값을 보여 주어야 합니다.

MyComponent.js 파일을 다음과 같이 수정합니다.
```jsx MyComponent.js
import React from "react";

const MyComponent = (props) => {
  return (
    <div>
      제 이름은 {props.name}입니다. <br />
      children 값은 {props.children}입니다.
    </div>
  );
};

MyComponent.defaultProps = {
  name: "기본 이름",
};

export default MyComponent;
```

* 브라우저에 결과물로 다음과 같이 나타납니다.
![defaultProps, props.children](/images/props_children.png)

------
### 비구조화 할당 문법을 통해 props 내부 값 추출

현재 MyComponent.js에서 props 값을 조회할 때 마다 props.name, props.children 형태로 `props.` 키워드를 앞에 붙여 주고 있습니다. 

이러한 작업을 더 편하기 위해 **ES6의 비구조화 할당 문법을 사용하여 내부 값을 바로 추출하는 방법**이 있습니다.

```jsx MyComponent.js 수정
import React from "react";

const MyComponent = (props) => {
  const { name, children } = props;
  return (
    <div>
      제 이름은 {name}입니다. <br />
      children 값은 {children}입니다.
    </div>
  );
};

MyComponent.defaultProps = {
  name: "기본 이름",
};

export default MyComponent;
```

이렇게 코드를 작성하면 name, children 값을 더 짧은 코드로 작성할 수 있습니다.

이렇게 사용하는 방법을(객체에서 값을 추출하는 문법) 비구조화 할당(destructuring assignment)라고 부릅니다.

* 비구조화 할당 문법은 **함수의 파라미터 부분에서도 사용할 수 있습니다.**
 만약 함수의 파라미터가 객체라면 그 값을 바로 비구조화해서 사용하는 것입니다.

```jsx 함수의 파라미터에서 사용
import React from "react";

const MyComponent = ({ name, children }) => {
  return (
    <div>
      제 이름은 {name}입니다. <br />
      children 값은 {children}입니다.
    </div>
  );
};

MyComponent.defaultProps = {
  name: "기본 이름",
};

export default MyComponent;
```

------
### propTypes를 통한 props 검증

* `propTypes`:
 컴포넌트의 필수 props를 지정하거나 props의 타입(type)을 지정할 때 사용


* propTypes를 사용하려면 코드 상단에 import 구문을 사용하여 불러와야 합니다.

```jsx propTypes 사용하여 type 값 지정
import React from "react";
import PropTypes from "prop-types";

const MyComponent = ({ name, children }) => {
  return (
    <div>
      제 이름은 {name}입니다. <br />
      children 값은 {children}입니다.
    </div>
  );
};

MyComponent.defaultProps = {
  name: "기본 이름",
};

MyComponent.propTypes = {
  name: PropTypes.string
};

export default MyComponent;
```

import 구문으로 propTypes를 불러와 `MyComponent.propTypes`를 위와 같이 설정해 주면
name 값은 무조건 문자열(string) 형태로 전달해야 된다는 것을 의미합니다.

App 컴포넌트 (부모 컴포넌트)에서 name 값을 문자열이 아닌 숫자로 전달하면 값은 표시되지만 콘솔창에 경고 메세지가 출력되며 개발자에게 propTypes이 잘못되었다는 것을 알려줍니다.

------
#### isRequired를 사용하여 필수 propTypes 설정

isRequired를 사용하여 propTypes를 지정하지 않았을 때 경고 메세지를 띄워 주는 작업을 해봅니다.

* propTypes를 지정할 때 뒤에 isRequired를 붙여 사용하면 됩니다.

```jsx isRequired 사용
import React from "react";
import PropTypes from "prop-types";

const MyComponent = ({ name, children, favoriteNumber }) => {
  return (
    <div>
      제 이름은 {name}입니다. <br />
      children 값은 {children}입니다. <br />
      제가 좋아하는 숫자는 {favoriteNumber}입니다.
    </div>
  );
};

MyComponent.defaultProps = {
  name: "기본 이름",
};

MyComponent.propTypes = {
  name: PropTypes.string,
  favoriteNumber: PropTypes.number.isRequired,
};

export default MyComponent;
```

favoriteNumber 값을 설정하지 않고 실행하면 에러가 발생합니다.
`Warnig Failed prop type: The prop 'favoriteNumber' is markes as requires in 'MyComponent', but its value is 'undefined'.`

MyComponent.js에게 favoriteNumber 값을 전달할 App.js에서 값을 전달해 줍니다.

```jsx App.js
(...)
const App = () => {
  return (
    <MyComponent name="React" favoriteNumber ={1}>
    리액트
    </MyComponent>
  );
};
(...)
```

* [더 많은 PropTypes 종류 참고: https://github.com/facebook/prop-types](https://github.com/facebook/prop-types)

------
### 클래스형 컴포넌트에서 props 사용하기

클래스형 컴포넌트에서 props를 사용할 때는 render 함수에서 `this.props`를 조회하면 됩니다.

defaultProps와 propTypes는 똑같은 방식으로 설정할 수 있습니다.

```jsx MyComponent 클래스형 컴포넌트로 변환
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class MyComponent extends Component {
  render() {
    const { name, favoriteNumber, children } = this.props; // 비구조화 할당
    return (
      <div>
        안녕하세요, 제 이름은 {name} 입니다. <br />
        children 값은 {children}입니다. <br />
        제가 좋아하는 숫자는 {favoriteNumber}입니다.
      </div>
    );
  }
}

MyComponent.defaultProps = {
  name: '기본 이름'
};

MyComponent.propTypes = {
  name: PropTypes.string,
  favoriteNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

**defaultProps와 propTypes을 설정할 때 class 내부에서 지정하는 방법**도 있습니다.

```jsx defaultProps, propTypes class 내부에서 지정
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class MyComponent extends Component {
  static defaultProps = { // class 내부에서 지정
    name: '기본 이름'
  };
  static propTypes = {
    name: PropTypes.string,
    favoriteNumber: PropTypes.number.isRequired
  };
  render() {
    const { name, favoriteNumber, children } = this.props; // 비구조화 할당
    return (
      <div>
        안녕하세요, 제 이름은 {name} 입니다. <br />
        children 값은 {children}입니다. <br />
        제가 좋아하는 숫자는 {favoriteNumber}입니다.
      </div>
    );
  }
}

export default MyComponent;
```

------
<h2 id="state">state</h2>

리액트에서 state는 **컴포넌트 내부에서 바뀔 수 있는 값을 의미합니다.**

* 참고:
  * props는 컴포넌트가 사용되는 과정에서 부모 컴포넌트가 설정하는 값입니다.
  * 컴포넌트 자신은 props를 읽기 전용으로만 사용할 수 있습니다.
  * props를 바꾸려면 부모 컴포넌트에서 바꾸어 주어야 합니다.
  (예를 들어 App 컴포넌트에서 MyComponent를 사용할 때 props를 바꾸어 주어야 값이 변경될 수 있습니다. MyComponent에서는 전달받은 값을 직접 바꿀 수 없습니다.)

**리액트에는 두 가지 종류의 state가 있습니다.**

  * <u>클래스형 컴포넌트가 지니고 있는 state</u>
  * <u>함수형 컴포넌트에서 useState 함수를 통해 사용하는 state</u>

------
### 클래스형 컴포넌트의 state

```jsx Counter 컴포넌트 만들어 보기 예제
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) { // 클래스형 컴포넌트에서 constructor를 작성할 때는 반드시
    super(props); // super(props)를 호출해 줘야함.
    // state 초기값 설정
    this.state = { // 컴포넌트의 state는 객체 형식이어야함.
      number = 0
    };
  }
  render() {
    const { number } = this.state; // state 를 조회 할 때에는 this.state 로 조회합니다.
    return (
      <div>
        <h1>{number}</h1>
        <button 
          // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
          onClick={() => {
          // this.setState를 사용하여 state에 새로운 값을 넣을 수 있습니다.
          this.setState({ number: number + 1 });
        }}
        >
          +1
        </button>
      </div>
    );
  }
}

export default Counter;
```

* 컴포넌트에 state를 설정할 때는 `constructor`(컴포넌트의 생성자 메서드) 메서드를 작성하여 설정합니다.
  * **클래스형 컴포넌트에서 `constructor`를 작성할 때는 반드시 `super(props)`를 호출해 주어야 합니다.**
  
  * **`super(props)`함수가 호출되면 현재 클래스형 컴포넌트가 상속받고 있는 리액트의 `Component 클래스`가 지닌 생성자 함수를 호출해 줍니다.**
  
  * 그 다음에 this.state 값에 초기값을 설정해 줬습니다.
   **컴포넌트의 state는 객체 형식이어야 합니다.**

* render() 함수에서 현재 state를 조회할 때는 `this.state`를 조회하면 됩니다.

* button 안에 props로 넣어준 onClick 값은 버튼이 클릭될 때 호출시킬 함수를 설정할 수 있게 해줍니다. 이를 "이벤트를 설정한다"라고 합니다.

* 이벤트로 설정할 함수를 넣어줄 때는 화살표 함수를 사용해 넣어줍니다.

* 함수 내부에 사용된 `this.setState` 함수가 `state`값을 바꿀 수 있게 해 줍니다.

작성된 Counter 컴포넌트를 App에서 불러와 렌더링합니다.

```jsx App.js
import React from 'react';
import Counter from './Counter';

const App = () => {
  return <Counter />;
};

export default App;
```

------
#### state를 constructor에서 꺼내기

`constructor`메서드를 선언하지 않고도 state 초깃값을 설정할 수 있는 방법이 있습니다.

```jsx constructor 메서드 사용 x
import React, { Component } from 'react';

//constructor 선언 안하고 사용
class Counter extends Component {
  state = {
    number: 0,
    fixedNumber: 0,
  };
(...)
// constructor 메서드 사용시에는
class Counter extends Component {
  constructor(props) { 
    super(props); 
    this.state = {
      number = 0
    };
  }
(...)
```

이렇게 사용하면 constructor 메서드를 선언하지 않고도 state 초깃값을 설정할 수 있습니다.

------
#### this.setState에 객체 대신 함수 인자 전달

this.setState를 사용하여 state 값을 업데이트할 때는 <u>상태가 비동기적으로 업데이트됩니다.</u>

```jsx onClick 내부에서 this.setState 두 번 호출
onClick={() => {
  this.setState({ number: number + 1  });
  this.setState({ number: this.state.number + 1  });
}}
```
이 처럼 작성하면 `this.setState`를 두 번 사용했음에도 버튼을 클릭할 때 숫자가 1씩 더해집니다.

`this.setState`를 사용할 때 객체 대신에 함수를 인자로 넣어주어야 합니다.

```jsx this.setState의 인자로 함수를 넣어주는 형태
this.setState((prevState, props) => {
  return {
    // 업데이트할 내용
  }
})
```

`prevState`는 기존 상태를 의미하고, `props`는 현재 지니고 있는 props를 가리킵니다. (업데이트하는 과정에서 props가 필요하지 않다면 생략할 수 있습니다.)

코드를 수정하여 작성하면 다음과 같이 됩니다.

```jsx
(...)
onClick={() => {
  this.setState(prevState => {
    return {
      number: prevState.number + 1
    };
  });

  this.setState(prevState => ({
    number: prevState.number + 1
  // 위 코드와 아래 코드는 완전히 똑같은 기능을 하는 코드입니다.
  // 아래 코드는 함수에서 바로 객체를 반환한다는 의미입니다.
  }));
(...)
```

버튼을 클릭하면 이제 값이 2씩 올라갑니다.

------
#### this.setState가 끝난 후 특정 작업

setState를 사용하여 값을 업데이트하고 난 다음에 특정 작업을 하고 싶을 때

**setState의 두 번째 파라미터로 콜백(callback)함수를 등록하여 처리할 수 있습니다.**

```jsx 예시
onClick={() =>{
  this.setState(
      {
        number: number + 1,
      },
      // 두 번째 파라미터로 콜백 함수 등록
      () => {
        console.log('방금 setState 가 호출되었습니다.');
        console.log(this.state);
      }
    );
  }}
```

------
### 함수형 컴포넌트에서 useState 사용

리액트 16.8 이전 버전에서는 함수형 컴포넌트에서 state를 사용할 수 없었습니다. 하지만 16.8이후부터는 useState라는 함수를 사용하여 함수형 컴포넌트에서도 state를 사용할 수 있게 되었습니다.

이 과정에서 `Hooks`를 사용하게 되는데 `Hooks`의 종류는 다양하지만 여기서는 `useState`를 사용합니다.

------
### useState 사용하기

```jsx Say.js
import React, { useState } from 'react';

const Say = () => {
  const [message, setMessage] = useState('');
  const onClickEnter = () => setMessage('안녕하세요!');
  const onClickLeave = () => setMessage('안녕히 가세요!');

  return (
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClick={onClickLeave}>퇴장</button>
      <h1>{message}</h1>
    </div>
  );
};

export default Say;
```

1. `useState` 함수의 인자에는 상태의 초깃값을 넣어줍니다.('')

  * **클래스형 컴포넌트에서 state 초깃값은 객체 형태로 넣어줘야합니다.**

  * **useState 에서는 객체가 아니어도 상관없습니다. 값 형태가 자유입니다.**

2. useState 함수 호출시 배열이 반환됩니다. 배열의 첫 번째 요소는 현재 상태이고, 두 번째 요소는 상태를 바꿔주는 함수입니다. **이 함수를 세터(Setter)함수라고 부릅니다.**

```jsx App.js
import React from 'react';
import Say from './Say';

const App = () => {
  return <Say />;
};

export default App;
```

![](/images/useState0.png) ![](/images/useState1.png)

클릭한 입장 버튼과 퇴장버튼에 따라 문구가 변하게 됩니다.

------
<h2 id="state_주의사항">state 사용시 주의 사항</h2>

클래스형 컴포넌트든 함수형 컴포넌트든 state를 사용할 때는 주의해야 할 사항이 있습니다.

**state 값을 바꿔야 할 때는 setState 혹은 useState를 통해 전달받은 세터 함수를 사용해야 합니다.**

* 배열이나 객체를 업데이트 할 때는
  1. 사본을 생성하고 
  2. 사본에 값을 업데이트한 후
  3. 그 사본의 상태를 setState 혹은 세터 함수를 통해 업데이트합니다.

```jsx 사본을 만들어 업데이트하는 예시
// 객체
const object = { a: 1, b: 2, c: 3};
const nextObject = {...object, b : 2}; // 사본을 만들어 b 값만 덮어씀

//배열
const array = {
  { id:1, value: true},
  { id:2, value: true},
  { id:3, value: false}
};

let nextArray = array.concat({ id: 4 }); // 새 항목 추가
nextArray.filter(item => item.id !==2); // id가 2인 항목 제거
nextArray.map(item => (item.id === 1 ? {...item, value: false } : item)); 
//id가 1인 항목의 value를 false로 설정
```

* 객체의 사본을 만들 때는 `spread` 연산자를 사용하여 처리합니다.


* 배열의 사본을 만들 때는 배열의 내장 함수들을 활용합니다.