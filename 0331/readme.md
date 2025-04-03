## 실행화면
- **날짜별 일기 작성 및 저장 기능 구현**

- **날씨 및 기분 선택 기능 추가**

- **이미지 업로드 및 미리보기 기능 구현**

- **음성 녹음 및 저장 기능 구현**

- **데이터 로컬 저장**

![image](https://github.com/user-attachments/assets/4ae35e6d-21c1-41a9-ba96-16a7378b8603)

## html 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>☆ MyDay ☆</title>
</head>
<body>
    <div class="container">
        <h1>☆ MyDay ☆</h1>
        <input type="date" id="datePicker">
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
        <input type="file" id="imageUpload" accept="image/*">
        <img id="uploadedImage" style="max-width: 100%; display: none;" />
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
        const recordButton = document.getElementById('recordButton');
        const audioPlayback = document.getElementById('audioPlayback');
        const saveButton = document.getElementById('saveButton');
        let mediaRecorder;
        let audioChunks = [];

        datePicker.addEventListener('change', () => {
            const savedEntry = JSON.parse(localStorage.getItem(datePicker.value)) || {};
            diaryText.value = savedEntry.text || '';
            weather.value = savedEntry.weather || 'sunny';
            mood.value = savedEntry.mood || 'happy';
            uploadedImage.src = savedEntry.image || '';
            uploadedImage.style.display = savedEntry.image ? 'block' : 'none';
            audioPlayback.src = savedEntry.audio || '';
            audioPlayback.style.display = savedEntry.audio ? 'block' : 'none';
        });

        imageUpload.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = () => {
                    uploadedImage.src = reader.result;
                    uploadedImage.style.display = 'block';
                };
                reader.readAsDataURL(file);
            }
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
    </script>
</body>
</html>
