## 💡 키포인트 - 다양한 기능을 "연결(hook into)"하기

<br/>

> 함수형 컴포넌트에서 React의 다양한 기능을 연결할 수 있게 해주는 함수들입니다.

> React의 다양한 기능(상태, 생명주기, 컨텍스트 등)을 함수형 컴포넌트에서 사용할 수 있게 해줍니다. 

> 간단히 말해서, Hook은 "함수형 컴포넌트에서 React의 핵심 기능들을 사용할 수 있게 해주는 API"라고 볼 수 있습니다.

<br/>

### Hook의 역할

1. **상태 관리** ([useState](../../../State/how/Readme.md#usestate), [useReducer](../../../State/how/Readme.md#usereducer))
   - 함수형 컴포넌트에서 상태를 저장하고 업데이트

2. **생명주기 관리** ([useEffect](./useEffect/Readme.md), [useLayoutEffect](./useLayoutEffect/Readme.md))
   - 컴포넌트 마운트, 업데이트, 언마운트 시점에 코드 실행
   - 데이터 fetching, 구독 설정, DOM 조작 등 부수 효과 처리

3. **컨텍스트 접근** ([useContext](../../../State/how/Readme.md#usecontext))
   - 컴포넌트 트리 전체에 걸쳐 데이터 공유

4. **참조 관리** ([useRef](./useRef/Readme.md))
   - DOM 요소에 직접 접근
   - 렌더링을 유발하지 않고 값 저장

5. **최적화** ([useMemo](./useMemo/Readme.md), [useCallback](./useCallback/Readme.md))
   - 계산 결과나 함수 인스턴스 메모이제이션
   - 불필요한 재계산, 재렌더링 방지

6. **사용자 정의 로직 추출** ([커스텀 Hook](./custom/Readme.md))
   - 컴포넌트 간 로직 재사용