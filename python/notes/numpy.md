# 📊 Python NumPy 기초부터 활용까지

Python의 **NumPy**(Numerical Python)는 **고성능 다차원 배열 및 행렬 연산을 지원하는 라이브러리**입니다.  
과학 계산, 데이터 분석, 머신러닝에서 필수적으로 사용됩니다.

---

## 1. NumPy란?

- **고속 연산이 가능한 다차원 배열(NumPy 배열, ndarray) 제공**
- **벡터화 연산을 지원하여 루프 없이 빠른 연산 가능**
- **선형대수, 난수 생성, FFT(푸리에 변환) 등의 수학 함수 제공**
- **Python 리스트보다 빠르고 메모리 효율적**

---

## 2. NumPy 설치 및 가져오기

### NumPy 설치
```sh
pip install numpy
```

### NumPy 불러오기
```python
import numpy as np
```

✔ `np`는 관례적으로 사용되는 NumPy의 별칭  

---

## 3. NumPy 배열 생성 (`np.array`)

### 리스트를 사용하여 배열 생성
```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
print(arr)  # [1 2 3 4 5]
print(type(arr))  # <class 'numpy.ndarray'>
```
✔ `np.array()`를 사용하여 **리스트를 NumPy 배열(ndarray)로 변환**  

---

### 다차원 배열 생성
```python
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr_2d)
```
#### 출력 결과
```
[[1 2 3]
 [4 5 6]]
```
✔ 2D 배열(행렬) 생성 가능  

---

### 특정 값으로 배열 생성 (`zeros`, `ones`, `full`)
```python
print(np.zeros((3, 3)))  # 3x3 영행렬
print(np.ones((2, 4)))  # 2x4 모든 값이 1인 배열
print(np.full((2, 3), 7))  # 2x3 모든 값이 7인 배열
```

---

### 연속된 숫자로 배열 생성 (`arange`, `linspace`)
```python
print(np.arange(1, 10, 2))  # [1 3 5 7 9] (1부터 10 전까지 2씩 증가)
print(np.linspace(0, 1, 5))  # [0.   0.25 0.5  0.75 1.] (0~1 사이를 5개 균등 분할)
```

---

## 4. 배열 정보 확인

### 배열의 차원 및 크기 확인
```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

print(arr.shape)  # (2, 3) → (행, 열)
print(arr.ndim)   # 2 → 배열의 차원 (2D 배열)
print(arr.size)   # 6 → 총 요소 개수
print(arr.dtype)  # int64 → 데이터 타입
```

---

## 5. 배열 인덱싱 & 슬라이싱

### 배열 인덱싱 (특정 요소 접근)
```python
arr = np.array([10, 20, 30, 40])

print(arr[1])   # 20 (0부터 시작)
print(arr[-1])  # 40 (뒤에서 첫 번째)
```

---

### 다차원 배열 인덱싱
```python
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])

print(arr_2d[0, 2])  # 3 (0행 2열)
print(arr_2d[1, 1])  # 5 (1행 1열)
```

---

### 배열 슬라이싱 (부분 선택)
```python
arr = np.array([10, 20, 30, 40, 50])

print(arr[1:4])  # [20 30 40] (1번 인덱스부터 3번 인덱스까지)
print(arr[:3])   # [10 20 30] (처음부터 3번 인덱스까지)
print(arr[::2])  # [10 30 50] (2칸씩 건너뛰기)
```

---

## 6. 배열 연산

### 기본 연산 (벡터화 연산)
```python
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])

print(arr1 + arr2)  # [5 7 9] (각 요소끼리 덧셈)
print(arr1 * arr2)  # [4 10 18] (각 요소끼리 곱셈)
print(arr1 ** 2)    # [1 4 9] (제곱 연산)
```

---

### 행렬 연산 (`dot`)
```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(np.dot(A, B))  # 행렬 곱
```

---

## 7. 유용한 함수

### 통계 연산 (`sum`, `mean`, `max`, `min`)
```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

print(np.sum(arr))   # 전체 합 (21)
print(np.mean(arr))  # 평균 (3.5)
print(np.max(arr))   # 최댓값 (6)
print(np.min(arr))   # 최솟값 (1)
```

---

### 난수 생성 (`random`)
```python
print(np.random.rand(3))  # [0.123, 0.456, 0.789] (0~1 사이 난수 3개)
print(np.random.randint(1, 10, (2, 2)))  # 1~9 사이 정수 난수 (2x2)
```

---

## 8. 배열 변형

### 배열 형태 변경 (`reshape`)
```python
arr = np.array([1, 2, 3, 4, 5, 6])

print(arr.reshape(2, 3))  # (2행 3열) 변환
```

---

### 배열 합치기 (`concatenate`)
```python
arr1 = np.array([[1, 2], [3, 4]])
arr2 = np.array([[5, 6]])

print(np.concatenate((arr1, arr2), axis=0))  # 행 방향으로 합치기
```

---

## 9. NumPy vs Python 리스트 성능 비교

NumPy는 **벡터 연산을 지원하여 Python 리스트보다 훨씬 빠름**.

### 성능 비교 예제
```python
import time

size = 1000000
a = list(range(size))
b = list(range(size))
np_a = np.array(a)
np_b = np.array(b)

# Python 리스트 연산
start = time.time()
c = [x + y for x, y in zip(a, b)]
print("Python 리스트 연산 시간:", time.time() - start)

# NumPy 배열 연산
start = time.time()
c = np_a + np_b
print("NumPy 배열 연산 시간:", time.time() - start)
```

✔ **NumPy가 훨씬 빠름** (C로 구현된 내부 연산 최적화 덕분)  

---

## 🎯 정리

✔ **NumPy 배열(ndarray)** → 고속 연산 지원하는 다차원 배열  
✔ **배열 생성** → `np.array()`, `np.zeros()`, `np.ones()`  
✔ **배열 연산** → 벡터화 연산으로 빠른 연산 수행  
✔ **배열 인덱싱 & 슬라이싱** → 리스트보다 강력한 기능 제공  
✔ **행렬 연산** → `np.dot()` 사용  
✔ **NumPy가 Python 리스트보다 훨씬 빠름**  
