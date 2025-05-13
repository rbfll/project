# 📘 MyDay 웹앱 주요 기능 정리

---

## 1. 🖼️ 랜딩 페이지 (Landing Page)

- **인트로 화면**으로 `☆ MyDay ☆` 로고만 화면 중앙에 크게 표시됨.
- 사용자가 로고를 클릭하면 본격적인 **다이어리 화면(diary-page)** 으로 전환됨.
- **구현 방식**:  
  `display: flex / none` 을 이용해 `#landing-page`와 `#diary-page`를 토글함.

---

## 2. 📅 날짜 선택기 (Date Picker)

- `<input type="date">` 요소를 사용해 날짜를 선택할 수 있음.
- **날짜별로 작성한 일기 데이터를 저장하거나 불러오는 기능**이 구현되어 있음.
- 사용자가 날짜를 변경하면 해당 날짜의 저장 데이터를 **자동으로 로딩**.

---

## 3. 🌤️ 날씨 선택 드롭다운

- 날씨를 선택할 수 있는 `<select id="weather">` 드롭다운이 제공.
- 각 옵션에는 **이모지와 텍스트**가 함께 표시되어 직관적임.
- 선택 가능한 옵션:
  - ☀️ 맑음  
  - ⛅ 흐림  
  - 🌧️ 비  
  - ❄️ 눈

---

## 4. 😊 기분 선택 드롭다운

- 하루의 감정을 기록할 수 있는 `<select id="mood">` 드롭다운이 제공.
- 각 감정은 **이모지와 함께 표시**되어 사용자 경험이 향상.
- 선택 가능한 감정 옵션:
  - 😊 행복  
  - 😢 슬픔  
  - 😡 화남  
  - 🤩 신남  
  - 😐 보통

---

## 5. 📝 일기 작성 텍스트박스
- `<textarea>`를 통해 자유롭게 텍스트 입력 가능.
- 높이는 150px이며 `resize: vertical` 속성으로 사용자가 조절할 수 있음.

---

## 6. 🖼️ 이미지 업로드 + 미리보기 기능
- `+` 버튼 클릭 → 숨겨진 `<input type="file" multiple>`이 열림.
- 업로드한 이미지는 썸네일 형태로 보여지고, 각각 X 버튼으로 제거 가능.
- 각 이미지 업로드 시 FileReader API로 미리보기 구현.

---

## 7. 🎤 오디오 녹음 + 미리듣기 기능
- 🎙️ 버튼 클릭 시 마이크 권한 요청 → 녹음 시작.
- 다시 클릭 시 녹음 종료 → `<audio controls>`로 미리듣기 가능.
- 각 오디오 요소 옆에도 삭제 X 버튼 제공.
- 구현 방식: MediaRecorder API 활용.

 ---

## 8. 💾 저장 기능
- 작성한 일기를 로컬스토리지에 저장.
- 저장 키는 날짜 (`YYYY-MM-DD`) 형식.

---

## 9. 🗑️ 삭제 기능
- 선택된 날짜에 저장된 일기가 있으면 삭제 가능.
- `confirm()`을 통해 사용자의 삭제 의사 재확인.
- 삭제 후 `resetForm()` 실행하여 입력창 초기화.

---

## 10. 🔄 자동 폼 초기화
- 다이어리 페이지 진입 시 `resetForm()` 함수 실행.
- 날짜는 오늘 날짜로 초기화되며, 모든 입력값/미디어 미리보기도 초기화됨.

---

## 11. 📱 반응형 디자인
- `max-width: 600px` 이하에서 `.row`의 `flex-direction`이 자동 전환되어 모바일에서도 보기 좋음.
- 미디어 쿼리 사용으로 화면 폭에 따라 레이아웃 자동 조정.

---

## 12. 🎨 사용자 인터페이스(UI) 개선 요소
- 버튼 hover 효과, 로고 애니메이션(scale 확대), 삭제 버튼 마우스오버 시만 표시.
- 각 요소가 둥근 모서리(`border-radius: 8px`) 및 여백 등으로 가독성 향상.

---

## 🧩 총정리: 사용자 플로우 예시

- 사용자가 사이트에 접속하면 로고만 보임
- 로고 클릭 → 다이어리 작성 화면 진입
- 날짜, 날씨, 기분 선택 후 일기 작성
- 이미지/녹음 추가
- 저장 버튼 클릭 → 로컬스토리지에 저장
- 날짜 변경 시 자동으로 불러오기
- 삭제도 가능

