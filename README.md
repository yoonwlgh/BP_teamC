# BP Team C — Cursor 응용과정 공유 공간

Cursor 응용과정을 수료한 **C팀** 멤버들이 **회고록**과 **현업 적용 계획**을 모아 공유하는 저장소입니다.

---

## 목적

| 구분 | 설명 |
|------|------|
| **회고록** | 교육 중 배운 내용, 인상 깊었던 점, 어려웠던 점, 팀 내 논의 내용 등을 정리합니다. |
| **현업 적용 계획** | 실제 업무에 Cursor·AI 보조 개발을 어떻게 도입할지, 단계·범위·기대 효과를 공유합니다. |

팀원 모두가 같은 맥락을 보며 학습 내용을 내 업무로 연결하고, 적용 아이디어를 서로 참고할 수 있도록 합니다.

---

## 팀 멤버 (C팀)

| No. | 이름 | 이메일 |
|-----|------|--------|
| 15 | 윤지호 | yoonwlgh@gmail.com |
| 16 | 이민준 | wcdksky@gmail.com |
| 17 | 이수진 | suuuuuujin909@gmail.com |
| 18 | 이승환 | sh.cat.lee@gmail.com |
| 19 | 이영실 | youngsillee35@gmail.com |
| 20 | 이재호 | amalanchi@gmail.com |
| 21 | 이진범 | jbjbljb@gmail.com |

---

## 폴더 구조

```
BP_teamC/
├── README.md
├── retrospectives/
│   ├── _templates/
│   │   └── 회고록-템플릿.md
│   ├── 윤지호/
│   │   └── 2026-05-29.md
│   ├── 이민준/
│   ├── 이수진/
│   ├── 이승환/
│   ├── 이영실/
│   └── 이재호/
|   └── 이진범/
└── application-plans/
    ├── _templates/
    │   └── 현업적용계획-템플릿.md
    ├── 윤지호/plan.md
    ├── 이민준/plan.md
    ├── 이수진/plan.md
    ├── 이승환/plan.md
    ├── 이영실/plan.md
    └── 이재호/plan.md
    └── 이진범/plan.md
```

추가 회고는 `retrospectives/{본인 이름}/YYYY-MM-DD.md` 로 새 파일을 만들면 됩니다.

---

## 멤버별 문서 바로가기

| 이름 | 회고록 | 현업 적용 계획 |
|------|--------|----------------|
| 윤지호 | [retrospectives/윤지호/2026-05-29.md](retrospectives/윤지호/2026-05-29.md) | [application-plans/윤지호/plan.md](application-plans/윤지호/plan.md) |
| 이민준 | [retrospectives/이민준/2026-05-29.md](retrospectives/이민준/2026-05-29.md) | [application-plans/이민준/plan.md](application-plans/이민준/plan.md) |
| 이수진 | [retrospectives/이수진/2026-05-29.md](retrospectives/이수진/2026-05-29.md) | [application-plans/이수진/plan.md](application-plans/이수진/plan.md) |
| 이승환 | [retrospectives/이승환/2026-05-29.md](retrospectives/이승환/2026-05-29.md) | [application-plans/이승환/plan.md](application-plans/이승환/plan.md) |
| 이영실 | [retrospectives/이영실/2026-05-29.md](retrospectives/이영실/2026-05-29.md) | [application-plans/이영실/plan.md](application-plans/이영실/plan.md) |
| 이재호 | [retrospectives/이재호/2026-05-29.md](retrospectives/이재호/2026-05-29.md) | [application-plans/이재호/plan.md](application-plans/이재호/plan.md) |
| 이진범 | [retrospectives/이진범/2026-05-29.md](retrospectives/이진범/2026-05-29.md) | [application-plans/이진범/plan.md](application-plans/이진범/plan.md) |

공통 템플릿: [회고록-템플릿](retrospectives/_templates/회고록-템플릿.md) · [현업적용계획-템플릿](application-plans/_templates/현업적용계획-템플릿.md)

---

## 작성 가이드

### 회고록 (`retrospectives/`)

다음 항목을 참고해 자유롭게 작성합니다.

- 교육에서 가장 도움이 된 내용
- 처음 써 본 기능·워크플로 (예: Agent, Rules, MCP 등)
- 막혔던 점과 해결 방법
- 팀 활동·페어 작업에서 배운 점
- 다음에 더 깊이 보고 싶은 주제

### 현업 적용 계획 (`application-plans/`)

다음 항목을 포함하면 팀 공유에 유리합니다.

- **대상 업무**: 어떤 프로젝트·업무에 적용할지
- **적용 범위**: 1차(시범) / 2차(확대) 등 단계
- **구체적 활용**: 코드 리뷰, 문서화, 리팩터링, 테스트, 온보딩 등
- **기대 효과·지표**: 시간 절감, 품질, 학습 곡선 등 (가능한 범위에서)
- **리스크·전제**: 보안, 사내 정책, 검수 필요 영역 등

---

## 기여 방법 (브랜치 → main 병합)

문서 수정은 **`main`에 직접 push하지 않고**, **본인 이름 브랜치**에서 작업한 뒤 **`main`으로 merge**하는 방식을 사용합니다. 서로의 작업이 겹치지 않도록 하기 위함입니다.

### 브랜치 이름 규칙

- 형식: `{이름}` (예: `윤지호`, `이민준`, `이수진`)
- 한 사람당 하나의 작업 브랜치를 쓰거나, 회고·계획을 나누고 싶다면 `{이름}/회고`, `{이름}/현업계획`처럼 붙여도 됩니다.

### 작업 순서

1. 저장소를 clone 하고 최신 `main`을 받습니다.

```bash
git clone https://github.com/yoonwlgh/BP_teamC.git
cd BP_teamC
git pull origin main
```

2. 본인 브랜치를 만들고 이동합니다.

```bash
git checkout -b 윤지호
```

3. `retrospectives/{본인 이름}/` 또는 `application-plans/{본인 이름}/` 아래 문서를 수정·작성합니다.

4. commit 후 **본인 브랜치**를 push 합니다.

```bash
git add .
git commit -m "docs: 윤지호 Cursor 교육 회고 (2026-05-29)"
git push -u origin 윤지호
```

5. GitHub에서 **Pull Request**를 열어 `main` ← `{본인 브랜치}` 로 merge 합니다.  
   (로컬에서 merge하는 경우: `main`으로 checkout → `git merge 윤지호` → `git push origin main`)

```bash
git checkout main
git pull origin main
git merge 윤지호
git push origin main
```

6. merge가 끝나면 필요 시 본인 브랜치를 삭제하거나, 다음 수정 때 같은 브랜치에서 `main`을 다시 pull 받고 이어서 작업합니다.

### 커밋 메시지 예시

- `docs: 윤지호 Cursor 교육 회고 (2026-05-29)`
- `docs: 이민준 현업 적용 계획 초안`

---

## 참고

- 민감한 정보(고객명, 내부 URL, 자격 증명, 미공개 소스)는 올리지 않습니다.
- 회고·계획은 **학습·협업 공유** 목적이며, 공식 산출물이 아닐 수 있습니다. 필요 시 팀 리더와 형식을 맞춥니다.

---

## 문의

C팀 내용·저장소 사용 방법은 팀 채널 또는 위 멤버 목록을 통해 협의합니다.
