# 일본 GTM WBS 대시보드

구글 스프레드시트로 관리하는 WBS 데이터를 실시간 HTML 대시보드로 시각화합니다.

## 구조

```
wbs-dashboard/
├── index.html       # 대시보드 (단일 파일, 의존성 없음)
├── sample-data.csv  # 샘플 데이터 (테스트용)
└── README.md
```

## 1. 구글 스프레드시트 설정

### 스프레드시트 생성
아래 칼럼 구조로 새 구글 스프레드시트를 만드세요:

| 칼럼명 | 설명 | 예시값 |
|--------|------|--------|
| Phase | 단계 | Phase 0, Phase 1, Phase 2 |
| WBS# | 번호 | 1.1, 2.1.1 |
| 구분 | 카테고리 | 사업/영업, B2B 프로덕트 |
| Task | 태스크명 | 매장 확보 |
| 세부Task | 세부 내용 | PoC 매장 5곳 확보 |
| 담당팀 | 팀명 | 개발팀 |
| 담당자 | 이름 | 홍길동 |
| 우선순위 | P0/P1/P2 | P0 |
| 진행상태 | 상태값 | 진행 중, 시작 전, 완료, 대기 중, 검토 중, 블로킹, 보류 |
| 리스크수준 | 높음/중간/낮음 | 높음 |
| 예상공수(일) | 숫자 | 20 |
| 시작일 | YYYY-MM-DD | 2026-03-10 |
| 종료일 | YYYY-MM-DD | 2026-04-10 |
| 진행률(%) | 0~100 | 45 |
| 블로커/이슈 | 텍스트 | LINE 파트너 승인 필요 |
| 참고링크 | URL | https://... |

> `sample-data.csv`를 구글 스프레드시트에 임포트하면 바로 시작할 수 있습니다.

### 웹에 게시 (CSV URL 생성)
1. 스프레드시트 열기
2. **파일 > 공유 > 웹에 게시**
3. 형식: **CSV** 선택
4. **게시** 클릭
5. 생성된 URL 복사 (형태: `https://docs.google.com/spreadsheets/d/.../pub?output=csv`)

## 2. 대시보드 연동

### 방법 A: 브라우저에서 직접 열기
1. `index.html`을 브라우저로 열기
2. **Settings** 버튼 클릭
3. "구글 스프레드시트 CSV URL"에 위에서 복사한 URL 붙여넣기
4. **저장 및 새로고침** 클릭

### 방법 B: URL 파라미터로 전달
```
index.html?sheet=YOUR_PUBLISHED_CSV_URL
```

## 3. 배포 (Confluence 임베드를 위해)

### GitHub Pages 배포
1. GitHub에 새 Repository 생성 (예: `japan-gtm-dashboard`)
2. `wbs-dashboard/` 폴더 내용을 push
3. **Settings > Pages > Source**: `main` 브랜치, `/ (root)` 선택
4. 배포 완료 후 URL 확인: `https://[username].github.io/japan-gtm-dashboard/`

### Netlify 배포 (대안)
1. [netlify.com](https://netlify.com)에서 "Deploy manually" 선택
2. `wbs-dashboard/` 폴더를 드래그앤드롭
3. 즉시 URL 생성됨

## 4. Confluence 임베드

### 방법 1: iframe 매크로 (권장)
1. Confluence 페이지 편집
2. `/iframe` 또는 "웹 콘텐츠" 매크로 삽입
3. URL에 GitHub Pages 주소 입력
4. 너비: 100%, 높이: 800px 권장

### 방법 2: HTML 매크로
1. Confluence 관리자가 HTML 매크로 허용 필요
2. 페이지 편집 > HTML 매크로 삽입
3. `index.html`의 전체 내용 붙여넣기

### 방법 3: Confluence 페이지에 링크만 추가
- GitHub Pages URL을 링크로 삽입
- 클릭 시 새 탭에서 대시보드 열림

## 5. 데이터 새로고침

- 구글 스프레드시트 수정 → 대시보드에서 **Refresh** 버튼 클릭
- 웹에 게시된 CSV는 보통 5분 이내 자동 반영
- 브라우저 캐시로 인해 즉시 반영 안 될 시 Ctrl+Shift+R

## 대시보드 기능

- **KPI 카드**: 전체 진행률, 진행 중/블로커/리스크 요약
- **Phase별 진행 현황**: Phase 0/1/2 각각의 진행률 바
- **간트 차트**: 카테고리별 타임라인 시각화 + 오늘 마커
- **카테고리별 현황**: 사업/개발/인프라 등 영역별 요약
- **리스크 보드**: 높음/중간/낮음 리스크 태스크 분류
- **태스크 테이블**: 전체 목록 + 상태별 필터링
