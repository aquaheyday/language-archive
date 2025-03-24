# ✅ Go 언어 단위 테스트 (Unit Testing)

Go는 표준 패키지 `testing`을 통해 **간결하고 강력한 테스트 기능**을 제공합니다.  
테스트 함수는 일반적으로 `_test.go` 파일에 작성하며, `go test` 명령어로 실행됩니다.

---

## 1️⃣ 기본 테스트 함수

```go
// calc.go
package calc

func Add(a, b int) int {
    return a + b
}
```

```go
// calc_test.go
package calc

import "testing"

func TestAdd(t *testing.T) {
    result := Add(2, 3)
    if result != 5 {
        t.Errorf("Add(2, 3) = %d; want 5", result)
    }
}
```

✔ 테스트 함수는 `Test`로 시작하고 `*testing.T`를 인자로 받음  
✔ 실패 시 `t.Errorf`, `t.Fatal` 사용  

---

## 2️⃣ 테스트 실행

```bash
go test
```

#### 옵션  
- `-v`: 상세 출력 (`go test -v`)  
- `-run=TestAdd`: 특정 테스트만 실행  

---

## 3️⃣ 서브 테스트

```go
func TestMultiply(t *testing.T) {
    tests := []struct {
        name     string
        a, b, want int
    }{
        {"2*3", 2, 3, 6},
        {"0*5", 0, 5, 0},
    }

    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            got := tc.a * tc.b
            if got != tc.want {
                t.Errorf("%d*%d = %d; want %d", tc.a, tc.b, got, tc.want)
            }
        })
    }
}
```

✔ `t.Run()`으로 **테이블 기반 테스트** 작성 가능  
✔ 반복되는 테스트 케이스를 깔끔하게 정리 가능

---

## 4️⃣ 커버리지 측정
**"내가 작성한 테스트 코드가 전체 코드 중 얼마나 실행되었는가?"**를 퍼센트로 보여주는 것

```bash
go test -cover
```

```bash
go test -coverprofile=cover.out
go tool cover -html=cover.out
```

✔ 테스트 커버리지 % 확인  
✔ `-html` 옵션으로 시각화 가능  

---

## 5️⃣ 벤치마크 테스트
특정 함수가 얼마나 빠르게 실행되는지(메모리 사용량 등)를 측정할 수 있음
```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(1, 2)
    }
}
```

```bash
go test -bench=.
```

✔ `b.N`은 벤치마크를 위한 반복 횟수  
✔ `-benchmem`으로 메모리 사용량 측정 가능

---

## 6️⃣ 테스트 예제

```go
func ExampleAdd() {
    fmt.Println(Add(2, 3))
    // Output: 5
}
```

✔ `ExampleXxx` 함수는 문서화 + 테스트용  
✔ `Output:` 주석이 결과와 일치해야 테스트 통과  

---

## 7️⃣ 테스트 디렉토리 구조 예시

```
project/
├── main.go
├── math/
│   ├── add.go
│   └── add_test.go
└── utils/
    ├── str.go
    └── str_test.go
```

✔ 패키지별로 테스트 파일 분리  
✔ `_test.go`는 테스트 전용 파일 (빌드 시 제외됨)

---

## 8️⃣ 테스트 실패 예외 처리

```go
if err != nil {
    t.Fatal("unexpected error:", err)
}
```

| 함수 | 설명 |
|------|------|
| `t.Error(...)` | 테스트 실패, 계속 진행 |
| `t.Fatal(...)` | 테스트 실패, 즉시 종료 |
| `t.Log(...)`   | 로그 출력 (디버깅용) |

---

## 🎯 정리

✔ 테스트는 가능한 한 **작고 빠르게**  
✔ 공통 셋업은 `TestMain(m *testing.M)` 활용 가능  
✔ 외부 의존성(Mock 등)은 **인터페이스 분리**로 해결  
✔ GitHub Actions, Drone 등으로 CI 테스트 자동화 추천  
✔ 실전에서는 `stretchr/testify` 같은 서드파티 assertion 패키지도 많이 사용  
