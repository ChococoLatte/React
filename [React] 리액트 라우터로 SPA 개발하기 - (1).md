## 1. 라우팅(Routing)이란?
사용자가 요청한 URL에 따라 알맞는 페이지를 보여주는 것을 의미

-리액트 라우터(React Router): 컴포넌트 기반으로 라우팅 시스템 설정 가능

## 2. SPA란?
Single Page Application의 약자로, 한 개의 페이지로 이루어진 어플리케이션을 의미한다.
- SPA에서는 리액트와 같은 라이브러리를 사용해서 뷰 렌더링을 사용자의 브라우저가 담당하게 하고,
한 번만 html을 받아온 뒤 인터렉션이 발생하면 필요한 부분만 JS를 사용하여 업데이트 하는 방식을 사용

 

※ MPA란?
Multi Page Application의 약자로, 인터렉션이 발생할 때마타 서버로부터 새로운 파일을 받아 해당 링크로 이동하며 페이지 전체를 새로 렌더링 하는 전통적인 웹페이지 구성 방식을 의미

- 인터렉션이 많고 다양한 정보를 제공하는 모던 웹 어플리케이션에서는 비효율적인 방법

※렌더링 방식

#### 1). CSR(Client Side Rendering)

- 사용자의 요청에 따라 필요한 부분만 응답 받아 렌더링 하는 방식
- 화면에 그려주기 위해 필요한 HTML을 웹 브라우저가 JavaScript를 실행하여 동적으로 만들어 주는 것을 의미
-> React, Vue, Angular, Svelte

**장점**

SPA 형태로 작동하기 때문에 로드가 완료된 후 아주 빠르게 동작한다. 변경 부분만 요청하면 되기 때문에 서버 부하가 적다.

**단점**

초기에 html,js,css를 로드해야 하므로  SSR보다 상대적으로 초기 로드가 오래 걸린다.
SEO(Search Engin Optimization, 검색 엔진 최적화) 한계로 인해 검색엔진에 노출하기 힘들다.

#### 2). SSR(Server Side Rendering)

- 서버로부터 완전하게 만들어진 html 파일을 받아와 페이지 전체를 렌더링 하는 방식
- 웹 브라우저가 서버에게 받은 HTML을 화면에 뿌려주는 것을 의미
-> next,nuxt

**장점**

SEO 가능
적은 파일을 로드하므로 초기 속도가 빠르다.

**단점**

페이지 전환 시 화면 깜박거림이 발생한다.  - html을 다시 받아와야 하므로
매번 페이지가 변경될 때마다 새로고침되고, 서버쪽에 요청을 할때마다 리소스를 준비해서
응답해야 하기 때문에 CSR보다 서버 부하가 크다.

## 3. 리액트 라우터 적용 및 기본 사용법

### 3-(1). 기본 사용 방법

라우팅 기능을 사용하려면 BrowserRouter 컴포넌트를 최상위 태그에 감싸주면 된다.
앱에서 단 하나의 라우터만을 사용해야 한다.

```javascript
src/index.js

import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

- BrowsRouter: 브라우저 History API를 사용해 현재 위치의 URL을 저장해주는 역할

### 3-(2). Route 컴포넌트로 특정 경로에 원하는 컴포넌트 보여주기

< Route path="주소규칙" element={보여줄 컴포넌트 JSX} >

```javascript
src/App.js

import React from "react";
import { Route, Routes } from "react-router-dom";
import About from "./pages/About";
import Home from "./pages/Home";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
    </Routes>
  );
}

export default App;
```

- Routes: 여러 Route를 감싸서 그 중 규칙이 일치하는 라우트 단 하나만을 렌더링 시켜주는 역할
- Route: path 속성에 경로, element 속성에는 컴포넌트를 넣어준다. 

### 3-(3). <Link> 컴포넌트 

<Link to="경로"> 링크 이름</Link>

```javascript
src/pages/Home.js

import { Link } from "react-router-dom";

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>가장 먼저 보여지는 페이지입니다.</p>
      <Link to="/about">소개</Link>
    </div>
  );
};

