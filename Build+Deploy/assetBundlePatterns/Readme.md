# assetBundlePatterns

Expo 앱을 빌드할 때, 프로젝트 안에 있는 폰트/이미지/사운드 같은 리소스를 번들(앱 내부 패키지)로 포함할지 말지 결정하는 옵션입니다.

``` json
"assetBundlePatterns": ["**/*"]
```
**/*는 모든 하위 디렉토리의 모든 파일을 포함하겠다는 의미입니다.

<br/>

즉, assets/fonts/*.otf 도 자동으로 앱 안에 포함됩니다.

❗ 이걸 안 쓰면 어떻게 되냐면,
앱이 빌드되고 나서 시뮬레이터나 기기에서 실행할 때,

`require('./assets/fonts/JalnanOTF.otf')` 로 폰트를 불러오려 하지만,

파일이 실제 앱 번들에 포함되지 않아서 에러가 납니다.

<br/>

### ✅ assetBundlePatterns 설정됨

빌드 시 .otf 파일이 앱 내부에 포함됨 → 정상 동작


### ❌ 설정 안함

	빌드 시 폰트 파일 누락 → 앱에서 찾을 수 없음 → 에러 발생


### app.json
```json
{
  "expo": {
    "name": "MyApp",
    "slug": "my-app",
    "version": "1.0.0",
    "assetBundlePatterns": ["**/*"] // ← 폰트, 이미지 등 포함하려면 꼭 필요!
  }
}
```
