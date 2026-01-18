<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --gold:#FFD700;
  --black:#111;
  --red:#FF3C38;
  --blue:#00BFFF;
  --white:#fff;
  --neon:#0ff;
}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:linear-gradient(120deg,#111,#222);
  color:var(--white);
}
header{
  padding:20px;
  text-align:center;
  font-size:28px;
  background:linear-gradient(90deg,var(--gold),var(--red));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
  text-shadow:0 0 10px var(--gold);
}
.page{
  max-width:450px;
  margin:20px auto;
  padding:20px;
  border-radius:12px;
  background:rgba(255,255,255,0.02);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:none;
  background:rgba(255,255,255,0.1);
  color:var(--white);
}
button{
  cursor:pointer;
  font-weight:700;
  color:var(--black);
  background:linear-gradient(90deg,var(--gold),var(--red));
  transition:0.3s;
  box-shadow:0 0 10px var(--gold);
}
button:hover{
  box-shadow:0 0 20px var(--gold),0 0 30px var(--red);
  transform:translateY(-2px);
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  background:rgba(0,0,0,0.85);
}
.nav div{
  text-align:center;
  cursor:pointer;
  transition:0.3s;
}
.nav div:hover{
  color:var(--gold);
  text-shadow:0 0 8px var(--gold);
}
.nav div .ico{
  font-size:20px;
  display:block;
  margin-bottom:4px;
}
.hidden{display:none;}
.box{
  padding:12px;
  margin:10px 0;
  border-radius:12px;
  background:rgba(255,255,255,0.05);
  box-shadow:0 0 8px var(--blue);
  transition:0.3s;
}
.box:hover{
  box-shadow:0 0 15px var(--blue),0 0 20px var(--red);
  transform:translateY(-2px);
}
.support-icon{
  display:flex;
  align-items:center;
  gap:6px;
  padding:10px;
  background:rgba(255,215,0,0.1);
  border-radius:10px;
  cursor:pointer;
  box-shadow:0 0 10px var(--gold);
  transition:0.3s;
}
.support-icon:hover{
  box-shadow:0 0 20px var(--gold),0 0 25px var(--red);
  transform:translateY(-2px);
}
img.random-img{
  width:100%;
  border-radius:10px;
  margin:4px 0;
  box-shadow:0 0 8px var(--blue);
  transition:0.3s;
}
img.random-img:hover{
  box-shadow:0 0 20px var(--blue),0 0 15px var(--red);
  transform:scale(1.02);
}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<div class="box">Username: <span id="dashUser"></span></div>
<div class="box">Balance: Rs <span id="dashBalance">0</span></div>
<div class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
<div class="box">Total Profit: Rs <span id="dashTotal">0</span></div>
<div class="box">Active Members: <span id="activeMembers">0</span></div>

<div class="box" id="randomPhotos"></div>

<div class="box">Welcome to NEXA EARN! Operating since 2022. Secure platform for investments & ads-based earnings with daily profits & special offers!</div>

<button onclick="showPage('plans')">View Plans</button>
<button onclick="showPage('ads')">Watch Ads</button>
<button onclick="showPage('deposit')">Deposit</button>
<button onclick="showPage('withdrawal')">Withdraw</button>
<button onclick="showPage('about')">About / Support</button>
<button onclick="logout()">Logout</button>
</div>

<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
<button onclick="showPage('dashboard')">Back</button>
</div>

<div id="ads" class="page hidden">
<h2>Watch Ads & Earn</h2>
<div id="adsList"></div>
<button onclick="showPage('dashboard')">Back</button>
</div>

<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex;gap:8px;align-items:center;margin-top:10px">
<input id="depositNumber" readonly style="flex:1"/>
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

<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>Since 2022, NEXA EARN offers fast, reliable investment & ads-earning platform. Daily profits & special offers included.</p>
<div class="support-icon" onclick="openSupport()">
<span class="ico">🛠️</span>Support
</div>
<button onclick="showPage('dashboard')">Back</button>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('ads')"><span class="ico">🎬</span>Watch Ads</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let currentPass = localStorage.getItem('nexa_pass')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');

