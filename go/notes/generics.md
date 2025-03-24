# 🔁 Go 언어 제네릭 (Go 1.18+)

Go 1.18부터 도입된 **제네릭(Generics)** 은 **타입에 의존하지 않는 재사용 가능한 함수와 타입**을 정의할 수 있게 해줍니다.

---

## 1️⃣ 제네릭이란?

- 함수나 타입을 선언할 때 **타입을 매개변수로 받을 수 있는 기능**
- 코드의 **유연성**과 **재사용성** 향상
- 타입 안정성(type safety)을 유지하면서 다양한 타입 처리 가능

---

## 2️⃣ 제네릭 함수 기본 구조

#### 예시 코드
```go
func PrintSlice[T any](s []T) {
    for _, v := range s {
        fmt.Println(v)
    }
}
```

```go
PrintSlice([]int{1, 2, 3})
PrintSlice([]string{"a", "b", "c"})
```

✔ `T`는 타입 매개변수 (Type Parameter)  
✔ `any`는 모든 타입을 의미 (`interface{}`의 대체 표현)  
✔ 함수 사용 시 타입을 **명시하거나 생략** 가능

---

## 3️⃣ 제네릭 타입 정의

#### 예시 코드
```go
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() T {
    n := len(s.items)
    item := s.items[n-1]
    s.items = s.items[:n-1]
    return item
}
```

```go
s := Stack[int]{}
s.Push(10)
s.Push(20)
fmt.Println(s.Pop()) // 출력: 20
```

✔ `Stack[T]`는 T 타입 요소를 갖는 스택  
✔ 메서드 정의 시에도 `[T]` 붙여야 함 (`func (s *Stack[T])`)  

---

## 4️⃣ 타입 제약 (Type Constraint)

- 제네릭 타입 매개변수에 **조건을 부여**할 수 있음
- 예: `int`, `float64`처럼 연산 가능한 타입만 허용

#### 예시 코드
```go
type Number interface {
    int | float64
}

func Sum[T Number](a, b T) T {
    return a + b
}
```

```go
fmt.Println(Sum(3, 5))         // int
fmt.Println(Sum(2.5, 1.5))     // float64
```

✔ `|`는 여러 타입을 나열하는 합집합(union)  
✔ Go 1.18+에선 타입 집합을 통한 제한 가능  

---

## 5️⃣ comparable 제약

Go에서는 `==`, `!=` 연산을 지원하는 타입을 `comparable`로 정의  

#### 예시 코드
```go
func Equal[T comparable](a, b T) bool {
    return a == b
}
```

```go
fmt.Println(Equal(10, 10))       // true
fmt.Println(Equal("go", "lang")) // false
```

✔ `comparable`은 map 키로 사용할 수 있는 타입과 동일  

---

## 6️⃣ 여러 타입 매개변수 사용

#### 예시 코드
```go
func Pair[K, V any](key K, value V) {
    fmt.Printf("Key: %v, Value: %v\n", key, value)
}

Pair[string, int]("age", 30)
```

✔ 여러 타입을 동시에 제네릭으로 지정 가능  
✔ 사용 시 타입을 명시하거나 생략 가능 (Go가 타입 추론함)  

---

## 7️⃣ 타입 추론과 명시적 지정

Go는 대부분의 경우 **타입을 추론**할 수 있지만, 명시적으로 지정할 수도 있음

#### 예시 코드
```go
Sum[int](1, 2)         // 명시적 지정
Sum(1, 2)              // 추론
```

---

## 🎯 정리

| 개념 | 설명 |
|------|------|
| `T any` | 모든 타입 허용 |
| `comparable` | 비교 가능한 타입 (==, !=) |
| `|` 연산자 | 타입 합집합 제한 |
| `func[T Type]` | 제네릭 함수 선언 |
| `type Struct[T]` | 제네릭 타입 선언 |

✔ 제네릭은 Go의 코드 재사용성을 높이는 강력한 기능  
✔ `any`, `comparable`, `interface`, 타입 집합 등을 활용해 유연하고 안전한 제네릭 코드 작성 가능  
✔ 제네릭은 성능 저하 없이 타입 안정성과 유연성을 동시에 제공  
