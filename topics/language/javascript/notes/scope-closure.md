# 📦 JavaScript - 스코프 & 클로저

JavaScript에서 **스코프(Scope)** 는 변수의 **유효 범위**를 의미하며, **클로저(Closure)** 는 **스코프를 기억하는 함수**입니다.

---

## 1️⃣ 스코프(Scope)란?

변수나 함수가 **접근 가능한 범위**를 의미합니다.

- **전역 스코프(Global Scope)**: 어디서든 접근 가능  
- **지역 스코프(Local Scope)**: 특정 블록 내에서만 접근 가능

```js
let globalVar = "I'm global!";

function testScope() {
  let localVar = "I'm local!";
  console.log(globalVar); // ✅ 가능
  console.log(localVar);  // ✅ 가능
}

console.log(globalVar); // ✅ 가능
console.log(localVar);  // ❌ 에러
```

---

## 2️⃣ 스코프 종류

### 1) 함수 스코프(Function Scope)

- `var`는 함수 단위로 스코프가 생성됨

```js
function funcScope() {
  var x = 10;
}
console.log(x); // ❌ 에러
```

### 2) 블록 스코프(Block Scope, ES6)

- `let`, `const`는 블록({}) 단위로 스코프가 생성됨

```js
{
  let y = 20;
  const z = 30;
}
console.log(y); // ❌ 에러
console.log(z); // ❌ 에러
```

---

## 3️⃣ 렉시컬 스코프 (Lexical Scope)

- 함수가 **어디서 호출되었는지**가 아니라, **어디서 정의되었는지**에 따라 스코프가 결정됨

```js
const a = 1;

function outer() {
  const b = 2;
  
  function inner() {
    console.log(a); // 1
    console.log(b); // 2
  }

  inner();
}

outer();
```

---

## 4️⃣ 클로저(Closure)란?

함수가 **자신이 선언될 당시의 외부 스코프를 기억**하는 것

- 함수가 외부 변수에 접근할 수 있도록 해줌  
- **상태 유지**에 유리 (예: 카운터)

```js
function makeCounter() {
  let count = 0;

  return function() {
    count++;
    return count;
  };
}

const counter = makeCounter();

console.log(counter()); // 1
console.log(counter()); // 2
```

---

## 5️⃣ 클로저 활용 예시

### 1) 데이터 은닉

```js
function secretHolder(secret) {
  return {
    getSecret: function() {
      return secret;
    }
  };
}

const holder = secretHolder("🔒 Classified");
console.log(holder.getSecret()); // 🔒 Classified
```

### 2) 콜백과 클로저

```js
function delayedGreeting(name) {
  setTimeout(function() {
    console.log(`Hello, ${name}`);
  }, 1000);
}

delayedGreeting("Alice");
```

---

## ⚠️ 주의: 클로저와 반복문

```js
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 3, 3, 3
  }, 1000);
}
```
- var는 함수 스코프 → 블록(중괄호 {})마다 새로 만들어지지 않음
- 즉, 루프 안에서 만들어지는 모든 콜백 함수(setTimeout) 는 동일한 i를 참조함
- setTimeout 안의 함수는 나중에 실행되기 때문에, for문은 이미 끝난 후 (i === 3) 상태에서 실행됨

```js
// 해결 방법: let 사용
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 0, 1, 2
  }, 1000);
}
```
- let은 블록 스코프
- for 루프가 돌 때마다 i는 새로운 변수로 생성됨 (각 반복마다 새로운 스코프)
- setTimeout 안의 콜백은 각기 다른 i를 클로저로 캡처

✔ 위에 내용을 봤을때, `let` 보다 `var` 가 성능적으론 좋아보이나, js 엔진이 `let` 에 최적화가 잘되어있어 차이가 없거나 `let`이 뛰어날 때도 있다.

---

## 🎯 정리

✔ 스코프는 **변수의 접근 범위**를 결정  
✔ `var`는 함수 스코프, `let`과 `const`는 블록 스코프  
✔ 자바스크립트는 **렉시컬 스코프** 기반  
✔ 클로저는 함수가 **외부 변수에 접근**할 수 있게 함  
✔ 클로저는 **데이터 보호**, **상태 유지**에 유용  
