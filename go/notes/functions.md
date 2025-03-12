# Go (Golang) 함수와 메서드 정리

Go에서 **함수(Function)**와 **메서드(Method)**는 코드의 재사용성을 높이고 구조화하는 중요한 개념입니다.  
둘 다 특정 작업을 수행하는 코드 블록이지만, 메서드는 **구조체(Struct)와 연결된 함수**라는 점이 다릅니다.  

---

## 1. 함수 (Function)

함수는 **독립적인 코드 블록**으로, 특정 기능을 수행하기 위해 사용됩니다.

```go
package main

import "fmt"

// 함수 정의
func add(a int, b int) int {
    return a + b
}

func main() {
    result := add(5, 10) // 함수 호출
    fmt.Println("결과:", result)
}
```

함수는 `func` 키워드로 정의하며, 매개변수와 반환 타입을 지정할 수 있습니다.  
`return` 키워드를 사용하여 값을 반환할 수 있습니다.

---

### 1.1 함수의 여러 반환값

Go에서는 **하나의 함수에서 여러 개의 값을 반환할 수 있습니다.**

```go
package main

import "fmt"

// 두 개의 값을 반환하는 함수
func swap(a, b int) (int, int) {
    return b, a
}

func main() {
    x, y := swap(3, 7)
    fmt.Println(x, y) // 출력: 7 3
}
```

**여러 값을 반환하는 기능은 Go의 주요 특징 중 하나입니다.**  
**반환값을 `_` (언더스코어)로 무시할 수도 있습니다.**

```go
_, y := swap(3, 7) // 첫 번째 반환값을 무시
fmt.Println(y) // 출력: 3
```

---

### 1.2 함수에서 가변 인자 사용 (Variadic Function)

가변 인자 함수는 **매개변수 개수가 정해지지 않은 함수**를 정의할 때 사용합니다.

```go
package main

import "fmt"

// 가변 인자 함수
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

func main() {
    fmt.Println(sum(1, 2, 3))       // 출력: 6
    fmt.Println(sum(10, 20, 30, 40)) // 출력: 100
}
```

**`...int`를 사용하면 가변 개수의 정수를 받을 수 있습니다.**  
**내부적으로 슬라이스(`[]int`)로 처리됩니다.**

---

## 2. 메서드 (Method)

메서드는 **구조체(Struct)와 연결된 함수**입니다.  
즉, 특정 구조체의 데이터를 다루는 함수입니다.

```go
package main

import "fmt"

// 구조체 정의
type Person struct {
    Name string
    Age  int
}

// 메서드 정의 (Person 구조체와 연결)
func (p Person) introduce() {
    fmt.Printf("안녕하세요, 제 이름은 %s이고 %d살입니다.\n", p.Name, p.Age)
}

func main() {
    person := Person{Name: "Alice", Age: 25}
    person.introduce() // 메서드 호출
}
```

메서드는 함수와 달리 특정 **구조체(Struct)와 연결**됩니다.  
`p Person`처럼 **리시버(Receiver)**를 사용하여 해당 구조체의 데이터를 다룰 수 있습니다.

---

### 2.1 값 리시버 vs 포인터 리시버

메서드를 정의할 때 **리시버(Receiver)를 값(Value)으로 받을지, 포인터(Pointer)로 받을지** 선택할 수 있습니다.

#### (1) 값 리시버 (Value Receiver)
```go
package main

import "fmt"

type Rectangle struct {
    Width, Height int
}

// 값 리시버 메서드
func (r Rectangle) Area() int {
    return r.Width * r.Height
}

func main() {
    rect := Rectangle{Width: 5, Height: 10}
    fmt.Println("넓이:", rect.Area()) // 출력: 50
}
```
**값 리시버**는 메서드 내부에서 구조체 값을 **복사**해서 사용합니다.  
따라서 **원본 값이 변경되지 않습니다.**  

---

#### (2) 포인터 리시버 (Pointer Receiver)
```go
package main

import "fmt"

type Counter struct {
    Value int
}

// 포인터 리시버 메서드
func (c *Counter) Increment() {
    c.Value++
}

func main() {
    counter := Counter{Value: 10}
    counter.Increment() // 값 증가
    fmt.Println("카운터 값:", counter.Value) // 출력: 11
}
```
**포인터 리시버**를 사용하면 원본 값을 직접 수정할 수 있습니다.  
**`*Counter`를 사용하면 구조체의 실제 값이 변경됩니다.**

**📌 언제 값 리시버 vs 포인터 리시버를 사용할까?**
| 리시버 유형 | 특징 | 사용 시기 |
|------------|----------------|------------------|
| **값 리시버** (`func (r Rectangle)`) | 구조체 값을 복사하여 사용 | 값이 변경되지 않는 경우 (읽기 전용) |
| **포인터 리시버** (`func (c *Counter)`) | 원본 값이 직접 변경됨 | 값이 변경되는 경우 (쓰기 작업) |

---

## 3. 익명 함수 (Anonymous Function)

익명 함수는 **이름 없이 즉시 실행하거나 변수에 저장하여 사용할 수 있는 함수**입니다.

```go
package main

import "fmt"

func main() {
    // 즉시 실행되는 익명 함수
    func() {
        fmt.Println("익명 함수 실행!")
    }()

    // 변수를 사용하여 익명 함수 할당
    add := func(a, b int) int {
        return a + b
    }

    fmt.Println(add(3, 4)) // 출력: 7
}
```

**익명 함수는 즉시 실행할 수도 있고, 변수에 저장하여 나중에 사용할 수도 있습니다.**  

---

## 4. 고차 함수 (Higher-Order Function)

Go에서는 함수를 **다른 함수의 매개변수로 전달하거나 반환할 수 있습니다.**  
이를 **고차 함수(Higher-Order Function)**라고 합니다.

```go
package main

import "fmt"

// 함수를 매개변수로 받는 함수
func applyFunction(f func(int, int) int, a int, b int) int {
    return f(a, b)
}

// 덧셈 함수
func add(x, y int) int {
    return x + y
}

func main() {
    result := applyFunction(add, 10, 20)
    fmt.Println("결과:", result) // 출력: 30
}
```

**`applyFunction`은 `add` 함수를 매개변수로 받아 실행합니다.**  
**이런 방식은 콜백 함수나 함수형 프로그래밍을 구현할 때 유용합니다.**

---

## 5. 함수와 메서드의 차이점

| 구분 | 함수(Function) | 메서드(Method) |
|------|--------------|--------------|
| **정의 방식** | `func 함수이름() {...}` | `func (리시버 타입) 메서드이름() {...}` |
| **연결 대상** | 독립적인 코드 블록 | 특정 구조체(Struct)와 연결 |
| **사용 예시** | `func add(a, b int) int` | `func (p Person) introduce()` |
| **데이터 접근** | 매개변수를 통해 데이터 전달 | 리시버(Receiver)를 통해 구조체 데이터 접근 |

---

## 결론
- **함수(Function)**: 독립적인 코드 블록으로, 특정 작업을 수행하는 데 사용됩니다.
- **메서드(Method)**: 특정 **구조체와 연결된 함수**로, 객체 지향적 접근 방식입니다.
- **포인터 리시버를 사용하면 구조체의 값을 직접 수정할 수 있습니다.**
