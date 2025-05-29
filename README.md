<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Photobooth SNP</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background: url('https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/bg-key-visual-run-as-one-2025-01-6838100db8936.png') no-repeat center center fixed;
      background-size: cover;
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    header {
      text-align: center;
      margin: 20px;
    }
    header img {
      max-width: 100px;
      height: auto;
    }
    video, canvas {
      width: 100%;
      max-width: 480px;
      border-radius: 10px;
      margin-top: 10px;
    }
    .controls {
      margin: 20px 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }
    select, button {
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 10px;
      border: none;
      cursor: pointer;
    }
    #download {
      color: #00f0ff;
      display: none;
      margin-top: 10px;
    }
  </style>
</head>
<body>

<header>
  <img src="https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/logo-run-as-one-2025-01-6838105ac2103.png" alt="Logo Thương hiệu">
  <h1>Photobooth SNP</h1>
</header>

<div class="controls">
  <label for="frameSelect">Chọn khung hình:</label>
  <select id="frameSelect">
    <option value="">-- Không khung --</option>
    <option value="https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/meet-tribe-06-1-683817714cf39.png">Khung RUN AS ONE 2025</option>
  </select>
  <button id="snap">📷 Chụp ảnh</button>
  <a id="download" download="photo.png">📥 Tải ảnh</a>
</div>

<video id="video" autoplay playsinline></video>
<canvas id="canvas" style="display:none;"></canvas>

<script>
  const video = document.getElementById('video');
  const canvas = document.getElementById('canvas');
  const ctx = canvas.getContext('2d');
  const snap = document.getElementById('snap');
  const download = document.getElementById('download');
  const frameSelect = document.getElementById('frameSelect');

  let frameImg = null;

  // Bật camera
  navigator.mediaDevices.getUserMedia({ video: true })
    .then(stream => video.srcObject = stream)
    .catch(err => alert("Không thể truy cập camera: " + err));

  // Khi chọn khung
  frameSelect.addEventListener('change', () => {
    const url = frameSelect.value;
    if (!url) {
      frameImg = null;
      return;
    }
    frameImg = new Image();
    frameImg.crossOrigin = "anonymous";
    frameImg.src = url;
  });

  // Chụp ảnh
  snap.addEventListener('click', () => {
    const w = video.videoWidth;
    const h = video.videoHeight;
    canvas.width = w;
    canvas.height = h;
    ctx.drawImage(video, 0, 0, w, h);
    if (frameImg) {
      frameImg.onload = () => {
        ctx.drawImage(frameImg, 0, 0, w, h);
        showDownload();
      };
      if (frameImg.complete) {
        ctx.drawImage(frameImg, 0, 0, w, h);
        showDownload();
      }
    } else {
      showDownload();
    }
  });

  function showDownload() {
    const dataURL = canvas.toDataURL('image/png');
    download.href = dataURL;
    download.style.display = 'inline';
  }
</script>

</body>
</html>
