# useCallback

React의 `useCallback`은 함수를 메모이제이션하여 불필요한 함수 재생성을 방지하는 Hook입니다.

## 기본 구조

```javascript
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

의존성 배열의 값이 변경될 때만 새로운 함수를 생성하고, 그렇지 않으면 이전 함수를 재사용합니다.

## 사용 사례

**1. 자식 컴포넌트 props 최적화**
```javascript
function Parent({ items }) {
  const [count, setCount] = useState(0);

  // useCallback 없이 - 매번 새로운 함수 생성
  const handleClick = (id) => {
    console.log('클릭된 아이템:', id);
  };

  // useCallback 사용 - 함수 재사용
  const handleClickMemo = useCallback((id) => {
    console.log('클릭된 아이템:', id);
  }, []); // 의존성이 없으므로 한 번만 생성

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>카운트: {count}</button>
      {items.map(item => (
        <ChildComponent 
          key={item.id} 
          item={item} 
          onClick={handleClickMemo} 
        />
      ))}
    </div>
  );
}

const ChildComponent = React.memo(({ item, onClick }) => {
  console.log('자식 컴포넌트 렌더링:', item.id);
  return (
    <div onClick={() => onClick(item.id)}>
      {item.name}
    </div>
  );
});
```

**2. 의존성이 있는 함수**
```javascript
function SearchComponent({ searchTerm, data }) {
  const [results, setResults] = useState([]);

  const handleSearch = useCallback(() => {
    const filtered = data.filter(item => 
      item.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
    setResults(filtered);
  }, [data, searchTerm]); // data나 searchTerm이 변경될 때만 새 함수 생성

  useEffect(() => {
    handleSearch();
  }, [handleSearch]);

  return (
    <div>
      <button onClick={handleSearch}>검색</button>
      {/* 결과 렌더링 */}
    </div>
  );
}
```

**3. 이벤트 핸들러 최적화**
```javascript
function TodoList({ todos, onToggle, onDelete }) {
  const handleToggle = useCallback((id) => {
    onToggle(id);
  }, [onToggle]);

  const handleDelete = useCallback((id) => {
    onDelete(id);  
  }, [onDelete]);

  return (
    <div>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={handleToggle}
          onDelete={handleDelete}
        />
      ))}
    </div>
  );
}
```

## React.memo와 함께 사용

`useCallback`은 `React.memo`로 최적화된 자식 컴포넌트에 함수를 전달할 때 특히 유용합니다:

```javascript
function Parent() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  // useCallback 없이 - Child가 매번 리렌더링됨
  const handleClick = () => console.log('클릭');

  // useCallback 사용 - Child 리렌더링 방지
  const handleClickMemo = useCallback(() => {
    console.log('클릭');
  }, []);

  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>카운트: {count}</button>
      <Child onClick={handleClickMemo} />
    </div>
  );
}

const Child = React.memo(({ onClick }) => {
  console.log('Child 렌더링');
  return <button onClick={onClick}>클릭</button>;
});
```

## 주의사항

**언제 사용하지 말아야 하는가**
- 의존성이 매번 변경되는 경우
- 함수가 자식 컴포넌트로 전달되지 않는 경우
- 단순한 함수 (오버헤드가 더 클 수 있음)

**잘못된 사용 예시**
```javascript
// 의존성이 매번 변경됨
const callback = useCallback(() => {
  doSomething(data);
}, [{ ...data }]);

// 불필요한 useCallback
const handleClick = useCallback(() => {
  console.log('간단한 로그');
}, []); // 자식에게 전달되지 않는 단순한 함수
```

<br/>

`useCallback`은 함수가 자식 컴포넌트로 전달되어 불필요한 리렌더링을 방지하고자 할 때 사용하는 것이 가장 효과적입니다.