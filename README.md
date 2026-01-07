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

.container {
  padding: 20px;
}

.card {
  background: #151522;
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 0 25px rgba(255,107,0,.4);
  margin-bottom: 20px;
}

.balance {
  font-size: 36px;
  color: #00ff9d;
  margin: 10px 0;
}

button {
  width: 100%;
  padding: 15px;
  border: none;
  border-radius: 30px;
  font-size: 18px;
  background: linear-gradient(90deg, #ff6b00, #00ff9d);
  cursor: pointer;
  margin-top: 10px;
}

input {
  width: 100%;
  padding: 14px;
  border-radius: 20px;
  border: none;
  margin-top: 10px;
  font-size: 16px;
}

.bank-card {
  height: 180px;
  border-radius: 20px;
  background: linear-gradient(45deg, #ff6b00, #00ff9d);
  color: #000;
  padding: 20px;
  position: relative;
}

.card-number {
  font-size: 20px;
  letter-spacing: 3px;
  margin-top: 40px;
}

.card-name {
  position: absolute;
  bottom: 20px;
  left: 20px;
  font-weight: bold;
}
</style>
</head>

<body>

<header>🏦 Brick Bank</header>

<div class="container">

<div class="card" id="loginBlock">
  <h2>Вход</h2>
  <input id="usernameInput" placeholder="Введите имя">
  <button onclick="login()">Войти</button>
</div>

<div class="card" id="bankBlock" style="display:none;">
  <h2 id="hello"></h2>

  <div class="bank-card">
    <div>VIRTUAL CARD</div>
    <div class="card-number">4000 1234 5678 9999</div>
    <div class="card-name" id="cardName"></div>
  </div>

  <h3>Баланс</h3>
  <div class="balance" id="balance">0 ₽</div>

  <button onclick="addMoney()">+100 ₽</button>
  <button onclick="reset()">Сбросить</button>
</div>

</div>

<script>
let balance = localStorage.getItem('balance') || 0;
let username = localStorage.getItem('username');

document.getElementById('balance').innerText = balance + ' ₽';

if (username) {
  showBank();
}

function login() {
  username = document.getElementById('usernameInput').value;
  if (!username) return;
  localStorage.setItem('username', username);
  showBank();
}

function showBank() {
  document.getElementById('loginBlock').style.display = 'none';
  document.getElementById('bankBlock').style.display = 'block';
  document.getElementById('hello').innerText = 'Привет, ' + username;
  document.getElementById('cardName').innerText = username.toUpperCase();
}

function addMoney() {
  balance = Number(balance) + 100;
  localStorage.setItem('balance', balance);
  document.getElementById('balance').innerText = balance + ' ₽';
}

function reset() {
  localStorage.clear();
  location.reload();
}
</script>

</body>
</html>
/* Для телефонов */
@media (max-width: 768px) {
    .main-layout {
        flex-direction: column; /* Боковая панель вниз */
        gap: 20px;
    }

    .sidebar {
        width: 100%;
        height: auto;
        position: relative;
        top: 0;
    }

    .cards-grid {
        grid-template-columns: 1fr; /* Карточки в одну колонку */
    }

    .balance-display {
        grid-template-columns: 1fr; /* Баланс тоже в одну колонку */
    }

    header .header-content {
        flex-direction: column;
        gap: 15px;
    }

    .nav-links {
        flex-wrap: wrap;
        justify-content: center;
    }
}
