# Custom Hook

Custom Hook은 React의 내장 Hook들을 조합하여 만든 사용자 정의 함수로, 컴포넌트 간에 상태 로직을 재사용할 수 있게 해주는 기능입니다.

## Custom Hook의 특징

Custom Hook은 반드시 `use`로 시작하는 이름을 가져야 하며, 내부에서 다른 Hook들을 호출할 수 있습니다. 일반 JavaScript 함수와 달리 React의 Hook 규칙을 따라야 합니다.

## 예시 1

```javascript
import { useState, useEffect } from 'react';

// 카운터 로직을 재사용 가능한 Hook으로 분리
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}

// 컴포넌트에서 사용
function Counter() {
  const { count, increment, decrement, reset } = useCounter(10);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

## 예시 2

```javascript
// API 데이터 페칭을 위한 Custom Hook
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [url]);
  
  return { data, loading, error };
}

// 여러 컴포넌트에서 재사용
function UserProfile({ userId }) {
  const { data: user, loading, error } = useFetch(`/api/users/${userId}`);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return <div>Hello, {user.name}!</div>;
}
```

## 장점

Custom Hook을 사용하면 복잡한 상태 로직을 컴포넌트에서 분리할 수 있어 코드가 더 깔끔해지고, 같은 로직을 여러 컴포넌트에서 쉽게 재사용할 수 있습니다. 또한 테스트하기도 더 쉬워지고, 관심사의 분리 원칙을 잘 지킬 수 있습니다.