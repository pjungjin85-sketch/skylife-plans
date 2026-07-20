# skylife-plans 대화 로그

---

## 2026-06-01 — 6월 요금제 데이터 추가

### 작업 내용

**소스 파일**: `(공지용) skylife 모바일 상품_2026년6월1일자.v2 (1).xlsx`

**1. plans.json에 `2026-06` 키 추가**
- 엑셀 `요금제` 시트에서 판매종료일 = `-` 인 활성 요금제만 추출
- 4월(118개) → 6월(124개): +6개 순증

**2. 신규 요금제 7개**
| 코드 | 이름 | 가격 | 출시 |
|------|------|------|------|
| PL2639485 | 모두 충분 7GB+(CU) | 16,700원 | 6/1 신규 |
| PL2639484 | 5G 슬림 20GB/200분 | 18,900원 | 6/1 신규 |
| PL261T206 | 5G 슬림 10GB/200분 | 10,900원 | 5/1 신규 |
| PL261T208 | 5G 통화 충분 3GB | 8,900원 | 5/1 신규 |
| PL261T209 | 5G 통화 충분 6GB | 11,900원 | 5/1 신규 |
| PL261T211 | 5G 통화 충분 10GB | 14,900원 | 5/1 신규 |
| PL208R995 | skylife LTE 데이터쉐어링 | 0원 | 신규 |

**3. 제거 요금제 1개**
- `PL2192920` | 초슬림 500M/60분 (판매종료)

### 커밋
- `cec6e54` — feat: 6월 요금제 데이터 추가 (2026-06)

---

## 2026-05-31 — 요금제 월별 자동 전환 기능 추가

### 작업 내용

**1. plans.json 구조 변경**
- 기존: 요금제 배열 (`[{...}, {...}]`)
- 변경: 월별 키 구조 (`{"2026-04": [{...}], "2026-06": [{...}]}`)
- 현재 4월 데이터 → `"2026-04"` 키로 이관

**2. index.html — 날짜 기반 자동 분기 로직 추가**
- `DOMContentLoaded`에서 `new Date()`로 현재 월 판단
- JSON 키 중 현재 월 이하 최신 키 자동 선택 (미래 키는 무시)
- 전역 변수: `ACTIVE_MONTH_KEY`, `BASE_MONTH_LABEL` 추가

**3. 하드코딩 기준월 제거**
- 배지 `"2026년 4월 기준"` → `<span id="baseMonthLabel">` (JS 자동 생성)
- 상세 모달 주의사항 → `BASE_MONTH_LABEL` 변수 사용
- 공지 팝업 텍스트 → `id="noticeContent"` (JS 자동 생성)

**4. 공지 팝업 로직 변경**
- 기존: 매일 1회 표시 (`notice_closed_date`)
- 변경: 월 전환 시 1회만 표시 (`notice_closed_YYYY-MM`)

### 동작 방식
| 접속 날짜 | JSON 키 | 표시 데이터 |
|-----------|---------|------------|
| 5월 30일 | `2026-04`, `2026-06` | 4월 데이터 (6월은 미래라 무시) |
| 6월 1일 | `2026-04`, `2026-06` | 6월 데이터 자동 전환 |

### 운영 규칙
- 새 달 데이터 추가: `plans.json`에 `"YYYY-MM": [...]` 키 추가 후 언제든 push
- 이전 달 삭제: 2달 지난 키는 삭제해도 무방 (직전 1개월 유지 권장)

### 커밋
- `fef4d7a` — feat: 요금제 월별 자동 전환 기능 추가

---

## 2026-04-02 — Vercel 마이그레이션 + 캐시 문제 해결 (전 프로젝트)

### 작업 내용

**1. 캐시 방지 메타태그 추가 (5개 프로젝트 전체)**
- `<meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">` 추가
- 대상: skylife-plans, skylife-addons, skylife-commission-calculator, skylife-mobile-faq, skylife-guide

**2. Vercel 마이그레이션 (GitHub Pages → Vercel)**
- 배경: GitHub Pages 캐시 강함 + 배포 지연 문제로 Vercel로 전환
- 각 프로젝트에 `vercel.json` 추가 (HTTP 헤더 레벨 캐시 완전 차단)
- skylife-guide는 `index.html` 없어서 루트 → `mobile_sales_guide.html` 리다이렉트 설정

