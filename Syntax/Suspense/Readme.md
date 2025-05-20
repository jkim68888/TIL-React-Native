# React Suspense

React Suspense는 데이터 불러오기와 같은 비동기 작업을 처리할 때 컴포넌트의 렌더링을 "일시 중단(suspend)"할 수 있게 해주는 React의 기능입니다. 이를 통해 비동기 처리를 더 선언적이고 일관된 방식으로 관리할 수 있습니다.

```jsx
function ProfilePage() {
  return (
    <div>
      <NavBar />
      <Suspense fallback={<h1>페이지 로딩 중...</h1>}>
        <ProfileHeader />
        
        <Suspense fallback={<div>사용자 정보 로딩 중...</div>}>
          <ProfileDetails />
        </Suspense>
        
        <Suspense fallback={<div>게시물 로딩 중...</div>}>
          <ProfilePosts />
        </Suspense>
      </Suspense>
      <Footer />
    </div>
  );
}
```