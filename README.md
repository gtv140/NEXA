<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --primary:#FFD700;
  --secondary:#FF4500;
  --dark:#111;
  --light:#f5f5f5;
}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:linear-gradient(120deg, #222, #444);
  color:#fff;
}
header{
  position:fixed;
  top:0;
  left:0;
  width:100%;
  text-align:center;
  padding:15px 0;
  font-size:22px;
  font-weight:bold;
  background:var(--dark);
  z-index:1000;
}
.page{
  max-width:430px;
  margin:80px auto 80px;
  padding:15px;
  background:rgba(255,255,255,0.02);
  border-radius:12px;
}
.card{
  background:rgba(255,255,255,0.05);
  margin:10px 0;
  padding:12px;
  border-radius:12px;
  border:1px solid rgba(255,215,0,0.3);
  box-shadow:0 4px 10px rgba(0,0,0,0.3);
}
button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border:none;
  border-radius:8px;
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  color:#111;
  font-weight:bold;
  cursor:pointer;
}
input,select{
  width:100%;
  padding:10px;
  margin-top:8px;
  border-radius:8px;
  border:1px solid rgba(255,215,0,0.3);
  background:rgba(255,255,255,0.05);
  color:#fff;
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  width:100%;
  display:flex;
  justify-content:space-around;
  background:rgba(0,0,0,0.9);
  padding:10px 0;
}
.nav div{
  text-align:center;
  cursor:pointer;
}
.nav div span{
  display:block;
  font-size:22px;
  margin-bottom:4px;
}
.hidden{display:none;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password" />
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="card">Username: <span id="dashUser"></span></div>
  <div class="card">Balance: Rs <span id="dashBalance">0</span></div>
  <div class="card">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div class="card">Active Members: <span id="activeMembers">0</span></div>
  <div class="card" id="companyInfo">
    <h3>About NEXA EARN</h3>
    <p>Operating since 2022, NEXA EARN provides premium earning plans, ads rewards, and daily profits. Secure & reliable.</p>
  </div>
  <div id="randomPhotos"></div>
</div>

<div id="plansPage" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<div id="adsPage" class="page hidden">
  <h2>Watch Ads</h2>
  <div id="adsList"></div>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; margin-top:10px;">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number"/>
  <input id="withdrawAmount" placeholder="Amount"/>
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div class="nav hidden">
  <div onclick="showPage('dashboard')"><span>🏠</span>Home</div>
  <div onclick="showPage('plansPage')"><span>📦</span>Plans</div>
  <div onclick="showPage('adsPage')"><span>🎬</span>Ads</div>
  <div onclick="showPage('deposit')"><span>💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span>💵</span>Withdraw</div>
</div>

<script>
let currentUser=localStorage.getItem('nexa_user')||null;
let balance=parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit=parseFloat(localStorage.getItem('nexa_daily')||'0');
let activeMembers=5000;

const plans=[];
for(let i=1;i<=50;i++){
  let invest=200 + (i-1)*100;
  let days=25 + i;
  let multiplier=i<=5?2.4:2.2;
  plans.push({id:i,name:'Plan '+i,invest,days,multiplier,total:Math.round(invest*multiplier),special:i<=5});
}

let ads = [];
for(let i=1;i<=10;i++){
  ads.push({id:i,name:'Ad '+i,profit:Math.floor(Math.random()*50+20)});
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username & password');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){balance=0; dailyProfit=0; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit);}
  updateDashboard();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('nexa_user');
  location.reload();
}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.querySelector('.nav').classList.remove('hidden');
  showPage('dashboard');
  document.getElementById('activeMembers').innerText='Active Members: '+Math.floor(Math.random()*activeMembers);
  displayRandomPhotos();
}

function displayRandomPhotos(){
  const container=document.getElementById('randomPhotos');
  container.innerHTML='';
  for(let i=0;i<5;i++){
    const img=document.createElement('img');
    img.src='https://picsum.photos/400/150?random='+Math.floor(Math.random()*1000);
    img.style.width='100%';
    img.style.margin='8px 0';
    img.style.borderRadius='12px';
    container.appendChild(img);
  }
}

function renderPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plans.forEach(p=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${p.name}${p.special?' (Special Offer)':''}</h3>
      <p>Invest: Rs ${p.invest}</p>
      <p>Total: Rs ${p.total} | Days: ${p.days}</p>
      <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
  });
}

function buyPlan(id){
  const plan=plans.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value=plan.invest;
  showPage('deposit');
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){
  navigator.clipboard.writeText(document.getElementById('depositNumber').value);
  alert('Number copied');
}

function submitDeposit(){
  const tx=document.getElementById('depositTxId').value.trim();
  const proof=document.getElementById('depositProof').files[0];
  if(!tx || !proof){alert('Enter Transaction ID & Upload Proof'); return;}
  balance+=parseFloat(document.getElementById('depositAmount').value);
  localStorage.setItem('nexa_balance',balance);
  alert('Deposit Successful!');
  updateDashboard();
}

function renderAds(){
  const container=document.getElementById('adsList');
  container.innerHTML='';
  ads.forEach(ad=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${ad.name}</h3>
      <p>Profit: Rs ${ad.profit}</p>
      <button onclick="watchAd(${ad.id})">Watch Ad</button>`;
    container.appendChild(div);
  });
}

function watchAd(id){
  const ad=ads.find(a=>a.id===id);
  if(!ad) return;
  alert('Watching Ad... 5 sec wait');
  setTimeout(()=>{
    balance+=ad.profit;
    dailyProfit+=ad.profit;
    localStorage.setItem('nexa_balance',balance);
    localStorage.setItem('nexa_daily',dailyProfit);
    updateDashboard();
    alert('Ad Completed! Profit Added');
  },5000);
}

function submitWithdraw(){
  alert('Withdrawal Requested!');
}

document.addEventListener('DOMContentLoaded',()=>{
  if(currentUser) updateDashboard();
  renderPlans();
  renderAds();
});
</script>
</body>
</html>
