<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --primary:#4a90e2;
  --secondary:#50e3c2;
  --bg:#0e0e0e;
  --card:#1c1c1c;
  --text:#ffffff;
}
body{
  margin:0;
  font-family: Arial, sans-serif;
  background: var(--bg);
  color: var(--text);
  overflow-x:hidden;
}
header{
  text-align:center;
  font-size:32px;
  font-weight:700;
  padding:20px;
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.page{
  max-width:480px;
  margin:20px auto;
  padding:20px;
  background: var(--card);
  border-radius:12px;
}
input, select, button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:none;
  background:#222;
  color:#fff;
}
button{
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  font-weight:700;
  cursor:pointer;
}
button:hover{opacity:0.9;}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:12px 0;
  background:#111;
}
.nav div{text-align:center; cursor:pointer;}
.nav div .ico{font-size:20px; display:block;}
.hidden{display:none;}
.plan-box{
  background:#222;
  margin:10px 0;
  padding:12px;
  border-radius:10px;
}
.plan-box b{font-size:16px;}
.plan-box .small{font-size:13px; color:#aaa;}
.countdown{color:#50e3c2; font-weight:700;}
.img-grid{display:flex; gap:8px; flex-wrap:wrap;}
.img-grid img{width:48%; border-radius:10px;}
.user-box{
  background:#222;
  padding:12px;
  border-radius:10px;
  display:flex;
  justify-content:space-between;
  align-items:center;
}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="user-box">
    <div>
      <div id="dashUser">—</div>
      <div class="small">Balance: Rs <span id="dashBalance">0</span></div>
      <div class="small">Daily Profit: Rs <span id="dashDaily">0</span></div>
      <div class="small">Active Members: <span id="activeMembers">0</span></div>
    </div>
  </div>
  <h3>About NEXA EARN</h3>
  <p>NEXA EARN is a digital investment platform offering fast, secure, and reliable profit opportunities. Join thousands of members and grow your earnings daily.</p>
  <div class="img-grid">
    <img src="https://via.placeholder.com/200x120?text=Photo1" />
    <img src="https://via.placeholder.com/200x120?text=Photo2" />
    <img src="https://via.placeholder.com/200x120?text=Photo3" />
    <img src="https://via.placeholder.com/200x120?text=Photo4" />
  </div>
  <h3>Plans</h3>
  <div id="plansList"></div>
  <button onclick="logout()">Logout</button>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; margin-top:10px;">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Amount" />
  <input id="depositTxId" placeholder="Transaction ID" />
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let activeMembers = 0;

let plansData = [];
for(let i=1;i<=50;i++){
  let invest=200 + (i-1)*100;
  let days=25 + Math.floor(i/2);
  let multiplier = i<=5 ? 2.4 : 2.2;
  let special = i<=5;
  let endTime = special ? Date.now()+24*60*60*1000 : 0;
  plansData.push({id:i, name:`Plan ${i}`, invest, days, multiplier, special, endTime, daily:Math.round((invest*multiplier)/days), total:Math.round(invest*multiplier)});
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  balance = 0;
  dailyProfit = 0;
  localStorage.setItem('nexa_balance', balance);
  localStorage.setItem('nexa_daily', dailyProfit);
  localStorage.setItem('nexa_userPlans', JSON.stringify(userPlans));
  updateDashboard();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('nexa_user');
  location.reload();
}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('activeMembers').innerText = Math.floor(Math.random()*5000);
  renderPlans();
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

function renderPlans(){
  const list = document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    let countdownHTML='';
    if(p.special){
      countdownHTML=`<div class="small countdown" id="countdown${p.id}"></div>`;
      startCountdown(p.id, p.endTime);
    }
    div.innerHTML=`<b>${p.name}</b>
      <div class="small">Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
      ${countdownHTML}
      <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function startCountdown(id,endTime){
  function tick(){
    let now = Date.now();
    let diff = endTime - now;
    if(diff<0){ document.getElementById(`countdown${id}`).innerText='Offer ended'; return;}
    let h = Math.floor(diff/3600000);
    let m = Math.floor((diff%3600000)/60000);
    let s = Math.floor((diff%60000)/1000);
    document.getElementById(`countdown${id}`).innerText=`Offer ends in ${h}h ${m}m ${s}s`;
    setTimeout(tick,1000);
  }
  tick();
}

function buyNow(id){
  const plan = plansData.find(p=>p.id===id);
  if(balance < plan.invest){alert('Insufficient balance'); return;}
  balance -= plan.invest;
  dailyProfit += plan.daily;
  userPlans.push(plan);
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
  alert(`You bought ${plan.name} for Rs ${plan.invest}`);
}

// Deposit functions
function updateDepositNumber(){
  const method = document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){
  navigator.clipboard.writeText(document.getElementById('depositNumber').value);
  alert('Number copied!');
}
function submitDeposit(){alert('Deposit submitted!');}

// Active members update
setInterval(()=>{
  if(currentUser) document.getElementById('activeMembers').innerText = Math.floor(Math.random()*5000);
},3000);

</script>
</body>
</html>
