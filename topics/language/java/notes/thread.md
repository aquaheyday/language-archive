# 🧵 Java - 스레드 기초

Java에서 **스레드(Thread)** 는 하나의 프로그램(프로세스) 내에서 **동시에 여러 작업을 처리**할 수 있게 하는 실행 단위입니다.  
스레드를 활용하면 병렬 처리와 성능 최적화가 가능합니다.

---

## 1️⃣ 스레드 생성 방법 ① - `Thread` 클래스 상속

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("스레드 실행 중!");
    }
}
```

```java
MyThread t = new MyThread();
t.start();
```

✔ `start()` → 새로운 스레드를 생성해서 run()을 실행함 (병렬 실행)  
✔ `run()` →  그냥 일반 메서드처럼 실행됨 (동기 실행, 싱글 스레드)

---

## 2️⃣ 스레드 생성 방법 ② - `Runnable` 인터페이스 구현

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable 스레드 실행!");
    }
}
```

```java
Thread t = new Thread(new MyRunnable());
t.start();
```

✔ `Runnable` 방식은 **다중 상속 제약을 피할 수 있어** 실무에서 더 많이 사용됨  
✔ Java는 클래스의 다중 상속을 허용하지 않음, 하나의 클래스는 오직 하나의 부모 클래스만 extends 할 수 있음

---

## 3️⃣ 람다식으로 스레드 생성 (간편한 방식)

```java
Thread t = new Thread(() -> {
    System.out.println("람다로 실행되는 스레드!");
});
t.start();
```

---

## 4️⃣ `Thread.sleep()` - 일시정지

```java
for (int i = 0; i < 3; i++) {
    System.out.println(i);
    Thread.sleep(1000);  // 1초 대기
}
```

✔ 단위는 **밀리초(ms)**  
✔ 예외 처리 필요 (`InterruptedException`)

---

## 5️⃣ `join()` - 스레드 종료 대기

```java
Thread t = new Thread(() -> {
    System.out.println("작업 중...");
});

t.start();
t.join();  // t가 끝날 때까지 대기
System.out.println("작업 완료 후 실행");
```

---

## 6️⃣ `isAlive()` - 스레드 생존 여부 확인

```java
if (t.isAlive()) {
    System.out.println("스레드가 아직 실행 중입니다.");
}
```

---

## 7️⃣ 여러 스레드 실행 예시

```java
for (int i = 0; i < 3; i++) {
    int num = i;
    new Thread(() -> {
        System.out.println("Thread " + num);
    }).start();
}
```

---

## 8️⃣ 스레드 동기화 (`synchronized`)

여러 스레드가 **공유 자원**에 동시에 접근할 때, 데이터 충돌을 막기 위해 **동기화**가 필요합니다.

```java
public synchronized void print() {
    // 한 번에 하나의 스레드만 접근 가능
}
```

또는 블록 단위로:

```java
synchronized (this) {
    // 동기화 영역
}
```

---

## 9️⃣ 공유 자원 예시와 경쟁 조건 (Race Condition)

```java
public class Counter {
    int count = 0;

    public void increase() {
        count++;
    }
}
```

✔ 여러 스레드가 동시에 `increase()`를 호출하면 `count` 값이 올바르게 증가하지 않을 수 있음 → **동기화 필요**

---

## 🔟 데몬 스레드

- **백그라운드 작업**을 위한 스레드
- 모든 일반(메인) 스레드가 종료되면 함께 종료됨

```java
Thread t = new Thread(() -> {
    while (true) {
        System.out.println("백업 중...");
    }
});
t.setDaemon(true);
t.start();
```

---

## 🎯 정리

✔ `Thread` 클래스 상속 or `Runnable` 구현으로 스레드 생성  
✔ `start()` → 새 스레드 실행 / `run()`은 직접 호출 ❌  
✔ `sleep(ms)` → 일정 시간 일시 정지  
✔ `join()` → 특정 스레드가 끝날 때까지 대기  
✔ `isAlive()` → 스레드 실행 여부 확인  
✔ `synchronized` → 공유 자원 보호 (동기화)  
✔ 람다식 사용 시 간결한 코드 작성 가능  
✔ 데몬 스레드 → 백그라운드 작업 전용  
✔ 멀티스레드는 **동시성 문제**와 **데이터 무결성** 주의 필요  
✔ 실무에선 `ExecutorService`, `Callable`, `Future` 등 고급 API 사용 권장