function loadData(){
currentUser = localStorage.getItem('nexa_user')||currentUser;
currentPass = localStorage.getItem('nexa_pass')||currentPass;
balance = parseFloat(localStorage.getItem('nexa_balance')||balance);
dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||dailyProfit);
totalProfit = parseFloat(localStorage.getItem('nexa_total')||totalProfit);
}

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

function login(){
const u=document.getElementById('user').value.trim();
const p=document.getElementById('pass').value.trim();
if(!u||!p){alert('Enter username and password');return;}
currentUser=u; currentPass=p;
localStorage.setItem('nexa_user',currentUser);
localStorage.setItem('nexa_pass',currentPass);
if(!localStorage.getItem('nexa_balance')){
balance=0; dailyProfit=0; totalProfit=0;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
localStorage.setItem('nexa_total',totalProfit);
}
updateDashboard();
}

function logout(){currentUser=null; currentPass=null; localStorage.removeItem('nexa_user'); localStorage.removeItem('nexa_pass'); showPage('loginPage');}

function updateDashboard(){
loadData();
document.getElementById('dashUser').innerText=currentUser||'Guest';
document.getElementById('dashBalance').innerText=balance;
document.getElementById('dashDaily').innerText=dailyProfit;
document.getElementById('dashTotal').innerText=totalProfit;
document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000+100);
document.getElementById('bottomNav').classList.remove('hidden');
showPage('dashboard');
updateRandomPhotos();
updatePlans();
updateAds();
}

function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
let amt=parseFloat(document.getElementById('depositAmount').value)||0;
balance+=amt; dailyProfit+=amt*0.05; totalProfit+=amt*1.1;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
localStorage.setItem('nexa_total',totalProfit);
alert('Deposit submitted & balance updated!');
updateDashboard();
}
function submitWithdraw(){alert('Withdrawal requested!');}
function updateRandomPhotos(){
let container=document.getElementById('randomPhotos'); container.innerHTML='';
for(let i=0;i<4;i++){
let img=document.createElement('img'); img.src='https://picsum.photos/100/80?random='+Math.floor(Math.random()*1000); img.className='random-img';
container.appendChild(img);
}
}

let plansData=[];
for(let i=1;i<=50;i++){
plansData.push({name:'Plan '+i,price:200*i,dailyProfit:200*i*0.02,totalProfit:200*i*0.22,days:25+Math.floor(i/2),special:i%5===0});
}
function updatePlans(){
let container=document.getElementById('plansList'); container.innerHTML='';
plansData.forEach((p,index)=>{
let div=document.createElement('div'); div.className='box';
div.innerHTML=`<b>${p.name}</b> <br>Price: Rs ${p.price}<br>Daily: Rs ${p.dailyProfit.toFixed(2)}<br>Total: Rs ${p.totalProfit.toFixed(2)}<br>Days: ${p.days}${p.special?' <br>🌟 Special Offer 24h':''}<br><button onclick="buyPlan(${index})">Buy Now</button>`;
container.appendChild(div);
});
});
function buyPlan(index){let plan=plansData[index];document.getElementById('depositAmount').value=plan.price; showPage('deposit');}

let adsPlansData=[];
for(let i=1;i<=10;i++){adsPlansData.push({name:'Ads Plan '+i,price:500*i,dailyAds:3});}
function updateAds(){
let container=document.getElementById('adsList'); container.innerHTML='';
adsPlansData.forEach((p,index)=>{
let div=document.createElement('div'); div.className='box';
div.innerHTML=`<b>${p.name}</b><br>Price: Rs ${p.price}<br>Daily Ads: ${p.dailyAds}<br><button onclick="buyAds(${index})">Buy Now</button>`;
container.appendChild(div);
});
}
function buyAds(index){alert('Ads Plan Bought!');}

document.addEventListener('DOMContentLoaded',()=>{loadData(); if(currentUser){updateDashboard();} else{showPage('loginPage');}});
</script>
</body>
</html>
