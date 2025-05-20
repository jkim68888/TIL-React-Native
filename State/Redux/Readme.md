# React Redux

React Redux는 React 애플리케이션에서 상태 관리를 위한 공식 Redux 바인딩 라이브러리입니다. 

## 1. Redux의 핵심 원칙

- **Single Source of Truth**: 애플리케이션의 전체 상태가 하나의 스토어에 저장됩니다.
- **State is Read-Only**: 상태를 변경하려면 액션을 디스패치해야 합니다.
- **Changes are Made with Pure Functions**: 리듀서는 이전 상태와 액션을 받아 새로운 상태를 반환하는 순수 함수입니다.

## 2. Redux 데이터 흐름

1. 사용자 이벤트 발생 → 액션 디스패치
2. 리듀서가 액션을 처리하여 새로운 상태 생성
3. 스토어가 상태 업데이트
4. UI가 새로운 상태에 따라 재렌더링

## 3. React Redux 주요 구성 요소

### 액션(Action)
액션은 무엇이 일어났는지 설명하는 자바스크립트 객체입니다.
```javascript
// 액션 타입 정의
const ADD_TODO = 'ADD_TODO';

// 액션 생성자 함수
const addTodo = (text) => ({
  type: ADD_TODO,
  id: Date.now(),
  text
});
```

### 리듀서(Reducer)
리듀서는 이전 상태와 액션을 받아 새로운 상태를 반환하는 순수 함수입니다.
```javascript
const todosReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          id: action.id,
          text: action.text,
          completed: false
        }
      ];
    default:
      return state;
  }
};
```

### 스토어(Store)
스토어는 애플리케이션의 상태를 저장하고, 액션을 처리하며, 상태 변경을 알립니다.
```javascript
import { createStore } from 'redux';
const store = createStore(rootReducer);
```

### Provider
Provider는 애플리케이션의 모든 컴포넌트가 Redux 스토어에 접근할 수 있게 합니다.
```javascript
import { Provider } from 'react-redux';

const App = () => (
  <Provider store={store}>
    <TodoApp />
  </Provider>
);
```

### connect 함수
connect 함수는 React 컴포넌트를 Redux 스토어에 연결합니다.
```javascript
const mapStateToProps = state => ({
  todos: state.todos
});

const mapDispatchToProps = {
  toggleTodo
};

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList);
```

### React Hooks API
최신 React Redux는 Hooks API를 제공합니다.
```javascript
import { useSelector, useDispatch } from 'react-redux';

// 상태 접근
const todos = useSelector(state => state.todos);

// 액션 디스패치
const dispatch = useDispatch();
dispatch(addTodo('새 할일'));
```

## 4. Redux Toolkit

Redux Toolkit은 Redux 로직 작성을 간소화하는 공식 권장 도구입니다.

```javascript
import { createSlice, configureStore } from '@reduxjs/toolkit';

const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    addTodo: {
      reducer: (state, action) => {
        state.push({
          id: action.payload.id,
          text: action.payload.text,
          completed: false
        });
      },
      prepare: (text) => ({
        payload: { id: Date.now(), text }
      })
    }
  }
});
```

Redux Toolkit의 장점:
- 보일러플레이트 코드 감소
- Immer를 내장하여 상태 불변성 관리 간소화
- 액션 생성자 자동 생성
- 개발자 도구와 미들웨어 설정 간소화

## 5. 실제 애플리케이션에서의 구조
```
src/
├── actions/       # 액션 타입과 액션 생성자
├── reducers/      # 리듀서 함수들
├── store/         # 스토어 설정
├── selectors/     # 상태에서 데이터를 선택하는 함수들
├── components/    # 프레젠테이션 컴포넌트
└── containers/    # Redux에 연결된 컨테이너 컴포넌트
```

Redux Toolkit을 사용하면 이 구조가 단순화됩니다:

```
src/
├── features/      # 기능별 슬라이스(액션+리듀서)
│   ├── todos/
│   └── filters/
├── store/         # 스토어 설정
└── components/    # React 컴포넌트
```
