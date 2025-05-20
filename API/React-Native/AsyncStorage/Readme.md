# React AsyncStorage

AsyncStorage는 React Native에서 비동기적으로 데이터를 로컬에 저장할 수 있게 해주는 API이며, 간단한 키-값 기반의 저장소입니다.

## AsyncStorage의 주요 특징

1. **비동기 API** - 모든 작업이 Promise 기반으로 비동기적으로 처리됩니다.
2. **전역 저장소** - 애플리케이션 전체에서 접근 가능한 글로벌 저장소입니다.
3. **간단한 키-값 저장소** - 문자열 키를 기준으로 데이터를 저장합니다.
4. **모바일 플랫폼 지원** - iOS와 Android 모두에서 작동합니다.
5. **단순한 API** - 사용하기 쉬운 간단한 API를 제공합니다.

## 기본 사용법

AsyncStorage는 React Native에서 기본으로 제공되었지만, 최근 버전에서는 별도의 패키지로 분리되었습니다. 따라서 별도로 설치해야 합니다.

```bash
npm install @react-native-async-storage/async-storage
```

### 데이터 저장하기

```javascript
import AsyncStorage from '@react-native-async-storage/async-storage';

const storeData = async (key, value) => {
  try {
    // 문자열이 아닌 값은 JSON.stringify로 변환해야 함
    const jsonValue = typeof value === 'string' ? value : JSON.stringify(value);
    await AsyncStorage.setItem(key, jsonValue);
    console.log('데이터 저장 성공');
  } catch (e) {
    console.error('데이터 저장 실패', e);
  }
};

// 사용 예시
storeData('username', '홍길동');
storeData('userProfile', { id: 1, name: '홍길동', age: 30 });
```

### 데이터 불러오기

```javascript
import AsyncStorage from '@react-native-async-storage/async-storage';

const getData = async (key) => {
  try {
    const value = await AsyncStorage.getItem(key);
    if (value !== null) {
      // 저장된 값이 있는 경우
      try {
        // JSON 형태인지 확인 시도
        return JSON.parse(value);
      } catch (e) {
        // JSON이 아닌 경우 문자열 그대로 반환
        return value;
      }
    }
    return null; // 저장된 값이 없는 경우
  } catch (e) {
    console.error('데이터 불러오기 실패', e);
    return null;
  }
};

// 사용 예시
const username = await getData('username');
const userProfile = await getData('userProfile');
```

### 데이터 삭제하기

```javascript
import AsyncStorage from '@react-native-async-storage/async-storage';

const removeData = async (key) => {
  try {
    await AsyncStorage.removeItem(key);
    console.log('데이터 삭제 성공');
  } catch (e) {
    console.error('데이터 삭제 실패', e);
  }
};

// 사용 예시
removeData('username');
```

### 여러 항목 한번에 처리하기

```javascript
// 여러 항목 저장
const storeMultipleData = async () => {
  try {
    const firstPair = ['@MyApp_user', JSON.stringify({name: '홍길동'})];
    const secondPair = ['@MyApp_key', 'value'];
    
    await AsyncStorage.multiSet([firstPair, secondPair]);
  } catch (e) {
    console.error('다중 저장 실패', e);
  }
};

// 여러 항목 불러오기
const getMultipleData = async () => {
  try {
    const values = await AsyncStorage.multiGet(['@MyApp_user', '@MyApp_key']);
    return values.map(item => {
      // 첫 번째 요소는 키, 두 번째 요소는 값
      try {
        return [item[0], JSON.parse(item[1])];
      } catch (e) {
        return item;
      }
    });
  } catch (e) {
    console.error('다중 불러오기 실패', e);
  }
};
```

### 모든 데이터 삭제하기

```javascript
const clearAll = async () => {
  try {
    await AsyncStorage.clear();
    console.log('모든 데이터 삭제 성공');
  } catch (e) {
    console.error('데이터 삭제 실패', e);
  }
};
```

## React 훅과 함께 사용하기

AsyncStorage를 React 훅과 함께 사용하여 더 편리하게 상태 관리를 할 수 있습니다.

```javascript
import React, { useState, useEffect } from 'react';
import AsyncStorage from '@react-native-async-storage/async-storage';

function useAsyncStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(initialValue);
  
  // 초기에 저장된 값 불러오기
  useEffect(() => {
    const loadStoredValue = async () => {
      try {
        const item = await AsyncStorage.getItem(key);
        const value = item ? JSON.parse(item) : initialValue;
        setStoredValue(value);
      } catch (error) {
        console.error(error);
        setStoredValue(initialValue);
      }
    };
    
    loadStoredValue();
  }, [key, initialValue]);
  
  // 값을 업데이트하고 AsyncStorage에 저장하는 함수
  const setValue = async (value) => {
    try {
      // 함수도 처리 가능하도록 함
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      await AsyncStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [storedValue, setValue];
}

// 사용 예시
function UserProfile() {
  const [user, setUser] = useAsyncStorage('user', { name: '', email: '' });
  
  const updateUserName = (name) => {
    setUser({...user, name});
  };
  
  return (
    // 컴포넌트 내용
  );
}
```

## 주의사항

1. **저장 용량 제한** - AsyncStorage는 모바일 기기의 저장 공간을 사용하므로 대용량 데이터 저장에는 적합하지 않습니다.
2. **암호화 없음** - 민감한 정보(비밀번호, 토큰 등)는 별도의 암호화 없이 저장되므로 주의가 필요합니다.
3. **비동기 처리** - 모든 작업이 비동기적으로 처리되므로 적절한 에러 처리가 필요합니다.
4. **문자열만 저장 가능** - 객체나 배열은 JSON 형태로 변환하여 저장해야 합니다.

<br/>

AsyncStorage는 간단한 데이터 지속성이 필요한 React Native 앱에서 매우 유용하게 사용될 수 있으며, 사용자 설정, 로그인 상태, 캐시 데이터 등을 저장하는 데 적합합니다.