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
      max-width: 180px;
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

<div class="
