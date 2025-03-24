# 📌 React 프로젝트 설정

React 프로젝트를 시작하기 위해 환경을 설정하는 방법을 정리합니다.  
**CRA(Create React App)와 Vite**를 사용한 설치 방법을 다룹니다.

---

## 1️⃣ React 개발 환경 준비

#### Node.js 설치
React를 실행하려면 **Node.js**가 필요합니다.  

```sh
node -v  # Node.js 버전 확인
npm -v   # npm (Node 패키지 매니저) 버전 확인
```

✔ [Node.js 공식 사이트](https://nodejs.org/)에서 최신 버전을 다운로드하세요.

---

## 2️⃣ React 프로젝트 생성 방법

### 1) CRA(Create React App) 사용
```sh
npx create-react-app my-app
cd my-app
npm start
```
- **npx**: 패키지를 실행하는 명령어 (설치 없이 최신 버전 사용 가능)
- **my-app**: 프로젝트 폴더명 (원하는 이름으로 변경 가능)
- **npm start**: 개발 서버 실행 (`http://localhost:3000`)

---

### 2) Vite 사용
```sh
npm create vite@latest my-app --template react
cd my-app
npm install
npm run dev
```
- **Vite는 CRA보다 빠른 개발 서버**를 제공합니다.
- `http://localhost:5173`에서 실행됩니다.

---

## 3️⃣ 프로젝트 구조 이해

#### 기본 폴더 구조 (CRA 기준)
```
my-app/
├── node_modules/    # 설치된 패키지
├── public/          # 정적 파일 (HTML, 이미지 등)
├── src/             # 주요 코드 (컴포넌트, 스타일 등)
│   ├── App.js       # 메인 컴포넌트
│   ├── index.js     # 진입점 파일
│   ├── components/  # 재사용 가능한 컴포넌트 (폴더 추가 가능)
│   ├── styles/      # 스타일 파일 (폴더 추가 가능)
├── .gitignore       # Git에 포함되지 않을 파일 목록
├── package.json     # 프로젝트 정보 및 종속성 관리
├── README.md        # 프로젝트 설명
```

---

## 4️⃣ 주요 명령어

| 명령어 | 설명 |
|--------|------|
| `npm start` | 개발 서버 실행 (CRA) |
| `npm run dev` | 개발 서버 실행 (Vite) |
| `npm run build` | 프로젝트 빌드 (배포용) |
| `npm install [패키지명]` | 패키지 설치 |
| `npm uninstall [패키지명]` | 패키지 삭제 |

---

## 5️⃣ ESLint & Prettier 설정 (코드 스타일 정리)

#### 1. ESLint 설치
```sh
npm install eslint --save-dev
npx eslint --init
```
✔ 설정 파일을 생성하고 원하는 스타일을 선택하세요.

#### 2. Prettier 설치
```sh
npm install --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```
✔ `.eslintrc.js` 또는 `.eslintrc.json` 파일을 수정하여 **Prettier를 ESLint와 함께 사용**하도록 설정하세요.

---

## 6️⃣ React 프로젝트 실행 확인
`npm start` 또는 `npm run dev` 실행 후 브라우저에서 확인하세요.
- CRA: `http://localhost:3000`
- Vite: `http://localhost:5173`

---

## 🎯 정리
✔ React 프로젝트 생성 방법 → `npx create-react-app my-app (CRA)` 또는 `npm create vite@latest my-app --template react` (Vite)  
✔ Vite는 CRA보다 더 빠르고 가벼운 개발 환경을 제공  
✔ 프로젝트 구조 이해 → `src/`에서 주요 컴포넌트와 스타일 관리  
✔ 자주 사용하는 명령어 → `npm start`(CRA) / `npm run dev`(Vite) / `npm run build`(배포)  
✔ ESLint & Prettier 설정 → 코드 품질 유지 및 자동 정리 가능  