export default Home;
``` 

- 웹페이지에서는 원래 링크를 보여줄 때 a 태그를 사용하지만, a 태그는 클릭 시 새로운 페이지를 불러오기 때문에
사용하지 않는다.
- Link 컴포넌트도 a태그를 사용하긴 하지만, 페이지를 새로 불러오는 것을 막고 History API를 통해 브라우저
주소의 경로만 바꾸는 기능이 내장되어 있다.

### 3-(4). URL 파라미터와 쿼리 스트링

- URL 파라미터 예시: `/profile/velopert`
- 쿼리스트링 예시: `/articles?page=1&keyword=react`

URL 파라미터는 주소의 경로에 유동적인 값을 넣는 형태고, 쿼리스트링은 주소 뒷부분에 ? 문자열 이후에 key=value로 값을 정의하며 &로 구분하는 형태이다.

#### 3-(4)-(1). URL 파라미터

```javascript
src/pages/Profile.js

import { useParams } from "react-router-dom";

const data = {
  velopert: {
    name: "김민준",
    description: "리액트를 좋아하는 개발자",
  },
  gildong: {
    name: "홍길동",
    description: "고전 소설 홍길동전의 주인공",
  },
};

const Profile = () => {
  const params = useParams();
  const profile = data[params.username];

  return (
    <div>
      <h1>사용자 프로필</h1>
      {profile ? (
        <div>
          <h2>{profile.name}</h2>
          <h2>{profile.description}</h2>
        </div>
      ) : (
        <div>
          <p>존재하지 않는 프로필입니다.</p>
        </div>
      )}
    </div>
  );
};

export default Profile;
``` 

```javascript
src/App.js

import { Route, Routes } from "react-router-dom";

import Home from "./pages/Home";
import About from "./pages/About";
import Profile from "./pages/Profile";

function App() {
  return (
    <div>
      <Routes>
        <Route path="/" element={<Home />}></Route>
        <Route path="/about" element={<About />}></Route>
        <Route path="/profiles/:username" element={<Profile />}></Route>
      </Routes>
    </div>
  );
}

export default App;
```

URL 파라미터는 useParams라는 Hook을 사용하여 객체 형태로 조회할 수 있다.

#### 3-(4)-(2). 쿼리 스트링

```javascript
src/pages/About.js

import { useLocation } from "react-router-dom";

const About = () => {
  const location = useLocation();

  return (
    <div>
      <h1>소개</h1>
      <p>리액트 라우터를 사용해 보는 프로젝트입니다.</p>
      <p>쿼리스트링: {location.search}</p>
    </div>
  );
};

export default About;
```

-useLocation: 사용자가 현재 머물러있는 페이지에 대한 정보를 알려주는 hooks
- pathname: 현재 주소의 경로(쿼리스트링 제외)
- search: 맨 앞의 ? 문자를 포함한 쿼리스트링 값
- hash: 주소의 # 문자열 뒤의 값
- state: 페이지를 이동할 때 임의로 넣을 수 있는 상태 값
- key: location 객체의 고유값, 초기에는 default이며 페이지가 변경될 때마다 고유의 값이 생성됨.

```javascript
src/pages/About.js

import { useSearchParams } from "react-router-dom";

const About = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const detail = searchParams.get("detail");
  const mode = searchParams.get("mode");

  const onToggleDetail = () => {
    setSearchParams({ mode, detail: detail === "true" ? false : true });
  };

  const onIncreaseMode = () => {
    const nextMode = mode === null ? 1 : parseInt(mode) + 1;
    setSearchParams({ mode: nextMode, detail });
  };

  return (
    <div>
      <h1>소개</h1>
      <p>리액트 라우터를 사용해 보는 프로젝트입니다.</p>
      <p>detail: {detail}</p>
      <p>mode: {mode}</p>
      <button onClick={onToggleDetail}>Toggle detail</button>
      <button onClick={onIncreaseMode}>mode+1</button>
    </div>
  );
};

export default About;
```

- useSearchparams: 배열 타입의 값을 반환하며, 첫 번째 원소는 쿼리 파라미터를 조회하거나 수정하는 메서드들이 담긴 객체를 반환한다. get 메서드를 통해 특정 파라미터를 조회할 수 있고, set 메서드를 통해 특정 파라미터를 업데이트 할 수 있다. 두 번째 원소는 쿼리파라미터를 객체 형태로 업데이트 할 수 있는 함수를 반환한다.

 

참고: 리액트를 다루는 기술
