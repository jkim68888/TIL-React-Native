# React-Native APIs

React Native에서는 **모바일 앱 개발에 필요한 다양한 기본 API**들을 제공합니다. 이 API들은 브리지(Bridge)를 통해 네이티브(Android/iOS) 기능에 접근할 수 있게 도와줍니다.


## 주요 React Native API 카테고리별 정리

### 1. 기기 기능 접근 (Device APIs)

| API                  | 설명                                |
| -------------------- | --------------------------------- |
| `CameraRoll`         | 사진 앨범 접근 (deprecated, 라이브러리로 대체됨) |
| `Clipboard`          | 클립보드 복사/붙여넣기                      |
| `Geolocation`        | 위치 정보 (현재는 community module로 사용)  |
| `Vibration`          | 진동                                |
| `PermissionsAndroid` | Android 권한 요청 처리                  |
| `Linking`            | 외부 링크 열기, 앱 간 이동 등                |
| `BackHandler`        | 안드로이드 백 버튼 처리                     |



### 2. UI 및 사용자 인터랙션

| API               | 설명                    |
| ----------------- | --------------------- |
| `Dimensions`      | 화면 크기 가져오기            |
| `PixelRatio`      | 픽셀 밀도 관련 계산           |
| `Keyboard`        | 키보드 상태 감지 및 제어        |
| `Appearance`      | 다크모드/라이트모드 감지         |
| `ToastAndroid`    | Android 전용 Toast 메시지  |
| `Alert`           | 플랫폼 공통 Alert 다이얼로그 표시 |
| `Animated`        | 고성능 애니메이션 처리          |
| `LayoutAnimation` | 레이아웃 변경 애니메이션         |



### 3. 네트워크 및 저장소

| API             | 설명                                          |
| --------------- | ------------------------------------------- |
| `Fetch` (전역 함수) | 네트워크 요청 처리                                  |
| `NetInfo`       | 네트워크 상태 확인 (React Native Community 모듈로 분리됨) |
| `AsyncStorage`  | 로컬 key-value 저장소 (현재는 community 모듈 사용 권장)   |



### 4. App 관련 정보

| API           | 설명                              |
| ------------- | ------------------------------- |
| `AppState`    | 앱의 상태 (active, background 등) 감지 |
| `Platform`    | iOS/Android 분기 처리               |
| `DevSettings` | 개발자 설정 (디버깅 시 사용)               |


## 그 외 외부 API (Community 제공)

React Native 공식 API 외에 **react-native-community** 혹은 **expo**에서 더 많은 고급 기능을 제공하는 API들을 쓸 수 있습니다.

* `react-native-camera`, `react-native-image-picker` (카메라 및 이미지 선택)
* `react-native-maps` (지도)
* `react-native-push-notification` (푸시 알림)
* `react-native-gesture-handler`, `react-native-reanimated` (제스처 및 애니메이션)
