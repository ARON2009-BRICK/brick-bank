<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Brick Bank</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
:root {
    --bg-dark: #0a0a14;
    --neon-orange: #ff6b00;
    --neon-green: #00ff9d;
    --text-light: #e0e0e0;
    --card-bg: #151522;
    --glow-orange: 0 0 15px rgba(255,107,0,0.7);
}

* {margin:0; padding:0; box-sizing:border-box; font-family:'Segoe UI',sans-serif;}

body {
    background-color: var(--bg-dark);
    color: var(--text-light);
}

header {
    display:flex; justify-content:space-between; align-items:center;
    padding:15px 20px; background-color: var(--card-bg); border-bottom:2px solid var(--neon-orange);
}

header .logo {
    font-size:24px; font-weight:bold; color: var(--neon-orange);
}

.nav-btn {background: var(--neon-orange); border:none; padding:8px 15px; border-radius:10px; color:white; cursor:pointer;}

.container {padding:20px;}

.card {
    background-color: var(--card-bg); padding:20px; border-radius:15px;
    margin-bottom:20px; box-shadow: var(--glow-orange); transition:0.3s;
}

.card:hover {transform: translateY(-5px);}

.btn {background: var(--neon-orange); color:white; border:none; padding:10px 15px; border-radius:10px; cursor:pointer; margin-top:10px;}

.modal {
    display:none; position:fixed; top:0; left:0; width:100%; height:100%;
    background: rgba(0,0,0,0.8); justify-content:center; align-items:center;
}

.modal-content {
    background: var(--card-bg); padding:20px; border-radius:15px; max-width:400px; width:90%;
    position:relative;
}

.close-modal {
    position:absolute; top:10px; right:10px; background:none; border:none; font-size:20px; color: var(--neon-orange); cursor:pointer;
}
</style>
</head>
<body>

<header>
    <div class="logo">Brick Bank</div>
    <button class="nav-btn" id="open-add-card">Добавить карту</button>
</header>

<div class="container">
    <div class="card">
        <h2>Баланс</h2>
        <p id="balance">₽ 0</p>
        <button class="btn" id="add-money-btn">Пополнить</button>
    </div>

    <div class="card">
        <h2>Карты</h2>
        <div id="cards-container">
            <!-- Карты появятся здесь -->
        </div>
    </div>
</div>

<!-- Модалка -->
<div class="modal" id="modal">
    <div class="modal-content">
        <button class="close-modal" id="close-modal">&times;</button>
        <h3>Добавить карту</h3>
        <input type="text" id="card-number" placeholder="Номер карты">
        <button class="btn" id="save-card">Сохранить карту</button>
    </div>
</div>

<script>
// Элементы
const modal = document.getElementById('modal');
const openAddCardBtn = document.getElementById('open-add-card');
const closeModalBtn = document.getElementById('close-modal');
const saveCardBtn = document.getElementById('save-card');
const cardsContainer = document.getElementById('cards-container');
const addMoneyBtn = document.getElementById('add-money-btn');
const balanceEl = document.getElementById('balance');

let balance = 0;

// Открытие модалки
openAddCardBtn.addEventListener('click', () => {
    modal.style.display = 'flex';
});

// Закрытие модалки
closeModalBtn.addEventListener('click', () => {
    modal.style.display = 'none';
});

// Сохранение карты
saveCardBtn.addEventListener('click', () => {
    const cardNumber = document.getElementById('card-number').value;
    if(cardNumber.trim() === '') return alert('Введите номер карты!');
    
    const cardEl = document.createElement('div');
    cardEl.classList.add('card');
    cardEl.textContent = 'Карта: ' + cardNumber;
    cardsContainer.appendChild(cardEl);
    
    document.getElementById('card-number').value = '';
    modal.style.display = 'none';
});

// Пополнение баланса
addMoneyBtn.addEventListener('click', () => {
    const amount = prompt('Сколько добавить?');
    if(!amount || isNaN(amount)) return;
    balance += Number(amount);
    balanceEl.textContent = '₽ ' + balance;
});

// Закрытие модалки при клике вне контента
window.addEventListener('click', (e) => {
    if(e.target === modal) modal.style.display = 'none';
});
</script>

</body>
</html>
