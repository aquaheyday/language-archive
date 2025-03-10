# Flutter 에러 처리 및 디버깅

Flutter에서 에러를 방지하고 디버깅하는 방법을 정리합니다.  
**런타임 에러, 예외 처리, 디버깅 툴, 로그 출력, 크래시 리포팅** 등을 다룹니다.

---

## 1. 에러의 종류

### 1.1 컴파일 에러 (Compile-Time Errors)
- 문법 오류, 타입 오류 등이 원인
- **IDE에서 즉시 감지 가능**
- 예) `int` 타입 변수에 `String` 값을 할당하는 경우

```dart
int number = "Hello"; // 오류 발생!
```

---

### 1.2 런타임 에러 (Runtime Errors)
- 실행 중 발생하는 예외 (예: `null` 참조, 네트워크 오류)
- **`try-catch`로 예외 처리 가능**

```dart
try {
  int result = 100 ~/ 0; // 0으로 나누기 에러 발생
} catch (e) {
  print("에러 발생: $e");
}
```

---

### 1.3 논리 에러 (Logic Errors)
- 코드 실행은 되지만, 예상한 결과가 나오지 않음
- **디버깅과 로그를 활용하여 해결**

```dart
int sum(int a, int b) {
  return a - b; // 논리 오류 (더하기가 아니라 빼기)
}
```

---

## 2. 예외 처리 (`try-catch`)

에러를 처리하지 않으면 앱이 충돌할 수 있으므로, **`try-catch`**를 사용합니다.

```dart
void fetchData() async {
  try {
    final response = await http.get(Uri.parse('https://example.com/data'));
    if (response.statusCode == 200) {
      print("데이터 로드 성공");
    } else {
      throw Exception("데이터 로드 실패");
    }
  } catch (e) {
    print("예외 발생: $e");
  }
}
```

✅ `try-catch`를 사용하여 **비동기 오류 처리 가능**  
✅ `throw`를 사용하여 **명시적으로 예외 발생 가능**  

---

## 3. Flutter 디버깅 도구

### 3.1 `print()` 로그 출력
```dart
print("디버깅 메시지 출력");
```

✅ 간단한 디버깅 시 유용  

---

### 3.2 `debugPrint()`
- **긴 로그를 자동으로 줄여서 출력**
- **로그 길이 제한 없음**

```dart
debugPrint("긴 디버깅 메시지 출력", wrapWidth: 1024);
```

✅ `wrapWidth`를 설정하여 가독성 높이기 가능  

---

### 3.3 `assert()` (개발 환경에서만 동작)
- 특정 조건이 참이 아니면 **앱 실행 중단**
- **릴리즈(Release) 모드에서는 자동 비활성화**

```dart
void checkValue(int value) {
  assert(value > 0, "값이 0보다 커야 합니다!");
}
```

✅ **개발 중 논리 오류 발견에 유용**  
❌ **프로덕션(Release) 빌드에서는 비활성화됨**  

---

## 4. Flutter 디버깅 툴

### 4.1 DevTools (Flutter 공식 디버깅 툴)
- **UI 검사, 성능 모니터링, 로그 확인, 네트워크 요청 확인 가능**
- 실행 방법:
  ```sh
  flutter pub global activate devtools
  flutter run --debug
  ```

✅ UI 프레임 분석, 성능 개선에 유용  

---

### 4.2 `dart:developer`의 `log()`
```dart
import 'dart:developer';

log("디버깅 메시지", name: "MyApp");
```

✅ **필터링이 가능한 로깅 기능 제공**  
✅ **IDE의 Debug Console에서 확인 가능**  

---

### 4.3 Flutter 오류 위젯 (`ErrorWidget`)
Flutter에서는 기본적으로 **에러 발생 시 빨간색 화면**이 나타납니다.  
이를 **커스텀 에러 위젯**으로 변경할 수 있습니다.

```dart
import 'package:flutter/material.dart';

void main() {
  ErrorWidget.builder = (FlutterErrorDetails details) {
    return Center(child: Text("앱에 문제가 발생했습니다!", style: TextStyle(color: Colors.red)));
  };

  runApp(MyApp());
}
```

✅ **사용자 친화적인 에러 메시지 제공 가능**  

---

## 5. 글로벌 예외 처리

**앱 전체에서 발생하는 오류를 처리하려면 `FlutterError.onError`를 사용합니다.**

```dart
void main() {
  FlutterError.onError = (FlutterErrorDetails details) {
    debugPrint("Flutter 오류 발생: ${details.exception}");
  };

  runApp(MyApp());
}
```

✅ **앱 전체에서 예외를 감지하고 로깅 가능**  

---

## 6. 크래시 리포팅 도구

### 6.1 Firebase Crashlytics (에러 리포팅)
- **Flutter 앱에서 오류 발생 시 Firebase에 자동 보고**
- **실제 사용자 환경에서 발생하는 오류 추적 가능**

#### 6.1.1 Firebase Crashlytics 설정 방법
1. Firebase 프로젝트 생성
2. `firebase_crashlytics` 패키지 추가
   ```yaml
   dependencies:
     firebase_core: latest_version
     firebase_crashlytics: latest_version
   ```
3. `main.dart`에 Crashlytics 초기화 추가
   ```dart
   void main() async {
     WidgetsFlutterBinding.ensureInitialized();
     await Firebase.initializeApp();
     FlutterError.onError = FirebaseCrashlytics.instance.recordFlutterError;
     runApp(MyApp());
   }
   ```

✅ **실제 사용자의 충돌 로그 수집 가능**  
✅ **자동 오류 보고 및 분석 가능**  

---

## 7. 결론

| 디버깅 방법 | 설명 |
|-------------|------------------------------|
| `print()` | 간단한 디버깅 메시지 출력 |
| `debugPrint()` | 긴 로그 출력 시 사용 |
| `assert()` | 개발 모드에서 논리 오류 감지 |
| `log()` | `dart:developer`를 이용한 상세 로그 |
| `DevTools` | UI/성능 디버깅 툴 |
| `ErrorWidget` | 기본 에러 화면을 커스텀 가능 |
| `FlutterError.onError` | 앱 전체 예외 감지 및 처리 |
| `Firebase Crashlytics` | 실시간 크래시 리포팅 및 분석 |

Flutter 앱 개발에서 **디버깅과 에러 처리는 필수적**입니다.  
적절한 방법을 사용하여 **안정적인 앱**을 개발하세요! 🚀
