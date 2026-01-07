<!DOCTYPE html><html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Brick Bank</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
:root {
    --bg-dark: #0a0a14;
    --neon-green: #00ff9d;
    --neon-purple: #9d4edd;
    --text-light: #e0e0e0;
    --card-bg: #151522;
    --glow-purple: 0 0 15px rgba(157, 78, 221, 0.7);
}
* {margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Arial,sans-serif;}
body {background: var(--bg-dark);color: var(--text-light);}
.container {max-width:1200px;margin:0 auto;padding:20px;}
header{display:flex;justify-content:space-between;align-items:center;padding:15px;border-bottom:1px solid rgba(157,78,221,0.3);}
header h1{background: linear-gradient(to right,var(--neon-purple),var(--neon-green));-webkit-background-clip:text;color:transparent;}
.nav-btns button{margin-left:10px;padding:8px 12px;border:none;border-radius:8px;background:var(--neon-purple);color:white;cursor:pointer;transition:0.3s;}
.nav-btns button:hover{opacity:0.8;}
.main{display:flex;gap:20px;margin-top:20px;}
.sidebar{width:200px;flex-shrink:0;}
.sidebar button{width:100%;padding:12px;border:none;border-radius:8px;margin-bottom:8px;background:rgba(157,78,221,0.2);color:var(--text-light);cursor:pointer;transition:0.3s;text-align:left;}
.sidebar button.active{background:linear-gradient(90deg, var(--neon-purple), var(--neon-green));color:white;}
.sidebar button:hover{opacity:0.8;}
.content{flex:1;}
.section{display:none;}
.section.active{display:block;}
.card{background:var(--card-bg);padding:20px;border-radius:15px;margin-bottom:20px;position:relative;}
.card h2{margin-bottom:15px;color:var(--neon-purple);}
.balance-amount{font-size:32px;font-weight:bold;color:var(--neon-purple);}
.virtual-card{background:linear-gradient(135deg, rgba(157,78,221,0.5), rgba(0,255,157,0.3));padding:15px;border-radius:15px;margin-bottom:15px;}
.card-number{font-family:'Courier New',monospace;letter-spacing:2px;color:white;}
.clicker-btn{padding:30px;border-radius:50%;border:none;font-size:20px;font-weight:bold;background:linear-gradient(135deg, var(--neon-purple), var(--neon-green));color:white;cursor:pointer;transition:0.2s;}
.clicker-btn:active{transform:scale(0.95);}
.history-table{width:100%;border-collapse:collapse;}
.history-table th, .history-table td{padding:10px;border-bottom:1px solid rgba(255,255,255,0.1);}
.notification{position:fixed;top:20px;right:20px;background:linear-gradient(135deg, var(--neon-purple), var(--neon-green));padding:15px;border-radius:10px;color:white;display:none;box-shadow:var(--glow-purple);}
.form-group{margin-bottom:15px;}
.form-group label{display:block;margin-bottom:5px;}
.form-group input{width:100%;padding:8px;border-radius:8px;border:1px solid rgba(157,78,221,0.3);background:rgba(0,0,0,0.3);color:white;}
.btn{padding:10px 20px;border:none;border-radius:8px;background:var(--neon-purple);color:white;cursor:pointer;}
</style>
</head>
<body>
<header>
<h1>Brick Bank</h1>
<div class="nav-btns">
<button id="notifyBtn">🔔 Уведомление</button>
</div>
</header>
<div class="main container">
<div class="sidebar">
<button class="active" data-section="balance">Баланс</button>
<button data-section="cards">Карты</button>
<button data-section="transfer">Переводы</button>
<button data-section="clicker">Кликер</button>
<button data-section="history">История</button>
<button data-section="profile">Профиль</button>
</div>
<div class="content">
<div id="balance" class="section active">
<div class="card">
<h2>Баланс 🧱</h2>
<div class="balance-amount" id="balanceAmount">100 🧱</div>
</div>
</div>
<div id="cards" class="section">
<div class="card">
<h2>Ваши карты</h2>
<div id="cardsList"></div>
<button class="btn" id="addCardBtn">Добавить карту</button>
</div>
</div>
<div id="transfer" class="section">
<div class="card">
<h2>Перевести 🧱</h2>
<div class="form-group">
<label>Карта получателя</label>
<input type="text" id="recipientCard" placeholder="0000 0000 0000 0000">
</div>
<div class="form-group">
<label>Сумма 🧱</label>
<input type="number" id="transferAmount" placeholder="Сколько 🧱">
</div>
<button class="btn" id="sendBtn">Отправить</button>
</div>
</div>
<div id="clicker" class="section">
<div class="card" style="text-align:center;">
<h2>Кликер 🧱</h2>
<div id="clickerCount">0 🧱</div>
<button class="clicker-btn" id="clickerBtn">Клик</button>
</div>
</div>
<div id="history" class="section">
<div class="card">
<h2>История переводов</h2>
<table class="history-table" id="historyTable">
<tr><th>Карта</th><th>Сумма 🧱</th><th>Тип</th></tr>
</table>
</div>
</div>
<div id="profile" class="section">
<div class="card">
<h2>Профиль</h2>
<p>Имя: <span id="profileName">Aron</span></p>
<p>Email: <span id="profileEmail">aron@mail.com</span></p>
<p>Баланс: <span id="profileBalance">100 🧱</span></p>
</div>
</div>
</div>
</div>
<div class="notification" id="notification">Уведомление!</div>
<script>
// Меню
const sidebarBtns = document.querySelectorAll('.sidebar button');
const sections = document.querySelectorAll('.section');
sidebarBtns.forEach(btn=>{
  btn.addEventListener('click',()=>{
    sidebarBtns.forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    sections.forEach(sec=>sec.classList.remove('active'));
    document.getElementById(btn.dataset.section).classList.add('active');
  });
});// Кликер let clickerCount=0; const clickerBtn=document.getElementById('clickerBtn'); const clickerDisplay=document.getElementById('clickerCount'); clickerBtn.addEventListener('click',()=>{ clickerCount++; clickerDisplay.textContent=clickerCount+' 🧱'; });

