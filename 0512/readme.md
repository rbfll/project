

## 프로젝트 개요
MyDay는 사용자가 일상을 기록할 수 있는 간편한 웹 기반 다이어리 애플리케이션입니다. 날씨, 기분, 텍스트 메모와 함께 이미지와 음성도 함께 저장할 수 있습니다.

## 주요 변경사항

### 1. 랜딩 페이지 추가

**변경 내용:**
- 애플리케이션 첫 접속 시 "☆ MyDay ☆" 로고만 표시되는 미니멀한 랜딩 페이지 추가
- 로고 클릭 시 일기 작성 페이지로 전환
- 기존 다이어리 페이지에서 로고 클릭 시 랜딩 페이지로 돌아가는 기능 구현

**기술적 구현:**
```javascript
// 페이지 전환 함수
function showLandingPage() {
  document.getElementById('landing-page').style.display = 'flex';
  document.getElementById('diary-page').style.display = 'none';
}

function showDiaryPage() {
  document.getElementById('landing-page').style.display = 'none';
  document.getElementById('diary-page').style.display = 'block';
  resetForm(); // 다이어리 페이지 로드 시 폼 초기화
}
```

### 2. 단일 파일 구조로 통합

**변경 내용:**
- 기존 HTML, CSS 파일을 하나의 `MyDiary.html` 파일로 통합
- 내부 스타일시트 사용으로 외부 CSS 파일 의존성 제거
- JavaScript 코드도 동일 파일 내에 통합하여 배포 및 관리 용이성 향상

**구조 변경:**
```html
<!-- 랜딩 페이지 -->
<div id="landing-page" class="landing-container">
  <a href="#" class="logo" onclick="showDiaryPage()">☆ MyDay ☆</a>
</div>

<!-- 일기 페이지 -->
<div id="diary-page" class="diary-container">
  <!-- 일기 작성 인터페이스 -->
</div>
```

### 3. 사용자 인터페이스 개선

**변경 내용:**
- 랜딩 페이지 로고에 호버 효과 추가 (확대 및 색상 변경)
- 로고 사이즈 강조 (4em)로 시각적 중요성 부여
- 전체 화면 중앙 정렬을 통한 사용자 경험 개선

**CSS 구현:**
```css
.landing-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100%;
}

.logo {
  font-size: 4em;
  font-weight: bold;
  cursor: pointer;
  text-decoration: none;
  color: black;
  transition: transform 0.3s ease, color 0.3s ease;
}

.logo:hover {
  transform: scale(1.1);
  color: #888;
}
```

### 4. 페이지 전환 시 상태 관리

**변경 내용:**
- 다이어리 페이지로 전환 시 자동 폼 초기화 기능 추가
- 사용자가 작성 중이던 내용을 유지하면서 페이지 전환 가능
- localStorage를 활용한 데이터 지속성 유지

## 개발 결과

이번 변경을 통해 MyDay 애플리케이션은 다음과 같은 개선 효과를 얻었습니다:

1. **사용자 경험 향상**: 초기 접속 시 심플한 인터페이스로 시작하여 사용자에게 편안한 느낌 제공
2. **코드 유지보수성 향상**: 단일 파일 구조로 변경하여 관리 및 배포 용이성 증가
3. **브랜딩 강화**: 로고를 강조하여 애플리케이션의 아이덴티티 강화


---

