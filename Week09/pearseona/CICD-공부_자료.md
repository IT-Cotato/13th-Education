## 1. CI/CD가 왜 필요한가

- 개발이 끝나도 **코드 병합 → 테스트 → 빌드 → 서버 반영 → 최종 확인**이라는 절차가 남아있음
- 이 과정을 사람이 매번 손으로 하면 단계가 빠지거나, 사람마다 순서가 달라질 수 있음

→ **코드를 작성하는 것만큼, 작성한 코드를 안전하게 전달하는 과정도 중요하다**

---

## 2. CI와 CD 각각의 역할

| 구분 | 하는 일 |
| --- | --- |
| **CI** (Continuous Integration) | 변경된 코드를 **기존 코드에 합쳐도 되는지** 자동으로 검증 |
| **CD** (Continuous Delivery/Deployment) | 검증된 결과물을 **원하는 환경에 전달** |

→ **CI/CD의 목적은 무조건 빠른 배포가 아니라, 같은 절차를 반복해 더 자주, 더 안전하게 전달하는 것**

---

## 3. CI란 무엇인가

- 여러 사람이 각자의 브랜치에서 작업한 코드를 기준 브랜치(main)로 **자주** 합침
- 합칠 때마다 테스트·빌드 같은 검증을 자동으로 수행
- 변경 크기가 작을수록 문제 발생 시 확인해야 할 범위도 작아짐

---

## 4. 왜 코드를 자주 합쳐야 하는가

**큰 변경, 늦은 통합**:

- 변경 20개를 한 번에 병합 → 충돌 원인 불명확 → 수정 범위 큼

**작은 변경, 잦은 통합**:

- 변경 2~3개씩 자주 병합 → 최근 변경만 확인하면 됨 → 수정 범위 작음

→ **자주 통합한다고 충돌이 사라지진 않지만, 문제가 커지기 전에 더 빨리 발견하고 작은 범위에서 해결할 수 있다**

---

## 5. CI가 검사하는 항목

| 항목 | 확인하는 것 |
| --- | --- |
| Lint | 코드 스타일 검사 |
| Test | 기능 동작 검증 |
| Build | 빌드 가능 여부 확인 |
| Security (선택) | 취약점·패키지 검사 |

→ **CI는 모든 문제를 알아서 찾아 주는 것이 아니라, 팀이 정한 기준을 반복 검사하는 도구다**

---

## 6. 테스트는 어떻게 나뉘는가

- **단위 테스트**: 함수 하나, 컴포넌트 하나 — 작은 범위를 빠르게 확인
- **통합 테스트**: 서버 + DB처럼 여러 요소가 함께 정상 동작하는지 검사
- **E2E 테스트**: 로그인 → 메인 화면 도착처럼 전체 사용자 흐름을 확인

위로 갈수록 실제 상황과 비슷하지만 범위·시간·비용이 커짐

→ **한 종류만 쓰는 게 아니라 목적에 맞게 조합해서 사용한다**

## 7. CI 실패를 대하는 태도

- 실패는 부정적으로 느껴지지만, CI 실패는 **배포 전에** 문제를 발견했다는 뜻이라 오히려 좋은 신호일 수 있음

→ **중요한 건 실패를 없애는 게 아니라, 어느 단계에서 왜 실패했는지 빠르게 확인하고 수정하는 것**

---

## 8. 파이프라인은 언제 시작되는가 (Trigger)

파이프라인은 항상 도는 게 아니라, 정해진 이벤트(**Trigger**)가 발생할 때만 실행됨.

| Trigger | 시점 |
| --- | --- |
| Pull Request | PR 생성 시 |
| Push | 특정 브랜치에 코드 반영 시 |
| Schedule | 정해진 시간 |
| Manual | 사용자가 직접 실행 |

---

## 9. Trigger에 따라 실행 범위가 다른 이유

- Pull Request → 빠른 검사로 병합 가능 여부만 확인
- Main Push → Staging 환경 배포
- Version Tag / Manual → Production 배포 (승인 등 더 제한된 조건)

**Version Tag** = "이 버전을 배포하겠다"라는 표시

→ **모든 이벤트에서 모든 작업을 실행할 필요는 없다. 이벤트마다 목적이 다르기 때문**

---

## 10. 파이프라인 내부 구조 — Workflow / Job / Step

| 용어 | 의미 |
| --- | --- |
| Workflow | 전체 자동화 흐름 |
| Job | 목적별 독립적인 작업 묶음 (예: Frontend, Backend) |
| Step | Job 안에서 실행되는 실제 단위 (Checkout, Install, Test 등) |

