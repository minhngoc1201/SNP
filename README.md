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
      padding: 10px;
    }
    header {
      text-align: center;
      margin: 10px 0;
    }
    header img {
      max-width: 80px;
    }
    header h1 {
      font-size: 1.2rem;
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
      background-color: white;
      object-fit: contain;
    }
    .frame-option.selected {
      border-color: #00f0ff;
    }
    .video-container {
      position: relative;
      width: 100%;
      max-width: 480px;
      aspect-ratio: 3/4;
      margin-top: 10px;
    }
    video, #frameOverlay {
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      object-fit: cover;
      border-radi
