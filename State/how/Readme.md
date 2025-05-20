## 💡 키포인트 - React 내장 기능과 외부 라이브러리 사용

<br/>

> React에서는 기본적으로 `props` 를 사용하여 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달 할 수 있습니다.

> 상태 관리 방법으로는 React의 내장 기능인 useState, useReducer, Context API가 있으며, 더 복잡한 애플리케이션을 위해 Redux, Recoil, Zustand, Jotai 같은 외부 라이브러리도 사용됩니다.

<br/>

### useState

> React의 가장 기본적인 [훅](../../API/React/Hook/Readme.md)으로, 함수형 컴포넌트에서 상태(state)를 관리할 수 있게 해줍니다.

- 기본 구조

  ```jsx
  const [state, setState] = useState(initialValue);
  ```

  - **state**: 현재 상태 값
  - **setState**: 상태를 업데이트하는 함수
  - **initialValue**: 상태의 초기값

  <br/>

-  기본 사용법

    ```jsx
    import { useState } from 'react';

    function Counter() {
      const [count, setCount] = useState(0);
      
      return (
        <div>
          <p>현재 카운트: {count}</p>
          <button onClick={() => setCount(count + 1)}>증가</button>
        </div>
      );
    }
    ```

- 상태 업데이트 방법

  - **직접 값 전달**:
    ```jsx
    setCount(5); // count를 5로 설정
    ```

  - **이전 상태 기반 업데이트** (함수형 업데이트):
    ```jsx
    // 이전 상태를 사용해 새 상태 계산
    setCount(prevCount => prevCount + 1);
    ``` 

  - **객체형 상태 관리**

    ```jsx
    const [user, setUser] = useState({ name: '김철수', age: 25 });

    // 객체 업데이트 (스프레드 연산자 사용)
    const updateAge = () => {
      setUser(prevUser => ({ ...prevUser, age: prevUser.age + 1 }));
    };
    ```

    > 객체나 배열 상태를 업데이트할 때는 항상 새 객체/배열을 생성해야 합니다 (불변성 유지).

<br/>

### useReducer

> Redux의 패턴에서 영감을 받았으며, 상태 변화를 더 예측 가능하고 구조화된 방식으로 다룰 수 있게 해줍니다.

-  기본 구조

    ```jsx
    const [state, dispatch] = useReducer(reducer, initialState);
    ```

    - **state**: 현재 상태 값
    - **dispatch**: 액션을 발생시키는 함수
    - **reducer**: 현재 상태와 액션을 받아 새 상태를 반환하는 함수
    - **initialState**: 초기 상태 값

    <br/>

- 작동 방식

  1. **리듀서 함수 정의**: 상태 변화 로직을 담당
  2. **초기 상태 설정**: 상태의 초기값 지정
  3. **useReducer 호출**: 상태와 디스패치 함수 반환
  4. **디스패치 호출**: 액션을 발생시켜 상태 변경

   <br/>

-  예제: 간단한 카운터

    ```jsx
    import { useReducer } from 'react';

    // 리듀서 함수 정의
    function counterReducer(state, action) {
      switch (action.type) {
        case 'INCREMENT':
          return { count: state.count + 1 };
        case 'DECREMENT':
          return { count: state.count - 1 };
        case 'RESET':
          return { count: 0 };
        default:
          return state;
      }
    }

    function Counter() {
      // 초기 상태 설정 및 useReducer 호출
      const [state, dispatch] = useReducer(counterReducer, { count: 0 });

      return (
        <div>
          <p>현재 카운트: {state.count}</p>
          <button onClick={() => dispatch({ type: 'INCREMENT' })}>증가</button>
          <button onClick={() => dispatch({ type: 'DECREMENT' })}>감소</button>
          <button onClick={() => dispatch({ type: 'RESET' })}>초기화</button>
        </div>
      );
    }
    ```

### useContext

> React의 Context API를 함수형 컴포넌트에서 쉽게 사용할 수 있게 해주는 Hook입니다.

> Context API란?
> - Context API는 컴포넌트 트리 전체에 데이터를 직접 전달할 수 있는 방법으로, props를 여러 단계에 걸쳐 전달하는 "prop drilling" 문제를 해결합니다.

- 기본 사용법

  1. **Context 생성하기**:
    ```jsx
    // UserContext.js
    import { createContext } from 'react';
    export const UserContext = createContext(null);
    ```

  2. **Context Provider로 컴포넌트 감싸기**:
    ```jsx
    // App.js
    import { UserContext } from './UserContext';

    function App() {
      const user = { name: '김철수', isLoggedIn: true };
      
      return (
        <UserContext.Provider value={user}>
          <MainComponent />
        </UserContext.Provider>
      );
    }
    ```

  3. **useContext로 데이터 사용하기**:
    ```jsx
    // DeepChildComponent.js
    import { useContext } from 'react';
    import { UserContext } from './UserContext';

    function DeepChildComponent() {
      const user = useContext(UserContext);
      
      return (
        <div>
          {user.isLoggedIn ? `${user.name}님 환영합니다!` : '로그인이 필요합니다.'}
        </div>
      );
    }
    ```

<br/>

### 외부 라이브러리
- [Redux](../Redux/Readme.md)
- [Recoil](../Recoil/Readme.md)
- [Zustand](../Zustand/Readme.md)