→ **Workflow 안에 Job이 있고, Job 안에 Step이 있다**

---

## 11. Job의 병렬 실행

서로의 결과를 기다릴 필요가 없는 Job(프론트 테스트 vs 백엔드 테스트)은 동시에 실행 가능.

```
코드 변경
   ↓
Frontend Test(40초)   Backend Test(50초)
   ↓
Build Job
```

- 순차 실행: 90초 / 병렬 실행: 약 50초

→ **독립적인 작업을 병렬로 실행하면 파이프라인의 대기 시간을 줄일 수 있다**

---

## 12. Artifact란 무엇인가

- 빌드를 통해 만들어진 **배포 가능한 결과물**
- Frontend → dist 폴더 / Backend → JAR 파일 / Container → Docker Image

```
Source → Build → Artifact
```

→ **CI가 Artifact를 만들고 검증하면, CD는 그 결과물을 전달한다**

## 13. 왜 환경마다 다시 빌드하면 안 되는가 (Build Once, Deploy Many)

- 같은 코드라도 빌드 시점의 패키지 버전·실행 환경 차이로 서로 다른 결과물이 나올 수 있음
- 스테이징에서 검증한 게 Artifact A인데, 운영에서 새로 빌드한 Artifact B를 배포하면 **검증한 것과 다른 결과물**을 사용자에게 전달하는 셈

→ **한 번 빌드하고 검증한 동일한 Artifact를 여러 환경에 배포한다**

---

## 14. 배포 환경 3단계

| 환경 | 역할 |
| --- | --- |
| Development | 개발 중 기능을 빠르게 확인 |
| Staging | 운영과 비슷한 조건에서 배포 전 QA |
| Production | 실제 사용자에게 서비스 |

→ **환경마다 다른 코드를 쓰는 게 아니라, 코드와 Artifact는 동일하게 유지하고 API 주소·실행 모드 같은 환경별 설정만 분리한다**

---

### 15. Continuous Delivery vs Continuous Deployment

두 방식 모두 테스트, 빌드, 스테이징 배포까지는 자동화됨. 차이는 마지막 단계.

| 구분 | Continuous Delivery | Continuous Deployment |
| --- | --- | --- |
| 흐름 | Test → Build → Ready | Test → Build → Deploy |
| 운영 배포 | 사람이 마지막 승인 | 모든 검사 통과 시 자동 배포 |

→ **차이는 자동화 유무가 아니라, 마지막 운영 배포를 누가 결정하는가다**

---

## 16. 환경 변수와 Secret 구분하기

| 구분 | 예시 | 특징 |
| --- | --- | --- |
| Environment Variable | API URL, MODE=production, REGION=seoul | 일반적인 설정값 |
| Secret | DATABASE_PASSWORD, ACCESS_TOKEN, API_KEY | 공개되면 안 되는 값 |
- Secret은 코드에 직접 작성하지 않고, 파이프라인 실행 중 필요한 위치에만 전달
- 코드에 없어도 실행 로그에 값이 출력되면 그대로 노출될 수 있음

→ **Secret은 코드·저장소·실행 로그 어디에도 노출되면 안 된다**

---

## 17. 전체 파이프라인

```
Pull Request(코드 변경)
   ↓
CI (Lint / Test / Build)
   ↓
Artifact (빌드 결과물)
   ↓
Staging (검증 환경)
   ↓ 승인 / 조건
Production (실제 사용자)
```

- 어느 단계에서든 실패하면 이후 단계는 진행되지 않음

→ **CI/CD는 코드를 검증하고, 검증한 동일한 결과물을 단계적으로 전달하는 과정이다**

---

## 18. 핵심 요약

- CI = 변경된 코드를 자동으로 검증
- CD = 검증된 결과물을 안전하게 전달
- 목표 = 더 자주, 더 안전하게 배포하기

→ **자동화의 목적은 사람을 없애는 것이 아니라, 반복 작업에서 발생하는 실수를 줄이고 사람이 중요한 판단에 집중하도록 돕는 것**

---

**참고 자료**

GitHub Actions 공식 문서: https://docs.github.com/en/actions

Understanding GitHub Actions: https://docs.github.com/en/actions/get-started/understand-github-actions

Continuous Integration | Martin Fowler: https://martinfowler.com/articles/continuousIntegration.html

Continuous Delivery | Martin Fowler: https://martinfowler.com/bliki/ContinuousDelivery.html