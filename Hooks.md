## 1. Hooks

 Hooks는 리액트 v16.8에 새로 도입된 기능으로, 함수형 컴포넌트에서 react state와 생명주기 기능(lifecycle features)을 연동(hook into) 할 수 있게 해준다.  

-이전에 리액트가 겪던 문제들을 해결해주며 클래스형 컴포넌트를 사용하지 않고도 함수형 컴포넌트에서 상태값 접근과 자식 요소에 접근이 가능해졌다.

 
-Hook 사용 규칙
* 최상위에서만 Hooks을 호출해야 한다.
* 리액트 함수 컴포넌트에서만 Hooks을 호출해야 한다.

 

### 1-(1). useState

 가장 기본적인 Hook이며, 함수형 컴포넌트에서도 가변적인 상태를 지닐 수 있게 해준다.

https://daldaguri-love.tistory.com/manage/newpost/349?type=post&returnURL=https%3A%2F%2Fdaldaguri-love.tistory.com%2F349  
https://daldaguri-love.tistory.com/manage/newpost/349?returnURL=https%3A%2F%2Fdaldaguri-love.tistory.com%2F349&type=postdaldaguri-love.tistory.com
 

### 1-(2). useEffect

리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다.

 

-클래스형 컴포넌트에서는 생명주기 메소드를 사용할 수 있었는데, 이를 함수형 컴포넌트에서도 사용할 수
있게 되었다. 즉, 라이프 사이클 훅을 대체할 수 있게 되었다.

 
**※ 리액트의 생명주기**

컴포넌트는 생성(mounting) -> 업데이트(updating) -> 제거(unmounting)의 생명주기를 갖는다. 리액트 클래스 컴포넌트는 라이프 사이클 메서드를 사용하고, 함수형 컴포넌트는 Hook을 사용한다.

-기본 형태: useEffect(function, deps)

* function: 수행하고자 하는 작업
* deps: 배열 형태이며, 배열 안에는 검사하고자 하는 특정 값 or 빈 배열

 

-useEffect 함수 불러오기

```javascript
import React, {useEffect} from 'react'
```

-리렌더링 될 때마다 실행

```javascript
useEffect(()=>{
	console.log('렌더링 될 때마다 살행')
});
```

-마운트 될 때만 실행하고 싶을 때

: useEffect에서 설정한 함수를 컴포넌트가 맨 처음 렌더링 될 때만 실행하고, 업데이트 될 때는 실행하지 않으려면 함수의 두 번째 파라미터로 비어있는 배열을 넣어주면 된다.

```javascript
useEffect(()=>{
	console.log('마운트 될 때만 실행');
},[]);
```

- 특정 값이 업데이트 될 때만 실행하고 싶을 때
useEffect의 두 번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣어주면 된다.

```javascript
useEffect(()=>{
	console.log(name);
},[name]);
```

- 컴포넌트가 언마운트 되기 전이나 업데이트 되기 직전에 어떠한 작업을 수행하고 싶을 때

useEffect에서 cleanup 함수를 반환

### 1-(3). useReducer

- useState를 대체할 수 있는 함수

- 좀 더 복잡한 상태 관리가 필요한 경우 reducer를 사용할 수 있다.

- 리듀서는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션(action) 값을 전달받아

새로운 상태를 반환하는 함수이다.

**※ Reducer란?**

Reducer(리듀서)는 기본적으로 두 개의 인수(current state와 action)를 받고, 이렇게 받은 두 인수를 기반으로 해서 새 상태를 반환하는 함수이다.

```javascript
const reducer = (state,action) => newState;
```

reducer(리듀서)와 initialState(초기 상태)를 전달하면 useReducer란 훅(Hook)이
새로운 상태(state)와 dispath(디스패치) 함수를 리턴해 준다. state는 우리가 앞으로 컴포넌트에서
사용할 수 있는 상태를 가르키게 되고, dispatch는 액션을 발생시키는 함수라고 이해하면 된다.

```javascript
const [state,dispatch] = useReducer(reducer, initialState);
```
useReducer에 넣는 첫번째 파라미터는 reducer 함수이고, 두 번째 파라미터는 초기 상태이다.

 

-예제

Info.js

```javascript
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { value: state.value + 1 };
    case "DECREMENT":
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
        현재 카운터 값은 <b>{state.value}</b> 입니다.
      </p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+1</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>-1</button>
    </div>
  );
};

export default Counter;
``` 

App.js

```javascript
import Counter from "./Counter";

function App() {
  return <Counter />;
}

export default App;
```

Info.js

```javascript
import React, { useReducer } from "react";

const reducer = (state, action) => {
  return {
    ...state,
    [action.name]: action.value,
  };
};

const Info = () => {
  const [state, dispatch] = useReducer(reducer, {
    name: "",
    nickname: "",
  });

  const { name, nickname } = state;
  const onChange = (e) => {
    dispatch(e.target);
  };

  return (
    <div>
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="nickname" value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름: </b> {name}
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

### 1-(4). useMemo

함수형 컴포넌트 내부에서 발행하는 연산을 최적화 가능
-렌더링하는 과정에서 특정 값이 바뀌었을 때만 연산을 실행하고, 원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용하는 방식


**※메모이제이션(Memoization)**

기존에 수행한 연산의 결과값을 어딘가에 저장해두고 동일한 입력이 들어오면 재활용하는 프로그래밍 기법.
이것을 활용하면 중복 연산을 피할 수 잇기 때문에 애플리케이션의 성능을 최적화 가능

 

- useMemo는 메모이제이션 된 값을 반환하는 함수이다.

```javascript
useMemo(()=>fn,[deps])
```
deps로 지정한 값이 변하게 된다면 ()=>fn 함수를 실행하고, 그 함수의 반환 값을 반환해준다.

 

Average.js

```javascript
import React, { useState, useMemo } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중...");
  if (numbers.length === 0) return 0;
  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (e) => {
    setNumber(e.target.value);
  };

  const onInsert = () => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>value</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;
```

### 1-(5). useCallback

주로 렌더링 성능을 최적화해야 하는 상황에서 사용하는데, 이 Hook을 사용하면 이벤트 핸들러 함수를

필요할 때만 사용할 수 있다.

 

**※useMemo와 useCallback의 차이**

-useMemo: 메모이제이션 된 "값"을 반환
-useCallback: "함수"를 반환

 

### 1-(6). useRef

함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 해준다.

https://daldaguri-love.tistory.com/354

 
[React] ref: DOM에 이름 달기

1. ref ref는 'reference'의 약자로, HTML에서 id를 사용해 DOM에 이름을 다는 것처럼 리액트 프로젝트 내부에서 DOM 에 이름을 다는 방법을 ref라고 한다. - DOM을 직접 건드려야 할 때 사용 

### 1-(7). 커스텀 Hooks 만들기

여러 컴포넌트에서 비슷한 기능을 공유할 경우, 커스텀 Hooks 를 만들어서 반복되는 로직을 쉽게 재사용 할 수 있다.
