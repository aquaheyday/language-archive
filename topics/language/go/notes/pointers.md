# 🧠 Go 언어 포인터와 메모리

Go는 포인터(pointer)를 통해 **메모리 주소를 직접 다루는 기능**을 제공하지만, 다른 언어보다 **안전하고 단순하게** 설계되어 있습니다.  

---

## 1️⃣ 포인터(Pointer)란?

- **값의 메모리 주소를 저장하는 변수**
- `*T`는 "T 타입의 포인터"를 의미
- `&`는 변수의 주소를 얻는 연산자

```go
package main

import "fmt"

func main() {
    var a int = 10
    var p *int = &a

    fmt.Println("a의 값:", a)
    fmt.Println("a의 주소:", &a)
    fmt.Println("포인터 p가 가리키는 주소:", p)
    fmt.Println("포인터 p가 가리키는 값:", *p)
}
```

✔ `*p`는 포인터가 가리키는 값을 가져옴 (역참조)  
✔ `&a`는 변수 `a`의 메모리 주소를 반환  

---

## 2️⃣ 포인터를 함수에 전달하기

Go는 기본적으로 **값에 의한 전달(call by value)** 를 사용하지만, 포인터를 전달하면 원본 값을 변경할 수 있음.  

```go
func updateValue(n *int) {
    *n = 20
}

func main() {
    x := 10
    updateValue(&x)
    fmt.Println(x) // 출력: 20
}
```

✔ 포인터를 통해 함수 안에서 외부 변수 값을 수정 가능  

---

## 3️⃣ new 함수

- `new(T)`는 **T 타입의 메모리를 할당하고 포인터를 반환**
- 초기값은 해당 타입의 **제로 값(zero value)**

```go
func main() {
    p := new(int) // int에 대한 포인터 생성
    fmt.Println(*p) // 출력: 0
    *p = 42
    fmt.Println(*p) // 출력: 42
}
```

✔ `new`는 값 할당이 필요 없는 간단한 구조체 초기화에 유용  

---

## 4️⃣ 구조체와 포인터

구조체(struct)를 포인터로 다루면 **복사 비용 없이 필드에 접근**할 수 있음.

```go
type User struct {
    Name string
}

func changeName(u *User) {
    u.Name = "Alice"
}

func main() {
    user := User{Name: "Bob"}
    changeName(&user)
    fmt.Println(user.Name) // 출력: Alice
}
```

✔ 구조체 포인터도 `.`으로 필드 접근 가능 (`(*u).Name` 대신 `u.Name` 사용 가능)  

---

## 5️⃣ 포인터 배열 vs 배열 포인터

구분이 헷갈릴 수 있으므로 아래 예제를 참고하세요.

| 선언 | 의미 |
|------|------|
| `*[3]int` | **배열의 포인터** (포인터가 배열을 가리킴) |
| `[]*int`  | **포인터들의 슬라이스** (슬라이스 요소가 포인터) |

```go
func main() {
    a := [3]int{1, 2, 3}
    p := &a       // *[3]int

    b := []*int{&a[0], &a[1], &a[2]} // []*int
    fmt.Println(*p)
    fmt.Println(*b[0], *b[1], *b[2])
}
```

---

## 6️⃣ nil 포인터 주의

- 포인터는 기본적으로 `nil` 값으로 초기화됨
- 역참조 시 런타임 패닉 발생

```go
func main() {
    var p *int
    fmt.Println(p)  // 출력: <nil>
    // fmt.Println(*p) // ❌ 런타임 에러 (panic: invalid memory address)
}
```

✔ 포인터를 사용하기 전에 `nil` 체크 필수  

---

## 7️⃣ 포인터와 슬라이스/맵/채널

Go의 **슬라이스(slice), 맵(map), 채널(channel)** 은 내부적으로 포인터처럼 동작하므로 함수에 그대로 전달해도 원본 값이 수정됨 (포인터 전달 불필요).  

```go
func modify(s []int) {
    s[0] = 99
}

func main() {
    nums := []int{1, 2, 3}
    modify(nums)
    fmt.Println(nums) // 출력: [99 2 3]
}
```

✔ 슬라이스, 맵, 채널은 참조 타입이라 포인터 없이도 동작  

---

## 🎯 정리

✔ 포인터는 **메모리 주소**를 저장하는 변수이며, `*`, `&` 연산자를 사용해 다룸  
✔ 함수에 포인터를 전달하면 **원본 데이터를 수정** 가능  
✔ 구조체, 배열, 슬라이스 등 다양한 타입과 포인터를 함께 사용할 수 있음  
✔ 슬라이스, 맵, 채널은 포인터처럼 동작하므로 별도 포인터 전달 없이도 참조 가능  
✔ `nil` 포인터 사용 시 반드시 주의 (런타임 패닉 발생 위험)
