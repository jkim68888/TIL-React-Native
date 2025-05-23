# useLayoutEffect

React의 `useLayoutEffect`는 `useEffect`와 매우 유사하지만, 실행 타이밍에서 중요한 차이가 있는 Hook입니다.

## useEffect vs useLayoutEffect 실행 타이밍

```javascript
// useEffect - 비동기적으로 실행 (브라우저가 화면을 그린 후)
useEffect(() => {
  console.log('useEffect 실행');
}, []);

// useLayoutEffect - 동기적으로 실행 (브라우저가 화면을 그리기 전)
useLayoutEffect(() => {
  console.log('useLayoutEffect 실행');
}, []);
```

**실행 순서:**
1. 컴포넌트 렌더링
2. DOM 변경사항 적용
3. **useLayoutEffect 실행** (화면에 그리기 전)
4. 브라우저가 화면에 그리기
5. **useEffect 실행** (화면에 그린 후)

## 사용 사례

**1. DOM 측정이 필요한 경우**
```javascript
function MeasureComponent() {
  const [height, setHeight] = useState(0);
  const divRef = useRef(null);

  useLayoutEffect(() => {
    // DOM이 업데이트된 직후, 화면에 그리기 전에 측정
    if (divRef.current) {
      const rect = divRef.current.getBoundingClientRect();
      setHeight(rect.height);
    }
  }, []);

  return (
    <div>
      <div ref={divRef}>측정할 내용</div>
      <p>높이: {height}px</p>
    </div>
  );
}
```

**2. 깜빡임 방지**
```javascript
function AnimatedComponent() {
  const [position, setPosition] = useState(0);
  const elementRef = useRef(null);

  // useEffect 사용 시 - 깜빡임 발생 가능
  useEffect(() => {
    if (elementRef.current) {
      elementRef.current.style.transform = `translateX(${position}px)`;
    }
  }, [position]);

  // useLayoutEffect 사용 시 - 깜빡임 없음
  useLayoutEffect(() => {
    if (elementRef.current) {
      elementRef.current.style.transform = `translateX(${position}px)`;
    }
  }, [position]);

  return (
    <div>
      <div ref={elementRef}>애니메이션 요소</div>
      <button onClick={() => setPosition(position + 100)}>
        이동
      </button>
    </div>
  );
}
```

**3. 스크롤 위치 복원**
```javascript
function ScrollRestoreComponent({ items }) {
  const containerRef = useRef(null);
  const [savedScrollTop, setSavedScrollTop] = useState(0);

  useLayoutEffect(() => {
    // 아이템이 변경된 후 즉시 스크롤 위치 복원
    if (containerRef.current) {
      containerRef.current.scrollTop = savedScrollTop;
    }
  }, [items, savedScrollTop]);

  const handleScroll = (e) => {
    setSavedScrollTop(e.target.scrollTop);
  };

  return (
    <div 
      ref={containerRef} 
      onScroll={handleScroll}
      style={{ height: '300px', overflow: 'auto' }}
    >
      {items.map(item => (
        <div key={item.id}>{item.content}</div>
      ))}
    </div>
  );
}
```

**4. 툴팁 위치 조정**
```javascript
function Tooltip({ targetRef, content, visible }) {
  const tooltipRef = useRef(null);
  const [position, setPosition] = useState({ top: 0, left: 0 });

  useLayoutEffect(() => {
    if (visible && targetRef.current && tooltipRef.current) {
      const targetRect = targetRef.current.getBoundingClientRect();
      const tooltipRect = tooltipRef.current.getBoundingClientRect();
      
      // 화면에 그리기 전에 위치 계산 및 조정
      let top = targetRect.bottom + 5;
      let left = targetRect.left;
      
      // 화면을 벗어나면 위치 조정
      if (left + tooltipRect.width > window.innerWidth) {
        left = window.innerWidth - tooltipRect.width - 10;
      }
      
      if (top + tooltipRect.height > window.innerHeight) {
        top = targetRect.top - tooltipRect.height - 5;
      }
      
      setPosition({ top, left });
    }
  }, [visible, content]);

  if (!visible) return null;

  return (
    <div
      ref={tooltipRef}
      style={{
        position: 'fixed',
        top: position.top,
        left: position.left,
        backgroundColor: 'black',
        color: 'white',
        padding: '8px',
        borderRadius: '4px',
        zIndex: 1000
      }}
    >
      {content}
    </div>
  );
}
```

## 언제 사용해야 하는가?

**useLayoutEffect를 사용해야 하는 경우:**
- DOM 측정이 필요한 경우
- 시각적 깜빡임을 방지해야 하는 경우
- 스크롤 위치나 포커스 관리
- 애니메이션 전 DOM 조작이 필요한 경우

**useEffect를 사용해야 하는 경우:**
- 데이터 fetching
- 구독 설정/해제
- 타이머 설정
- 대부분의 사이드 이펙트

## 성능 고려사항

```javascript
// useLayoutEffect는 동기적으로 실행되므로 무거운 작업은 피해야 함
useLayoutEffect(() => {
  // ❌ 피해야 할 것 - 무거운 계산
  const result = heavyComputation();
  
  // ✅ 권장 - 빠른 DOM 조작
  element.style.transform = 'translateX(100px)';
}, []);
```

**주의사항:**
- `useLayoutEffect`는 동기적으로 실행되므로 무거운 작업을 수행하면 UI가 블로킹될 수 있습니다
- 대부분의 경우 `useEffect`로 충분하며, 시각적 문제가 있을 때만 `useLayoutEffect`를 사용하는게 좋습니다.
- 서버 사이드 렌더링에서는 `useLayoutEffect`가 경고를 발생시킬 수 있습니다

<br/>

`useLayoutEffect`는 DOM 조작과 관련된 시각적 문제를 해결할 때 사용하는 특수한 Hook입니다.