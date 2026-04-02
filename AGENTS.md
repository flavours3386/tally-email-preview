# Tally 이메일 미리보기 - Agent Guide

> 이 파일은 목차다. 상세 내용은 각 링크를 따라가라.

## Quick Start

1. 로컬에서 미리보기: `test_preview_early.html` (또는 general, vip)을 브라우저에서 열기
2. 운영 가이드 확인: `operations_guide.html`을 브라우저에서 열기
3. Netlify 배포 확인: https://stirring-sprinkles-29cf5a.netlify.app/

## Golden Principles

1. **이 저장소는 Tally_webhook의 이메일 HTML과 동기화되어야 한다** — buildEmailHtml()이나 getMail*Content()가 변경되면 여기 미리보기 HTML도 함께 업데이트할 것.
2. **정적 HTML만 관리한다** — 빌드 도구, 프레임워크 없음. HTML 파일을 그대로 Netlify에 배포.
3. **미리보기는 실제 발송 메일과 동일한 HTML 구조를 사용한다** — 테이블 레이아웃, 인라인 스타일, 600px 폭 등 이메일 호환 HTML을 그대로 유지.
4. **운영 가이드는 비개발자가 읽는 문서다** — 기술 용어를 최소화하고, 시스템 변경 전/후 비교를 명확히 보여준다.
5. **프로모션 유형별 3벌을 유지한다** — VIP, 얼리아답터, 일반 각각의 미리보기를 별도 파일로 관리. 공통 부분이라도 분리 유지.

## Key Files

```
tally-email-preview/
├── CLAUDE.md                    # 프로젝트 문서
├── AGENTS.md                    # 이 파일 (목차 + 원칙)
├── ARCHITECTURE.md              # 시스템 구조
├── .gitignore
├── operations_guide.html        # 운영 가이드 (비개발자 대상, 공유용)
├── project_summary.html         # 프로젝트 요약 (내부 공유)
├── test_preview_early.html      # 얼리아답터 이메일 시퀀스 미리보기 (메일 1/2/3)
├── test_preview_general.html    # 일반 이메일 시퀀스 미리보기 (메일 1/2/3)
├── test_preview_vip.html        # VIP 이메일 시퀀스 미리보기 (메일 1/2/3)
└── docs/
    ├── PRODUCT_SENSE.md         # 제품 방향
    ├── PLANS.md                 # 우선순위, 로드맵
    ├── design-docs/.gitkeep
    └── exec-plans/.gitkeep
```

## Docs Map

| 문서 | 용도 | 변경 빈도 |
|------|------|----------|
| [CLAUDE.md](CLAUDE.md) | 프로젝트 개요, 배포 정보 | 작업 시마다 |
| [AGENTS.md](AGENTS.md) | 목차 + 핵심 원칙 | 원칙 변경 시 |
| [ARCHITECTURE.md](ARCHITECTURE.md) | 파일 구성, 동기화 규칙 | 구조 변경 시 |
| [docs/PRODUCT_SENSE.md](docs/PRODUCT_SENSE.md) | 제품 미션, 사용자 | 분기별 |
| [docs/PLANS.md](docs/PLANS.md) | 로드맵, 기술 부채 | 월별 |

## 알파리뷰 프로모션 연관 프로젝트

이 프로젝트는 **알파리뷰 프로모션** 그룹의 최종 산출물 미리보기/공유 레이어다.

```
[Tally] 폼 생성/관리
   |
   | Tally 폼 제출 -> Google Sheets
   v
[Tally_webhook] 제출 감지 + 자동화
   |
   | buildEmailHtml() -> GmailApp 발송
   v
[tally-email-preview] 미리보기 + 운영 가이드 (이 프로젝트)
   |
   | Netlify 정적 배포
   v
   브라우저에서 미리보기/공유 가능
```

| 프로젝트 | 경로 | 역할 |
|----------|------|------|
| **Tally** | `/02.PJT/Work/Tally/` | Tally API로 폼 생성/수정 |
| **Tally_webhook** | `/02.PJT/Work/Tally_webhook/` | 시트 감지 -> 알림/이메일 발송 (HTML 원본 소스) |
| **tally-email-preview** (현재) | `/02.PJT/Work/tally-email-preview/` | 이메일 HTML 미리보기 + 운영 가이드 |

**교차 영향 주의사항**:
- Tally_webhook의 `buildEmailHtml()`, `getMail1Content()`, `getMail2Content()`, `getMail3Content()` 변경 시 이 프로젝트의 test_preview_*.html을 동기화해야 함
- 배너 이미지 URL, CTA 색상(#837AFF), 수신거부 링크 등이 양쪽에서 일치해야 함
- operations_guide.html의 시스템 흐름 설명이 실제 Tally_webhook 동작과 일치하는지 확인
