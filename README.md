<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --neon-blue: #00f7ff;
  --neon-red: #ff073a;
  --neon-purple: #c700ff;
  --dark-bg: #0f0f0f;
  --box-bg: rgba(255,255,255,0.05);
}
body {
  margin:0;
  font-family: Arial, sans-serif;
  background: var(--dark-bg);
  color: #fff;
  overflow-x: hidden;
}
header {
  text-align:center;
  font-size:28px;
  padding:20px;
  background: linear-gradient(90deg, var(--neon-blue), var(--neon-red), var(--neon-purple));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.page { max-width:450px; margin:20px auto; padding:20px; background: var(--box-bg); border-radius:12px; border:1px solid rgba(0,255,255,0.06);}
input, select, button { width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid rgba(0,255,240,0.08); background:transparent; color:#e6f7fb;}
button { background: linear-gradient(90deg, var(--neon-blue), var(--neon-red)); color:#001; font-weight:700; cursor:pointer; transition:0.3s; }
button:hover{ transform:translateY(-2px);}
.nav { position: fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px 0; background: rgba(0,0,0,0.8);}
.nav div { text-align:center; cursor:pointer;}
.nav div .ico { font-size:20px; display:block; margin-bottom:4px;}
.hidden { display:none;}
.box { background: rgba(0,0,0,0.2); margin:10px 0; padding:10px; border-radius:10px; transition:0.3s; }
.box:hover { box-shadow: 0 0 20px rgba(255,0,0,0.4); transform: translateY(-2px);}
.plan-card{background: rgba(255,255,255,0.05); border-radius:12px; padding:12px; margin:8px 0; cursor:pointer; transition:0.3s;}
.plan-card:hover{box-shadow:0 0 20px rgba(0,255,255,0.4);}
.countdown{font-weight:700; color: var(--neon-red);}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="box">Username: <span id="dashUser"></span></div>
  <div class="box">Balance: Rs <span id="dashBalance">0</span></div>
  <div class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div class="box">Active Members: <span id="activeMembers">0</span></div>
  <div class="box">Company Info: NEXA EARN provides fast, secure digital investments with exciting plans since 2022.</div>
  <div id="photosContainer" class="box"></div>
  <div id="plansList" class="box"></div>
  <button onclick="logout()">Logout</button>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount" />
  <input id="depositTxId" placeholder="Transaction ID" />
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN is a digital investment platform providing secure and profitable investment plans. Our support team is ready 24/7.</p>
  <div class="box" onclick="openSupport()">🛠️ Support</div>
</div>

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plansList')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');

const plansData = [];
for(let i=1;i<=50;i++){
  const invest = 200 + (i-1)*100;
  const days = 25 + (i-1);
  const multiplier = i<=5?2.4:2.2;
  plansData.push({id:i, name:`Plan ${i}`, invest:invest, days:days, total:Math.round(invest*multiplier), multiplier, offer:i<=5});
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  if(id==='plansList') renderPlans();
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0;
  localStorage.setItem('nexa_balance', balance);
  localStorage.setItem('nexa_daily', dailyProfit);
  updateDashboard();
}

function logout(){
  currentUser=null; localStorage.removeItem('nexa_user');
  location.reload();
}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000);
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
  renderPhotos();
}

function renderPhotos(){
  const container=document.getElementById('photosContainer');
  container.innerHTML='';
  for(let i=1;i<=4;i++){
    const img=document.createElement('img');
    img.src=`https://picsum.photos/200/120?random=${Math.floor(Math.random()*1000)+i}`;
    img.style.width='100%'; img.style.marginTop='5px'; img.style.borderRadius='10px';
    container.appendChild(img);
  }
}

function renderPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-card';
    div.innerHTML=`<b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total}<br>Days: ${p.days}${p.offer?` | <span class="countdown" id="count${p.id}">24:00:00</span>`:''}<br><button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
    if(p.offer) startCountdown(`count${p.id}`,24*60*60);
  });
}

function startCountdown(id,seconds){
  const el=document.getElementById(id);
  let s=seconds;
  setInterval(()=>{
    const h=Math.floor(s/3600);
    const m=Math.floor((s%3600)/60);
    const sec=s%60;
    el.innerText=`${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${sec.toString().padStart(2,'0')}`;
    if(s>0) s--; 
  },1000);
}

function buyPlan(id){
  const plan=plansData.find(p=>p.id===id);
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted'); balance+=parseFloat(document.getElementById('depositAmount').value); dailyProfit+=balance*0.01; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); updateDashboard();}
function submitWithdraw(){alert('Withdrawal requested');}

function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}
</script>
</body>
</html>
