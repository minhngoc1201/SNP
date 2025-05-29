<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Photobooth SNP</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: sans-serif;
      background: url('https://cdn.saigonnewport.com.vn/uploads/images/2025/05/29/bg-key-visual-run-as-one-2025-01-6838100db8936.png') no-repeat center center fixed;
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
    header h1 {
      font-size: 1.5rem;
    }
    .frame-options {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin: 15px 0;
    }
    .frame-option-container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .frame-option {
      width: 80px;
      height: 80px;
      border: 2px solid transparent;
      border-radius: 8px;
      cursor: pointer;
      background-color: white;
      object-fit: contain;
    }
    .frame-option.selected {
      border-color: #00f0ff;
    }
    .frame-label {
      font-size: 0.75rem;
      color: white;
      margin-top: 4px;
    }
    .video-container {
      position: relative;
      width: 100%;
      max-width: 480px;
      height: 640px;
      margin-top: 10px;
      background: black;
      overflow: hidden;
      border-radius: 10px;
    }
    video, #frameOverlay {
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      object-fit: cover;
      border-radius: 10px;
    }
    video {
      z-index: 1;
      background: black;
    }
    #frameOverlay {
      pointer-events: none;
      z-index: 2;
      display: none;
    }
    canvas {
      display: none;
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
      background-color: #00f0ff;
      color: black;
    }
    #download {
      display: none;
      color: #00f0ff;
      font-weight: bold;
      text-decoration: none;
      background-color: white;
      padding: 8px 16px;
      border-radius: 8px;
    }
  </style>
</head>
<body>

<header>
  <h1>Photobooth SNP</h1>
</header>

<div class="frame-options" id="frameOptions">
  <!-- Không khung -->
  <div class="frame-option-container">
    <img class="frame-option selected"
         src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mP8/wcAAwAB/VRlF64AAAAASUVORK5CYII="
         data-url="" alt="No Frame">
    <div class="frame-label">❌ Không khung</div>
  </div>

  <!-- Khung PNG nền trong -->
  <div class="frame-option-container">
    <img class="frame-option"
         src="./images/Frame test.png"
         data-url="./images/Frame test.png"
         alt="Khung SNP">
    <div class="frame-label">🌟 Khung SNP</div>
  </div>
</div>

<div class="video-container">
  <video id="video" autoplay playsinline muted></video>
  <img id="frameOverlay" src="" alt="Khung Overlay">
</div>

<canvas id="canvas"></canvas>

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

  async function startCamera() {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: "user" },
        audio: false
      });
      video.srcObject = stream;
      await video.play();
    } catch (err) {
      alert("Không thể truy cập camera: " +
