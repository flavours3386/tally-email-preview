# tally-email-preview

## 프로젝트 개요
알파리뷰 x 아임웹 프로모션 신청자(VIP, 얼리아답터, 일반)에게 발송되는 이메일 시퀀스 및 운영 자동화 가이드를 브라우저에서 미리 볼 수 있도록 정리한 정적 HTML 문서 모음.

## 기술 스택
- 정적 HTML (인라인 CSS, 테이블 레이아웃 기반 이메일 템플릿)
- 외부 폰트: Google Fonts (Noto Sans KR)
- 이메일 발송 서비스: Stibee (이미지 호스팅 및 수신거부 링크 포함)

## 주요 명령어
해당 없음 (정적 HTML 파일로, 브라우저에서 직접 열어서 확인)

## 폴더 구조
```
tally-email-preview/
├── operations_guide.html       # 아임웹 프로모션 신청 자동화 운영 가이드 문서
├── project_summary.html        # 프로젝트 전체 요약 문서
├── test_preview_early.html     # 얼리아답터 이메일 시퀀스 미리보기 (전체 시퀀스)
├── test_preview_general.html   # 일반 신청자 이메일 시퀀스 미리보기
└── test_preview_vip.html       # VIP 이메일 시퀀스 미리보기
```
