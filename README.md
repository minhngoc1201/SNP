<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Photobooth RUN AS ONE 2025</title>
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

    .frame-options {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin: 15px 0;
    }

    .frame-option {
      width: 80px;
      height: 80px;
      border: 2px solid transparent;
      border-radius: 8px;
      cursor: pointer;
      object-fit: contain;
      background-color: #fff;
    }

    .frame-option.selected {
      border-color: #00f0ff;
    }

    .video-container {
      position: relative;
      width: 100%;
      max-width: 480px;
    }

    video, canvas {
      width: 100%;
      border-radius: 10px;
    }

    #frameOverlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      pointer-events: none;
    }

    .controls {
      margin-top: 15px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }

    button {
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
      font-weight: bold;
    }
  </style>
</head>
<body>

<header>
  <img src="https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/logo-run-as-one-2025-01-6838105ac2103.png" alt="Logo Thương hiệu">
  <h1>Photobooth SNP</h1>
</header>

<div class="frame-options" id="frameOptions">
  <img class="frame-option" src="https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/meet-tribe-06-1-683817714cf39.png" data-url="https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/meet-tribe-06-1-683817714cf39.png" alt="Khung 1">
</div>

<div class="video-container">
  <video id="video" autoplay playsinline></video>
  <img id="frameOverlay" src="" alt="Khung chồng lên video">
</div>

<canvas id="canvas" style="display:none;"></canvas>

<div class="controls">
  <button id="snap">📷 Chụp ảnh</button>
  <a id="download" download="photo.png">📥 Tải ảnh</a>
</div>

<script>
  const video = document.getElementById('video');
  const canvas = document.getElementById('canvas');
  const ctx = canvas.getContext('2d');
  const snap = document.getElementById('snap');
  const download = document.getElementById('download');
  const frameOverlay = document.getElementById('frameOverlay');
  const frameOptions = document.getElementById('frameOptions');

  let selectedFrameUrl = "";

  // Khởi động camera
  navigator.mediaDevices.getUserMedia({ video: true })
    .then(stream => video.srcObject = stream)
    .catch(err => alert("Không thể truy cập camera: " + err));

  // Xử lý chọn khung hình
  frameOptions.addEventListener('click', (e) => {
    if (e.target.classList.contains('frame-option')) {
      document.querySelectorAll('.frame-option').forEach(el => el.classList.remove('selected'));
      e.target.classList.add('selected');
      selectedFrameUrl = e.target.dataset.url;
      frameOverlay.src = selectedFrameUrl;
    }
  });

  // Chụp ảnh
  snap.addEventListener('click', () => {
    const w = video.videoWidth;
    const h = video.videoHeight;
    canvas.width = w;
    canvas.height = h;
    ctx.drawImage(video, 0, 0, w, h);

    if (selectedFrameUrl) {
      const img = new Image();
      img.crossOrigin = "anonymous";
      img.onload = () => {
        ctx.drawImage(img, 0, 0, w, h);
        showDownload();
      };
      img.src = selectedFrameUrl;
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
