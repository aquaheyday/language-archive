# 🔐 PHP 해싱과 암호화 정리

웹 애플리케이션에서 **비밀번호 보호, 민감 데이터 저장, 인증 토큰 생성** 등을 위해 PHP는 **해싱(Hashing)** 과 **암호화(Encryption)** 기능을 제공합니다.

---

## 1️⃣ 해싱(Hashing)이란?

- **일방향 변환**: 입력값 → 고정된 길이의 암호화된 문자열  
- 원래 값으로 **복호화가 불가능**  
- 주로 **비밀번호 저장** 등에 사용

---

### 1) ✅ 해싱 함수: `password_hash()` / `password_verify()`

```php
<?php
$pw = "mypassword";

// 비밀번호 해시 생성
$hash = password_hash($pw, PASSWORD_DEFAULT);

// 비밀번호 검증
if (password_verify("mypassword", $hash)) {
    echo "일치합니다!";
}
?>
```

| 함수               | 설명                             |
|--------------------|----------------------------------|
| `password_hash()`   | 비밀번호를 안전한 방식으로 해시화 |
| `password_verify()` | 입력값과 해시값을 비교            |
| `PASSWORD_DEFAULT` | 현재 PHP가 권장하는 알고리즘 사용 (`bcrypt`, 향후 `argon2` 등) |

✔ 해시는 **같은 비밀번호라도 매번 다른 결과가 생성**됩니다 (솔트 자동 포함)  
✔ 솔트란, 비밀번호에 추가하는 랜덤 문자열로 해시값을 고의적으로 매번 다르게 만들어주는 보안 기술  

---

### 2) ❌ 사용하지 말아야 할 해시 함수

`md5()`, `sha1()` 은 빠르고 충돌에 취약하여 **비밀번호 저장에 절대 사용 금지**

---

## 2️⃣ 암호화(Encryption)이란?

- **양방향 변환**: 데이터를 암호화하여 저장하고, **복호화로 원래 값 복원 가능**  
- 민감한 사용자 정보(주민번호, 카드번호 등) 저장 시 사용

### `openssl_encrypt()` / `openssl_decrypt()`

```php
<?php
$data = "Secret Data";
$key = "mySecretKey123"; // 키는 길이와 보안에 따라 설정
$method = "AES-256-CBC";
$iv = openssl_random_pseudo_bytes(16);

// 암호화
$encrypted = openssl_encrypt($data, $method, $key, 0, $iv);

// 복호화
$decrypted = openssl_decrypt($encrypted, $method, $key, 0, $iv);
?>
```

| 요소         | 설명                                           |
|--------------|------------------------------------------------|
| `$method`    | 암호화 방식 (`AES-256-CBC` 등 OpenSSL 알고리즘) |
| `$key`       | 암호화/복호화에 사용하는 비밀키                 |
| `$iv`        | 초기화 벡터 (CBC 모드에서는 필수)               |

✔ `AES-256-CBC`는 널리 쓰이는 대칭 키 암호화 방식입니다.  
✔ `$iv`와 `$key`는 안전하게 저장 또는 함께 암호문에 포함해야 합니다.  

---

## 3️⃣ 해싱 vs 암호화 차이

| 항목       | 해싱(Hashing)                        | 암호화(Encryption)                       |
|------------|---------------------------------------|------------------------------------------|
| 방향성     | 일방향 (복호화 불가능)                | 양방향 (복호화 가능)                     |
| 목적       | 비밀번호 검증                         | 민감 정보 저장                           |
| 예시 함수 | `password_hash()`, `md5()`, `sha1()` | `openssl_encrypt()`, `mcrypt` (deprecated) |
| 안전성     | 매우 강함 (솔트 포함 시)               | 키 관리에 따라 다름                      |

---

## 4️⃣ 실무 적용 예시

#### 1. 비밀번호 저장

```php
$password = $_POST["password"];
$hash = password_hash($password, PASSWORD_DEFAULT);
// DB에 $hash 저장
```

#### 2. 비밀번호 로그인 검증

```php
if (password_verify($_POST["password"], $user["hashed_password"])) {
    echo "로그인 성공!";
}
```

#### 3. 주민등록번호 암호화 저장

```php
$iv = openssl_random_pseudo_bytes(16);
$encrypted = openssl_encrypt($ssn, "AES-256-CBC", $key, 0, $iv);
// $encrypted 와 $iv 를 함께 DB에 저장
```

---

## 5️⃣ 보안 주의사항

| 항목                 | 설명                                                 |
|----------------------|------------------------------------------------------|
| 비밀번호 저장 시     | 반드시 `password_hash()` 사용 (md5, sha1 금지)        |
| 암호화 키 관리        | `.env` 파일 또는 안전한 환경변수에 저장               |
| IV(초기화 벡터) 관리 | 암호문과 함께 저장하거나 안전하게 전달해야 함         |
| TLS/HTTPS 사용       | 전송 중 데이터 보호를 위해 필수                       |
| 타이밍 공격 방지     | `hash_equals()` 함수 사용 (고정 시간 비교)            |

---

## 🎯 정리

✔ 해싱은 복호화 불가능한 방식으로, **비밀번호 검증에만 사용**됩니다.  
✔ 암호화는 복호화 가능한 방식으로, **민감 정보 저장 및 복원**이 가능합니다.  
✔ 해싱에는 `password_hash()`, `password_verify()`를, 암호화에는 `openssl_encrypt()`, `openssl_decrypt()`을 사용합니다.  
✔ 보안에서 가장 중요한 것은 **사용자 입력은 항상 안전하게 처리하고, 키와 암호문을 안전하게 관리**하는 것입니다.

