# 📖 Python Notes

이 폴더는 **Python 학습 과정에서 정리한 개념 노트**를 보관하는 공간입니다.  
각 노트는 개념 정의, 기본 문법, 예제 코드, 실전 팁, 관련 링크 순으로 구성해 체계적인 학습 자료로 활용할 수 있습니다.

---

## 📋 Python 개념 정리 목록

### 📌 기본 개념
| 번호 | 주제 | 파일명 | 설명 |
|---|---|---|---|
| 01 | Python 개요 | [python-intro.md](./python-intro.md) | Python이란? 특징과 장점 |
| 02 | 개발 환경 설정 | [setup-python.md](./setup-python.md) | Python 설치, 가상 환경, 패키지 관리 |
| 03 | 변수와 자료형 | [variables.md](./variables.md) | 숫자, 문자열, 리스트, 딕셔너리 등 |
| 04 | 연산자와 표현식 | [operators.md](./operators.md) | 산술, 비교, 논리, 비트 연산자 |
| 05 | 조건문과 반복문 | [control-flow.md](./control-flow.md) | if, for, while 문법과 사용법 |

### 🎯 함수와 객체지향 프로그래밍
| 번호 | 주제 | 파일명 | 설명 |
|---|---|---|---|
| 06 | 함수 사용법 | [functions.md](./functions.md) | 함수 정의, 매개변수, 반환값, 람다 |
| 07 | 모듈과 패키지 | [modules.md](./modules.md) | 모듈 가져오기, 패키지 구조 이해 |
| 08 | 클래스와 객체 | [oop.md](./oop.md) | 클래스, 객체, 상속, 다형성 |
| 09 | 예외 처리 | [exceptions.md](./exceptions.md) | try-except, raise, 사용자 정의 예외 |

### 🔄 데이터 처리
| 번호 | 주제 | 파일명 | 설명 |
|---|---|---|---|
| 10 | 파일 입출력 | [file-io.md](./file-io.md) | 텍스트 파일, CSV, JSON 다루기 |
| 11 | 데이터베이스 | [database.md](./database.md) | SQLite, MySQL, PostgreSQL 활용 |
| 12 | 웹 스크래핑 | [web-scraping.md](./web-scraping.md) | BeautifulSoup, Selenium 사용법 |

### 🌍 네트워크와 API 연동
| 번호 | 주제 | 파일명 | 설명 |
|---|---|---|---|
| 13 | HTTP 요청 | [http-requests.md](./http-requests.md) | requests 라이브러리, API 호출 |
| 14 | 비동기 프로그래밍 | [async.md](./async.md) | async/await, asyncio 활용법 |
| 15 | 웹 개발 | [web-frameworks.md](./web-frameworks.md) | Flask, FastAPI, Django 기본 |

### 🚀 고급 개념
| 번호 | 주제 | 파일명 | 설명 |
|---|---|---|---|
| 16 | 정규 표현식 | [regex.md](./regex.md) | re 모듈, 패턴 매칭 활용 |
| 17 | 멀티스레딩 | [multithreading.md](./multithreading.md) | threading, multiprocessing 비교 |
| 18 | 데코레이터 | [decorators.md](./decorators.md) | 함수형 프로그래밍, 고차 함수 |
| 19 | 제너레이터와 이터레이터 | [generators.md](./generators.md) | yield, lazy evaluation |

### 🛠️ 데이터 과학 및 머신러닝
| 번호 | 주제 | 파일명 | 설명 |
|---|---|---|---|
| 20 | NumPy 기본 | [numpy.md](./numpy.md) | 배열 생성, 연산, 슬라이싱 |
| 21 | Pandas 기본 | [pandas.md](./pandas.md) | 데이터프레임 생성, 필터링, 그룹화 |
| 22 | 데이터 시각화 | [visualization.md](./visualization.md) | Matplotlib, Seaborn 그래프 그리기 |
| 23 | 머신러닝 개요 | [ml-basics.md](./ml-basics.md) | Scikit-learn을 활용한 기본 모델 |
| 24 | 딥러닝 기초 | [deep-learning.md](./deep-learning.md) | TensorFlow, PyTorch 활용 |

### 🛠️ 테스트 및 배포
| 번호 | 주제 | 파일명 | 설명 |
|---|---|---|---|
| 25 | 단위 테스트 | [testing.md](./testing.md) | unittest, pytest 활용법 |
| 26 | 패키징과 배포 | [packaging.md](./packaging.md) | pip, setuptools, PyPI 배포 |

---

## 📝 작성 가이드

- 파일명은 `스네이크케이스`로 작성 (ex: `basic_syntax.md`)
- 각 노트는 다음 형식을 추천:
    - 개념 정의
    - 기본 문법 예제
    - 실전 팁 & 주의사항
    - 관련 링크 (공식 문서, 블로그 등)
- 코드 예제는 가능하면 별도 파일(`examples/`)로 관리하고, 노트에서는 링크로 연결
