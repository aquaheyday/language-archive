# 🔌 Go 언어 인터페이스

Go에서 **인터페이스(interface)** 는 **특정 메서드 집합을 만족하는 모든 타입**을 나타내는 **추상 타입**입니다.  
Go는 **명시적 implements가 없고**, **암묵적(implicit)으로 자동 구현**된다는 점이 특징입니다.

---

## 1️⃣ 인터페이스 선언

```go
type Speaker interface {
    Speak() string
}
```

✔ `Speaker` 인터페이스는 `Speak()` 메서드를 가진 타입이면 모두 구현된 것으로 간주

---

## 2️⃣ 인터페이스 구현

```go
type Dog struct {
    Name string
}

func (d Dog) Speak() string {
    return d.Name + " says Woof!"
}
```

```go
var s Speaker
s = Dog{Name: "Buddy"}
fmt.Println(s.Speak()) // Buddy says Woof!
```

✔ `Dog` 타입이 `Speak()` 메서드를 가지고 있으므로 `Speaker` 인터페이스를 **자동으로 만족(암묵적 구현)**

---

## 3️⃣ 인터페이스 사용의 이점

- 여러 타입을 하나의 공통된 **행동(메서드)** 로 묶을 수 있음
- **유연하고 확장성 높은 코드** 작성 가능
- **의존성 주입**, **Mock 객체 테스트**, **다형성 구현** 등에 유용

---

## 4️⃣ 다중 인터페이스 구현

인터페이스는 **다른 인터페이스를 포함**할 수 있음  

```go
type Walker interface {
    Walk()
}

type Animal interface {
    Speaker
    Walker
}
```

✔ 타입이 포함된 모든 메서드를 구현하면 자동 만족  

---

## 5️⃣ 타입 어설션 (Type Assertion)

인터페이스를 **원래 타입으로 변환**할 때 사용  

```go
var s Speaker = Dog{Name: "Max"}

d, ok := s.(Dog)
if ok {
    fmt.Println("타입 변환 성공:", d.Name)
} else {
    fmt.Println("타입 변환 실패")
}
```

✔ `s.(Dog)` → `Speaker` 인터페이스 값을 `Dog`로 변환  
✔ 두 번째 리턴 값 `ok`로 성공 여부 판단

---

## 6️⃣ type switch

인터페이스의 실제 타입을 분기 처리할 때 사용  

```go
func describe(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Println("int:", v)
    case string:
        fmt.Println("string:", v)
    default:
        fmt.Println("unknown type")
    }
}
```

---

## 7️⃣ 빈 인터페이스 (`interface{}`)

```go
var any interface{}
any = "Hello"
any = 123
any = []string{"a", "b"}
```

✔ **모든 타입을 받을 수 있는 인터페이스** (`interface{}`)   
✔ Go에서의 최상위 타입 → **"아무 타입이나 받을 수 있다"**  

⚠️ 실제 사용할 땐 타입 어설션 또는 타입 스위치가 필요함  

---

## 8️⃣ 실무 예시: 인터페이스 매개변수

```go
func PrintGreeting(s Speaker) {
    fmt.Println(s.Speak())
}
```

✔ `PrintGreeting()` 함수는 Speaker 인터페이스만 알면 그 안에 어떤 구조체가 들어오든 **동작을 보장**

---

## 9️⃣ 인터페이스 비교

| 항목         | 설명 |
|--------------|------|
| 명시적 구현 | ❌ 없음 (자동 구현) |
| 추상 클래스 | ❌ 없음 |
| 다중 구현    | ✅ 가능 (여러 인터페이스 동시 만족 가능) |
| 용도         | 다형성, 의존성 주입, 테스트, 유연한 설계 등 |

---

## 🎯 정리

✔ Go 인터페이스는 **암묵적으로 구현**되며, 메서드 집합을 기준으로 타입을 추상화합니다.  
✔ 인터페이스는 코드의 **유연성, 테스트 용이성, 확장성**을 높이는 핵심 도구입니다.  
✔ `interface{}`는 모든 값을 담을 수 있는 **빈 인터페이스**지만, 타입 확인이 필요합니다.  
✔ 실무에서는 인터페이스를 통해 **느슨한 결합**과 **모듈화된 설계**를 구현할 수 있습니다.  
