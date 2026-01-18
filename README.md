<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body {
    margin:0;
    font-family: Arial, sans-serif;
    background:#f5f5f5;
    color:#000;
}
header {
    text-align:center;
    font-size:32px;
    padding:20px;
    font-weight:bold;
    background:linear-gradient(90deg,#4caf50,#2196f3);
    -webkit-background-clip:text;
    -webkit-text-fill-color:transparent;
}
.page { max-width:500px; margin:20px auto; padding:20px; border-radius:12px; background:#fff; box-shadow:0 4px 15px rgba(0,0,0,0.2);}
input,select,button {width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid #ccc; font-size:14px;}
button {background:#4caf50; color:#fff; font-weight:bold; cursor:pointer; transition:0.2s; }
button:hover {transform:translateY(-2px);}
.nav {position:fixed;bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px 0; background:#eee; border-top:1px solid #ccc;}
.nav div{text-align:center; cursor:pointer;}
.nav div .ico{display:block; font-size:20px; margin-bottom:4px;}
.hidden{display:none;}
.photo-box {display:flex; gap:10px; margin:10px 0;}
.photo-box img {width:48%; border-radius:12px; object-fit:cover;}
.info-box {background:#e0f7fa; padding:14px; border-radius:12px; margin-bottom:12px; font-weight:bold;}
.info-box div {margin:4px 0;}
.alert-box {background:#ffecb3; padding:10px; border-radius:10px; margin-bottom:12px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
<input id="user" placeholder="Username">
<input id="pass" placeholder="Password" type="password">
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="info-box">
<div>Username: <span id="dashUser">—</span></div>
<div>Balance: Rs <span id="dashBalance">0</span></div>
<div>Daily Profit: Rs <span id="dashDaily">0</span></div>
<div>Total Profit: Rs <span id="dashTotal">0</span></div>
<div>Active Members: <span id="activeMembers">0</span></div>
</div>

<div class="photo-box">
<img src="https://picsum.photos/200/150?random=1" alt="photo1">
<img src="https://picsum.photos/200/150?random=2" alt="photo2">
</div>
<div class="photo-box">
<img src="https://picsum.photos/200/150?random=3" alt="photo3">
<img src="https://picsum.photos/200/150?random=4" alt="photo4">
</div>

<div class="alert-box" id="randomAd">Special Offer: Earn 2.4x profit on new deposits!</div>

<button onclick="logout()">Logout</button>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex; gap:8px; align-items:center; margin-top:10px">
<input id="depositNumber" readonly style="flex:1">
<button onclick="copyDepositNumber()">Copy Number</button>
</div>
<input id="depositAmount" placeholder="Enter Amount">
<input id="depositTxId" placeholder="Transaction ID">
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- ABOUT -->
<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN is a safe digital earning platform. Deposit and track profits easily. Our support team is always available to assist.</p>
<div class="support-icon" onclick="openSupport()">
<span class="ico">🛠️</span> Support
</div>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = 0;

function showPage(id){
document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
document.getElementById(id).classList.remove('hidden');
}

function login(){
const u=document.getElementById('user').value.trim();
if(!u){alert('Enter username'); return;}
currentUser=u; localStorage.setItem('nexa_user',currentUser);
if(document.getElementById('userOption').value==='signup'){
balance=0; dailyProfit=0; totalProfit=0;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
localStorage.setItem('nexa_total',totalProfit);
}
updateDashboard();
}

function logout(){
currentUser=null; 
localStorage.removeItem('nexa_user'); 
showPage('loginPage');
}

function updateDashboard(){
document.getElementById('dashUser').innerText=currentUser;
document.getElementById('dashBalance').innerText=balance.toFixed(2);
document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
document.getElementById('bottomNav').classList.remove('hidden');
showPage('dashboard');
randomActiveMembers();
randomAds();
}

function randomActiveMembers(){
activeMembers = Math.floor(Math.random()*5000)+1;
document.getElementById('activeMembers').innerText=activeMembers;
setTimeout(randomActiveMembers,5000);
}

function randomAds(){
const ads = [
"Special Offer: Earn 2.4x profit on new deposits!",
"Limited Time: Deposit today and get bonus!",
"Exclusive: Track profits daily!",
"Hot Deal: Join NEXA EARN now!"
];
document.getElementById('randomAd').innerText = ads[Math.floor(Math.random()*ads.length)];
setTimeout(randomAds,7000);
}

function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}

function submitDeposit(){ 
alert('Deposit submitted. Admin will verify.');
balance += parseFloat(document.getElementById('depositAmount').value)||0;
dailyProfit += (parseFloat(document.getElementById('depositAmount').value)||0)*0.02;
totalProfit += (parseFloat(document.getElementById('depositAmount').value)||0)*0.04;
localStorage.setItem('nexa_balance',balance);
localStorage.setItem('nexa_daily',dailyProfit);
localStorage.setItem('nexa_total',totalProfit);
updateDashboard();
}

function submitWithdraw(){alert('Withdrawal request submitted.');}

if(currentUser){updateDashboard();}
</script>
</body>
</html>
