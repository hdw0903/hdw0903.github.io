---
title: 리액트 라우터로 SPA 개발하기
tags:
  - null
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
date: 2020-06-12 20:54:50
categories: React
---

* [SPA 란?](/2020/06/12/리액트-라우터로-SPA-개발하기/#SPA)
* [SPA의 단점](/2020/06/12/리액트-라우터로-SPA-개발하기/#SPA_단점)
* [기본적인 사용법](/2020/06/12/리액트-라우터로-SPA-개발하기/#기본적인_사용법)
* [URL 파라미터와 쿼리](/2020/06/12/리액트-라우터로-SPA-개발하기/#URL)
* [서브 라우트](/2020/06/12/리액트-라우터로-SPA-개발하기/#서브_라우트)
* [리액트 라우터 부가 기능](/2020/06/12/리액트-라우터로-SPA-개발하기/#부가기능)

<!-- more -->

------
<h2 id='SPA'>SPA 란?</h2>

SPA는 Single Page Application(싱글 페이지 애플리케이션)의 약어입니다.

말 그대로 한 개의 페이지로 이루어진 애플리케이션이라는 의미입니다.

싱글 페이지라고 해서 화면이 한 종류는 아닙니다.

SPA의 경우 서버에서 사용자에게 제공하는 페이지는 한 종류이지만, 해당 페이지에서 로딩된 자바스크립트와 현재 사용자 브라우저의 주소 상태에 따라 다양한 화면을 보여 줄 수 있습니다.

<u>다른 주소에 다른 화면을 보여주는 것은 라우팅이라고 합니다.</u>

------
<h2 id='SPA_단점'>SPA의 단점</h2>

SPA의 단점은 앱의 규모가 커지면 자바스크립트 파일이 너무 커진다는 것입니다.

페이지 로딩 시 사용자가 실제로 방문하지 않을 수도 있는 페이지의 스크립트도 불러오기 때문입니다.

이러한 점은 코드 스플리팅(`code splitting`)을 사용하여 라우트별로 파일을 나누어 트래픽과 로딩 속도를 개선할 수 있습니다.

------
<h2 id='기본적인_사용법'>기본적인 사용법</h2>

------
### 라이브러리 설치 및 적용

리액트 라우터를 설치할 때는 react-router-dom 라이브러리를 설치하면 됩니다.

> npm install react-router-dom
또는
> yarn add react-router-dom

프로젝트에 리액트 라우터를 적용할 때는 src/index.js 파일에서 react-router-dom에 내장되어 있는 BrowserRouter라는 컴포넌트를 사용하여 감싸면 됩니다.

**이 컴포넌트는 웹 애플리케이션에 HTML5의 History API를 사용하여 페이지를 새로고침하지 않고도 주소를 변경하고, 현재 주소에 관련된 정보를 props로 쉽게 조회하거나 사용할 수 있도록 해 줍니다.**

```jsx index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);

serviceWorker.unregister();
```

------
### 페이지 만들기 

사용자가 웹 사이트에 들어왔을 때 맨 처음 보여 줄 Home 컴포넌트와 웹 사이트를 소개하는 About 컴포넌트를 만들어 보겠습니다.

```jsx Home.js
import React from 'react';

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>홈, 가장 먼저 보여지는 페이지.</p>
    </div>
  );
};

export default Home;
```

```jsx About.js
import React from 'react';

const About = () => {
  return (
    <div>
      <h1>소개</h1>
      <p>이 프로젝트는 리액트 라우터 기초를 실습해 보는 예제 프로젝트입니다.</p>
    </div>
  );
};

export default About;
```

------
### Route 컴포넌트로 특정 주소에 컴포넌트 연결

Route 컴포넌트를 사용하면 규칙을 가진 경로에 어떤 컴포넌트를 보여 줄지 정의할 수 있습니다.

```jsx Route 컴포넌트
<Route path="주소규칙" component={보여 줄 컴포넌트} />
```

Route 컴포넌트를 사용하여 Home 컴포넌트 혹은 About 컴포넌트를 보여 주도록 설정해봅니다.

```jsx App.js
import React from 'react';
import { Route } from 'react-router-dom';
import About from './About';
import Home from './Home';

const App = () => {
  return (
    <div>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
};

export default App;
```

이 상태에서 locallhost:3000/about 경로를 입력해 들어가보면 

About 컴포넌트만 나올 것 같지만 두 컴포넌트가 모두 나타납니다.

/about 경로가 / 규칙에도 일치하기 때문에 발생한 현상입니다.

이를 수정하려면 Home을 위한 Route 컴포넌트를 사용할 때 `exact`라는 props를 true로 설정하면 됩니다.

```jsx exact
<Route path="/" component={Home} exact={true} />
```

------
### a 태그 대신 Link 컴포넌트

Link 컴포넌트는 클릭하면 다른 주소로 이동시켜 주는 컴포넌트입니다.

일반 웹 어플리케이션에서는 a 태그를 사용하여 페이지를 전환하지만 리액트 라우터를 사용할 때는 a 태그를 직접 사용하면 안됩니다.

<mark>a 태그는 페이지를 전환하는 과정에서 페이지를 새로 불러오기 때문에 애플리케이션이 들고 있던 상태들을 모두 날려 버리게 됩니다. 렌더링된 컴포넌트들도 모두 사라지고 다시 처음부터 렌더링하게 됩니다.</mark>

Link 컴포넌트를 사용하여 페이지를 전환하면, 페이지를 새로 불러오지 않고 애플리케이션은 그대로 유지한 상태에서 HTML5 History API를 사용하여 페이지의 주소만 변경해 줍니다.

<u>Link 컴포넌트에는 페이지 전환을 방지하는 기능이 내장돠어 있습니다.</u>

다음은 App 컴포넌트에서 "/", "/about" 경로로 이동하는 Link 컴포넌트를 만든 상태입니다.

```jsx Link 컴포넌트
const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
      </ul>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
};
```

------
### Route 하나에 여러 개의 path 설정하기

path props를 배열로 설정해 주면 여러 경로에서 같은 컴포넌트를 보여 줄 수 있습니다.

```jsx App.js
<Route path={['./about', './info']} component={About} />
```

------
<h2 id='URL'>URL 파라미터와 쿼리</h2>

페이지 주소를 정의할 때 유동적인 값을 전달해야 할 때도 있습니다.
이는 파라미터와 쿼리로 나눌 수 있습니다.

* 파라미터 ex) /profiles/velopert

* 쿼리 ex) /about?defails=true

유동적인 값을 사용해야 하는 상황에서 파라미터를 써야 할지 쿼리를 써야 할지 정할 때, 무조건 따라야 하는 규칙은 없습니다.

일반적으로 파라미터는 특정 아이디 혹은 이름을 사용하여 조회할 때 사용하고,

쿼리는 키워드를 검색하거나 페이지에 필요한 옵션을 전달할 때 사용합니다.

------
### URL 파라미터

Profile 페이지에서 파라미터를 사용하여 /profile/velopert와 같은 형식으로 뒷부분에 유동적인 username 값을 넣어 줄 때 해당 값을 props로 받아 와서 조회하는 방법을 알아봅니다.

```jsx profile.js
import React from 'react';

const data = {
  velopert: {
    name: '김민준',
    description: '리액트를 좋아하는 개발자',
  },
  gildong: {
    name: '홍길동',
    description: '고전 소설 홍길동전의 주인공',
  },
};

const Profile = ({ match }) => {
  const { username } = match.params;
  const profile = data[username];
  if (!profile) {
    return <div>존재하지 않는 사용자입니다.</div>;
  }
  return (
    <div>
      <h3>
        {username}({profile.name})
      </h3>
      <p>{profile.description}</p>
    </div>
  );
};

export default Profile;
```

<mark>URL 파라미터를 사용할 때는 라우트로 사용되는 컴포넌트에서 받아 오는 match라는 객체 안의 params 값을 참조합니다. match 객체 안에는 현재 컴포넌트가 어떤 경로 규칙에 의해 보이는지에 대한 정보가 들어 있습니다.</mark>

App 컴포넌트에서 Profile 컴포넌트를 위한 라우트를 정의해봅니다.
이번 path 규칙에는 /profiles/:username이라고 넣어주어 match.params.username 값을 통해 현재 username 값을 조회할 수 있게 합니다.

```jsx App.js
import React from 'react';
import { Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';
import Profile from './Profile';

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">홈</Link>
        </li>
        <li>
          <Link to="/about">소개</Link>
        </li>
        <li>
          <Link to="/profile/velopert">velopert 프로필</Link>
        </li>
        <li>
          <Link to="/profile/gildong">gildong 프로필</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" component={Home} exact={true} />
      <Route path="/about" component={About} />
      <Route path="/profile/:username" component={Profile} />
    </div>
  );
};

export default App;
```

------
### URL 쿼리

About 페이지에서 쿼리를 받아와봅니다.

<mark>쿼리는 location 객체에 들어 있는 search 값에서 조회할 수 있습니다. location 객체는 라우트로 사용된 컴포넌트에게 props로 전달되며, 웹 애플리케이션의 현재 주소에 대한 정보를 지니고 있습니다.</mark>

location의 형태는 다음과 같습니다.

```jsx
{
  "pathname": "/about",
  "search": "?detail=true",
  "hash": ""
}
```

위 location 객체는 http://locallhost:3000/about?detail=true 주소로 들어갔을 때의 값입니다.

* URL 쿼리를 읽을 때는 search 값을 확인해야 합니다. 이 값은 문자열로 되어 있습니다.

* URL 쿼리는 ?detail=ture&anoter=1과 같이 문자열에 여러 가지 값을 설정해 줄 수 있습니다.

* search 값에서 특정 값을 읽어 오기 위해서는 이 문자열을 객체 형태로 변환해 주어야 합니다.

<u>쿼리 문자열을 객체로 변환할 때는 qs라는 라이브러리를 사용합니다.</u>
> npm install qs
또는
> yarn add qs

About 컴포넌트에서 location.search 값에 있는 detail이 ture인지 아닌지에 따라 추가 정보를 보여 주도록 만들겠습니다. 

```jsx About.js
import React from 'react';
import qs from 'qs';

const About = ({ location }) => {
  const query = qs.parse(location.search, {
    ignoreQueryPrefix: true,
  });
  const showDetail = query.detail === 'true';
  return (
    <div>
      <h1>소개</h1>
      <p>이 프로젝트는 리액트 라우터 기초를 실습해 보는 예제 프로젝트입니다.</p>
      {showDetail && <p>detail 값을 true로 설정하셨군요!</p>}
    </div>
  );
};

export default About;
```

쿼리를 사용할 때는 쿼리 문자열을 객체로 파싱하는 과정에서 결과 값은 언제나 문자열이라는 점을 주의합니다.

?value=1 혹은 ?value=true와 같이 숫자나 논리 자료형을 사용한다고 해도 "1", "true"와 같이 문자열 형태로 받아집니다.

그렇기 때문에 숫자를 받아 와야 하면 parseInt 함수를 통해 꼭 숫자로 변환해 주고, 지금처럼 논리 자료형 값을 사용해야 하는 경우에는 정확하 "true" 문자열이랑 일치하는지 비교해 줘야합니다.

------
<h2 id='서브_라우트'>서브 라우트</h2>

서브 라우트는 라우트 내부에 또 라우트를 정의하는 것을 말합니다.
라우트로 사용되고 있는 컴포넌트의 내부에 Route 컴포넌트를 또 사용하면 됩니다.

기존의 App 컴포넌트에서는 두 종류의 Profile 링크를 보여 주었는데, 이를 잘라내서 프로필 링크를 보여 주는 Profiles 라우트 컴포넌트를 따로 만들고, 그 안에서 Profile 컴포넌트를 서브 라우트로 사용하도록 작성해 봅니다.

```jsx profiles

```

------
<h2 id='부가기능'>리액트 라우터 부가 기능</h2>
