# useMemo

React의 `useMemo`는 계산 비용이 큰 연산 결과를 메모이제이션하여 성능을 최적화하는 Hook입니다.

## 기본 구조

```javascript
const memoizedValue = useMemo(() => {
  return expensiveCalculation(a, b);
}, [a, b]);
```

첫 번째 인자는 계산 함수, 두 번째 인자는 의존성 배열입니다. 의존성 배열의 값이 변경될 때만 함수가 다시 실행됩니다.

## 사용 사례

**1. 비용이 큰 계산 최적화**
```javascript
function ExpensiveComponent({ items, multiplier }) {
  const expensiveValue = useMemo(() => {
    console.log('비용이 큰 계산 실행');
    return items.reduce((sum, item) => sum + item.value * multiplier, 0);
  }, [items, multiplier]);

  return <div>결과: {expensiveValue}</div>;
}
```

**2. 객체/배열 참조 안정화**
```javascript
function UserList({ users, searchTerm }) {
  const filteredUsers = useMemo(() => {
    return users.filter(user => 
      user.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [users, searchTerm]);

  return (
    <div>
      {filteredUsers.map(user => (
        <UserItem key={user.id} user={user} />
      ))}
    </div>
  );
}
```

**3. 자식 컴포넌트 props 최적화**
```javascript
function Parent({ data }) {
  const processedData = useMemo(() => ({
    sorted: data.sort((a, b) => a.name.localeCompare(b.name)),
    count: data.length
  }), [data]);

  return <Child data={processedData} />;
}
```

## 주의사항

**언제 사용하지 말아야 하는가**
- 단순한 계산 (오히려 성능 저하)
- 매번 다른 의존성을 가지는 경우
- 메모이제이션 오버헤드가 계산 비용보다 큰 경우

**잘못된 사용 예시**
```javascript
// 불필요한 useMemo 사용
const simpleValue = useMemo(() => a + b, [a, b]);

// 의존성이 매번 변경되는 경우
const value = useMemo(() => calculate(data), [{ ...data }]);
```

## useCallback과의 차이점

- `useMemo`: 계산된 **값**을 메모이제이션
- `useCallback`: **함수 자체**를 메모이제이션

```javascript
// useMemo - 값 메모이제이션
const expensiveValue = useMemo(() => calculate(data), [data]);

// useCallback - 함수 메모이제이션
const handleClick = useCallback(() => {
  doSomething(data);
}, [data]);
```

<br/>

`useMemo`는 렌더링 성능 최적화에 유용하지만, 남용하면 오히려 성능이 저하될 수 있으므로 실제 성능 문제가 있을 때 사용하는 것이 좋습니다.