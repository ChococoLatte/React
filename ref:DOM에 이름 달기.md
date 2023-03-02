## 1. ref

ref는 'reference'의 약자로, HTML에서 id를 사용해 DOM에 이름을 다는 것처럼 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법을 ref라고 한다.
- DOM을 직접 건드려야 할 때 사용

### 1-(1). DOM을 꼭 사용해야 하는 상황

 - 특정 input에 포커스 주기
 - 스크롤 박스 조작하기
 - Canvas 요소에 그림 그리기 등

### 2. useRef

함수형 컴포넌트에서 ref를 사용할 때에는 useRef라는  Hook 함수를 사용한다. 

#### 2-(1). 예시

```javascript
import React, { useState, useRef } from "react";

const InputSample = () => {
  const [inputs, setInputs] = useState({
    name: "",
    nickname: "",
  });

  const nameInput = useRef();

  const { name, nickname } = inputs;
  const onChange = (e) => {
    const { value, name } = e.target;
    setInputs({
      ...inputs,
      [name]: value,
    });
  };

  const onReset = () => {
    setInputs({
      name: "",
      nickname: "",
    });
    nameInput.current.focus();
  };

  return (
    <div>
      <input
        name="name"
        placeholder="이름"
        onChange={onChange}
        value={name}
        ref={nameInput}
      />
      <input
        name="nickname"
        placeholder="닉네임"
        onChange={onChange}
        value={nickname}
      />
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {name} ({nickname})
      </div>
    </div>
  );
};

export default InputSample;
```

#### 2-(2). 용도

- 컴포넌트에서 특정 DOM을 선택해야 할 때
- 컴포넌트 안에서 조회 및 수정할 수 있는 변수를 관리하는 것

useRef로 관리하고 있는 변수는 설정 후 바로 조회 가능

 
