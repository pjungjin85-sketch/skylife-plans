# skylife-plans 프로젝트 규칙

공통 규칙은 상위 디렉토리 `/Users/jaypark/workspace/CLAUDE.md` 참고.

## 이 페이지 정보
- **용도**: 스카이라이프 모바일 전체 요금제 목록 (대리점 판매용)
- **파일**: `index.html` (단일 파일)
- **GitHub Pages URL**: https://pjungjin85-sketch.github.io/skylife-plans/

## 기능 구조
- 요금제 카드 그리드 + 검색 + 카테고리 필터
- 카드 클릭 시 상세 정보 (모달 또는 확장)
- max-width: 1400px (다른 페이지는 1200px — 이 페이지만 더 넓음)

## 수정 시 주의사항
- 요금제 데이터 변경 시 가격/혜택 정확성 확인 필수
- max-width가 1400px임을 주의 (공통 헤더 CSS의 1200px와 다름)
- 필터 버튼과 데이터의 카테고리 키값이 일치해야 함
