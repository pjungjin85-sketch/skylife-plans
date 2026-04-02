# skylife-plans 대화 로그

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

## 2026-03-27 — 푸터 디자인 통일

- 기존: `<footer>` inline style, 왼쪽 정렬 (`Created by 박정진`)
- 변경: 중앙정렬 + border-top + 페이지명 포함
  - `Created by 박정진 | 스카이라이프 모바일 요금제 안내`
- skylife-addons 푸터 스타일 기준으로 전 프로젝트 통일 작업의 일환
- 커밋: `c7831de` — style: 푸터 디자인 통일
