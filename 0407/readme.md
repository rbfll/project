### ✅ 기능 추가 및 개선 사항

- `☆ MyDay ☆` 클릭 시 입력값 초기화 (홈 화면 느낌 구현)
- 사진 업로드 시 기본 input 숨기고 `사진 추가` 버튼으로 대체
- 업로드된 이미지 위에 마우스 올리면 X 버튼 노출
- 반응형 레이아웃 적용 (모바일 대응)
- 전체 HTML/CSS/JS 코드 정리 및 최적화

### 💡 기술 스택

- HTML5 / CSS3 / JavaScript 
- localStorage 활용 (데이터 저장 및 불러오기)

### 🎯 핵심 결과

- 가독성 및 유지보수성 개선
- 모바일에서도 자연스럽게 작동


## 실행화면

![image](https://github.com/user-attachments/assets/3db80466-a5cf-4701-8455-3f269a72c4d5)

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
    <h1 class="title">
      <a href="#" onclick="resetForm()">☆ MyDay ☆</a>
    </h1>

    <input type="date" id="datePicker" />

    <label for="weather">날씨:</label>
    <select id="weather">
      <option value="sunny">☀️ 맑음</option>
      <option value="cloudy">⛅ 흐림</option>
      <option value="rainy">🌧️ 비</option>
      <option value="snowy">❄️ 눈</option>
    </select>

    <label for="mood">기분:</label>
    <select id="mood">
      <option value="happy">😊 행복</option>
      <option value="sad">😢 슬픔</option>
      <option value="angry">😡 화남</option>
      <option value="excited">🤩 신남</option>
      <option value="neutral">😐 보통</option>
    </select>

    <textarea id="diaryText" placeholder="오늘 하루를 기록하세요"></textarea>

    <input type="file" id="imageUpload" accept="image/*" style="display: none;" />
    <button id="customUploadButton">사진 추가</button>

    <div id="imagePreview" class="image-preview"
      onmouseover="document.getElementById('removeImageButton').style.display='block'"
      onmouseout="document.getElementById('removeImageButton').style.display='none'">
      <img id="uploadedImage" class="uploaded-image" />
      <button id="removeImageButton" class="remove-button">X</button>
    </div>

    <button id="recordButton">녹음 시작</button>
    <audio id="audioPlayback" controls style="display: none;"></audio>
    <button id="saveButton">저장</button>
  </div>

  <script>
    const datePicker = document.getElementById('datePicker');
    const diaryText = document.getElementById('diaryText');
    const weather = document.getElementById('weather');
    const mood = document.getElementById('mood');
    const imageUpload = document.getElementById('imageUpload');
    const uploadedImage = document.getElementById('uploadedImage');
    const imagePreview = document.getElementById('imagePreview');
    const removeImageButton = document.getElementById('removeImageButton');
    const recordButton = document.getElementById('recordButton');
    const audioPlayback = document.getElementById('audioPlayback');
    const saveButton = document.getElementById('saveButton');

    let mediaRecorder;
    let audioChunks = [];

    function resetForm() {
      datePicker.value = new Date().toISOString().split('T')[0];
      weather.value = 'sunny';
      mood.value = 'happy';
      diaryText.value = '';
      uploadedImage.src = '';
      imageUpload.value = '';
      imagePreview.style.display = 'none';
      audioPlayback.src = '';
      audioPlayback.style.display = 'none';
    }

    datePicker.addEventListener('change', () => {
      const savedEntry = JSON.parse(localStorage.getItem(datePicker.value)) || {};
      diaryText.value = savedEntry.text || '';
      weather.value = savedEntry.weather || 'sunny';
      mood.value = savedEntry.mood || 'happy';

      if (savedEntry.image) {
        uploadedImage.src = savedEntry.image;
        imagePreview.style.display = 'block';
      } else {
        uploadedImage.src = '';
        imagePreview.style.display = 'none';
      }

      audioPlayback.src = savedEntry.audio || '';
      audioPlayback.style.display = savedEntry.audio ? 'block' : 'none';
    });

    imageUpload.addEventListener('change', (event) => {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = () => {
          uploadedImage.src = reader.result;
          imagePreview.style.display = 'block';
        };
        reader.readAsDataURL(file);
      }
    });

    removeImageButton.addEventListener('click', () => {
      uploadedImage.src = '';
      imageUpload.value = '';
      imagePreview.style.display = 'none';
    });

    recordButton.addEventListener('click', async () => {
      if (!mediaRecorder) {
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
        mediaRecorder = new MediaRecorder(stream);
        mediaRecorder.ondataavailable = (event) => audioChunks.push(event.data);
        mediaRecorder.onstop = () => {
          const audioBlob = new Blob(audioChunks, { type: 'audio/mp3' });
          audioPlayback.src = URL.createObjectURL(audioBlob);
          audioPlayback.style.display = 'block';
          audioChunks = [];
        };
        mediaRecorder.start();
        recordButton.textContent = '녹음 중지';
      } else {
        mediaRecorder.stop();
        mediaRecorder = null;
        recordButton.textContent = '녹음 시작';
      }
    });

    saveButton.addEventListener('click', () => {
      const entry = {
        text: diaryText.value,
        weather: weather.value,
        mood: mood.value,
        image: uploadedImage.src,
        audio: audioPlayback.src
      };
      localStorage.setItem(datePicker.value, JSON.stringify(entry));
      alert('저장되었습니다!');
    });

    document.getElementById('customUploadButton').addEventListener('click', () => {
      imageUpload.click();
    });
  </script>
</body>
</html>
```

## css코드


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

/* 반응형 - 화면이 좁아지면 정렬 변경 */
@media (max-width: 600px) {
  .row {
    flex-direction: column;
    align-items: stretch;
  }
}

/* 입력 요소 및 버튼 기본 스타일 */
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

/* 텍스트 박스 */
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

/* 이미지 미리보기 영역 */
.image-preview {
  display: none;
  position: relative;
  display: inline-block;
}

/* 업로드된 이미지 스타일 */
.uploaded-image {
  max-width: 300px;
  max-height: 300px;
  border-radius: 10px;
}

/* X 버튼 스타일 */
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

/* 이미지에 마우스 올리면 X 버튼 보이기 */
.image-preview:hover .remove-button {
  display: block;
}
