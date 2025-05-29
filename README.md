<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Photobooth Chụp Hình Thương Hiệu</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: #000;
      color: white;
      font-family: sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    h1 {
      margin-top: 1rem;
    }

    #video-container {
      position: relative;
      width: 100%;
      max-width: 480px;
      aspect-ratio: 3/4;
      overflow: hidden;
      border-radius: 12px;
      margin-top: 1rem;
    }

    video, canvas {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border-radius: 12px;
    }

    .controls {
      margin-top: 1rem;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }

    button, input[type="file"] {
      background: #00f0ff;
      color: #000;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 10px;
      cursor: pointer;
    }

    button:hover {
      background: #00d0e0;
    }

    #download {
      color: #00f0ff;
      text-decoration: underline;
      display: none;
    }
  </style>
</head>
<body>

  <h1>📸 Photobooth Chụp Hình</h1>

  <div class="controls">
    <label>
      🖼️ Tải lên khung PNG:
      <input type="file" id="frameUpload" accept="image/png">
    </label>

    <button id="snap">📷 Chụp ảnh</button>
    <a id="download" download="photo.png">📥 Tải ảnh về</a>
  </div>

  <div id="video-container">
    <video id="video" autoplay playsinline></video>
    <canvas id="canvas" style="display: none;"></canvas>
  </div>

  <script>
    const video = document.getElementById('video');
    const canvas = document.getElementById('canvas');
    const context = canvas.getContext('2d');
    const snapBtn = document.getElementById('snap');
    const downloadLink = document.getElementById('download');
    const frameUpload = document.getElementById('frameUpload');

    let frameImage = null;

    // Mở webcam
    navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } })
      .then(stream => {
        video.srcObject = stream;
      })
      .catch(err => {
        alert("Không thể mở camera: " + err.message);
      });

    // Xử lý file khung tải lên
    frameUpload.addEventListener('change', e => {
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
        alert("Vui lòng chọn file PNG hợp lệ.");
      }
    });

    // Chụp ảnh
    snapBtn.addEventListener('click', () => {
      const width = video.videoWidth;
      const height = video.videoHeight;

      canvas.width = width;
      canvas.height = height;

      // Chụp hình từ webcam
      context.drawImage(video, 0, 0, width, height);

      // Chồng khung PNG nếu có
      if (frameImage) {
        context.drawImage(frameImage, 0, 0, width, height);
      }

      canvas.style.display = 'block';
      const dataURL = canvas.toDataURL('image/png');
      downloadLink.href = dataURL;
      downloadLink.style.display = 'inline-block';
    });
  </script>

</body>
</html>
