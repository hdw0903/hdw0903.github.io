---
title: Hooks로 컨테이너 컴포넌트 만들기
tags:
  - useSelector
  - useDispatch
  - useCallback
  - useStore
  - connect
toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right
sidebar:
  right:
    sticky: true
date: 2020-07-09 23:16:30
categories: React
---

* Hooks로 컨테이너 컴포넌트 만들기
 * useSelector
 * useDispatch
 * useStore
 * connect 함수와의 주요 차이점
 <!-- more -->

------
<h2>Hooks를 사용하여 컨테이너 컴포넌트 만들기</h2>

리덕스 스토어와 연동된 컨테이너 컴포넌트를 만들 때 `connect` 함수를 사용하는 대신 react-redux에서 제공하는 Hooks를 사용할 수도 있습니다.


### useSelector

useSelector Hook을 사용하면 connect 함수를 사용하지 않고도 리덕스의 상태를 조회할 수 있습니다.

> const 결과 = useSelector(상태 선택 함수)

상태 선택 함수는 mapStateToprops와 형태가 똑같습니다.

### useDispatch

useDispatch Hook은 컴포넌트 내부에서 스토어의 내장 함수 dispatch를 사용할 수 있게 해 줍니다.
컨테이너 컴포넌트에서 액션을 디스패치할 때 이 Hook을 사용합니다.

> const dispatch = useDispatch();
dispatch({ type: 'SAMPLE_ACTION' });


```jsx CounterContainer.js
import React from 'react';
import Counter from '../components/Counter';
import { increase, decrease } from '../modules/counter';
import { useSelector, useDispatch } from 'react-redux';

// useSelector 사용
const CounterContainer = () => {
  const number = useSelector((state) => state.counter.number);
  //useDispatch 사용
  const dispatch = useDispatch();
  return (
    <Counter
      number={number}
      onIncrease={() => dispatch(increase())}
      onDecrease={() => dispatch(decrease())}
    />
  );
};
export default CounterContainer;
```

useCallback으로 컴포넌트 성능을 최적화 해준다면 다음과 같이 작성합니다.

```jsx
import React, { useCallback } from 'react';
import Counter from '../components/Counter';
import { increase, decrease } from '../modules/counter';
import { useSelector, useDispatch } from 'react-redux';

// useSelector 사용
const CounterContainer = () => {
  const number = useSelector((state) => state.counter.number);
  //useDispatch 사용
  const dispatch = useDispatch();
  //useCallback 사용
  const onIncrease = useCallback(() => dispatch(increase()), [dispatch]);
  const onDecrease = useCallback(() => dispatch(decrease()), [dispatch]);

  return (
    <Counter number={number} onIncrease={onIncrease} onDecrease={onDecrease} />
  );
};
export default CounterContainer;
```

### useStore

useStore Hook를 사용하면 컴포넌트 내부에서 리덕스 스토어 객체를 직접 사용할 수 있습니다.

> const store = useStore();
store.dispatch({ type: 'SAMPLE_ACTION'});
stroe.getState();

스토어에 직접 접근해야 하는 상황에만 사용해야 합니다.

### connect 함수와의 주요 차이점

Hooks를 사용하여 컨테이너 컴포넌트를 만들 때 와 connect 함수를 사용시 주요 차이점이 있습니다.

`connect` 함수로 컨테이너 컴포넌트를 만들면 부모 컴포넌트가 리렌더링될 때 해당 컨테이너 컴포넌트의 props가 바뀌지 않았다면 리렌더링이 자동으로 방지됩니다. 성능 최적화

`useSelector`를 사용하여 리덕스 상태를 조회했을 때는 최적화 작업이 자동으로 이루어지지 않으므로, 성능 최적화를 위해서는 `React.memo`를 컨테이너 컴포넌트에 사용해 줘야합니다.