<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Brick Bank</title>
<style>
:root {
    --bg-dark: #0a0a14;
    --neon-orange: #ff6b00;
    --neon-green: #00ff9d;
    --neon-pink: #ff006e;
    --text-light: #e0e0e0;
    --card-bg: #151522;
    --glow-orange: 0 0 15px rgba(255,107,0,0.7);
}

body {margin:0; font-family:sans-serif; background:var(--bg-dark); color:var(--text-light);}
header {display:flex; justify-content:space-between; align-items:center; padding:15px; background:var(--card-bg);}
header .logo {font-size:24px; font-weight:bold; color:var(--neon-orange);}
button {cursor:pointer; border:none; border-radius:10px; padding:8px 15px;}
.nav-btn {background:var(--neon-orange); color:white;}
.container {padding:20px;}
.card {background:var(--card-bg); padding:20px; border-radius:15px; margin-bottom:20px; box-shadow:var(--glow-orange);}
.btn {background:var(--neon-orange); color:white; margin-top:10px; display:inline-block;}
.modal {display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.8); justify-content:center; align-items:center;}
.modal-content {background:var(--card-bg); padding:20px; border-radius:15px; max-width:400px; width:90%; position:relative;}
.close-modal {position:absolute; top:10px; right:10px; background:none; font-size:20px; color:var(--neon-orange);}
input {width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid var(--neon-orange); background:var(--bg-dark); color:var(--text-light);}
.history-item {padding:10px; border-bottom:1px solid rgba(255,107,0,0.2);}
.clicker-btn {width:120px; height:120px; border-radius:50%; background:var(--neon-green); display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:18px; margin:10px auto; box-shadow:0 0 15px rgba(0,255,157,0.7);}
.profile {background:var(--card-bg); padding:20px; border-radius:15px; margin-bottom:20px;}
.profile img {width:80px; height:80px; border-radius:50%; margin-right:15px;}
.flex {display:flex; align-items:center;}
</style>
</head>
<body>

<header>
    <div class="logo">Brick Bank</div>
    <button class="nav-btn" id="openModalBtn">Добавить карту</button>
</header>

<div class="container">

    <!-- Профиль -->
    <div class="profile flex">
        <img src="https://via.placeholder.com/80" alt="Avatar">
        <div>
            <div style="color:var(--neon-orange); font-weight:bold;">Арон</div>
            <div id="profileBalance">Баланс: ₽0</div>
        </div>
    </div>

    <!-- Баланс -->
    <div class="card">
        <h2>Баланс</h2>
        <p id="balance">₽ 0</p>
        <button class="btn" id="addMoneyBtn">Пополнить</button>
    </div>

    <!-- Карты -->
    <div class="card">
        <h2>Карты</h2>
        <div id="cardsContainer"></div>
    </div>

    <!-- История переводов -->
    <div class="card">
        <h2>История переводов</h2>
        <div id="historyContainer"></div>
    </div>

    <!-- Кликер игра -->
    <div class="card">
        <h2>Кликер игра</h2>
        <div id="clickerCounter" style="font-size:30px; text-align:center;">0 ₽</div>
        <button class="clicker-btn" id="clickerBtn">Клик!</button>
    </div>

</div>

<!-- Модалка карты -->
<div class="modal" id="modal">
    <div class="modal-content">
        <button class="close-modal" id="closeModalBtn">&times;</button>
        <h3>Добавить карту</h3>
        <input type="text" id="cardNumberInput" placeholder="Номер карты">
        <button class="btn" id="saveCardBtn">Сохранить карту</button>
    </div>
</div>

<script>
// --- Переменные ---
const modal = document.getElementById('modal');
const openModalBtn = document.getElementById('openModalBtn');
const closeModalBtn = document.getElementById('closeModalBtn');
const saveCardBtn = document.getElementById('saveCardBtn');
const cardsContainer = document.getElementById('cardsContainer');
const addMoneyBtn = document.getElementById('addMoneyBtn');
const balanceEl = document.getElementById('balance');
const profileBalanceEl = document.getElementById('profileBalance');
const cardNumberInput = document.getElementById('cardNumberInput');
const historyContainer = document.getElementById('historyContainer');
const clickerBtn = document.getElementById('clickerBtn');
const clickerCounter = document.getElementById('clickerCounter');

let balance = 0;
let clicker = 0;

// --- Модалка ---
openModalBtn.onclick = () => modal.style.display = 'flex';
closeModalBtn.onclick = () => modal.style.display = 'none';
window.onclick = (e) => { if(e.target === modal) modal.style.display = 'none'; }

// --- Добавление карты ---
saveCardBtn.onclick = () => {
    const number = cardNumberInput.value.trim();
    if(number === '') return alert('Введите номер карты!');
    const div = document.createElement('div');
    div.textContent = 'Карта: ' + number;
    div.style.background = 'var(--card-bg)';
    div.style.padding = '10px';
    div.style.marginTop = '10px';
    div.style.borderRadius = '8px';
    cardsContainer.appendChild(div);
    cardNumberInput.value = '';
    modal.style.display = 'none';
}

// --- Пополнение баланса ---
addMoneyBtn.onclick = () => {
    const sum = prompt('Сколько добавить?');
    if(!sum || isNaN(sum)) return;
    balance += Number(sum);
    updateBalance();
    addHistory('Пополнение', Number(sum));
}

// --- История переводов ---
function addHistory(type, amount) {
    const div = document.createElement('div');
    div.className = 'history-item';
    const funnyTexts = [
        'Перевёл другу на мороженое 🍦',
        'Отправил маме на чай ☕',
        'Скинул на космическую станцию 🚀',
        'Оплатил невидимый кофе 👻',
        'Подарил банану 🍌'
    ];
    const text = funnyTexts[Math.floor(Math.random()*funnyTexts.length)];
    div.textContent = `${type}: ₽${amount} — ${text}`;
    historyContainer.prepend(div);
}

// --- Кликер игра ---
clickerBtn.onclick = () => {
    clicker += 1;
    balance += 1; // клик добавляет рубль
    clickerCounter.textContent = clicker + ' ₽';
    updateBalance();
    addHistory('Клик', 1);
}

// --- Обновление баланса на странице ---
function updateBalance() {
    balanceEl.textContent = '₽ ' + balance;
    profileBalanceEl.textContent = 'Баланс: ₽' + balance;
}
</script>

</body>
</html>
