# skylife-plans 대화 로그

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
