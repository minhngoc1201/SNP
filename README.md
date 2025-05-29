<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Photobooth SNP</title>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: url('https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/bg-key-visual-run-as-one-2025-01-6838100db8936.png') no-repeat center center fixed;
      background-size: cover;
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 10px;
    }
    header {
      text-align: center;
      margin: 10px 0;
    }
    header img {
      max-width: 60px;
      height: auto;
    }
    header h1 {
      font-size: 1.2rem;
      margin: 10px 0 0;
    }
    .controls {
      width: 100%;
      max-width: 480px;
      margin: 15px 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
      background: rgba(0, 0, 0, 0.4);
      padding: 15px;
      border-radius: 12px;
    }
    select, button {
      width: 100%;
      max-width: 300px;
      padding: 10px;
      font-size: 16px;
      border-radius: 8px;
      border: none;
      cursor: pointer;
    }
    video, canvas {
      width: 100%;
      max-width: 480px;
      border-radius: 10px;
      margin-top: 10px;
    }
    #download {
      display: none;
      margin-top: 10px;
      color: #00f0ff;
      font-weight: bold;
    }

    @media (max-width: 480px) {
      header h1 {
        font-size: 1rem;
      }
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

  navigator.mediaDevices.getUserMedia({ video: true })
    .then(stream => video.srcObject = stream)
    .catch(err => alert("Không thể truy cập camera: " + err));

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
