# 🧬 SQL 서브쿼리(Subquery)

**서브쿼리(Subquery)** 는 SQL문 안에 **포함된 또 다른 SELECT 문**입니다.  
주 쿼리(Main Query)를 보조하며, **필터링, 비교, 값 계산, 집합 조건** 등에 자주 사용됩니다.

---

## 1️⃣ 서브쿼리란?

SQL 문 내에 포함된 **중첩된 SELECT 문**, 주로 `WHERE`, `FROM`, `SELECT` 절 안에서 사용됨

✔ 결과를 단일 값, 리스트, 테이블 형태로 반환  
✔ SELECT, INSERT, UPDATE, DELETE 문 어디서든 사용 가능

---

## 2️⃣ 서브쿼리 위치별 분류

| 유형 | 위치 | 설명 |
|------|------|------|
| **스칼라 서브쿼리** | `SELECT` | 단일 값 반환 |
| **인라인 뷰** | `FROM` | 임시 테이블로 사용 |
| **서브쿼리 in WHERE** | `WHERE` | 조건식 안에서 사용 |
| **EXISTS 서브쿼리** | `WHERE EXISTS` | 존재 여부 검사 |
| **상관 서브쿼리** | `WHERE`, `SELECT` | 외부 쿼리와 연관된 값 사용 |

---

## 3️⃣ 스칼라 서브쿼리 (SELECT 절 내부)

```sql
SELECT name,
  (SELECT COUNT(*) FROM orders WHERE user_id = users.id) AS order_count
FROM users;
```

✔ 사용자별 주문 수를 함께 조회  
✔ 서브쿼리가 단일 값을 반환 (스칼라 값)

---

## 4️⃣ WHERE 절에서 사용하는 서브쿼리

```sql
SELECT name FROM users
WHERE id IN (SELECT user_id FROM orders WHERE item = 'Book');
```

✔ 주문한 아이템이 "Book"인 사용자만 조회  
✔ `IN`, `=`, `>`, `<` 등과 함께 자주 사용됨

---

## 5️⃣ IN vs EXISTS

### 1) `IN` 사용

```sql
SELECT name FROM users
WHERE id IN (SELECT user_id FROM orders);
```

---

### 2) `EXISTS` 사용

```sql
SELECT name FROM users u
WHERE EXISTS (
  SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

---

### 3) `IN` 과 `EXISTS` 비교

| 비교 항목 | IN | EXISTS |
|-----------|----|--------|
| 처리 방식 | 결과 집합 비교 | 존재 여부만 확인 |
| 서브쿼리 결과 | 명시적 비교 대상 | TRUE/FALSE 반환 |
| 성능 | 작은 서브쿼리일 땐 IN | 큰 데이터셋은 EXISTS 효율적 |

---

## 6️⃣ 상관 서브쿼리 (외부 쿼리 참조)

```sql
SELECT name FROM users u
WHERE EXISTS (
  SELECT 1 FROM orders o WHERE o.user_id = u.id AND o.item = 'Pen'
);
```

✔ 서브쿼리에서 **외부 쿼리의 컬럼(u.id)** 를 참조함  
✔ 외부 쿼리의 각 행마다 **서브쿼리를 반복 수행**  
✔ 효율성 측면에서 주의 필요

---

## 7️⃣ FROM 절의 서브쿼리 (인라인 뷰)

```sql
SELECT name, order_count
FROM (
  SELECT user_id, COUNT(*) AS order_count
  FROM orders
  GROUP BY user_id
) AS order_stats
JOIN users ON users.id = order_stats.user_id;
```

✔ 집계 결과를 하나의 **임시 테이블**처럼 JOIN에 활용  
✔ 실무에서 **통계/분석 쿼리**에 자주 사용

---

## 8️⃣ UPDATE / DELETE에서의 서브쿼리

```sql
-- 특정 조건을 만족하는 사용자만 삭제
DELETE FROM users
WHERE id IN (SELECT user_id FROM orders WHERE item = 'Expired');

-- 조건에 맞는 주문 상태 업데이트
UPDATE orders
SET status = 'archived'
WHERE user_id IN (SELECT id FROM users WHERE is_deleted = 1);
```

✔ 서브쿼리는 조회뿐 아니라 **데이터 수정/삭제** 조건에도 활용됨

---

## 9️⃣ 서브쿼리 vs JOIN

| 항목 | 서브쿼리 | JOIN |
|------|----------|------|
| 사용 목적 | 값 기반 조건, 존재 여부 | 다중 테이블 결합 |
| 성능 | 단순 쿼리에 적합 | 복잡한 관계 처리 유리 |
| 가독성 | 짧고 단순 | 복잡한 로직은 JOIN이 명확 |

---

## 🎯 정리

| 구분 | 설명 |
|------|------|
| 스칼라 서브쿼리 | SELECT 절에서 단일 값 반환 |
| WHERE 서브쿼리 | 조건 비교에 사용 (IN, EXISTS 등) |
| FROM 서브쿼리 | 인라인 뷰 (임시 테이블) |
| 상관 서브쿼리 | 외부 쿼리의 컬럼을 참조 |
| EXISTS | 존재 여부 판단 (TRUE/FALSE) |
| 성능 팁 | 작은 집합 → IN, 큰 집합 → EXISTS |
