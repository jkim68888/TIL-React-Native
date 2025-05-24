# 로컬 폰트 추가 방법 (with Expo)

## 1. .otf 폰트 파일 추가
프로젝트 내에 assets/fonts 폴더를 생성합니다.

.otf 파일을 해당 폴더에 복사합니다. 

```
project-root/
├── assets/
│   └── fonts/
│       └── MyCustomFont.otf
```


## 2. expo 설정에 폰트 등록
expo의 app.json 또는 app.config.js에 [assetBundlePatterns](../assetBundlePatterns/Readme.md) 옵션을 추가합니다.

```json
// app.json
{
  "expo": {
    // ...
    "assetBundlePatterns": ["**/*"],
  }
}
```


## 3. 폰트 로딩 (예: App.tsx 또는 App.js)
```tsx
import React, { useCallback, useEffect, useState } from 'react';
import * as Font from 'expo-font';
import AppLoading from 'expo-app-loading'; // SDK 46 이전
import { Text, View } from 'react-native';

export default function App() {
  const [fontsLoaded, setFontsLoaded] = useState(false);

  const loadFonts = async () => {
    await Font.loadAsync({
      'MyCustomFont': require('./assets/fonts/MyCustomFont.otf'),
    });
    setFontsLoaded(true);
  };

  useEffect(() => {
    loadFonts();
  }, []);

  if (!fontsLoaded) {
    return null; // 또는 <AppLoading /> (Expo SDK 46 이하)
  }

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text style={{ fontFamily: 'MyCustomFont', fontSize: 24 }}>
        커스텀 폰트입니다
      </Text>
    </View>
  );
}
```