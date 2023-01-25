# 1. JSX란?

- 자바스크립트의 확장 문법

- 브라우저에서 실행되기 전에 코드가 번들링 되는 과정에서 바벨(Babel ->)을 사용하여 일반 자바스크립트의

형태로 변환

 

## ※Babel

자바스크립트의 문법을 확장해주는 도구. 아직 지원되지 않는 최신 문법이나, 편의상 사용하거나 실험적인 자바스크립트 문법들을 정식 자바스크립트 형태로 변환해줌으로써 구형 브라우저 같은 환경에서도 제대로 실행할 수 있게 해주는 역할을 한다.

 

### - 장점

1. 보기 쉽고 익숙하다.

2. 활용도가 높다.

 

### - 문법

1. 컴포넌트에 여러 요소가 있다면 반드시 부모 요소 하나로 감싸야한다. -> Fragment 사용 가능

2. JSX 안에서는 자바스크립트 표현식을 쓸 수 있다. 자바스크립트 표현식을 작성하려면 JSX 내부에서 코드를 {}로 감싸면 된다.

3. JSX에서 태그에 style과 CSS class를 설정하는 방법

 

- 인라인 스타일: 객체 형태로 작성해야 하며, camelCase의 형태로 네이밍 해주어야 함

- CSS class를 설정할 때에는 className으로 설정해야 함.

 

4. if문 대신 조건부 연산자 사용(삼항 연산자)

5. undefined를 렌더링 하지 않기(단, JSX 내부에서 undefined를 렌더링 하는 것은 괜찮음)

6. JSX에서는 태그를 닫아야함

7. 주석: {/* */}

 

참조: 리액트를 다루는 기술