---

## html 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>☆ MyDay ☆</title>
  <style>
    /* 공통 스타일 */
    body {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
    }

    /* 랜딩 페이지 스타일 */
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

    /* 일기 페이지 스타일 */
    .diary-container {
      display: none; /* 처음에는 숨김 */
    }

    .row {
      display: flex;
      flex-direction: column;
      gap: 12px;
      max-width: 600px;
      margin: auto;
      padding: 16px;
    }

    /* 반응형 */
    @media (max-width: 600px) {
      .row {
        flex-direction: column;
        align-items: stretch;
      } 
    }

    /* 공통 입력 요소 스타일 */
    input,
    select,
    textarea,
    button {
      font-size: 16px;
      padding: 8px;
      border-radius: 8px;
      border: 1px solid #ccc;
      box-sizing: border-box;
    }

    /* 텍스트박스 */
    textarea {
      width: 100%;
      height: 150px;
      resize: vertical;
    }

    /* 제목 스타일 */
    .title {
      text-align: center;
      margin-bottom: 20px;
    }

    .title a {
      text-decoration: none;
      color: black;
      font-size: 2em;
      font-weight: bold;
    }

    .title a:hover {
      color: #888;
    }

    /* 이미지 미리보기 컨테이너 */
    .image-preview-container {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 10px;
    }

    .uploaded-image-wrapper,
    .audio-wrapper {
      position: relative;
      display: inline-block;
    }

    .uploaded-image {
      max-width: 300px;
      max-height: 300px;
      border-radius: 10px;
    }

    /* 삭제 버튼 공통 스타일 */
    .remove-button {
      position: absolute;
      top: 5px;
      right: 5px;
      background-color: rgba(0, 0, 0, 0.6);
      color: white;
      border: none;
      border-radius: 50%;
      width: 30px;
      height: 30px;
      cursor: pointer;
      font-size: 16px;
      z-index: 10;
      display: none;
    }

    .uploaded-image-wrapper:hover .remove-button,
    .audio-wrapper:hover .remove-button {
      display: block;
    }

    /* 버튼 공통 스타일 */
    .round-icon-button {
      font-size: 24px;
      padding: 12px;
      background-color: #f0f0f0;
      border-radius: 50%;
      border: none;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 50px;
      height: 50px;
      cursor: pointer;
      transition: background-color 0.2s ease;
    }

    .round-icon-button:hover {
      background-color: #ddd;
    }

    /* 동작 버튼 그룹 정렬 */
    .action-buttons {
      display: flex;
      justify-content: center;
      gap: 15px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <!-- 랜딩 페이지 -->
  <div id="landing-page" class="landing-container">
    <a href="#" class="logo" onclick="showDiaryPage()">☆ MyDay ☆</a>
  </div>

  <!-- 일기 페이지 -->
  <div id="diary-page" class="diary-container">
    <div class="row">
      <!-- 타이틀 -->
      <h1 class="title">
        <a href="#" onclick="showLandingPage()">☆ MyDay ☆</a>
      </h1>

      <!-- 날짜 선택 -->
      <input type="date" id="datePicker" />

      <!-- 날씨 선택 -->
      <label for="weather">날씨:</label>
      <select id="weather">
        <option value="sunny">☀️ 맑음</option>
        <option value="cloudy">⛅ 흐림</option>
        <option value="rainy">🌧️ 비</option>
        <option value="snowy">❄️ 눈</option>
      </select>

      <!-- 기분 선택 -->
      <label for="mood">기분:</label>
      <select id="mood">
        <option value="happy">😊 행복</option>
        <option value="sad">😢 슬픔</option>
        <option value="angry">😡 화남</option>
        <option value="excited">🤩 신남</option>
        <option value="neutral">😐 보통</option>
      </select>

      <!-- 일기 내용 -->
      <textarea id="diaryText" placeholder="오늘 하루를 기록하세요"></textarea>

      <!-- 이미지 미리보기 영역 -->
      <div id="imagePreviewContainer" class="image-preview-container"></div>

      <!-- 오디오 미리보기 영역 -->
      <div id="audioPreviewContainer" class="audio-preview-container"></div>

      <!-- 이미지 업로드 숨김 처리 -->
      <input type="file" id="imageUpload" accept="image/*" multiple hidden />

      <!-- 버튼 영역 (업로드, 녹음) -->
      <div class="action-buttons">
        <button id="customUploadButton"> + </button>
        <button id="recordButton">🎙️</button>
      </div>

      <!-- 버튼 영역 (삭제, 저장) -->
      <div class="action-buttons">
        <button id="deleteEntryButton">삭제</button>
        <div style="flex: 1;"></div>
        <button id="saveButton">저장</button>
      </div>
    </div>
  </div>

  <script>
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

    // 일기 페이지 기능 구현
    const datePicker = document.getElementById('datePicker');
    const diaryText = document.getElementById('diaryText');
    const weather = document.getElementById('weather');
    const mood = document.getElementById('mood');
    const imageUpload = document.getElementById('imageUpload');
    const imagePreviewContainer = document.getElementById('imagePreviewContainer');
    const audioPreviewContainer = document.getElementById('audioPreviewContainer');
    const recordButton = document.getElementById('recordButton');
    const saveButton = document.getElementById('saveButton');

    let mediaRecorder;
    let audioChunks = [];

    function resetForm() {
      datePicker.value = new Date().toISOString().split('T')[0];
      weather.value = 'sunny';
      mood.value = 'happy';
      diaryText.value = '';
      imagePreviewContainer.innerHTML = '';
      audioPreviewContainer.innerHTML = '';
      imageUpload.value = '';
    }

    // 이미지 업로드 처리
    imageUpload.addEventListener('change', (event) => {
      const files = Array.from(event.target.files);
      files.forEach(file => {
        const reader = new FileReader();
        reader.onload = () => {
          const wrapper = document.createElement('div');
          wrapper.className = 'uploaded-image-wrapper';

          const img = document.createElement('img');
          img.src = reader.result;
          img.className = 'uploaded-image';

          const removeBtn = document.createElement('button');
          removeBtn.textContent = 'X';
          removeBtn.className = 'remove-button';
          removeBtn.onclick = () => wrapper.remove();

          wrapper.appendChild(img);
          wrapper.appendChild(removeBtn);
          imagePreviewContainer.appendChild(wrapper);
        };
        reader.readAsDataURL(file);
      });
      imageUpload.value = '';
    });

    // 녹음 버튼 처리
    recordButton.addEventListener('click', async () => {
      if (!mediaRecorder) {
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
        mediaRecorder = new MediaRecorder(stream);
        audioChunks = [];

        mediaRecorder.ondataavailable = (event) => audioChunks.push(event.data);

        mediaRecorder.onstop = () => {
          const audioBlob = new Blob(audioChunks, { type: 'audio/mp3' });
          const audioURL = URL.createObjectURL(audioBlob);

          const wrapper = document.createElement('div');
          wrapper.className = 'audio-wrapper';

          const audioElement = document.createElement('audio');
          audioElement.src = audioURL;
          audioElement.controls = true;
          audioElement.className = 'audio-preview';

          const removeBtn = document.createElement('button');
          removeBtn.textContent = 'X';
          removeBtn.className = 'remove-button';
          removeBtn.onclick = () => wrapper.remove();

          wrapper.appendChild(audioElement);
          wrapper.appendChild(removeBtn);
          audioPreviewContainer.appendChild(wrapper);
        };

        mediaRecorder.start();
        recordButton.textContent = '⏺️';
        recordButton.style.backgroundColor = '#ff5c5c';
      } else {
        mediaRecorder.stop();
        mediaRecorder = null;
        recordButton.textContent = '🎙️';
        recordButton.style.backgroundColor = '';
      }
    });

    // 저장 버튼 처리
    saveButton.addEventListener('click', () => {
      const images = Array.from(imagePreviewContainer.querySelectorAll('img')).map(img => img.src);
      const audioElements = Array.from(audioPreviewContainer.querySelectorAll('audio')).map(audio => audio.src);
      
      const entry = {
        text: diaryText.value,
        weather: weather.value,
        mood: mood.value,
        images: images,
        audioSources: audioElements
      };
      localStorage.setItem(datePicker.value, JSON.stringify(entry));
      alert('저장되었습니다!');
    });

    // 삭제 버튼 처리
    document.getElementById('deleteEntryButton').addEventListener('click', () => {
      const selectedDate = datePicker.value;
      if (localStorage.getItem(selectedDate)) {
        if (confirm("정말로 이 날짜의 일기를 삭제하시겠습니까?")) {
          localStorage.removeItem(selectedDate);
          alert("일기가 삭제되었습니다.");
          resetForm();
        }
      } else {
        alert("해당 날짜에는 저장된 일기가 없습니다.");
      }
    });

    // 날짜 변경 시 저장된 데이터 불러오기
    datePicker.addEventListener('change', () => {
      const savedEntry = JSON.parse(localStorage.getItem(datePicker.value)) || {};
      diaryText.value = savedEntry.text || '';
      weather.value = savedEntry.weather || 'sunny';
      mood.value = savedEntry.mood || 'happy';
    
      // 이미지 복원
      imagePreviewContainer.innerHTML = '';
      (savedEntry.images || []).forEach(src => {
        const wrapper = document.createElement('div');
        wrapper.className = 'uploaded-image-wrapper';

        const img = document.createElement('img');
        img.src = src;
        img.className = 'uploaded-image';

        const removeBtn = document.createElement('button');
        removeBtn.textContent = 'X';
        removeBtn.className = 'remove-button';
        removeBtn.onclick = () => wrapper.remove();

        wrapper.appendChild(img);
        wrapper.appendChild(removeBtn);
        imagePreviewContainer.appendChild(wrapper);
      });

      // 오디오 복원
      audioPreviewContainer.innerHTML = '';
      (savedEntry.audioSources || []).forEach(src => {
        const wrapper = document.createElement('div');
        wrapper.className = 'audio-wrapper';
        
        const audioElement = document.createElement('audio');
        audioElement.src = src;
        audioElement.controls = true;
        audioElement.className = 'audio-preview';
        
        const removeBtn = document.createElement('button');
        removeBtn.textContent = 'X';
        removeBtn.className = 'remove-button';
        removeBtn.onclick = () => wrapper.remove();
        
        wrapper.appendChild(audioElement);
        wrapper.appendChild(removeBtn);
        
        audioPreviewContainer.appendChild(wrapper);
      });
    });
    
    // 업로드 버튼 커스텀 트리거
    document.getElementById('customUploadButton').addEventListener('click', () => {
      imageUpload.click();
    });

    // 초기 페이지 설정
    showLandingPage();
  </script>
</body>
</html>
```

---

## css 코드

```css
/* 전체 컨테이너 */
.row {
  display: flex;
  flex-direction: column;
  gap: 12px;
  max-width: 600px;
  margin: auto;
  padding: 16px;
}
/* 반응형 */
@media (max-width: 600px) {
  .row {
    flex-direction: column;
    align-items: stretch;
  } 
}
/* 공통 입력 요소 스타일 */
input,
select,
textarea,
button {
  font-size: 16px;
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #ccc;
  box-sizing: border-box;
}
/* 텍스트박스 */
textarea {
  width: 100%;
  height: 150px;
  resize: vertical;
}
/* 제목 스타일 */
.title {
  text-align: center;
  margin-bottom: 20px;
}
.title a {
  text-decoration: none;
  color: black;
  font-size: 2em;
  font-weight: bold;
}
.title a:hover {
  color: #888;
}
/* 이미지 미리보기 컨테이너 */
.image-preview-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}
.uploaded-image-wrapper,
.audio-wrapper {
  position: relative;
  display: inline-block;
}
.uploaded-image {
  max-width: 300px;
  max-height: 300px;
  border-radius: 10px;
}
/* 삭제 버튼 공통 스타일 */
.remove-button {
  position: absolute;
  top: 5px;
  right: 5px;
  background-color: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  cursor: pointer;
  font-size: 16px;
  z-index: 10;
  display: none;
}
.uploaded-image-wrapper:hover .remove-button,
.audio-wrapper:hover .remove-button {
  display: block;
}
/* 버튼 공통 스타일 */
.round-icon-button {
  font-size: 24px;
  padding: 12px;
  background-color: #f0f0f0;
  border-radius: 50%;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50px;
  height: 50px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}
.round-icon-button:hover {
  background-color: #ddd;
}
/* 동작 버튼 그룹 정렬 */
.action-buttons {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 20px;
}

/* 랜딩 페이지 스타일 */
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


