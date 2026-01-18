<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f4f4f4;}
header{font-size:32px;text-align:center;padding:20px;background:#0a74da;color:#fff;font-weight:700;}
.page{max-width:500px;margin:20px auto;padding:20px;background:#fff;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin:10px 0;border-radius:8px;border:1px solid #ccc;}
button{background:#0a74da;color:#fff;font-weight:700;cursor:pointer;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;background:#fff;border-top:1px solid #ccc;padding:10px 0;}
.nav div{text-align:center;cursor:pointer;}
.hidden{display:none;}
.plan-box{border:1px solid #0a74da;border-radius:10px;padding:12px;margin:10px 0;display:flex;justify-content:space-between;align-items:center;transition:0.2s;}
.plan-box:hover{box-shadow:0 8px 20px rgba(0,0,0,0.2);transform:translateY(-3px);}
.box{border-radius:12px;padding:15px;margin:10px 0;box-shadow:0 4px 15px rgba(0,0,0,0.1);}
.box h3{margin:0 0 10px 0;}
.company-img{width:100%;border-radius:12px;margin:10px 0;}
.ad-box{background:#ffdd57;color:#111;padding:12px;border-radius:10px;margin:10px 0;text-align:center;font-weight:700;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username">
  <input id="pass" placeholder="Password" type="password">
  <button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="box"><h3>Welcome, <span id="dashUser"></span></h3></div>
  <div class="box">Balance: Rs <span id="dashBalance">0</span></div>
  <div class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div class="box">Active Members: <span id="activeMembers">0</span></div>

  <div id="adsContainer"></div>

  <h3>Company Highlights</h3>
  <p>NEXA EARN helps you grow your funds quickly and safely. Trusted by thousands of users.</p>
  <div id="companyPhotos"></div>

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
  <div style="display:flex;gap:8px;align-items:center">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Amount">
  <input id="depositTxId" placeholder="Transaction ID">
  <input type="file" id="depositProof">
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

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

<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN is a trusted digital investment platform helping users grow funds safely.</p>
  <div class="support-icon" onclick="openSupport()">🛠️ Support</div>
</div>

<div class="nav hidden" id="bottomNav">
  <div onclick="showPage('dashboard')">🏠 Home</div>
  <div onclick="showPage('deposit')">💰 Deposit</div>
  <div onclick="showPage('withdrawal')">💵 Withdraw</div>
  <div onclick="showPage('about')">ℹ️ About</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user');
let balance = parseFloat(localStorage.getItem('nexa_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')) || 0;
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')) || [];

const plansData=[];
for(let i=1;i<=50;i++){
  let invest=200 + (i-1)*100;
  let days=25 + Math.floor(i/5)*5;
  let multiplier=i<=5?2.4:2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days)});
}

const ads=["🔥 Special Offer! 2.4x Profit on Plan 1-5!","💥 Limited Time Offer: Invest now!","⚡ Join 5000+ active members!"];
const photos=[
"https://via.placeholder.com/400x200?text=Photo+1",
"https://via.placeholder.com/400x200?text=Photo+2",
"https://via.placeholder.com/400x200?text=Photo+3",
"https://via.placeholder.com/400x200?text=Photo+4",
"https://via.placeholder.com/400x200?text=Photo+5"
];

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){
    balance=0; dailyProfit=0; userPlans=[];
    localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  }
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  updateDashboard();
}

function logout(){ localStorage.removeItem('nexa_user'); location.reload(); }

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000)+1;

  // Ads
  const adsDiv=document.getElementById('adsContainer'); adsDiv.innerHTML='';
  const randAd=ads[Math.floor(Math.random()*ads.length)];
  adsDiv.innerHTML=`<div class="ad-box">${randAd}</div>`;

  // Company Photos
  const photoDiv=document.getElementById('companyPhotos'); photoDiv.innerHTML='';
  const shuffled=photos.sort(()=>0.5-Math.random()).slice(0,3);
  shuffled.forEach(p=>{
    const img=document.createElement('img'); img.src=p; img.className='company-img';
    photoDiv.appendChild(img);
  });

  renderPlans();
}

function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div class='meta'><b>${p.name}</b><div>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div></div>
    <button onclick='buyNow(${p.id})'>Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyNow(id){
  const plan=plansData.find(p=>p.id===id);
  if(balance<plan.invest){alert('Insufficient balance'); return;}
  balance-=plan.invest; dailyProfit+=plan.daily;
  userPlans.push(plan);
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  alert(`Purchased ${plan.name}. Go to Deposit to complete payment.`);
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){ navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied'); }
function submitDeposit(){ alert('Deposit submitted'); }
function submitWithdraw(){ alert('Withdrawal requested'); }
function openSupport(){ window.open('https://chat.whatsapp.com/Example','_blank'); }

window.onload=function(){ if(currentUser){document.getElementById('loginPage').classList.add('hidden');document.getElementById('bottomNav').classList.remove('hidden');updateDashboard();} }
</script>
</body>
</html>
