# useEffect

React의 `useEffect`는 함수형 컴포넌트에서 사이드 이펙트를 처리하기 위한 Hook입니다. 컴포넌트의 생명주기와 관련된 작업을 수행할 수 있습니다.

## 기본 구조

```javascript
useEffect(() => {
  // 사이드 이펙트 코드
  return () => {
    // 정리(cleanup) 함수 (선택사항)
  };
}, [dependencies]); // 의존성 배열 (선택사항)
```

## 사용 패턴

**1. 컴포넌트 마운트 시에만 실행**
```javascript
useEffect(() => {
  console.log('컴포넌트가 마운트되었습니다');
  
  // API 호출
  fetchUserData();
}, []); // 빈 배열 = 마운트 시에만 실행
```

**2. 특정 값이 변경될 때마다 실행**
```javascript
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // userId가 변경될 때마다 실행

  return <div>{user?.name}</div>;
}
```

**3. 매 렌더링마다 실행**
```javascript
useEffect(() => {
  console.log('매번 실행됩니다');
  document.title = `카운트: ${count}`;
}); // 의존성 배열 없음 = 매번 실행
```

## 정리(Cleanup) 함수

```javascript
useEffect(() => {
  // 구독 설정
  const subscription = subscribeTo(something);
  
  // 타이머 설정
  const timer = setInterval(() => {
    console.log('타이머 실행');
  }, 1000);

  // 정리 함수 - 컴포넌트 언마운트나 의존성 변경 시 실행
  return () => {
    subscription.unsubscribe();
    clearInterval(timer);
  };
}, []);
```

## 실제 사용 예시

**1. 데이터 fetching**
```javascript
function PostList() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const response = await fetch('/api/posts');
        const data = await response.json();
        setPosts(data);
      } catch (error) {
        console.error('데이터 로딩 실패:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  if (loading) return <div>로딩 중...</div>;
  return (
    <div>
      {posts.map(post => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}
```

**2. 이벤트 리스너 등록**
```javascript
function WindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };

    window.addEventListener('resize', handleResize);

    // 정리 함수
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return <div>{windowSize.width} x {windowSize.height}</div>;
}
```

**3. 타이머와 인터벌**
```javascript
function Timer() {
  const [seconds, setSeconds] = useState(0);
  const [isActive, setIsActive] = useState(false);

  useEffect(() => {
    let interval = null;
    
    if (isActive) {
      interval = setInterval(() => {
        setSeconds(seconds => seconds + 1);
      }, 1000);
    }

    return () => {
      if (interval) clearInterval(interval);
    };
  }, [isActive]);

  return (
    <div>
      <div>시간: {seconds}초</div>
      <button onClick={() => setIsActive(!isActive)}>
        {isActive ? '정지' : '시작'}
      </button>
    </div>
  );
}
```

## 여러 useEffect 사용

```javascript
function UserDashboard({ userId }) {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);

  // 사용자 정보 로딩
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]);

  // 사용자 게시물 로딩
  useEffect(() => {
    fetchUserPosts(userId).then(setPosts);
  }, [userId]);

  // 페이지 타이틀 업데이트
  useEffect(() => {
    if (user) {
      document.title = `${user.name}의 대시보드`;
    }
  }, [user]);

  return (
    <div>
      {user && <h1>{user.name}</h1>}
      {posts.map(post => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}
```

## 주의사항

**1. 무한 루프 주의**
```javascript
// 잘못된 예시 - 무한 루프
useEffect(() => {
  setData(processData(data));
}, [data]); // data가 변경되면 다시 setData 호출

// 올바른 예시
useEffect(() => {
  setData(processData(originalData));
}, [originalData]);
```

**2. 의존성 배열 누락**
```javascript
function Timer({ delay }) {
  const [count, setCount] = useState(0);

  // 잘못된 예시 - delay 의존성 누락
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(c => c + 1);
    }, delay);
    return () => clearInterval(timer);
  }, []); // delay가 변경되어도 타이머 간격이 업데이트되지 않음

  // 올바른 예시
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(c => c + 1);
    }, delay);
    return () => clearInterval(timer);
  }, [delay]); // delay 의존성 포함
}
```

<br />

`useEffect`는 React 함수형 컴포넌트에서 사이드 이펙트를 관리하는 핵심 도구로, 올바른 의존성 배열 관리와 정리 함수 사용이 중요합니다.