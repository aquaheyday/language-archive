# 🔒 Java - 접근 제어자 (Access Modifiers)

접근 제어자는 클래스, 변수, 메서드 등에 접근 가능한 **범위를 제어**하는 키워드입니다.  
코드의 **캡슐화, 보안, 구조적 안정성**을 유지하는 데 중요한 역할을 합니다.

---

## 1️⃣ 종류 요약

Java에는 총 4가지 접근 제어자가 존재합니다.

| 제어자 | 클래스 내 | 같은 패키지 | 상속 관계 | 전체 접근 |
|--------|-----------|--------------|------------|------------|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| (default) | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

✔ (default)는 **제어자를 쓰지 않은 상태**를 의미합니다.

---

## 2️⃣ `public`

- 모든 클래스, 패키지, 외부에서 접근 가능
- 라이브러리의 API, main 메서드 등에 주로 사용

```java
public class Person {
    public String name;

    public void sayHello() {
        System.out.println("Hello!");
    }
}
```

---

## 3️⃣ `private`

- **해당 클래스 내부에서만 접근 가능**
- 외부에서 직접 접근 ❌ → getter/setter 사용

```java
public class Account {
    private int balance;

    public int getBalance() {
        return balance;
    }

    public void deposit(int amount) {
        balance += amount;
    }
}
```

---

## 4️⃣ `protected`

- 같은 패키지 안에서는 접근 가능
- 다른 패키지라도 **상속 관계**라면 접근 가능
- 주로 **상속을 고려한 설계**에서 사용

```java
class Animal {
    protected String type = "포유류";
}

class Dog extends Animal {
    void showType() {
        System.out.println(type);  // 가능
    }
}
```

---

## 5️⃣ (default)

- 접근 제어자를 생략한 상태
- **같은 패키지 안에서만** 접근 가능
- 패키지 내부용 클래스, 유틸 클래스 등에서 사용

```java
class Helper {
    void help() {
        System.out.println("도와줄게요");
    }
}
```

---

## 6️⃣ 클래스에 사용 가능한 제어자

| 대상 | 사용 가능 제어자 |
|------|------------------|
| 클래스 | `public`, (default) |
| 내부 클래스 | `public`, `protected`, `private`, (default) |
| 필드/메서드 | 모두 사용 가능 |

✔ 클래스에는 `private`, `protected` 사용 ❌ → 컴파일 에러

---

## 7️⃣ 접근 제어자 사용 예시

```java
package a;

public class A {
    public int x = 1;
    protected int y = 2;
    int z = 3;           // default
    private int w = 4;
}
```

```java
package b;

import a.A;

class B extends A {
    void print() {
        System.out.println(x); // ✅ public
        System.out.println(y); // ✅ protected (상속 관계)
        // System.out.println(z); // ❌ default (다른 패키지)
        // System.out.println(w); // ❌ private
    }
}
```

---

## 🎯 정리

✔ `public` → 어디서든 접근 가능 (전역 공개)  
✔ `private` → 현재 클래스 내부에서만 사용 가능  
✔ `protected` → 같은 패키지 + 상속 관계에서만 접근 가능  
✔ `default` → 같은 패키지 내부에서만 접근 가능  
✔ 클래스에는 `public`, (default)만 사용 가능  
✔ 필드/메서드/내부 클래스에는 모든 제어자 사용 가능  
✔ 접근 제어자는 **캡슐화**와 **정보 은닉**의 핵심 도구

