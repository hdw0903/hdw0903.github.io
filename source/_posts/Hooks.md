---
title: Hooks
tags:
  - Hooks
  - useState
  - useEffect
  - useReducer
  - useMemo
  - useCallback
  - useRef
  - HTMLElement.focus

disqusId: tunas-blog-1
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

date: 2020-06-03 18:23:21
categories: React
---

Hooks는 리액트 v16.8에 새로 도입된 기능으로 **함수형 컴포넌트에서도 상태 관리**를 할 수 있는 useState, useEffect등의 기능을 제공합니다.

* [useState](/2020/06/03/Hooks/#useState)
* [useEffect](/2020/06/03/Hooks/#useEffect)
* [useReducer](/2020/06/03/Hooks/#useReducer)
* [useMemo](/2020/06/03/Hooks/#useMemo)
* [useCallback](/2020/06/03/Hooks/#useCallback)
* [useRef](/2020/06/03/Hooks/#useRef)
* [커스텀 Hooks 만들기](/2020/06/03/Hooks/#커스텀_Hooks)
* [다른 개발자가 만든 Hooks](/2020/06/03/Hooks/#Hooks_Link)

<!-- more -->

------
<h2 id="useState">useState</h2>

useState는 가장 기본적인 Hooks이며, 함수형 컴포넌트에서도 가변적인 상태를 지닐 수 있게 해줍니다.

useState 기능을 사용하여 숫자 카운터를 구현해 봅니다.

```jsx Counter.js
import React, { useState } from 'react';

const Counter = () => {
  const [value, setValue] = useState(0);

  return (
    <div>
      <p>
        현재 카운터 값은 <b>{value}</b>입니다.
      </p>
      <button onClick={() => setValue(value + 1)}>+1</button>
      <button onClick={() => setValue(value - 1)}>-1</button>
    </div>
  );
};

export default Counter;
```

useState는 코드 상단에서 import 구문으로 불러오고 다음과 같이 사용합니다.
```jsx useState
const [value, setValue] = useState(0);
```


* useState 함수의 파라미터에는 상태의 기본값을 넣어 줍니다. (카운터의 기본값을 0으로 잡음)


* **useState 함수가 호출되면 배열을 반환하고, 배열의 첫 번째 요소는 상태값, 두 번째 요소는 상태를 설정하는 함수입니다.**


* useState() 함수를 호출하면 파라미터로 넘긴 값으로 값이 바뀌고 컴포넌트가 리렌더링됩니다.

------
### useState 여러 상태 값 관리

하나의 useState 함수는 하나의 상태 값만 관리할 수 있습니다.

컴포넌트에서 상태를 관리해야할 대상이 여러 개라면 useState를 여러번 사용해야 합니다.

```jsx info.js
import React, { useState } from 'react';

const Info = () => {
  const [name, setName] = useState('');
  const [nickname, setNickname] = useState('');

  const onChangeName = (e) => {
    setName(e.target.value);
  };
  const onChangeNickname = (e) => {
    setNickname(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName}></input>
        <input value={nickname} onChange={onChangeNickname}></input>
      </div>
      <div>
        <div>
          <b>이름:</b>
          {name}
        </div>
        <div>
          <b>닉네임:</b>
          {nickname}
        </div>
      </div>
    </div>
  );
};

export default Info;
```

관리할 상태가 2개인 (이름, 닉네임) 예시입니다. useState도 마찬가지로 2개를 사용하여 관리해줬습니다.

------
<h2 id="useEffect">useEffect</h2>

<mark>useEffect는 리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정해 주는 Hooks 입니다.</mark>

클래스형 컴포넌트의 `componentDidMount` 와 `componentDidUpdate`를 합친 형태와 비슷합니다.

위에 만든 Info 컴포넌트에 useEffect를 적용해 봅니다.

```jsx Info.js - useEffect
import React, { useState, useEffect } from 'react';

const Info = () => {
  const [name, setName] = useState('');
  const [nickname, setNickname] = useState('');
  //useEffect
  useEffect(() => {
    console.log('렌더링이 완료되었습니다!');
    console.log({ name, nickname });
  });
  const onChangeName = (e) => {
    setName(e.target.value);
  };
  const onChangeNickname = (e) => {
    setNickname(e.target.value);
  };

  return (
    <div>
      <div>
        <input value={name} onChange={onChangeName}></input>
        <input value={nickname} onChange={onChangeNickname}></input>
      </div>
      <div>
        <div>
          <b>이름:</b>
          {name}
        </div>
        <div>
          <b>닉네임:</b>
          {nickname}
        </div>
      </div>
    </div>
  );
};

export default Info;
```

컴포넌트가 렌더링될 때마다 `'렌더링이 완료되었습니다!'` 문구와 `name`, `nickname` 값을 출력합니다.
개발자도구의 콘솔창에서 확인할 수 있습니다.

------
### 마운트될 때만 실행하고 싶을 때

**useEffect에서 설정한 함수를 컴포넌트가 화면에 맨 처음 렌더링될 때만 실행**하고, 

업데이트 때는 실행하고 싶지 않다면 

**함수의 두 번째 파라미터로 빈 배열 []을 넣어 주면 됩니다.**

```jsx info.js - useEffect 마운트될 때만 실행
useEffect (() => {
  console.log('마운트될 때만 실행됩니다.')
}, [] ); // 두 번째 파라미터에 []
```

위와 같이 수정해주면 컴포넌트가 맨 처음 렌더링될 때만 콘솔에 `'마운트될 때만 실행됩니다'` 문구가 출력되고 그 이후에는 나타나지 않습니다.

------
### 특정 값이 변경될 때만 실행

특정 값이 업데이트될 때만 실행하고 싶은 경우

클래스형 컴포넌트라면 다음과 같이 작성할 것입니다.

```jsx 클래스형 컴포넌트 componentDidUpdate
componentDidUpdate (prevProps, prevState) {
  if (prevProps.value !== this.props.value) {
    function();
  }
}
```
props의 value 값이 변할 때 마다 특정 함수를 실행하는 클래스형 컴포넌트에서의 예시입니다.

**useEffect를 사용하여 작성하려면 두 번째 파라미터에 검사하고 싶은 값을 []로 감싸 넣어 주면 됩니다.**

```jsx info - useEffect
//두 번째 파라미터에 검사하고 싶은 값을 작성
useEffect(() => {
  console.log(name);
}, [name] );
```

이렇게 작성하면 name 값을 검사하여 name 값이 변경되지 않으면 실행되지 않습니다.

배열 안에는 useState로 관리하고 있는 상태를 넣어줘도 되고, props로 전달받은 값을 넣어줘도 됩니다.


<mark>useEffect는 기본적으로 렌더링이되고 난 직후마다 실행되며, 두 번째 파라미터 배열에 무엇을 넣는지에 따라 실행되는 조건이 달라집니다.</mark>

------
### 뒷정리 함수

컴포넌트가 언마운트되기 전이나 업데이트되기 직전에 어떠한 작업을 수행하고 싶다면 useEffect에서 뒷정리(cleanup)함수를 반환해 줘야 합니다.

* App 컴포넌트에서 Info 컴포넌트의 가시성을 바꿀수 있게 하는 예제
```jsx info.js - useEffect
useEffect(() => {
    console.log('effect');
    console.log(name);
    return () => {
      console.log('cleanup');
      console.log(name);
    };
  });
```

Info 컴포넌트의 useEffect 부분을 위와 같이 수정하고

이제 App 컴포넌트에서 Info 컴포넌트의 가시성을 바꿀 수 있게 해봅니다.

```jsx App.js
App.js
import React, { useState } from 'react';
import Info from './info';

const App = () => {
  const [visible, setVisible] = useState(false);
  return (
    <div>
      <button
        onClick={() => {
          setVisible(!visible);
        }}
      >
        {visible ? '숨기기' : '보이기'}
      </button>
      <hr />
      {visible && <Info />}
    </div>
  );
};
export default App;
```

브라우저에서 보이기/숨기기 버튼을 눌러보면

컴포넌트가 나타날 때 useEffect 함수가 실행되어 콘솔에 effect가 출력되고, 사라질 때 뒷정리 함수가 호출되어 cleanup이 출력됩니다.

인풋에 작성해보면 콘솔에 렌더링될 때마다 뒷정리 함수가 계속 나타나는 것을 확인할 수 있습니다.

**뒷정리 함수가 호출될 때는 업데이트 직전의 값을 보여 줍니다.**


------
<h2 id="useReducer">useReducer</h2>

`useReducer`는 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트 해줄 때 사용하는 Hook입니다.

리듀서는 현재 상태, 업데이트를 위해 필요한 정보를 담은 액션(action) 값을 전달받아 새로운 상태를 반환하는 함수입니다.

<mark>리듀서 함수에서 새로운 상태를 만들 때는 반드시 불변성을 지켜줘야합니다.</mark>

```jsx 리듀서
function reducer(state, action) {
  return {...} // 불변성을 지키면서 업데이트한 새로운 상태를 반환
}
// 액션 값은 주로 다음과 같은 형태로 이루어져 있습니다.
{
  type: 'INCREMENT'
}
```

`리덕스`에서 사용하는 액션 객체는 어떤 액션인지 알려주는 type 필드가 꼭 필요하지만,
`useReducer`에서 사용하는 액션 객체는 객체가 아니라 문자열, 숫자여도 상관없고 반드시 type을 지니고 있을 필요도 없습니다.

------
### 카운터 구현

useReducer를 사용하여 Counter 컴포넌트를 구현해봅니다.

```jsx Counter.js -useReducer
import React, { useReducer } from 'react';

function reducer(state, action) {
  //action.type에 따라 다른 작업 수행
  switch (action.type) {
    case 'INCREMENT':
      return { value: state.value + 1 };
    case 'DECREMENT':
      return { value: state.value - 1 };
    default:
      return state;
  }
}

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 });
  return (
    <div>
      <p>
        현재 카운터 값은 <b>{state.value}</b>입니다.
      </p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+1</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-1</button>
    </div>
  );
};

export default Counter;
```

useReducer 함수의 첫 번째 파라마터에는 리듀서 함수를 넣고, 두 번째 파라미터에는 해당 리듀서의 기본값을 넣어줍니다.

이 useReducer Hook을 사용하면 `state` 값과 `dispatch` 함수를 받아옵니다.

state는 현재 가리키는 상태이고, dispatch는 액션을 발생시키는 함수입니다.
dispatch(action)과 같은 형태로, 함수 안에 파라미터로 액션 값을 넣어 주면 리듀서 함수가 호출되는 구조입니다.

useReducer를 사용했을 때 가장 큰 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 점입니다.

App 컴포넌트에서 Counter를 렌더링해주고 브라우저에서 실행시 카운터가 잘 작동합니다.

------
### input 상태 관리

input이 여러 개 일때 상태 관리하려면 useState를 여러 개 사용해 줬었습니다.

이번에는 useReducer를 사용하여 Info 컴포넌트에서 input 상태 관리를 해봅니다.

useReducer를 사용하면 클래스형 컴포넌트에서 input 태그에 name 값을 할당하고 e.target.name을 참조하여 setState 해 준 것과 유사한 방식으로 처리할 수 있습니다.

```jsx Info.js -useReducer
import React, { useReducer } from 'react';

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

const Info = () => {
  const [state, dispatch] = useReducer(reducer, { name: '', nickname: '' });
  const { name, nickname } = state;
  const onChange = (e) => {
    dispatch(e.target);
  };
  return (
    <div>
      <div>
        <input name="name" value={name} onChange={onChange}></input>
        <input name="nickname" value={nickname} onChange={onChange}></input>
      </div>
      <div>
        <div>
          <b>이름:</b>
          {name}
        </div>
        <div>
          <b>닉네임:</b>
          {nickname}
        </div>
      </div>
    </div>
  );
};

export default Info;
```

useReducer에서의 액션은 그 어떤 값도 사용 가능합니다. (이번에는 이벤트 객체가 가지고 있는 e.target 값 자체가 액션 값으로 사용되었습니다.)

이런 식으로 input을 관리하면 input 개수가 많아져도 코드를 짧고 깔끔하게 유지할 수 있습니다.

------
<h2 id="useMemo">useMemo</h2>

useMemo를 사용하면 함수형 컴포넌트에서 **연산을 최적화**할 수 있습니다.

우선 숫자를 추가하고 추가된 숫자들의 평균값을 구해주는 함수형 컴포넌트 예제를 작성해봅니다.

```jsx Average.js
import React, { useState } from 'react';

const getAverage = (number) => {
  console.log('평균값 계산중..');
  if (number.length === 0) return 0;
  const sum = number.reduce((a, b) => a + b);
  return sum / number.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  const onChange = (e) => {
    setNumber(e.target.value);
  };
  const onInsert = (e) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
  };
  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b>
        {getAverage(list)}
      </div>
    </div>
  );
};

export default Average;
```

브라우저에서 실행해보면 숫자를 등록할 때뿐만 아니라 input 내용이 수정될 때도 getAverage 함수가 호출되는 것을 확인할 수 있습니다.

`useMemo` 를 사용하면 렌더링 과정에서 특정 값이 바뀌었을 때만 연산을 실행하고, 원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용하는 방식으로 최적화할 수 있습니다. 

```jsx Average -useMemo
import React, { useState, useMemo } from 'react';

const getAverage = (number) => {
  console.log('평균값 계산중..');
  if (number.length === 0) return 0;
  const sum = number.reduce((a, b) => a + b);
  return sum / number.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  const onChange = (e) => {
    setNumber(e.target.value);
  };
  const onInsert = (e) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
  };
  // useMemo 사용
  const avg = useMemo(() => getAverage(list), [list]);
  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b>
        {avg}
      </div>
    </div>
  );
};

export default Average;
```

useMemo 함수의 첫 번째 인자로 getAverage 함수를 넘기고, 두 번째 인자로 list prop을 사용하여 달라질 경우에만 호출하고 동일하다면 이전 결과값을 사용하게 합니다.

이제 list 배열의 내용이 바뀔 때만 getAverage 함수가 호출됩니다.

### useMemo 특징

  * <mark>useMemo가 적용된 레퍼런스는 재활용을 위해 GC에서 제외됩니다.</mark>
     즉, 재사용을 통해 중복 연산을 줄여 성능 최적화에 많이 사용되지만, 메모리를 더 잡아먹는 점도 있습니다.
  

  * 수 초 이상 걸리는 로직이 프론트엔드에 흔하지 않으므로 useMemo 사용할 일이 잘 없습니다.
    있다해도 useEffect 등으로 비동기 처리하는 방안을 먼저 고려하게 됩니다.

------
<h2 id="useCallback">useCallback</h2>

useCallback은 useMemo와 상당히 비슷한 함수입니다.

주로 렌더링 성능을 최적화해야 하는 상황에 사용합니다. (ex 이벤트 핸들러 함수를 필요할 때만 생성)

Average 컴포넌트를 살펴보면 작성된 onChange와 onInsert라는 함수는 **컴포넌트가 리렌더링될 때마다 이 함수들이 새로 생성됩니다.**

컴포넌트의 렌더링이 자주 발생하거나 렌더링해야 할 컴포넌트의 개수가 많아지면 이부분을 최적화해 주는 것이 좋습니다.

```jsx useCallback
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = (number) => {
  console.log('평균값 계산중..');
  if (number.length === 0) return 0;
  const sum = number.reduce((a, b) => a + b);
  return sum / number.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  const onChange = useCallback((e) => {
    setNumber(e.target.value);
  }, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성
  const onInsert = useCallback(
    (e) => {
      const nextList = list.concat(parseInt(number));
      setList(nextList);
      setNumber('');
    },
    [number, list]
  ); // number 혹은 list가 바뀌었을 때만 함수 생성
  // useMemo 사용
  const avg = useMemo(() => getAverage(list), [list]);
  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b>
        {avg}
      </div>
    </div>
  );
};

export default Average;
```

`useCallback`의 첫 번째 파라미터에는 <u>생성하고 싶은 함수</u>를 작성하고, 두 번째 파라미터에는 배열을 작성합니다. **이 배열 값에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야하는지 명시합니다.**

onChange 처럼 빈 배열 []을 넣게 되면 컴포넌트가 렌더링될 때 단 한번만 함수가 생성되며,
onInsert 처럼 배열 안에 number 와 list를 넣게 되면 input내용이 바뀌거나 새로운 항목이 추가될 때마다 함수가 생성됩니다.

------
### useCallback 과 useMemo

다음의 useCallback 과 useMemo는 완전히 같은 코드입니다.

```jsx useCallback 과 useMemo
useCallback(() => {
  console.log('hello world!');
}, [])

useMemo(() => {
  const fn = () => {
    console.log('hello world!');
  };
  return fn;
}, [])
```

useCallback은 결국 useMemo로 함수를 반환하는 상황에서 더 간단히 사용할 수 있는 Hook입니다.

숫자, 문자열, 객체 등 일반 값을 재사용하려면 useMemo를
함수를 재활용 하려면 useCallback을 사용합니다.

------
<h2 id="useRef">useRef</h2>

**useRef Hook은 함수형 컴포넌트에서 ref을 쉽게 사용할 수 있게 해 줍니다.**

Average 컴포넌트에서 등록 버튼을 눌렀을 때 포커스가 input 쪽으로 넘어가도록 코드를 작성해 봅니다.

```jsx Average.js
import React, { useState, useMemo, useCallback, useRef } from 'react';

const getAverage = (number) => {
  console.log('평균값 계산중..');
  if (number.length === 0) return 0;
  const sum = number.reduce((a, b) => a + b);
  return sum / number.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');
  //useRef 사용
  const inputEl = useRef(null);

  const onChange = useCallback((e) => {
    setNumber(e.target.value);
  }, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성
  const onInsert = useCallback(
    (e) => {
      const nextList = list.concat(parseInt(number));
      setList(nextList);
      setNumber('');
      // 실제 엘리먼트 input을 가리킴
      inputEl.current.focus();
    },
    [number, list]
  ); // number 혹은 list가 바뀌었을 때만 함수 생성
  // useMemo 사용
  const avg = useMemo(() => getAverage(list), [list]);
  return (
    <div>
      <input value={number} onChange={onChange} ref={inputEl} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b>
        {avg}
      </div>
    </div>
  );
};

export default Average;
```

useRef을 사용하여 ref을 설정하면 useRef를 통해 만든 객체(inputEl) 안의 current 값이 실제 엘리먼트(input)를 가리킵니다.

> HTMLElement.focus(option) 

HTML 요소에 포커스를 줌

* option 
  preventScroll : focus를 맞춘 후 요소를 보기위해 브라우저가 스크롤 해야하는 지에 대한 boolean 값
  기본값은 false, false는 focus를 맞춘 후 브라우저가 요소를 보기위해 스크롤 함을 의미

------
### 로컬 변수 사용하기

컴포넌트 로컬 변수를 사용해야 할 때도 useRef를 활용할 수 있습니다.

여기서 로컬 변수란 렌더링과 상관없이 바뀔 수 있는 값을 의미합니다.

클래스형 컴포넌트로 작성된 로컬 변수 사용해야 할 때

```jsx 클래스형
import React, { Component } from 'react';

class MyComponent extends Component {
  id = 1
  setID = (n) => {
    this.id = n
  }
  printId = () => {
    console.log(this.id);
  }
  render() {
    return (
      <div>
      Mycomponet
      </div>
    );
  }
}

export default MyComponent;
```

함수형 컴포넌트로 작성시

```jsx 함수형 컴포넌트
import React, { useRef } from 'react';

const RefSample = () => {
  const id = useRef(1);
  const setId = (n) => {
    console.log(id.current);
  }
  return (
    <div>
    RefSample
    </div>
  );
}
```

이렇게 ref 안의 값이 바뀌어도 컴포넌트가 렌더링되지 않는다는 점을 주의하여 렌더링과 관련되지 않은 값을 관리할 때만 이러한 방식으로 코드를 작성합니다. 

------
<h2 id="커스텀_Hooks">커스텀 Hooks 만들기</h2>

여러 컴포넌트에서 비슷한 기능을 공유할 경우, 커스텀 Hook으로 작성하여 로직을 재사용할 수 있습니다.

useReducer로 작성했던 로직을 useInputs라는 Hook으로 따로 분리해 보겠습니다.

```jsx useInputs.js
import { useReducer } from 'react';

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value
  };
}

export default function useInputs(initialForm) {
  const [state, dispatch] = useReducer(reducer, initialForm);
  const onChange = e => {
    dispatch(e.target);
  };
  return [state, onChange];
}
```

이 Hook을 info 컴포넌트에서 사용해 보겠습니다.

```jsx info.js
import React from 'react';
import useInputs from './useInputs';

const Info = () => {
  const [state, onChange] = useInputs({
    name: '',
    nickname: ''
  });
  const { name, nickname } = state;

  return (
    <div>
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="nickname" value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임: </b> {nickname}
        </div>
      </div>
    </div>
  );
};

export default Info;
```



------
<h2 id="Hooks_Link">다른 개발자가 만든 Hooks</h2>

커스텀 Hooks를 만들어서 사용했던 것처럼, 다른 개발자가 만든 Hooks도 라이브러리로 설치하여 사용할 수 있습니다.

다른 개발자가 만든 다양한 Hooks 리스트
  * https://nikgraf.github.io/react-hooks/
  * https://github.com/rehooks/awesome-react-hooks