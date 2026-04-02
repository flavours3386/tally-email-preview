# Tally 이메일 미리보기 - Architecture

## 시스템 개요

Tally_webhook에서 GmailApp으로 발송하는 이메일 시퀀스의 HTML 미리보기와 운영 가이드를 정적 HTML로 제공하는 Netlify 배포 사이트.

## 데이터 흐름

```
+----------------------------------+
| Tally_webhook                    |
| (tally_notification.gs)          |
|                                  |
| buildEmailHtml()                 |
| getMail1Content()                |
| getMail2Content()                |
| getMail3Content()                |
+----------------------------------+
         |
         | HTML 구조/템플릿 동기화 (수동)
         v
+----------------------------------+
| tally-email-preview              |
| (이 프로젝트)                     |
|                                  |
| test_preview_early.html          |
| test_preview_general.html        |
| test_preview_vip.html            |
| operations_guide.html            |
| project_summary.html             |
+----------------------------------+
         |
         | git push -> Netlify 자동 배포
         v
+----------------------------------+
| Netlify                          |
| stirring-sprinkles-29cf5a        |
| .netlify.app                     |
+----------------------------------+
         |
         | URL 공유
         v
+----------------------------------+
| 마케팅팀 / CSM팀 / 경영진        |
| (브라우저에서 미리보기)            |
+----------------------------------+
```

## 모듈 경계

| 파일 | 역할 | 내용 |
|------|------|------|
| `test_preview_early.html` | 얼리아답터 이메일 미리보기 | 메일 1(접수 확인 + 설치 안내) + 메일 2(데모 사이트 + GIF) + 메일 3(도움 제안) |
| `test_preview_general.html` | 일반 이메일 미리보기 | 얼리아답터와 동일 구조, 프로모션 텍스트만 다름 ("런칭 프로모션") |
| `test_preview_vip.html` | VIP 이메일 미리보기 | 전담 매니저 톤, 설치 가이드 대신 직접 연락 안내 |
| `operations_guide.html` | 운영 가이드 | 시스템 배경/변경점/이메일 시퀀스/협업팀 소개 (비개발자 대상) |
| `project_summary.html` | 프로젝트 요약 | 기술적 구현 포인트 + 시스템 아키텍처 요약 (내부 공유) |

## 이메일 HTML 구조

모든 미리보기 HTML은 동일한 테이블 레이아웃을 따른다:

```
<table width="600">                    // 600px 고정 폭
  <tr> 안내 문구 (12px, #999)  </tr>   // "본 메일은 ... 발송됩니다"
  <tr> 배너 이미지             </tr>   // stibee 이미지 URL
  <tr> 본문 (15px, #333)      </tr>   // 프로모션 유형별 텍스트
  <tr> CTA 버튼 (#837AFF)     </tr>   // 설치하기/데모보기/가이드보기
  <tr> 푸터 (회사정보+수신거부) </tr>   // Saladlab + unsubscribe 링크
</table>
```

## 동기화 규칙

| Tally_webhook 변경 | 이 프로젝트에서 할 일 |
|-------------------|---------------------|
| `buildEmailHtml()` 구조 변경 | 3개 preview HTML의 테이블 구조 업데이트 |
| `getMail*Content()` 텍스트 변경 | 해당 프로모션 유형의 preview HTML 텍스트 업데이트 |
| BANNER_IMAGE_URL 변경 | preview HTML의 `<img src>` 업데이트 |
| DEMO_GIF_URL 변경 | 메일 2의 GIF `<img src>` 업데이트 |
| UNSUBSCRIBE_URL 변경 | 푸터의 수신거부 `<a href>` 업데이트 |
| CTA 색상/텍스트 변경 | CTA `<td>` 스타일 + `<a>` 텍스트 업데이트 |

## 에러 처리 전략

정적 HTML 사이트이므로 런타임 에러 처리가 없다. 주의사항:

| 상황 | 대응 |
|------|------|
| 외부 이미지 URL 만료 | 배너/GIF가 깨져 보임. Stibee 이미지 URL 갱신 필요 |
| Netlify 배포 실패 | git push 후 Netlify 대시보드에서 빌드 로그 확인 |
| 미리보기와 실제 발송 불일치 | Tally_webhook 코드와 수동 대조 필요 (자동 동기화 없음) |

## 제약사항

- 이메일 HTML은 인라인 스타일만 사용 (외부 CSS 불가)
- 테이블 기반 레이아웃 (div/flexbox 이메일 클라이언트 호환성 문제)
- 이미지는 외부 URL 참조 (Stibee CDN). 로컬 이미지 불가.
- 미리보기와 실제 발송 HTML의 동기화는 수동 (자동화되어 있지 않음)
