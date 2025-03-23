# 🧩 Go 언어 함수 선언과 호출

Go 언어는 **함수형 스타일을 지원하면서도 간결한 함수 선언 문법**을 제공합니다.  
모든 함수는 `func` 키워드로 선언되며, **다중 반환값, 가변 인자, 클로저**도 지원됩니다.  

---

## 1️⃣ 함수 기본 구조

```go
func 함수이름(매개변수) 반환타입 {
    // 실행 코드
    return 값
}
```

#### 예제

```go
func add(a int, b int) int {
    return a + b
}
```

✔ 매개변수가 여러 개고 타입이 같을 경우 생략 가능 `func add(a, b int) int`

---

## 2️⃣ 함수 호출

```go
result := add(2, 3)
fmt.Println(result) // 5
```

---

## 3️⃣ 반환값이 여러 개인 함수

```go
func divide(a, b int) (int, int) {
    return a / b, a % b
}
```

```go
quotient, remainder := divide(10, 3)
fmt.Println(quotient, remainder) // 3 1
```

✔ Go는 **다중 반환값(multiple return values)**을 기본 지원합니다.

---

## 4️⃣ 이름 있는 반환값 (Named Return)

```go
func getUser() (name string, age int) {
    name = "Alice"
    age = 30
    return // 이름 있는 경우 return만으로 반환 가능
}
```

✔ 명시적으로 `return name, age` 하지 않아도 됩니다.

---

## 5️⃣ 가변 인자 함수 (`...` 사용)

```go
func sum(nums ...int) int {
    total := 0
    for _, v := range nums {
        total += v
    }
    return total
}
```

```go
result := sum(1, 2, 3, 4) // 총합: 10
```

✔ `...int`는 `[]int` 슬라이스로 처리됩니다.

---

## 6️⃣ 함수도 값이다 (함수 타입 변수)

```go
func multiply(a, b int) int {
    return a * b
}

var op func(int, int) int = multiply
fmt.Println(op(3, 4)) // 12
```

✔ 함수는 변수에 저장하거나 매개변수로 전달 가능

---

## 7️⃣ 익명 함수 (함수 리터럴)

```go
add := func(a, b int) int {
    return a + b
}
fmt.Println(add(5, 6)) // 11
```

✔ 익명 함수는 변수에 담거나 즉시 호출 가능

```go
func() {
    fmt.Println("즉시 실행 함수")
}()
```

---

## 8️⃣ 클로저 (Closure)

```go
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

c := counter()
fmt.Println(c()) // 1
fmt.Println(c()) // 2
```

✔ **외부 변수에 접근하고 상태를 유지하는 함수**를 클로저라고 합니다.

---

## 9️⃣ 함수 네이밍 규칙

| 규칙           | 설명 |
|----------------|------|
| 소문자 시작    | 패키지 내부(private) 함수 |
| 대문자 시작    | 외부 패키지에서 접근 가능한 공개 함수(exported) |
| 간결한 이름    | Go 스타일 권장: `calc`, `sum`, `doTask` 등 |
| 접두사 사용 지양 | `Get`, `Set`보다 직관적인 이름 권장 (`Name()` 대신 `UserName()` 등) |

---

## 🎯 정리

✔ 함수는 `func` 키워드로 선언하며, **매개변수와 반환타입을 명시**합니다.  
✔ Go는 **다중 반환값**을 기본으로 지원하며, **클로저 및 익명 함수**도 작성할 수 있습니다.  
✔ 함수는 값으로 다룰 수 있으며, **변수에 저장하거나 전달 가능**합니다.  
✔ `...` 문법으로 가변 인자도 처리할 수 있습니다.
