<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#ff0044;
  --secondary:#00fff7;
  --accent:#ffcc00;
  --bg:#111;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:#fff;overflow-x:hidden;}
header{text-align:center;font-size:28px;padding:20px;background:linear-gradient(90deg,var(--primary),var(--secondary));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;border-radius:12px;background:rgba(255,255,255,0.03);border:1px solid rgba(255,204,0,0.2);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:rgba(255,255,255,0.05);color:#fff;}
button{background:linear-gradient(90deg,var(--primary),var(--accent));font-weight:700;cursor:pointer;transition:0.3s;}
button:hover{transform:scale(1.05);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.8);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{padding:15px;margin:10px 0;border-radius:12px;border:1px solid rgba(255,204,0,0.2);background:linear-gradient(145deg,rgba(255,0,68,0.1),rgba(0,255,247,0.1));transition:0.3s;}
.box:hover{box-shadow:0 4px 20px rgba(255,204,0,0.5);transform:scale(1.02);}
.icon-box{display:flex;flex-wrap:wrap;gap:10px;margin:15px 0;}
.icon-item{flex:1 1 45%;background:rgba(255,255,255,0.05);border-radius:12px;padding:15px;text-align:center;cursor:pointer;border:1px solid rgba(255,204,0,0.2);transition:0.3s;}
.icon-item:hover{box-shadow:0 4px 15px rgba(255,204,0,0.3);transform:scale(1.05);}
img{width:100%;border-radius:12px;margin-top:10px;}
.carousel{position:relative;width:100%;height:200px;overflow:hidden;border-radius:12px;margin:10px 0;}
.carousel img{width:100%;height:200px;object-fit:cover;position:absolute;top:0;left:0;opacity:0;transition:opacity 1s;}
.carousel img.active{opacity:1;}
.progress-bar{background:rgba(255,255,255,0.05);border-radius:12px;overflow:hidden;margin-top:10px;}
.progress-bar-inner{height:20px;width:0%;background:linear-gradient(90deg,var(--primary),var(--accent));transition:0.3s;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="box" id="userInfo">
<strong>Username:</strong> <span id="dashUser">-</span><br>
<strong>Balance:</strong> Rs <span id="dashBalance">0</span><br>
<strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span><br>
<strong>Total Profit:</strong> Rs <span id="dashTotal">0</span><br>
<strong>Active Members:</strong> <span id="activeMembers">0</span>
</div>

<div class="box" id="companyInfo">
<h3>About NEXA EARN</h3>
<p>Since 2022, NEXA EARN provides digital investment & ad-earning opportunities. Watch ads or buy plans to earn daily rewards!</p>
<div class="carousel">
<img src="https://picsum.photos/400/200?random=1" class="active"/>
<img src="https://picsum.photos/400/200?random=2"/>
<img src="https://picsum.photos/400/200?random=3"/>
<img src="https://picsum.photos/400/200?random=4"/>
</div>
</div>

<div class="icon-box">
<div class="icon-item" onclick="showPage('plans')">📦<br>Plans</div>
<div class="icon-item" onclick="showPage('ads')">🎬<br>Watch Ads</div>
<div class="icon-item" onclick="showPage('deposit')">💰<br>Deposit</div>
<div class="icon-item" onclick="showPage('withdrawal')">💵<br>Withdraw</div>
<div class="icon-item" onclick="showPage('history')">🕒<br>History</div>
<div class="icon-item" onclick="showPage('support')">🛠️<br>Support</div>
<div class="icon-item" onclick="invite()">👥<br>Invite</div>
</div>
<button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h2>Investment Plans</h2>
<div id="plansList"></div>
</div>

<!-- ADS WATCH -->
<div id="ads" class="page hidden">
<h2>Watch Ads</h2>
<div id="adsList"></div>
<div class="progress-bar"><div class="progress-bar-inner" id="adsProgress"></div></div>
<button onclick="completeAd()">Complete Ad</button>
</div>

<!-- DEPOSIT -->
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

<!-- WITHDRAWAL -->
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

<!-- HISTORY -->
<div id="history" class="page hidden">
<h2>History</h2>
<div id="historyList"></div>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
<h2>Support</h2>
<p>Contact via WhatsApp: <a href="https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu" target="_blank">Join Group</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let adsProgress=0;

// Carousel
let carouselIndex=0;
function slideImages(){
const imgs=document.querySelectorAll('.carousel img');
imgs.forEach(img=>img.classList.remove('active'));
carouselIndex=(carouselIndex+1)%imgs.length;
imgs[carouselIndex].classList.add('active');
}
setInterval(slideImages,3000);

function updateDashboard(){
document.getElementById('dashUser').innerText=currentUser||'-';
document.getElementById('dashBalance').innerText=balance.toFixed(2);
document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000)+100;
showPage('dashboard');
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
if(!localStorage.getItem('nexa_balance')) localStorage.setItem('nexa_balance',0);
if(!localStorage.getItem('nexa_daily')) localStorage.setItem('nexa_daily',0);
if(!localStorage.getItem('nexa_total')) localStorage.setItem('nexa_total',0);
balance=parseFloat(localStorage.getItem('nexa_balance'));
dailyProfit=parseFloat(localStorage.getItem('nexa_daily'));
totalProfit=parseFloat(localStorage.getItem('nexa_total'));
updateDashboard();
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}

function submitDeposit(){
const amount=parseFloat(document.getElementById('depositAmount').value);
if(!amount){alert('Enter amount');return;}
balance+=amount;
totalProfit+=amount*0.1;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_total',totalProfit);
alert('Deposit submitted');
updateDashboard();
}

function submitWithdraw(){
const amount=parseFloat(document.getElementById('withdrawAmount').value);
if(!amount||amount>balance){alert('Invalid amount');return;}
balance-=amount;
localStorage.setItem('nexa_balance',balance);
alert('Withdrawal requested');
updateDashboard();
}

function invite(){prompt('Share code with friends: NEXA123');}

// Plans
let plans=[];
for(let i=1;i<=50;i++){
plans.push({id:i,name:'Plan '+i,invest:200+i*50,days:25+Math.floor(i*2),multiplier:i<=5?2.4:2.2,special:i<=5});
}
function showPlans(){
let html='';
plans.forEach(p=>{
html+=`<div class="box"><strong>${p.name} ${p.special?'[Special Offer]':''}</strong><br>Invest: Rs ${p.invest}<br>Days: ${p.days}<br>Total: Rs ${Math.round(p.invest*p.multiplier)}<br><button onclick="buyPlan(${p.id})">Buy Now</button></div>`;
});
document.getElementById('plansList').innerHTML=html;
}
function buyPlan(id){
const plan=plans.find(p=>p.id===id);
document.getElementById('depositAmount').value=plan.invest;
showPage('deposit');
}

// Ads
let adsPlans=[];
for(let i=1;i<=7;i++){
adsPlans.push({id:i,name:'Ads Plan '+i, invest:500+i*50, days:10});
}
function showAds(){
let html='';
adsPlans.forEach(a=>{
html+=`<div class="box"><strong>${a.name}</strong><br>Invest: Rs ${a.invest}<br>Days: ${a.days}<br><button onclick="buyAds(${a.id})">Buy Now</button></div>`;
});
document.getElementById('adsList').innerHTML=html;
}
function buyAds(id){
const ad=adsPlans.find(a=>a.id===id);
document.getElementById('depositAmount').value=ad.invest;
showPage('deposit');
}

// Ads watch progress
function completeAd(){
adsProgress+=10;
if(adsProgress>100) adsProgress=100;
document.getElementById('adsProgress').style.width=adsProgress+'%';
if(adsProgress>=100){
alert('Daily ads completed! Profit added.');
dailyProfit+=50; 
balance+=50;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
updateDashboard();
adsProgress=0;
document.getElementById('adsProgress').style.width='0%';
}
}

// Init
if(currentUser) updateDashboard();
showPlans();
showAds();
</script>
</body>
</html>
