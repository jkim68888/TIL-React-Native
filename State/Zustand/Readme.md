# React Zustand

Zustand는 React 애플리케이션을 위한 경량 상태 관리 라이브러리입니다. Redux나 MobX와 같은 다른 상태 관리 라이브러리들과 비교했을 때, Zustand는 매우 간단하고 직관적인 API를 제공합니다.

## Zustand의 주요 특징

1. **간결한 API** - 보일러플레이트 코드가 거의 없으며 사용하기 쉽습니다.
2. **작은 번들 크기** - 약 1KB 정도로 매우 가벼운 라이브러리입니다.
3. **React 훅 기반** - React 훅을 활용한 직관적인 사용법을 제공합니다.
4. **컴포넌트 외부에서도 상태 접근 가능** - 컴포넌트 트리 외부에서도 상태에 접근할 수 있습니다.
5. **컨텍스트 없음** - React Context API를 사용하지 않아 불필요한 리렌더링을 방지합니다.

## 기본 사용법

```javascript
// store.js
import create from 'zustand'

// 스토어 생성
const useStore = create((set) => ({
  // 초기 상태
  bears: 0,
  
  // 상태를 변경하는 액션
  increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
  removeAllBears: () => set({ bears: 0 }),
}))

export default useStore
```

```javascript
// BearCounter.js
import useStore from './store'

function BearCounter() {
  // 스토어에서 상태 가져오기
  const bears = useStore((state) => state.bears)
  
  return <h1>{bears} 마리의 곰이 있습니다</h1>
}
```

```javascript
// Controls.js
import useStore from './store'

function Controls() {
  // 스토어에서 액션 가져오기
  const increasePopulation = useStore((state) => state.increasePopulation)
  
  return <button onClick={increasePopulation}>한 마리 추가</button>
}
```

## 선택적 업데이트와 성능 최적화

Zustand는 필요한 상태만 선택적으로 구독할 수 있어 불필요한 리렌더링을 방지합니다.

```javascript
// 필요한 상태만 가져와서 해당 상태가 변경될 때만 리렌더링
const bears = useStore((state) => state.bears)

// 여러 상태를 한번에 가져올 수도 있음
const { bears, tigers } = useStore((state) => ({ 
  bears: state.bears, 
  tigers: state.tigers 
}))

// 객체 비교 방식을 변경하여 최적화 (shallow equality)
import shallow from 'zustand/shallow'
const { bears, tigers } = useStore(
  (state) => ({ bears: state.bears, tigers: state.tigers }),
  shallow
)
```

## 비동기 액션 처리

Zustand에서 비동기 액션 처리는 간단합니다.

```javascript
const useStore = create((set) => ({
  bears: 0,
  fetchBears: async () => {
    const response = await fetch('https://api.example.com/bears')
    const result = await response.json()
    set({ bears: result.count })
  }
}))
```

## 미들웨어 지원

Zustand는 다양한 미들웨어를 제공합니다.

```javascript
import create from 'zustand'
import { persist, devtools } from 'zustand/middleware'

// 로컬 스토리지에 상태 저장
const useStore = create(
  persist(
    (set) => ({
      bears: 0,
      increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
    }),
    { name: 'bear-storage' }
  )
)

// Redux DevTools 지원
const useStore = create(
  devtools(
    (set) => ({
      bears: 0,
      increasePopulation: () => set((state) => ({ bears: state.bears + 1 })),
    })
  )
)
```

<br/>

Zustand는 특히 작은 규모의 프로젝트나 간단한 상태 관리가 필요한 경우에 매우 효과적이며, 상태 관리의 복잡성을 크게 줄여줍니다. 상태 관리를 위한 설정이 적고 학습 곡선이 낮아 빠르게 적용할 수 있다는 것이 큰 장점입니다.