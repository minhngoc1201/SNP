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
      color: #0
