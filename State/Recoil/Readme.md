# Recoil

Recoil은 Facebook에서 개발한 React 애플리케이션을 위한 상태 관리 라이브러리입니다. React의 내부 작동 방식과 호환되도록 설계되어 있으며, 복잡한 상태 관리와 비동기 데이터 처리에 최적화되어 있습니다.

## 1. Recoil의 핵심 개념

### 기본 철학
- **React 친화적**: React의 내부 작동 방식과 조화롭게 동작하도록 설계
- **분산된 상태**: 전역 스토어 대신 여러 개의 독립적인 atom으로 상태 관리
- **파생된 상태**: 기존 상태에서 계산된 값을 selector로 쉽게 생성
- **비동기 지원**: React Suspense와 통합되어 비동기 데이터 흐름 간소화

## 2. Recoil의 주요 구성 요소

### Atom
Atom은 상태의 단위로, 컴포넌트가 구독할 수 있는 상태 조각입니다.

```javascript
const todoListState = atom({
  key: 'todoListState', // 고유한 ID
  default: [] // 기본값
});
```

### Selector
Selector는 파생된 상태를 나타내며, 순수 함수를 사용해 상태를 변환합니다.

```javascript
const filteredTodoListState = selector({
  key: 'filteredTodoListState',
  get: ({get}) => {
    const filter = get(todoFilterState);
    const list = get(todoListState);

    switch (filter) {
      case 'Show Completed':
        return list.filter((item) => item.isComplete);
      case 'Show Uncompleted':
        return list.filter((item) => !item.isComplete);
      default:
        return list;
    }
  }
});
```

### RecoilRoot
RecoilRoot는 Recoil 상태의 컨텍스트를 제공합니다. 일반적으로 애플리케이션의 최상위에 배치합니다.

```javascript
function App() {
  return (
    <RecoilRoot>
      <TodoList />
    </RecoilRoot>
  );
}
```

## 3. Recoil Hooks

### useRecoilState
useState와 유사하게 상태와 업데이트 함수를 반환합니다.

```javascript
const [todoList, setTodoList] = useRecoilState(todoListState);
```

### useRecoilValue
상태 값만 읽고 싶을 때 사용합니다.

```javascript
const todoList = useRecoilValue(filteredTodoListState);
```

### useSetRecoilState
상태를 업데이트만 하고 싶을 때 사용합니다.

```javascript
const setTodoList = useSetRecoilState(todoListState);
```

### useResetRecoilState
상태를 기본값으로 리셋하고 싶을 때 사용합니다.

```javascript
const resetFilter = useResetRecoilState(todoFilterState);
```

## 4. 비동기 데이터 처리

Recoil의 강점 중 하나는 비동기 데이터 처리 방식입니다.

```javascript
const userDataQuery = selector({
  key: 'userDataQuery',
  get: async ({get}) => {
    const userId = get(currentUserIdState);
    const response = await fetch(`/api/users/${userId}`);
    return await response.json();
  }
});

function UserInfo() {
  const userData = useRecoilValue(userDataQuery);
  return <div>{userData.name}</div>;
}

// React Suspense와 함께 사용
function App() {
  return (
    <RecoilRoot>
      <Suspense fallback={<div>로딩 중...</div>}>
        <UserInfo />
      </Suspense>
    </RecoilRoot>
  );
}
```

## 5. Atoms Family

AtomFamily와 SelectorFamily는 매개변수에 따라 다른 atom이나 selector를 동적으로 생성합니다.

```javascript
const todoItemState = atomFamily({
  key: 'todoItemState',
  default: id => ({
    id,
    text: '',
    isComplete: false
  })
});

// 사용 예시
function TodoItem({id}) {
  const [item, setItem] = useRecoilState(todoItemState(id));
  // ...
}
```

## 6. Redux와 Recoil 비교

### Redux
- 중앙 집중식 스토어
- 불변성 유지를 위한 복잡한 업데이트 패턴
- 보일러플레이트 코드가 많음 (Redux Toolkit으로 개선)
- 미들웨어 기반 비동기 처리
- 강력한 개발자 도구

### Recoil
- 분산된 원자적 상태
- React 스타일의 API (useState와 유사)
- 간결한 코드 작성 가능
- [React Suspense](../../Syntax/Suspense/Readme.md) 활용 비동기 처리
- 상태 의존성 자동 추적
- 점진적 채택 가능

## 7. Recoil 장점

1. **간결한 API**: React의 useState와 유사한 직관적인 API
2. **성능 최적화**: 필요한 컴포넌트만 리렌더링
3. **비동기 처리 간소화**: Suspense와 통합된 비동기 데이터 흐름
4. **상태 파생 용이**: Selector를 통한 간편한 상태 변환
5. **동시성 모드 지원**: React의 동시성 모드와 호환
6. **상태 패밀리**: 동적인 상태 집합 생성 용이
7. **스냅샷 & 시간 여행**: 디버깅 및 상태 관리 기능

## 8. 실제 사용 패턴

### 상태 설계
```
- 기본 상태는 atom으로 정의
- 파생된 상태는 selector로 정의
- 관련 상태끼리 묶어서 관리
```

### 파일 구조
```
src/
├── recoil/
│   ├── atoms.js        # 기본 atom들
│   ├── selectors.js    # 파생된 selector들
│   └── constants.js    # 키 상수 등
├── components/
│   ├── TodoList.js
│   └── ...
└── App.js
```