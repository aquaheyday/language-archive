# 🏗️ System Design - 시스템 설계 정리

대규모 서비스를 위한 시스템 설계 개념, 아키텍처 패턴, 컴포넌트 구성, 확장 전략 등을 정리합니다.  
분산 시스템, 데이터 일관성, 성능 최적화 등 실무/인터뷰 핵심 주제를 포함합니다.

---

### 🧱 시스템 설계 기초
| 주제 | 파일명 | 설명 |
|------|--------|------|
| 시스템 설계란? | [what-is-system-design.md](./what-is-system-design.md) | 개념, 접근 방식, 인터뷰 문제 유형 |
| 요구사항 정의 | [requirements.md](./requirements.md) | 기능/비기능 요구사항 수집 및 우선순위 |
| 트래픽 추정 | [traffic-estimation.md](./traffic-estimation.md) | QPS, 요청 수, 대역폭 계산법 |
| 용량 계획 (Capacity Planning) | [capacity-planning.md](./capacity-planning.md) | 스토리지/네트워크/서버 리소스 예측 |

---

### ⚙️ 핵심 컴포넌트
| 주제 | 파일명 | 설명 |
|------|--------|------|
| 웹 서버 & 애플리케이션 서버 | [web-app-server.md](./web-app-server.md) | 요청 처리의 역할 분담 |
| 데이터베이스 설계 | [database-design.md](./database-design.md) | 정규화, 샤딩, 복제 등 |
| 캐시 계층 | [caching.md](./caching.md) | Redis, CDN, Local Cache 전략 |
| 로드 밸런서 | [load-balancer.md](./load-balancer.md) | L4 vs L7, 라운드로빈, 헬스체크 |
| 메시지 큐 | [message-queue.md](./message-queue.md) | 비동기 처리, Kafka, RabbitMQ |
| 스토리지 시스템 | [storage.md](./storage.md) | 객체 스토리지, 블록 스토리지 개념 |

---

### 📐 설계 패턴 & 구조
| 주제 | 파일명 | 설명 |
|------|--------|------|
| 모놀리식 vs 마이크로서비스 | [monolith-vs-microservice.md](./monolith-vs-microservice.md) | 아키텍처 패턴 비교 |
| 서버리스 아키텍처 | [serverless.md](./serverless.md) | Lambda, Cloud Function 개요 |
| API 게이트웨이 | [api-gateway.md](./api-gateway.md) | 인증, 라우팅, 요청 필터링 등 |
| 백프레셔 & 리트라이 전략 | [backpressure-retry.md](./backpressure-retry.md) | 과부하 제어, 안정성 향상 방법 |

---

### 🛰️ 분산 시스템 개념
| 주제 | 파일명 | 설명 |
|------|--------|------|
| 분산 시스템 기본 개념 | [distributed-system.md](./distributed-system.md) | CAP 이론, 분산 환경 특징 |
| 일관성 모델 | [consistency-models.md](./consistency-models.md) | Strong, Eventual, Causal 등 |
| 리더-팔로워 패턴 | [leader-follower.md](./leader-follower.md) | 마스터/슬레이브 구성 전략 |
| 데이터 복제와 샤딩 | [replication-sharding.md](./replication-sharding.md) | 확장성과 가용성 향상 기법 |

---

### 📈 확장성 & 성능 최적화
| 주제 | 파일명 | 설명 |
|------|--------|------|
| 수평/수직 확장 | [scalability.md](./scalability.md) | Scale Out vs Scale Up |
| CDN 적용 | [cdn.md](./cdn.md) | 정적 리소스 최적화 |
| 레이트 리미팅 | [rate-limiting.md](./rate-limiting.md) | 유저/서버 과부하 제어 기법 |
| 성능 모니터링 | [monitoring.md](./monitoring.md) | 로그, 메트릭, 알림 시스템 설계 |

