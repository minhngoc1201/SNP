<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Photobooth + Khung Thương Hiệu</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background: #111;
      color: white;
      margin: 0;
      padding: 2rem;
    }
    video, canvas {
      width: 100%;
      max-width: 400px;
      border-radius: 10px;
      margin-bottom: 1rem;
    }
    button, input[type="file"] {
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 8px;
      border: none;
      margin: 0.5rem;
      background-color: #00d4ff;
      color: black;
      cursor: pointer;
    }
    a {
      display: block;
      margin-top: 1rem;
      color: #00d4ff;
      text-decoration: underline;
    }
  </style>
</head>
<body>
  <h1>📸 Photobooth Online</h1>

  <video id="video" autoplay playsinline></video>
  <br>

  <!-- Nút upload khung PNG -->
  <input type="file" id="frameUpload" accept="image/png">
  <br>

  <button id="snap">Chụp ảnh</button>
  <canvas id="canvas" style="display:none;"></canvas>
  <br>
  <a id="download" href="#" download="photobooth.png" style="display:none;">Tải ảnh về</a>

  <script>
    const video = document.getElementById('video');
    const canvas = document.getElementById('canvas');
    const snap = document.getElementById('snap');
    const download = document.getElementById('download');
    const context = canvas.getContext('2d');

    const frameUpload = document.getElementById('frameUpload');
    let frameImage = null;

    // Mở camera
    navigator.mediaDevices.getUserMedia({ video: true })
      .then(stream => {
        video.srcObject = stream;
      })
      .catch(error => {
        alert('Không thể mở camera: ' + error.message);
      });

    // Xử lý ảnh khung PNG
    frameUpload.addEventListener('change', (e) => {
      const file = e.target.files[0];
      if (file && file.type === 'image/png') {
        const reader = new FileReader();
        reader.onload = () => {
          const img = new Image();
          img.onload = () => {
            frameImage = img;
          };
          img.src = reader.result;
        };
        reader.readAsDataURL(file);
      } else {
        alert("Vui lòng chọn file PNG có nền trong suốt.");
      }
    });

    // Chụp ảnh
    snap.addEventListener('click', () => {
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;

      // Chụp ảnh từ video
      context.drawImage(video, 0, 0, canvas.width, canvas.height);

      // Chồng khung lên nếu có
      if (frameImage) {
        context.drawImage(frameImage, 0, 0, canvas.width, canvas.height);
      }

      canvas.style.display = 'block';
      const dataURL = canvas.toDataURL('image/png');
      download.href = dataURL;
      download.style.display = 'block';
    });
  </script>
</body>
</html>
