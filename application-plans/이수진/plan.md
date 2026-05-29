# 현업 적용 계획 — 이수진

- **작성자**: 이수진
- **작성일**: 2026-05-29
- **팀**: C팀
- **상태**: 완료

---

## 1. 대상 업무

- **입력 검증 + 도메인 로직이 함께 있는 기능 개발**  
  교육 과정 MagicSquare처럼 Boundary(입력·UI)와 Domain(판정·계산)이 분리되는 업무에 우선 적용한다. “기능 추가”가 아니라 **관찰된 제약·오류 상황을 AC로 정의**한 뒤 구현하는 방식으로 문제를 쓴다.

- **신규·개선 기능의 TDD 기반 개발**  
  PRD·AC ID(`AC-FR01-01` 등)로 요구사항–테스트–코드를 연결하고, RED → GREEN → REFACTOR 사이클로 진행한다.

- **코드 리뷰·팀 협업**  
  리뷰를 “통과/반려”가 아니라 **AC·Golden Master·RED/GREEN 단계 합의** 과정으로 운영한다.

---

## 2. 적용 범위 (단계)

| 단계 | 기간(예상) | 범위 | 목표 |
|------|------------|------|------|
| 1차 (시범) | 4주 | 담당 기능 1개 슬라이스 — Track A(입력 검증) + Track B(도메인 로직) Dual-Track, `.cursorrules`·SSOT 상수·AC ID | **1커밋 = 1 RED** 준수, Ask → Agent 워크플로 정착, PR 리뷰에 AC ID·GM 기준 반영 |
| 2차 (확대) | 8주~ | 동일 패턴을 담당 모듈 전반·팀 공통 규칙(CI Gate, GM 회귀)으로 확대 | Boundary 85% / Domain 95% 커버리지, GM 회귀 0건, Phase 0 Gate CI 자동화 |

---

## 3. 구체적 활용 방안

### Keep — 교육에서 효과가 있었던 것을 현업에 그대로 유지

- **Dual-Track 병렬 설계**  
  Track A(Boundary/UI)와 Track B(Domain/Logic) RED를 분리해, 한쪽이 막혀도 다른 트랙은 진행한다. 브랜치·PR도 슬라이스 단위로 나눈다.

- **1커밋 = 1 RED**  
  실패 테스트 추가 → GREEN 구현 → 스켈레톤 추가를 커밋 단위로 분리한다. GREEN 전에는 `pytest.fail("RED: …")` 스켈레톤만 두고, RED/GREEN을 한 커밋에 섞지 않는다.

- **ECB 책임 분리**  
  Boundary → Control → Entity 의존 방향을 `.cursorrules`에 고정한다. “Domain resolver” 같은 모호한 용어는 `Control.resolve` 등 **테스트·문서·코드에 동일한 이름**으로 통일한다.

- **Golden Master**  
  핵심 API·함수의 기대 출력(TD-01/TD-02, `INVALID_SIZE` 등)을 SSOT로 두고, 구현·리뷰·회귀 기준으로 사용한다.

- **Cursor Ask 모드 선행**  
  Agent로 코드를 만들기 전 Ask 모드로 PRD·AC·설계를 질문·정리한다. 이후 Agent 모드에서 ECB·TDD 규칙에 맞춰 구현한다.

### Try — 회고 Problem을 줄이기 위한 개선

- **AC ID 명시 관행 강화**  
  테스트명·커밋 메시지·리뷰 코멘트에 `AC-FR01-01`, `TC-SIZE-001` 등 ID를 필수로 적는다.

- **GM approve 절차 문서화**  
  Golden Master 기대값 확정 → 팀 리뷰 승인 → 구현 착수 순서를 팀 문서로 정리하고, 리뷰 시 “값이 맞는지”부터 합의한다.

- **도메인 Mock 금지**  
  Track B 테스트에서 도메인 객체 mock을 쓰지 않고, 실제 Entity·Control 경로로 검증한다. Boundary 실패 시 `Control.resolve` 미호출(`AC-FR01-05`) 같은 **격리 테스트**는 mock/spy로 유지한다.

- **SSOT 상수 선행 정의**  
  오류 코드·메시지·고정 크기(예: 4×4) 등을 구현 전 `constants`·계약 문서에 먼저 적어 둔다.

- **Phase 0 Gate 자동화 (팀·DevOps 협업)**  
  RED-only 커밋 검사, 커버리지 하한(80%), ECB import 방향 위반 검사를 CI에 넣는다.

