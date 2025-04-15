## ✅ 기능적 개선 사항

1. **📅 날짜별 저장/불러오기 기능 추가**
   - `localStorage`를 사용해 날짜별로 일기 저장 및 불러오기 가능
   - 날짜 변경 시 해당 날짜의 일기 내용, 이미지, 오디오 자동 복원

2. **📸 이미지 여러 장 업로드**
   - `<input type="file" multiple>` 사용하여 복수 이미지 업로드 지원
   - 업로드한 이미지 미리보기 가능, `X` 버튼을 통해 삭제 기능 제공

3. **🎤 오디오 녹음 기능 + 미리듣기 + 삭제**
   - `MediaRecorder` API를 활용하여 브라우저에서 오디오 녹음 가능
   - 녹음 종료 시 `<audio>` 태그로 미리듣기 제공 및 삭제 버튼 생성
   - 녹음 상태에 따라 버튼 아이콘이 `🎙️` ↔ `⏺️` 로 전환되어 직관적

4. **💾 저장 기능 강화**
   - 저장 시 `alert("저장되었습니다!")`로 사용자에게 피드백 제공
   - JSON 형태로 `localStorage`에 저장 (`{ text, weather, mood, images, audioSources }` 구조)

5. **🧹 삭제 기능 개선**
   - 저장된 일기가 있을 경우만 삭제되도록 확인창 추가
   - 삭제 후 자동으로 입력 요소 초기화 수행

6. **🔄 초기화 기능**
   - 상단 타이틀 클릭 시 `resetForm()` 함수 호출 → 모든 입력 초기화
   - 날짜 입력값은 자동으로 오늘 날짜로 재설정 (`new Date().toISOString().split('T')[0]`)


---



## 🎨 UI / UX 개선 사항

1. **버튼 시각화 개선**
   - 텍스트 기반 버튼에서 **의미 있는 아이콘 버튼**으로 전환  
     예: `+`(사진추가), `🎙️`(녹음), `⏺️`(녹음 중)

2. **버튼 스타일 & 크기 조절**
   - 저장 및 삭제 버튼 크기 축소로 공간 절약
   - `.round-icon-button` 클래스를 사용하여 둥글고 직관적인 버튼 디자인 적용

3. **Flex 정렬로 전체 레이아웃 정돈**
   - `.row`, `.action-buttons` 등에 Flexbox 적용
   - 버튼 및 요소 정렬을 개선하여 반응형 구성 가능

4. **오디오 및 이미지 삭제 버튼 hover 시 표시**
   - 마우스 hover 시에만 `X` 버튼이 표시되어 **UI가 깔끔하게 유지**

5. **반응형 UI 고려**
   - `@media (max-width: 600px)` 미디어 쿼리를 적용하여 모바일에서도 잘 작동


---



## 실행화면

![스크린샷 2025-04-15 171953](https://github.com/user-attachments/assets/d61c3052-c361-4df3-9b3b-527cd0210584)
![스크린샷 2025-04-15 172007](https://github.com/user-attachments/assets/f2d5ca38-aa66-41dd-b3fd-5fb13374f9ba)
![스크린샷 2025-04-15 172018](https://github.com/user-attachments/assets/9443e6d8-d2ab-4d9a-b521-d35cc4f01787)



---

## html 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>☆ MyDay ☆</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="row">
    <!-- 타이틀 -->
    <h1 class="title">
      <a href="#" onclick="resetForm()">☆ MyDay ☆</a>
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

  <script>
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

    // 초기 날짜 설정
    resetForm();
  </script>
</body>
</html>
```

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

```