**3. 새 Vercel URL**
| 프로젝트 | 구 URL (GitHub Pages) | 새 URL (Vercel) |
|---|---|---|
| skylife-plans | pjungjin85-sketch.github.io/skylife-plans | skylife-plans-bec2.vercel.app |
| skylife-addons | pjungjin85-sketch.github.io/skylife-addons | skylife-addons-ejol.vercel.app |
| skylife (수수료계산기) | pjungjin85-sketch.github.io/skylife | skylife-1s8i.vercel.app |
| skylife-mobile-faq | pjungjin85-sketch.github.io/skylife-mobile-faq | skylife-mobile-faq-um3h.vercel.app |
| skylife-guide | pjungjin85-sketch.github.io/skylife-guide/mobile_sales_guide.html | skylife-guide-vz8q.vercel.app |

**4. skylife-guide 아웃링크 URL 업데이트**
- 페이지 내 4개 링크를 GitHub Pages → Vercel URL로 교체

### 커밋
- 각 프로젝트: `fix: 캐시 방지 메타태그 추가`
- 각 프로젝트: `feat: vercel.json 추가 — 캐시 완전 차단`
- skylife-guide: `fix: vercel.json 루트 → mobile_sales_guide.html 리다이렉트 추가`
- skylife-guide: `fix: 아웃링크 GitHub Pages → Vercel URL로 변경`

---

## 2026-04-02 — 4월 요금제 데이터 업데이트 + 결합혜택 정보 추가

### 작업 내용

**1. 요금제 데이터 전면 업데이트 (4월 기준)**
- 엑셀 파일(`(공지용) skylife 모바일 상품_2026년4월1일자_...VIT여부.v3`) 기반
- 가격 수정: 슬림 3GB/300분 5,900원, 골드 2GB+ 5,900원 등 다수 반영

**2. 결합혜택 필드 전 요금제 추가**
- 기존 구조에 4개 필드 추가:
  - `nungu`: 누구나결합 추가 데이터 (AB열)
  - `uit`: SKY UIT결합 추가 데이터 (AC열)
  - `promo`: 프로모션 데이터 추가 (AD열)
  - `vit`: KT유선 알뜰폰 결합 VIT 가능 여부 (AF열, boolean)
- 값이 없는 요금제는 `null` / `false` 처리

**3. 요금제 상세 모달에 '결합 혜택' 섹션 추가**
- `openPlanModal()` 함수에 `combineSection` 블록 추가
- 누구나결합 / SKY UIT결합 / 프로모션 / KT유선 VIT 4개 항목 표시
- VIT 가능(✔ 초록) / 불가(✘ 빨강) 색상 구분
- CSS: `.pm-combine-grid`, `.pm-combine-row`, `.pm-vit-ok`, `.pm-vit-no` 추가

**4. 기준월 변경**
- 상단 표시 및 주의사항 문구: `2026년 3월 기준` → `2026년 4월 기준`

### 주요 데이터 패턴
| 구간 | 누구나결합 | UIT결합 | 프로모션 |
|---|---|---|---|
| 7GB+, 10GB+, 15GB+ | +5~10GB (LTE) | +5~10GB (LTE) | +10GB (24개월) |
| 11GB+, 100GB+ | +20GB (LTE) | +20GB (LTE) | 없음 |
| 5G 50~90GB+ | +10GB (5G) | +10GB (5G) | 없음 |
| 5G 110~200GB+ | +20GB (5G) | +20GB (5G) | 없음 |
| 선택형/음성무제한/시니어 등 | 없음 | 없음 | 없음 |

### 커밋
- `44fd130` — feat: 4월 요금제 업데이트 — 결합혜택(누구나/UIT/프로모션/VIT) 정보 추가
- `2151a09` — fix: 기준월 3월 → 4월로 변경

---

## 2026-03-27

### 작업 내용: 모바일 정렬 버튼 반응형 수정