### Cursor AI 워크플로

| 단계 | 모드 | 내용 |
|------|------|------|
| 1 | Ask | PRD·AC·In/Out-of-Scope·오류 계약 정리 |
| 2 | Agent | RED 테스트·스켈레톤 추가 (1 RED = 1 커밋) |
| 3 | Agent | GREEN 구현 (SSOT·ECB Rules 준수) |
| 4 | Ask/Agent | REFACTOR, transcript는 `Prompting/`에 남겨 맥락 공유 |

프롬프트는 `[P] QA`, `[C] Python`, `[T] 테스트 계획서`처럼 **역할·맥락·과제**를 나눠 작성한다. Agent 출력은 PRD·AC·오류 계약과 **한 항목씩 대조**해 검증한다.

---

## 4. 기대 효과·지표

| 항목 | 기대 효과 | 측정 방법(가능한 경우) |
|------|-----------|------------------------|
| RED/GREEN 경계 명확화 | RED·GREEN 혼용·되돌림 감소 | PR·커밋 단위 RED 확인, Phase 0 Gate 통과율 |
| 테스트·요구사항 추적 | 리뷰 코멘트가 AC 단위로 연결 | 테스트·커밋·리뷰에 AC ID 포함 비율 |
| 커버리지 | 품질 하한 확보 | CI pytest-cov — Boundary 85% / Domain 95% (팀 Gate 80% 이상) |
| Golden Master | 회귀 조기 발견 | GM 회귀 건수 0건 목표 |
| AI-assisted 개발 | 설계·구현 혼선 감소 | Ask 선행 후 Agent 사용 비율, transcript·리뷰 피드백 건수 |
| 협업·리뷰 | “LGTM” 대신 근거 있는 합의 | PR에 AC·GM·RED/GREEN 단계 명시 비율 |

---

## 5. 리스크·전제 조건

- **보안·컴플라이언스**:  
  Transcript Export·Prompting 기록에 고객명, 내부 URL, 자격 증명, 미공개 소스를 넣지 않는다. Cursor에 올리는 맥락은 익명화·범위 제한한다.

- **사내 정책·도구 제약**:  
  Cursor·CI 도구 사용은 사내 정책·라이선스 범위 안에서만 한다. Dual-Track 브랜치 규칙·PR 규칙은 Tech Lead와 합의된 문서를 따른다.

- **반드시 사람이 검수할 영역**:  
  Golden Master 기대값·오류 코드 계약 확정, AC와 PRD의 In/Out-of-Scope, AI가 생성한 Domain 로직·보안 관련 Boundary 처리, GM approve 최종 승인.

---

## 6. 필요한 지원

| 구분 | 필요 지원 | 비고 |
|------|-----------|------|
| Tech Lead | Dual-Track 브랜치·PR 규칙 문서화 (1주 내) | PR 규칙 문서 공유 |
| DevOps | CI/CD 커버리지 80% 하한·Phase 0 Gate (2주 내) | 파이프라인 통과 |
| QA | 핵심 API Golden Master 기준 파일 관리 (3주 내) | GM 회귀 0건 |
| 팀 전체 | 프로젝트별 `.cursorrules`(TDD·ECB) 작성·리뷰 준수 | 즉시 시작 |
| 개발자(본인 포함) | Ask → Agent 워크플로·1커밋=1 RED (4주 내) | 커밋당 RED 확인 |

- **pytest-cov** 도입·측정 기준(Boundary/Domain 분리) 팀 합의  
- Phase 0 Gate·GM approve 초안 리뷰를 위한 **짧은 팀 세션** (회고 KPT Try 항목 공유)

---

## 7. 일정·다음 액션

- [ ] 시범 기능 1개 선정 후 SSOT·AC ID 선행 정의
- [ ] `.cursorrules` 작성 및 Ask → Agent 워크플로로 Dual-Track RED 착수
- [ ] Golden Master 팀 approve 후 GREEN 진행
- [ ] 팀 PR·브랜치 규칙·커버리지 Gate·GM 기준 합의

---

**한 줄 요약**: Dual-Track TDD·ECB·AC ID·Golden Master를 현업 1개 슬라이스에 시범 적용하고, 팀 Gate·GM·브랜치 규칙과 맞춰 AI-assisted 개발의 RED/GREEN 혼선을 줄인다.