// Баланс let balance=100; const balanceAmount=document.getElementById('balanceAmount'); const profileBalance=document.getElementById('profileBalance'); function updateBalance(){ balanceAmount.textContent=balance+' 🧱'; profileBalance.textContent=balance+' 🧱'; }

// Карты let cards=[]; const cardsList=document.getElementById('cardsList'); document.getElementById('addCardBtn').addEventListener('click',()=>{ const cardNum=Math.floor(1000+Math.random()*9000)+' '+Math.floor(1000+Math.random()*9000)+' '+Math.floor(1000+Math.random()*9000)+' '+Math.floor(1000+Math.random()*9000); cards.push(cardNum); renderCards(); }); function renderCards(){ cardsList.innerHTML=''; cards.forEach(c=>{ const div=document.createElement('div'); div.className='virtual-card'; div.innerHTML='<div class="card-number">'+c+'</div>'; cardsList.appendChild(div); }); }

// Переводы document.getElementById('sendBtn').addEventListener('click',()=>{ const recipient=document.getElementById('recipientCard').value; const amount=parseInt(document.getElementById('transferAmount').value); if(!recipient||!amount||amount>balance){showNotification('Ошибка перевода');return;} balance-=amount; updateBalance(); const table=document.getElementById('historyTable'); const tr=document.createElement('tr'); tr.innerHTML='<td>'+recipient+'</td><td>'+amount+' 🧱</td><td>Отправка</td>'; table.appendChild(tr); showNotification('Перевод выполнен!'); });

// Уведомления const notification=document.getElementById('notification'); function showNotification(msg){ notification.textContent=msg; notification.style.display='block'; setTimeout(()=>{notification.style.display='none';},3000); } document.getElementById('notifyBtn').addEventListener('click',()=>showNotification('Это тест уведомления'));

updateBalance(); </script>

</body>
</html>
