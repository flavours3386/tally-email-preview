# Tally 이메일 미리보기

> 문서 목차 및 핵심 원칙: [AGENTS.md](AGENTS.md)

## 프로젝트 개요

알파리뷰 x 아임웹 프로모션 이메일 시퀀스(3통 x 3종)의 HTML 미리보기와 운영 가이드를 Netlify로 배포하는 정적 사이트.
Tally_webhook에서 GmailApp으로 발송하는 이메일의 실제 렌더링 결과를 브라우저에서 확인할 수 있다.

## 기술 스택

- 정적 HTML (빌드 도구 없음)
- 이메일 호환 HTML (테이블 레이아웃, 인라인 스타일, 600px 폭)
- Netlify (정적 배포)

## 배포 URL

- https://stirring-sprinkles-29cf5a.netlify.app/

## 주요 명령어

```bash
# 로컬 미리보기 (브라우저에서 직접 열기)
open test_preview_early.html
open test_preview_general.html
open test_preview_vip.html
open operations_guide.html

# 배포 (git push로 Netlify 자동 배포)
git add . && git commit -m "[docs] 이메일 미리보기 업데이트" && git push
```

## 폴더 구조

```
tally-email-preview/
├── CLAUDE.md                    # 이 파일
├── AGENTS.md                    # 문서 목차 + 핵심 원칙
├── ARCHITECTURE.md              # 시스템 구조
├── .gitignore
├── operations_guide.html        # 운영 가이드 (비개발자 대상)
├── project_summary.html         # 프로젝트 요약 (내부 공유)
├── test_preview_early.html      # 얼리아답터 이메일 미리보기 (메일 1/2/3)
├── test_preview_general.html    # 일반 이메일 미리보기 (메일 1/2/3)
├── test_preview_vip.html        # VIP 이메일 미리보기 (메일 1/2/3)
└── docs/
    ├── PRODUCT_SENSE.md         # 제품 방향
    ├── PLANS.md                 # 우선순위, 로드맵
    ├── design-docs/
    └── exec-plans/
```

## 이메일 디자인 사양

- 배너 이미지: `https://img2.stibee.com/69416_3277804_1773216108046346841.png`
- 데모 GIF (메일 2): `https://img2.stibee.com/69416_3277813_1773216294614660967.gif`
- CTA 버튼 색상: `#837AFF`
- 본문 폰트: -apple-system, BlinkMacSystemFont, 'Apple SD Gothic Neo'
- 발신자: 알파리뷰 (public@saladlab.co)

## 알파리뷰 프로모션 연관 프로젝트

| 프로젝트 | 역할 |
|----------|------|
| Tally | 폼 생성/수정 |
| Tally_webhook | 제출 감지 -> 알림/이메일 (HTML 원본) |
| **tally-email-preview** (현재) | 이메일 미리보기 + 운영 가이드 |

## 최근 변경사항

### 2026-03-17: 프로젝트 분리
- Tally_webhook에서 HTML 미리보기/운영 가이드 파일을 별도 레포로 분리
- 얼리아답터/일반/VIP 이메일 미리보기 HTML 3벌
- 운영 가이드 HTML + 프로젝트 요약 HTML
- Netlify 배포 설정
