# AppState

React Native의 AppState는 앱이 현재 어떤 상태에 있는지를 추적하고 상태 변화를 감지할 수 있게 해주는 API입니다.

## AppState의 세 가지 상태

**active**: 앱이 포그라운드에서 실행 중이고 사용자가 상호작용할 수 있는 상태입니다.

**background**: 앱이 백그라운드에서 실행 중인 상태입니다. 사용자가 홈 버튼을 누르거나 다른 앱으로 전환했을 때 이 상태가 됩니다.

**inactive**: 앱이 포그라운드와 백그라운드 사이의 전환 상태에 있거나, 멀티태스킹 뷰, 전화 수신, SMS 알림 등으로 인해 일시적으로 비활성화된 상태입니다.

## 주요 메서드

`AppState.currentState`를 통해 현재 상태를 확인할 수 있고, `AppState.addEventListener()`로 상태 변화를 감지할 수 있습니다.

## 사용 예시

```javascript
import { AppState } from 'react-native';

// 현재 상태 확인
console.log(AppState.currentState);

// 상태 변화 감지
const handleAppStateChange = (nextAppState) => {
  if (nextAppState === 'active') {
    console.log('앱이 활성화됨');
  } else if (nextAppState === 'background') {
    console.log('앱이 백그라운드로 이동');
  }
};

AppState.addEventListener('change', handleAppStateChange);
```

## 실제 활용 사례

앱이 백그라운드로 갈 때 타이머를 일시정지하거나, 포그라운드로 돌아올 때 데이터를 새로고침하거나, 보안상 민감한 화면을 숨기는 등의 용도로 많이 사용됩니다. 또한 앱 사용 시간을 추적하거나 백그라운드에서의 특정 작업을 제어할 때도 유용합니다.