**문제:**
모바일 화면에서 정렬/필터 영역의 `controls-row2` (정렬 버튼)가 화면 밖으로 넘쳐 가로 스크롤로만 접근 가능했음.

**원인:**
`@media(max-width:767px)` 에서 `.controls-row2`에 `flex-wrap:nowrap; overflow-x:auto` 가 적용되어 있어 버튼들이 줄바꿈되지 않고 가로 스크롤 방식으로 동작.

**수정 내용:**
```css
/* 변경 전 */
.controls-row2{flex-wrap:nowrap;overflow-x:auto;-webkit-overflow-scrolling:touch;padding-bottom:6px;}
.controls-row2::-webkit-scrollbar{display:none;}
.sort-btn{flex-shrink:0;white-space:nowrap;}

/* 변경 후 */
.controls-row2{flex-wrap:wrap;overflow-x:visible;padding-bottom:0;}
.sort-btn{flex-shrink:1;white-space:nowrap;}
```

**결과:**
모바일에서 정렬 버튼들이 화면 너비에 맞게 자동 줄바꿈되어 스크롤 없이 모두 표시됨.

**커밋:** `24580e9` — fix: 모바일 정렬 버튼 가로 스크롤 → 줄바꿈 방식으로 변경
**푸시:** main 브랜치 → GitHub Pages 자동 반영

---

## 2026-04-05 — 페이지 배경색 흰색으로 변경

- `body { background: var(--bg2) }` (#F7F7F7 회색) → `var(--bg)` (#FFFFFF 흰색)
- 커밋: `fd3585d`

---

## 2026-03-27 — 푸터 디자인 통일

- 기존: `<footer>` inline style, 왼쪽 정렬 (`Created by 박정진`)
- 변경: 중앙정렬 + border-top + 페이지명 포함
  - `Created by 박정진 | 스카이라이프 모바일 요금제 안내`
- skylife-addons 푸터 스타일 기준으로 전 프로젝트 통일 작업의 일환
- 커밋: `c7831de` — style: 푸터 디자인 통일

---

## 2026-06-11

### 작업 내용
- `<meta name="robots" content="noindex, nofollow">` 추가 (검색엔진 차단)
- fetch 에러 처리: async/await 전환 + `!res.ok` 체크 + 다시 시도 버튼 + `#baseMonthLabel` "로딩 실패" 표시
- localStorage 캐시 추가: `skylife_plans_v1` 키, 24h TTL
- 브랜드 표기 통일: title·히어로·note 등 → `kt skylife`
- footer 표기 통일: `Created by 박정진 | 모바일 요금제 안내`
- footer 스타일 통일 (FAQ 기준): `position:fixed;bottom:0;padding:10px;color:#666666;z-index:50`
- `main` padding-bottom 60px → 70px (fixed footer 콘텐츠 겹침 방지)

### 버그 수정 (코드 리뷰)
- 모달 내부 클릭 시 닫힘 버그: `planModalBox` onclick → `event.stopPropagation()` 으로 교체
- 가격 슬라이더 최대값 하드코딩 제거: `PRICE_CEILING` 변수 도입, 데이터 로드 후 동적 계산
- `noticeContent` null 방어: `getElementById` 결과 체크 후 접근
- `openPlanModal()` 작은따옴표 취약성: `onclick` → `data-code` 속성 + 이벤트 위임 방식 전환
- `err.message` XSS: innerHTML 삽입 전 HTML 이스케이프 처리

---

## 2026-07-16 업데이트 — Supabase 프로젝트 마이그레이션
- 로그인 게이트(`profiles.status` 조회)가 참조하던 기존 Supabase 프로젝트가 90일 초과 일시정지로 복구 불가 확인 → 신규 프로젝트 `skylife-shared`(ref `qvzlwhwxspmofrwdvgdd`)로 URL/KEY 교체
- 상세 배경은 skylife-inquiry의 `skylife-inquiry_CONVERSATION_LOG.md` 참고 (워크스페이스 6개 사이트 공용 이슈였음)
- 이 커밋에는 이미 작업 중이던 로그인월(lock-overlay) 기능도 미커밋 상태로 함께 포함되어 같이 push/배포됨
