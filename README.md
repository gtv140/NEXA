<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#111;color:#fff;overflow-x:hidden;}
.page{max-width:450px;margin:20px auto;padding:20px;border-radius:12px;background:rgba(255,255,255,0.05);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:rgba(255,255,255,0.1);color:#fff;}
button{cursor:pointer;font-weight:700;background:linear-gradient(90deg,#FFD700,#FF4500);color:#000;}
button:hover{background:linear-gradient(90deg,#FF4500,#FFD700);color:#fff;}
.hidden{display:none;}
.box{padding:12px;margin:10px 0;border-radius:12px;background:rgba(255,255,255,0.05);}
.icon-box{display:flex;justify-content:space-around;margin-top:10px;}
.icon-box div{padding:10px;background:rgba(255,255,255,0.05);border-radius:12px;text-align:center;cursor:pointer;}
img.photo{width:100%;border-radius:12px;margin:10px 0;transition:all 1s;}
.special-offer{background:linear-gradient(90deg,#FF69B4,#1E90FF);padding:12px;border-radius:12px;margin:10px 0;animation:blink 1s infinite alternate;}
@keyframes blink{0%{opacity:0.7;}100%{opacity:1;}}
</style>
</head>
<body>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<h2>Welcome <span id="dashUser"></span></h2>
<div class="box">Balance: Rs <span id="dashBalance">0</span></div>
<div class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
<div class="box">Total Profit: Rs <span id="dashTotal">0</span></div>
<div class="box">Active Members: <span id="activeMembers">0</span></div>

<div class="icon-box">
  <div onclick="showPage('plans')">📦 Plans</div>
  <div onclick="showPage('ads')">🎬 Watch Ads</div>
  <div onclick="showPage('deposit')">💰 Deposit</div>
  <div onclick="showPage('withdraw')">💵 Withdraw</div>
  <div onclick="showPage('about')">ℹ️ About</div>
</div>

<div id="photos">
  <img class="photo" src="https://picsum.photos/400/200?random=1">
  <img class="photo" src="https://picsum.photos/400/200?random=2">
  <img class="photo" src="https://picsum.photos/400/200?random=3">
</div>

<div id="specialOffers"></div>

<button onclick="logout()">Logout</button>
</div>

<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
<button onclick="showPage('dashboard')">Back</button>
</div>

<div id="ads" class="page hidden">
<h2>Watch Ads to Earn</h2>
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
<button onclick="showPage('dashboard')">Back</button>
</div>

<div id="withdraw" class="page hidden">
<h2>Withdraw</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
<button onclick="showPage('dashboard')">Back</button>
</div>

<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN - Trusted platform since 2022. Earn profits, watch ads, and grow your balance safely. Our support team is always ready to help.</p>
<button onclick="showPage('dashboard')">Back</button>
</div>

<script>
// Users & login
let users = JSON.parse(localStorage.getItem('nexa_users')||'{}');
let currentUser = localStorage.getItem('nexa_current')||null;

function saveUsers(){localStorage.setItem('nexa_users',JSON.stringify(users));}

function login(){
const u=document.getElementById('user').value.trim();
const p=document.getElementById('pass').value.trim();
if(!u||!p){alert('Enter username & password'); return;}
if(users[u]){
if(users[u].pass!==p){alert('Incorrect password'); return;}
}else{
users[u]={pass:p,balance:0,daily:0,total:0,ads:0};
saveUsers();
}
currentUser=u;
localStorage.setItem('nexa_current',currentUser);
updateDashboard();
}

function logout(){
currentUser=null;
localStorage.removeItem('nexa_current');
showPage('loginPage');
}

function showPage(id){
document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
document.getElementById(id).classList.remove('hidden');
}

// Dashboard update
function updateDashboard(){
if(!currentUser) return showPage('loginPage');
let userData = users[currentUser];
document.getElementById('dashUser').innerText=currentUser;
document.getElementById('dashBalance').innerText=userData.balance;
document.getElementById('dashDaily').innerText=userData.daily;
document.getElementById('dashTotal').innerText=userData.total;
document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000+100);
showSpecialOffers();
showPage('dashboard');
}

// Deposit system
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value = method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Number copied');}
function submitDeposit(){
if(!document.getElementById('depositTxId').value||!document.getElementById('depositProof').files[0]){
alert('Complete transaction ID & upload proof'); return;
}
alert('Deposit submitted! Pending approval...');
}

// Withdrawal
function submitWithdraw(){
if(!document.getElementById('withdrawAccount').value||!document.getElementById('withdrawAmount').value){alert('Complete all fields'); return;}
alert('Withdrawal requested! Pending approval...');
}

// Generate Plans
let plansList = document.getElementById('plansList');
for(let i=1;i<=50;i++){
let plan=document.createElement('div');
plan.className='box';
plan.innerHTML=`<b>Plan ${i}</b> - Amount Rs ${200*i} - Duration: ${25+i} days <button onclick="buyPlan(${200*i})">Buy Now</button>`;
plansList.appendChild(plan);
}
function buyPlan(amount){
alert('Clicked Buy Plan Rs '+amount);
showPage('deposit');
document.getElementById('depositAmount').value = amount;
}

// Ads Watch
let adsList = document.getElementById('adsList');
for(let i=1;i<=10;i++){
let ad=document.createElement('div');
ad.className='box';
ad.innerHTML=`Ad ${i} - Watch & Earn Rs ${50*i} <button onclick="watchAd(${50*i})">Watch</button>`;
adsList.appendChild(ad);
}
function watchAd(amount){
if(!currentUser){alert('Login first'); return;}
let userData = users[currentUser];
userData.balance += amount;
userData.total += amount;
userData.daily += amount;
saveUsers();
updateDashboard();
alert('Watched Ad! Earned Rs '+amount);
}

// Special Offers 24hr
function showSpecialOffers(){
let specialDiv=document.getElementById('specialOffers');
specialDiv.innerHTML='';
for(let i=1;i<=5;i++){
let offer=document.createElement('div');
offer.className='special-offer';
let endTime = Date.now() + 24*60*60*1000;
offer.innerHTML=`Special Offer ${i} - 2.4x Profit <span id="offer${i}"></span>`;
specialDiv.appendChild(offer);
(function updateOffer(id,end){
let timer = setInterval(()=>{
let diff = end-Date.now();
if(diff<=0){document.getElementById(id).innerText='Expired'; clearInterval(timer);}
else{
let h=Math.floor(diff/3600000); let m=Math.floor((diff%3600000)/60000); let s=Math.floor((diff%60000)/1000);
document.getElementById(id).innerText=`${h}h ${m}m ${s}s`;
}
},1000);
})(`offer${i}`,endTime);
}
}

// On load
if(currentUser && users[currentUser]){
updateDashboard();
}else{
showPage('loginPage');
}
</script>

</body>
</html>
