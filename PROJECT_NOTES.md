# 서울 미슐랭 웹사이트 프로젝트 노트

> 마지막 업데이트: 2026-03-13

---

## 📁 파일 위치

| 파일 | 경로 |
|------|------|
| 소스 파일 (작업용) | `/Users/kimtaekyun/Downloads/claude folder/seoul-michelin.html` |
| 배포 파일 (GitHub Pages용) | `/Users/kimtaekyun/Downloads/claude folder/index.html` |
| 이 노트 파일 | `/Users/kimtaekyun/Downloads/claude folder/PROJECT_NOTES.md` |

---

## 🔗 GitHub / 배포 정보

| 항목 | 값 |
|------|-----|
| GitHub 저장소 | https://github.com/naktaku-designer/seoul-michelin |
| GitHub Pages URL | https://naktaku-designer.github.io/seoul-michelin/ |
| 브랜치 | `main` |
| GitHub 계정 | `naktaku-designer` |

### 배포 명령어 (변경사항 반영 시)
```bash
cd "/Users/kimtaekyun/Downloads/claude folder"
cp seoul-michelin.html index.html
git add index.html
git commit -m "커밋 메시지"
git push
```

---

## 🌐 프로젝트 개요

서울 미슐랭 레스토랑 22곳을 정리한 단일 HTML 파일 정적 웹사이트.
Claude.ai 디자인 시스템을 참고한 다크 테마로 제작.

---

## 🎨 디자인 시스템

| 변수 | 값 | 용도 |
|------|----|------|
| `--bg` | `#131314` | 메인 배경 |
| `--bg-card` | `#1c1c1e` | 카드 배경 |
| `--accent` | `#d97757` | 강조색 (오렌지) |
| `--text` | `#f5f5f0` | 본문 텍스트 |
| `--text-muted` | `#8e8e93` | 보조 텍스트 |
| `--border` | `rgba(255,255,255,.08)` | 테두리 |

- **폰트**: Noto Sans KR (본문) + Noto Serif KR (제목)
- **효과**: `backdrop-filter: blur()` 글래스모피즘, `radial-gradient` 히어로 배경, 노이즈 텍스처 오버레이

---

## 🏗️ HTML 구조 (단일 파일)

```
seoul-michelin.html
├── <style>          ← 모든 CSS 인라인
├── <body>
│   ├── .hero        ← 타이틀 + hero-stats (3/2/1스타 개수)
│   ├── .controls    ← 검색창 + 필터 버튼
│   ├── #grid        ← 레스토랑 카드 그리드
│   └── .modal-backdrop ← 예약 모달 (3단계)
└── <script>         ← 모든 JS 인라인
```

---

## 🍽️ 레스토랑 데이터 구조

```javascript
{
  stars: 3,                    // 미슐랭 별 수 (1~3)
  name: "Gaon",                // 영문명
  nameKr: "가온",              // 한글명
  cuisine: "한식 파인다이닝",   // 요리 장르
  type: "korean",              // 내부 타입 (SVG 폴백용)
  area: "강남구 논현동",        // 위치
  price: 4,                    // 가격대 (1~4)
  phone: "02-545-3030",        // 전화번호
  desc: "설명 텍스트",
  reservation: "전화 예약",     // 예약 방법
  hours: "12:00–22:00",        // 영업시간
  timeSlots: ["12:00","13:00",...], // 예약 가능 시간 슬롯
  unavailable: ["13:00"],      // 예약 불가 슬롯
  photo: "https://images.unsplash.com/..." // 썸네일 이미지 URL
}
```

**총 22개 레스토랑**: 3스타 3개 / 2스타 8개 / 1스타 11개 (실제 데이터 기준)

---

## ✅ 지금까지 완료한 작업

1. **기본 웹사이트 생성** — 22개 미슐랭 레스토랑 목록 정적 HTML
2. **Claude.ai 스타일 리디자인** — 다크 테마, 오렌지 액센트, 글래스모피즘
3. **레스토랑 사진 썸네일** — Unsplash URL + SVG 폴백 일러스트 (한식/일식/프렌치/퓨전)
4. **예약 모달 (3단계)**
   - Step 1: 날짜·시간·인원 선택
   - Step 2: 이름·연락처·이메일 입력
   - Step 3: 예약 내용 확인 및 제출
5. **ngrok 임시 배포** — `brew install ngrok` 후 로컬 서버 터널링
6. **GitHub Pages 영구 배포** — `naktaku-designer/seoul-michelin` 저장소 생성 및 연결
7. **전화하기 버튼** — 각 카드에 `tel:` 링크 버튼 추가 (₩₩₩₩ 대체)
8. **지도 보기 버튼** — Google Maps 링크 버튼 추가 (전화하기 버튼 아래, 세로 배치)
9. **카드 하단 레이아웃 정리** — 시간·예약정보(좌) + 버튼 2개(우) 세로 중앙 정렬
10. **Hero 스탯 스타일 수정** — `border-radius: 16px` 둥근 사각형 (pill → rounded rect)

---

## 🔧 주요 코드 패턴

### 카드 하단 버튼 영역
```html
<div class="card-meta">
  <div class="meta-info">
    <span class="meta-hours">🕐 ${r.hours}</span>
    <span class="meta-reservation">📋 ${r.reservation}</span>
  </div>
  <div class="card-actions">
    <a class="call-btn" href="tel:${r.phone}" onclick="event.stopPropagation()">
      📞 전화하기
    </a>
    <a class="map-btn" href="https://maps.google.com/?q=..." onclick="event.stopPropagation()" target="_blank">
      📍 지도 보기
    </a>
  </div>
</div>
```

### 모달 방지 (버튼 클릭 시)
```js
onclick="event.stopPropagation()"
```

### GitHub Pages 활성화 (최초 1회)
```bash
echo '{"source":{"branch":"main","path":"/"}}' | gh api repos/naktaku-designer/seoul-michelin/pages --method POST --input -
```

---

## 🛠️ 개발 환경

- **OS**: macOS (Darwin 24.5.0)
- **로컬 서버**: `python3 -m http.server 8080`
- **터널링**: ngrok (`brew install ngrok/ngrok/ngrok`)
- **GitHub CLI**: `gh` (`brew install gh`)
- **Git 워킹 디렉토리**: `/Users/kimtaekyun/Downloads/claude folder/`

---

## 💡 다음에 이어서 할 수 있는 작업 아이디어

- [ ] 각 레스토랑에 실제 주소 데이터 추가 (현재는 지역명만 있음)
- [ ] 카카오맵 API 연동 (Google Maps 대신 국내 지도)
- [ ] 레스토랑 상세 페이지 또는 확장 모달
- [ ] 예약 폼 실제 백엔드 연동
- [ ] 즐겨찾기 / 북마크 기능 (localStorage)
- [ ] 다크/라이트 모드 토글
- [ ] 모바일 반응형 최적화
