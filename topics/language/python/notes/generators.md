# 🚀 Python 제너레이터(Generator)와 이터레이터(Iterator)

Python에서 **이터레이터(Iterator)** 와 **제너레이터(Generator)** 는  
큰 데이터를 효율적으로 처리하거나, 메모리를 절약하면서 반복 가능한 객체를 생성하는 데 사용됩니다.

---

## 1️⃣ 이터레이터(Iterator)란?

- **이터레이터(Iterator)** 는 **다음 요소를 계속해서 반환하는 객체**입니다.
- `for` 루프에서 반복 가능한(iterable) 객체(`list`, `tuple`, `dict`, `set`, `string`)를 순회할 때 내부적으로 사용됨.
- `iter()`와 `next()` 함수를 사용하여 **직접 요소를 가져올 수 있음**.

#### 이터레이터 만들기 (`iter()`, `next()` 사용)
```python
numbers = [1, 2, 3, 4, 5]
iterator = iter(numbers)  # 리스트를 이터레이터로 변환

print(next(iterator))  # 1
print(next(iterator))  # 2
print(next(iterator))  # 3
```
✔ `iter(객체)` → 이터레이터 객체 반환  
✔ `next(이터레이터)` → 다음 요소 반환, 요소가 없으면 `StopIteration` 예외 발생  

---

## 2️⃣ 이터레이터 클래스 만들기

`__iter__()` 와 `__next__()` 메서드를 구현하면 **사용자 정의 이터레이터를 만들 수 있음**.

#### 사용자 정의 이터레이터 예제
```python
class Counter:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):  # 이터레이터 객체 반환
        return self

    def __next__(self):  # 다음 요소 반환
        if self.current >= self.end:
            raise StopIteration
        value = self.current
        self.current += 1
        return value

counter = Counter(1, 5)

for num in counter:
    print(num)  # 1, 2, 3, 4
```
✔ `__iter__()` → 이터레이터 객체를 반환  
✔ `__next__()` → 현재 값을 반환 후 증가, 마지막 요소에서 `StopIteration` 예외 발생  

---

## 3️⃣ 제너레이터(Generator)란?

- **제너레이터(Generator)** 는 **이터레이터를 더 쉽게 만들 수 있는 함수**입니다.
- `yield` 키워드를 사용하여 **값을 하나씩 반환**하면서 상태를 유지.
- `return`을 만나면 실행이 종료됨.
- **제너레이터는 일반 함수보다 메모리를 절약**할 수 있음.

#### 기본적인 제너레이터 예제 (`yield` 사용)
```python
def my_generator():
    yield 1
    yield 2
    yield 3

gen = my_generator()
print(next(gen))  # 1
print(next(gen))  # 2
print(next(gen))  # 3
```
✔ `yield`는 함수의 실행을 멈추고 값을 반환, 다음 호출 시 멈춘 곳부터 실행됨  
✔ `next()`를 호출하면 다음 `yield` 문까지 실행됨  

---

## 4️⃣ 제너레이터와 이터레이터 비교

| 비교 항목 | 이터레이터(Iterator) | 제너레이터(Generator) |
|----------|------------------|------------------|
| 구현 방식 | `__iter__()`, `__next__()` 직접 구현 | `yield` 사용 |
| 상태 유지 | 명시적으로 변수 관리 | 자동으로 상태 유지 |
| 메모리 사용 | 모든 요소를 메모리에 저장 | 필요할 때마다 요소 생성 (메모리 절약) |
| 예외 처리 | `StopIteration` 예외 발생 | `yield` 사용 시 자동 처리 |

✔ **제너레이터는 상태를 유지하면서 값을 반환하므로 메모리 효율적**  

---

## 5️⃣ 제너레이터 실전 활용 예제

### 1) 큰 데이터 처리 (메모리 절약)
```python
def large_numbers():
    num = 0
    while True:
        yield num
        num += 1

gen = large_numbers()

print(next(gen))  # 0
print(next(gen))  # 1
print(next(gen))  # 2
```
✔ **무한 루프에서 데이터를 생성하면서도 메모리를 낭비하지 않음**  

---

### 2) 제너레이터 표현식 (Generator Expression)

리스트 컴프리헨션과 비슷하지만 **메모리를 절약**할 수 있음.

```python
nums = (x * 2 for x in range(5))  # 제너레이터 표현식
print(next(nums))  # 0
print(next(nums))  # 2
print(next(nums))  # 4
```
✔ **리스트 `[]` 대신 괄호 `()`를 사용하면 제너레이터 표현식이 됨**  
✔ `next()`로 값을 하나씩 가져올 수 있음  

---

### 3) 파일 읽기 (큰 파일 처리)
```python
def read_large_file(file_path):
    with open(file_path, "r") as file:
        for line in file:
            yield line.strip()  # 한 줄씩 반환

for line in read_large_file("example.txt"):
    print(line)
```
✔ 파일을 한 줄씩 읽어 메모리를 절약하면서 처리 가능  

---

### 4) 여러 개의 제너레이터 병렬 실행 (`yield from`)
```python
def generator1():
    yield from range(3)

def generator2():
    yield from "ABC"

for value in generator1():
    print(value)  # 0, 1, 2

for value in generator2():
    print(value)  # A, B, C
```
✔ `yield from`을 사용하면 **다른 이터러블 객체를 쉽게 포함 가능**  

---

## 6️⃣ `send()`, `throw()`, `close()` 메서드 활용

### 1) `send()` - 제너레이터에 값 전달
```python
def custom_counter():
    count = 0
    while True:
        value = yield count
        if value:
            count = value
        else:
            count += 1

gen = custom_counter()
print(next(gen))  # 0
print(gen.send(10))  # 10 (새로운 값으로 설정)
print(next(gen))  # 11
```
✔ `send(value)` → **제너레이터 내부로 값 전달 가능**  

---

### 2) `throw()` - 예외 발생
```python
def test():
    try:
        yield 1
    except ValueError:
        yield "에러 발생!"
    yield 2

gen = test()
print(next(gen))  # 1
print(gen.throw(ValueError))  # "에러 발생!"
print(next(gen))  # 2
```
✔ `throw(Exception)` → **제너레이터 내부에서 예외 발생 가능**  

---

### 3) `close()` - 제너레이터 종료
```python
gen = custom_counter()
print(next(gen))  # 0
gen.close()  # 제너레이터 종료
print(next(gen))  # 예외 발생 (StopIteration)
```
✔ `close()` → **제너레이터 실행을 중단하고 종료**  

---

## 🎯 정리

✔ **이터레이터(Iterator)** → `__iter__()`, `__next__()` 구현 필요  
✔ **제너레이터(Generator)** → `yield` 사용하여 간편하게 이터레이터 생성  
✔ **메모리 효율적** → 필요할 때만 데이터를 생성하여 메모리 절약  
✔ **`yield from` 사용 가능** → 다른 이터러블 객체 포함 가능  
✔ **실전 활용** → **큰 파일 읽기, 무한 수열 생성, 데이터 스트리밍 등**  
