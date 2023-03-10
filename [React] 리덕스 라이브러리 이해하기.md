1. 리덕스(Redux)

- 자바스크립트 애플리케이션에서 상태를 효율적으로 관리할 수 있게 도와주는 도구

- 리덕스를 사용하면 컴포넌트의 상태 업데이트 관련 로직을 파일을 분리시켜서 더욱

효율적으로 관리가 가능하다.

- 리덕스는 리액트에 종속되는 라이브러리가 아니라 다른 UI 라이브러리/프레임워크와 함께 사용 가능

 

1-(2). 액션(Action)

상태에 어떠한 변화가 필요하면 액션(action)을 발생

- 액션은 type 필드를 반드시 가지고 있어야 하며, 추가적으로 필요한 객체의 요소들은 필요에 의해

추가될 수 있다.

 

1-(3). 액션 생성 함수(Action Creator)

액션 객체를 만들어 주는 함수

 

1-(4). 리듀서(Reducer)

 변화를 일으키는 함수

 - 이전 상태와 액션을 파라미터로 입력 받는다.

- 리듀서 첫 호출 시, state 값은 undefined, undefined일 때, initialState를 기본 값으로 설정하기 위해

함수 파라미터에 기본 값을 설정한다.

 - 상태의 불변성을 유지하면서 데이터 변화를 시켜야 한다. -> spread 연산자를 사용한다.

 

1-(5). 스토어(Store)

컴포넌트 외부에 있는 상태 저장소

- 한 개의 프로젝트는 단 하나의 스토어만 가질 수 있으며, 스토어 안에는 현재 애플리케이션 상태, 리듀서,

몇 가지 내장 함수를 포함

 

1-(6). 디스패치(Dispatch)

스토어의 내장함수 중 하나로, 액션을 발생시키는 역할을 한다.

 - 형태: dispatch(action), 액션 객체를 파라미터로 넣어 호출한다.

 

1-(7). 구독(Subscribe)

스토어의 내장함수 중 하나로, 하나의 함수 형태의 값을 인자로 받는데 액션이 디스패치 될 때마다

전달해준 함수를 호출한다.

 

1-(8). 리덕스의 세 가지 규칙

 

- 단일 스토어

하나의 애플리케이션 안에는 하나의 스토어를 사용하는 것을 권장

 

- 읽기 전용 상태

기존의 state 고유 값은 수정하지 않고 새로운 state를 만들어 이를 수정하는 방식으로 업데이트를 한다.

이는 리덕스 고유의 불변성을 유지해야 하기 때문이다.

 

- 리듀서는 순수한 함수

1). 리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받는다.

2). 파라미터 외의 값에는 의존하면 안된다.

3). 이전 상태는 절대로 건드리지 않고, 변화를 준 새로운 상태 객체를 만들어 반환한다.

4). 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 값을 반환해야 한다.

 

2. 리덕스 툴 킷(toolkit)

 

리덕스에서 자주 사용되는 코드와 로직들을 훨씬 편하게 만들어주는 리덕스 라이브러리

 

2-(1). configureStore

- createStore와 동일한 기능을 제공, Reducer에서 반환된 새로운 state를 Store라는 객체로 정리해 관리하는 곳이다.

- configureStore 함수는 여러 개의 인자 대신 이름이 지정된 하나의 object를 인자로 받는다.

 

-리덕스 스토어 생성: store/index.js

import { configureStore } from "@reduxjs/toolkit";

const store = configureStore({
  reducer: {},
});

export default store;
 

 

 

2-(2). createSlice

 

createSlice는 생성된 reducer 함수를 reducer라는 필드를 포함하는 slice 객체와 action 이라는 객체 내부에서

생성된 액션 생성함수를 반환한다.

- reducer 함수와 action creator를 포함한 객체

 

- name: 리듀서의 이름

- initialState: 초기 상태값

- reducers: 리듀서를 넣어준다, 리듀서를 만들면 이름이 액션명이 되고 반환되는 객체가 액션값이 된다.

 

-createSlice 생성: store/counter.js

import { createSlice } from "@reduxjs/toolkit";

const initialCounterState = { counter: 0, showCounter: true };

const counterSlice = createSlice({
  name: "counter",
  initialState: initialCounterState,
  reducers: {
    increment(state) {
      state.counter++;
    },
    decrement(state) {
      state.counter--;
    },
    increase(state, action) {
      state.counter = state.counter + action.payload;
    },
    toggleCounter(state) {
      state.showCounter = !state.showCounter;
    },
  },
});

export const counterActions = counterSlice.actions;
export default counterSlice.reducer;
 

-스토어에 슬라이스 리듀서 추가: 

import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "../features/counter/counterSlice";

export const store = () =>
  configureStore({
    reducer: { counterReducer },
  });
 

2-(3). 컴포넌트 생성

 

-useSelector, useDispatch로 상태접근: components/Counter.js

import { useSelector, useDispatch } from "react-redux";

import { counterActions } from "../store/counter";
import classes from "./Counter.module.css";

const Counter = () => {
  const dispatch = useDispatch();
  const counter = useSelector((state) => state.counter.counter);
  const show = useSelector((state) => state.counter.showCounter);

  const incrementHandler = () => {
    dispatch(counterActions.increment());
  };
  const decrementHandler = () => {
    dispatch(counterActions.decrement());
  };

  const increaseHandler = () => {
    dispatch(counterActions.increase(10));
  };

  const toggleCounterHandler = () => {
    dispatch(counterActions.toggleCounter());
  };

  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      {show && <div className={classes.value}>{counter}</div>}
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increaseHandler}>Increse by 5</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};

export default Counter;
 

- useSelector()는 기존 리덕스의 connect()를 이용하지 않고 리덕스의 상태를 조회할 수 있다.

- useDispatch()는 생성한 액션을 발생시키며, 액션생성 함수를 가져온다.
