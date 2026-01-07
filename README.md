<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Brick Bank</title>
  <style>
    body {
      margin: 0;
      background: #0a0a14;
      font-family: Arial, sans-serif;
      color: #fff;
    }

    header {
      padding: 20px;
      text-align: center;
      font-size: 24px;
      font-weight: bold;
      background: linear-gradient(90deg, #ff6b00, #00ff9d);
      color: #000;
    }

    .card {
      margin: 20px;
      padding: 20px;
      background: #151522;
      border-radius: 15px;
      box-shadow: 0 0 20px rgba(255,107,0,.4);
    }

    .balance {
      font-size: 32px;
      color: #00ff9d;
      margin-top: 10px;
    }

    button {
      margin-top: 15px;
      width: 100%;
      padding: 15px;
      border: none;
      border-radius: 30px;
      font-size: 18px;
      background: linear-gradient(90deg, #ff6b00, #00ff9d);
      cursor: pointer;
    }
  </style>
</head>
<body>

<header>🏦 Brick Bank</header>

<div class="card">
  <h2>Баланс</h2>
  <div class="balance" id="balance">0 ₽</div>
  <button onclick="addMoney()">+100 ₽</button>
</div>

<script>
  let balance = 0;
  function addMoney() {
    balance += 100;
    document.getElementById('balance').innerText = balance + ' ₽';
  }
</script>

</body>
</html>
