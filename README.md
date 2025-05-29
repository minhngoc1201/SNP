<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Photobooth Chụp Hình Thương Hiệu</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: url('https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/bg-key-visual-run-as-one-2025-01-6838100db8936.png') no-repeat center center fixed;
      background-size: cover;
      color: white;
      font-family: sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
    }

    header {
      width: 100%;
      max-width: 480px;
      padding: 20px;
      text-align: center;
      background-color: rgba(0,0,0,0.5);
      border-bottom: 1px solid #00f0ff;
    }

    header img {
      max-width: 150px;
      height: auto;
      margin: 0 auto;
      display: block;
    }

    h1 {
      margin: 0.5rem 0 0 0;
      font-weight: normal;
      font-size: 1.6rem;
      text-shadow: 0 0 5px #00f0ff;
    }

    #video-container {
      position: relative;
      width: 100%;
      max-width: 480px;
      aspect-ratio: 3 / 4;
      overflow: hidden;
      border-radius: 12px;
      margin-top: 1rem;
      background: #000;
      box-shadow: 0 0 15px #00f0ff;
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
      width: 100%;
      max-width: 480px;
    }

    select, button {
      background: #00f0ff;
      color: #000;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 10px;
      cursor: pointer;
      width: 100%;
      max-width: 300px;
      box-sizing: border-box;
      transition: background-color 0.3s ease;
    }

    select:hover, button:hover {
      background: #00d0e0;
    }

    #download {
      color: #00f0ff;
      text-decoration: underline;
      display: none;
      margin-top: 10px;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <header>
    <img src="https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/logo-run-as-one-2025-01-6838105ac2103.png" alt="Logo Thương Hiệu" />
    <h1>📸 Photobooth Chụp Hình</h1>
  </header>

  <div class="controls">
    <label for="frameSelect">🖼️ Chọn khung hình thương hiệu:</label>
    <select id="frameSelect">
      <option value="">-- Chọn khung --</option>
      <option value="https://i.imgur.com/7V4I4hR.png">Khung Xanh Dương</option>
      <option value="https://i.imgur.com/8q4JcqA.png">Khung Hồng</option>
      <option value="https://i.imgur.com/oY6P2RF.png">Khung Vàng</option>
    </select>

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
    const frameSelect = document.getElementById('frameSelect');

    let frameImage = null;

    // Mở webcam
    navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } })
      .then(stream => {
        video.srcObject = stream;
      })
      .catch(err => {
        alert("Không thể mở camera: " + err.message);
      });

    // Khi chọn khung thay đổi
    frameSelect.addEventListener('change', () => {
      const url = frameSelect.value;
      if (url) {
        const img = new Image();
        img.crossOrigin = "anonymous";
        img.onload = () => {
          frameImage = img;
        };
        img.onerror = () => {
          alert('Không tải được khung hình, vui lòng kiểm tra URL.');
          frameImage = null;
        };
        img.src = url;
      } else {
        frameImage = null;
      }
    });

    // Chụp ảnh
    snapBtn.addEventListener('click', () => {
      const width = video.videoWidth;
      const height = video.videoHeight;

      if (!width || !height) {
        alert('Camera chưa sẵn sàng, vui lòng thử lại.');
        return;
      }

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
