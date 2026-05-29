# 현업 적용 계획 — 이재호

- **작성자**: 이재호
- **작성일**: 2026-05-29
- **팀**: C팀
- **상태**: 초안

---

## 1. 대상 업무

- C팀 담당 **초음파 진단 S/W** (C# / WPF / .NET Framework 4.8) Legacy 제품에 AI(Cursor) 기반 개발 방식을 도입하여, **기존 제품 기능 추가·변경** 업무의 예측 가능성과 검증 가능성을 높인다.
- 제품은 **Platform(공용 모듈) + Product(제품별 기능)** 이원 구조이며, 영상 품질 컨트롤·측정·병원 Workflow 등 다수 기능을 포함한다. 빌드 산출물 **10GB+** 규모로 AI 도입 시 회귀 리스크·검수 부담이 크다.
- MagicSquare PoC에서 검증한 **Dual-Track · ECB · Golden Master · Phase 0 Gate** 패턴을 Pilot 모듈에 축소 이식한다.
- **1차 Pilot 후보**: Measure Control, Workflow Step, Image Pipeline 서브모듈 중 **의존성이 적고 AC가 명확한 1개** *(팀 내 확정 필요)*

---

## 2. 적용 범위 (단계)

| 단계 | 기간(예상) | 범위 | 목표 |
|------|------------|------|------|
| 1차 (시범) | 4주 | Pilot 모듈 1개 (Product Track B 중심, Platform Contract 최소 연동) | Phase 0 Gate 구축, AC ID·GM baseline·CI 1 job 가동, **기능 1건 End-to-End (RED→GREEN→GM approve)** |
| 2차 (확대) | 8주 | 동일 Product 내 연관 모듈 2~3개 + Platform Interface SSOT | Dual-Track 병렬 운영, 레이어별 커버리지 gate, Control·Boundary smoke 테스트 확장 |
| 3차 (정착) | 12주~ | C팀 담당 Workflow·Measure 영역 전반, PR·Cursor Rule 팀 SSOT화 | AI-assisted 기능 추가가 **표준 개발 절차**로 정착, KPT 지표로 분기 회고 |

---

## 3. 구체적 활용 방안

**Keep — 유지·표준화**

- **Dual-Track**: Track A(Platform·Boundary·Contract) / Track B(Product·Entity·Control). 독립 RED 사이클, 통합은 GM·Integration test에서만 수행.
- **1커밋 = 1 RED**: 커밋·PR에 `AC-FR-xx-yy` + `RED-xxx` ID 필수. RED PR에서 `src/` 변경 시 CI reject.
- **ECB 책임 분리**: Boundary(View/ViewModel 검증) · Control(UseCase·Workflow) · Entity(영상·측정·Clinical). ViewModel 내 도메인 계산은 리뷰 reject.
- **Golden Master**: Workflow 출력·Measure 결과·Preset 등 baseline 고정, REFACTOR/릴리스 전 diff approve.
- **Cursor Ask 모드**: 코드 변경 전 영향 분석·ECB 위반·AC traceability. Agent는 **좁은 스코프**(파일 ≤5)만 사용.

**Problem → Try 대응**

| Problem | Try | 구체 조치 |
|---------|-----|-----------|
| RED↔GREEN 혼용 | Phase 분리 + Gate | PR 템플릿 Phase 체크, CI로 RED·GREEN 파일 변경 분리 |
| 프롬프트 부족 | AC ID 명시 강화 | 표준 프롬프트: AC · Phase · ECB Layer · In-scope files · Out of scope |
| AI 제안 검증 미흡 | GM approve + 검수 영역 정의 | 주 1회 AI diff ECB·Mock·범위 리뷰 |
| 커버리지 미달 | Phase 0 Gate 자동화 | Entity ≥95%, Control/Boundary ≥85%, View는 smoke+GM |
| ECB 혼재 | 도메인 Mock 금지 + SSOT 상수 선행 | Port만 mock; ErrorCode·PresetId·WorkflowStepId SSOT 선행 정의 |

**Cursor AI 운영**

- **Ask**: 영향 분석, ECB 스멜, GM diff 해석
- **Agent**: RED-only 또는 GREEN-only (cross-layer refactor·domain mock 금지)
- **Agent 세션 시작 4줄 (필수)**: AC ID · Phase · ECB Layer · In-scope files

**산출물 (SSOT)**

- `docs/traceability/` — AC ↔ Test ID ↔ Scenario 매핑
- `tests/golden_master/` — baseline + approve 이력
- `docs/gm_approval.md` — diff 유형별 승인 기준·담당
- `.cursor/rules/` — ECB·Phase·Mock 규칙
- CI: `dotnet test` + coverlet + GM diff check (Phase 0 Gate G-01~G-05)

---

## 4. 기대 효과·지표

| 항목 | 기대 효과 | 측정 방법(가능한 경우) |
|------|-----------|------------------------|
| 회귀 리스크 감소 | GM·Gate 통과 후 merge로 Legacy 변경 안정화 | GM diff 건수, unapproved merge 0건 |
| RED/GREEN 품질 | TDD Phase 혼용 제거, 리뷰·CI 부담 감소 | Phase 혼용 PR 0건/월 |
| 추적 가능성 | AC·Test·코드 1:1 매핑 | AC ID 없는 PR 0건 |
| ECB 준수 | 레이어 책임 명확, AI 제안 품질 향상 | ECB architecture test pass, 주간 위반 건수 |
| 커버리지 (Pilot) | Entity 중심 gate 충족 | coverlet: Entity ≥95%, Control/Boundary ≥85% |
| AI 활용 효율 | Ask(분석) + Agent(좁은 구현) 분업 | 기능 1건당 Agent 턴 수, 재작업 PR 비율 |
| 기능 추가 Lead Time | Pilot 1건 E2E AC+GM+Gate 통과 | 1차 4주 종료 시 1건 완료 여부 |
| 팀 역량 | 프롬프트·AI diff 검증 능력 | 분기 KPT 재회고 Problem 항목 추이 |

**1차(4주) 성공 기준**: Pilot 모듈 1건 · AC+GM approve · Phase 0 CI green · RED/GREEN 분리 PR 100%

---

## 5. 리스크·전제 조건

- **보안·컴플라이언스**:
  - Golden Master·테스트 입력에 **환자·병원 실데이터 사용 금지** (합성/익명 fixture만)
  - Cursor/AI 사용 시 **사내 정보보호·의료기기 SW 수명주기(SaMD/QMS) 정책** 준수 *(IEC 62304 등 해당 절차 확인)*
  - 소스·로그·프롬프트에 API Key, 라이선스 키, 내부 URL 포함 금지
  - AI 생성 코드는 **라이선스 호환** 및 내부 코드 리뷰·승인 후 merge

- **사내 정책·도구 제약**:
  - Cursor Agent **사내망/VDI**·외부 LLM 사용 허용 범위 *(IT·보안팀 확인 필요)*
  - 10GB+ 솔루션 — Pilot은 **서브솔루션/단일 csproj** 단위로 CI gate 분리
  - .NET Framework 4.8 + WPF — UI 테스트는 **ViewModel smoke + GM** 위주, full UI automation은 2차 검토

- **반드시 사람이 검수할 영역**:
  - Clinical·진단 관련 Entity 로직 (환자 안전·규제)
  - Golden Master baseline 변경·approve (회귀 기준 변경)
  - Platform Public API·Contract 변경 (다 Product 영향)
  - 성능·실시간 영상 Pipeline (AI는 단위 테스트, 통합 성능은 사람)
  - 보안·인증·감사 로그
  - RED/GREEN Phase 판정·ECB 위반 여부

- **전제 조건**: Pilot 모듈·AC 1건 팀 합의, GM baseline 저장소 위치, CI job 1개 할당, Cursor Rule·PR template merge 권한

---

## 6. 필요한 지원

- **C팀**: Pilot 모듈·AC 선정, Traceability 표 작성, GM baseline 1세트, 주 1회 AI diff 리뷰 rotation
- **Platform팀**: Pilot 연동 Contract·ErrorCode SSOT 검토, Public API 변경 시 사전 리뷰
- **QA**: GM approve 절차 합의, invalid case GM 시나리오, 커버리지 gate 임계값 합의
- **DevOps / IT**: 서브솔루션 CI job, coverlet 리포트, Cursor·LLM 사용 정책 clarification
- **아키텍트 / Tech Lead**: ECB 레이어 WPF 매핑 1페이지 SSOT, Phase 0 Gate G-01~G-05 dotnet版 sign-off
- **MagicSquare PoC**: Report/16·17 로드맵·Gate 체크리스트 C팀 워크숍 자료 *(이재호)*

---

## 7. 일정·다음 액션

- [ ] **D+3** Pilot 모듈·AC 1건 후보 3개 → 1개 확정 (C팀)
- [ ] **D+5** AC ID ↔ Test ID Traceability 초안 (이재호)
- [ ] **D+7** GM baseline 캡처·저장 경로·`gm_approval.md` 초안 (C팀 + QA)
- [ ] **D+10** `.cursor/rules` + PR template (Phase/AC/ECB) (이재호)
- [ ] **D+14** Phase 0 CI 1 job — dotnet test + coverlet Entity (DevOps)
- [ ] **D+21** Pilot RED-only PR 1건 merge (C팀)
- [ ] **D+28** Pilot GREEN + GM approve → 1차 시범 종료 리뷰 (C팀 + QA)
- [ ] 1차 4주 종료 KPT 재회고 일정 잡기

---

*본 문서는 MagicSquare REFACTOR Phase 0(Report/17) 및 KPT 회고(Keep/Problem/Try)를 C팀 Legacy 현업에 맞게 축소·번역한 초안이다. Pilot 모듈 확정 후 §2·§7 일정을 갱신한다.*
