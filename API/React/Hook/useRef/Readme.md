# useRef

React의 `useRef`는 함수형 컴포넌트에서 DOM 요소에 직접 접근하거나 값을 유지하기 위해 사용하는 Hook입니다.

## 주요 특징

`useRef`는 `.current` 프로퍼티를 가진 변경 가능한 ref 객체를 반환합니다. 이 객체는 컴포넌트의 전체 생명주기 동안 유지되며, 값이 변경되어도 컴포넌트가 리렌더링되지 않습니다.

## 사용 사례

**1. DOM 요소 접근**
```javascript
import { useRef, useEffect } from 'react';

function TextInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    // 컴포넌트 마운트 시 input에 포커스
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

**2. 이전 값 저장**
```javascript
function Counter() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef();

  useEffect(() => {
    prevCountRef.current = count;
  });

  const prevCount = prevCountRef.current;

  return (
    <div>
      <p>현재: {count}, 이전: {prevCount}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```

**3. 타이머나 구독 저장**
```javascript
function Timer() {
  const [seconds, setSeconds] = useState(0);
  const intervalRef = useRef(null);

  const startTimer = () => {
    intervalRef.current = setInterval(() => {
      setSeconds(s => s + 1);
    }, 1000);
  };

  const stopTimer = () => {
    clearInterval(intervalRef.current);
  };

  return (
    <div>
      <p>시간: {seconds}초</p>
      <button onClick={startTimer}>시작</button>
      <button onClick={stopTimer}>정지</button>
    </div>
  );
}
```

## useState와의 차이점

- `useState`: 값이 변경되면 컴포넌트가 리렌더링됨
- `useRef`: 값이 변경되어도 리렌더링되지 않음, 즉시 접근 

<br/>

`useRef`는 DOM 조작이나 리렌더링을 트리거하지 않고 값을 저장해야 할 때 매우 유용합니다.