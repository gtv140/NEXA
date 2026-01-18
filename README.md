<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#ffbb00;
  --secondary:#222;
  --accent:#ff7700;
  --text:#fff;
}
body{margin:0;font-family:sans-serif;background:linear-gradient(120deg,#222,#333);color:var(--text);}
header{padding:15px;text-align:center;font-size:28px;font-weight:bold;background:var(--secondary);color:var(--primary);}
.page{max-width:480px;margin:15px auto;padding:15px;background:#111;border-radius:12px;box-shadow:0 0 15px rgba(0,0,0,0.5);}
input,select,button{width:100%;margin:8px 0;padding:10px;border-radius:8px;border:none;}
button{background:var(--primary);color:#111;font-weight:bold;cursor:pointer;transition:0.3s;}
button:hover{opacity:0.8;}
.nav{display:flex;justify-content:space-around;padding:10px;background:#111;position:fixed;bottom:0;width:100%;box-shadow:0 -3px 10px rgba(0,0,0,0.5);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;display:block;margin-bottom:4px;}
.card{background:#222;padding:10px;margin:10px 0;border-radius:10px;box-shadow:0 0 10px rgba(0,0,0,0.3);}
.card h3{margin:5px 0;}
.icons{display:flex;justify-content:space-around;margin:10px 0;}
.icon-box{background:#222;padding:10px;border-radius:12px;text-align:center;flex:1;margin:0 5px;cursor:pointer;transition:0.3s;}
.icon-box:hover{box-shadow:0 0 15px var(--primary);transform:translateY(-3px);}
.hidden{display:none;}
</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- Login / Signup -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- Dashboard -->
<div id="dashboard" class="page hidden">
  <div class="card"><strong>Username:</strong> <span id="dashUser">-</span></div>
  <div class="card"><strong>Balance:</strong> Rs <span id="dashBalance">0</span></div>
  <div class="card"><strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span></div>
  <div class="card"><strong>Total Profit:</strong> Rs <span id="dashTotal">0</span></div>
  <div class="card"><strong>Active Members:</strong> <span id="activeMembers">0</span></div>

  <!-- Dashboard Icons -->
  <div class="icons">
    <div class="icon-box" onclick="showPage('plans')">📦<br>Plans</div>
    <div class="icon-box" onclick="showPage('ads')">🎬<br>Watch Ads</div>
    <div class="icon-box" onclick="showPage('history')">📜<br>History</div>
    <div class="icon-box" onclick="showPage('invite')">👥<br>Invite Friends</div>
    <div class="icon-box" onclick="showPage('support')">🛠️<br>Support</div>
  </div>

  <button onclick="logout()">Logout</button>
</div>

<!-- Plans Page -->
<div id="plans" class="page hidden">
  <h2>Investment Plans</h2>
  <div id="plansList"></div>
</div>

<!-- Ads Page -->
<div id="ads" class="page hidden">
  <h2>Watch Ads / Earn</h2>
  <div id="adsList"></div>
</div>

<!-- History Page -->
<div id="history" class="page hidden">
  <h2>History</h2>
  <div id="historyList"></div>
</div>

<!-- Invite Page -->
<div id="invite" class="page hidden">
  <h2>Invite Friends</h2>
  <p>Share your referral link and earn bonuses!</p>
  <input id="refLink" readonly value="https://nexaearn.com/ref/12345"/>
  <button onclick="copyRef()">Copy Link</button>
</div>

<!-- Support Page -->
<div id="support" class="page hidden">
  <h2>Support</h2>
  <p>Contact us on WhatsApp or Email:</p>
  <button onclick="window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank')">WhatsApp</button>
  <button onclick="window.open('mailto:rock.earn92@gmail.com','_blank')">Email</button>
</div>

<script>
// Users
let currentUser = localStorage.getItem('nexa_user') || null;
let balance = parseFloat(localStorage.getItem('nexa_balance') || '0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily') || '0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total') || '0');
let activeMembers = Math.floor(Math.random()*5000+100);

// Show Page
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

// Login / Signup
function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u || !p){alert('Enter username & password');return;}
  currentUser = u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){balance=0;dailyProfit=0;totalProfit=0;saveData();}
  updateDashboard();
}

// Logout
function logout(){currentUser=null;localStorage.removeItem('nexa_user');location.reload();}

// Update Dashboard
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  showPage('dashboard');
}

// LocalStorage Save
function saveData(){
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
}

// Plans Data
const plansData=[];
for(let i=1;i<=50;i++){
  const invest = 200 + (i-1)*100;
  const days = 25 + i;
  const multiplier = i<=5?2.4:2.2;
  plansData.push({
    id:i,name:'Plan '+i,invest:invest,days:days,total:Math.round(invest*multiplier),multiplier:multiplier
  });
}

// Render Plans
function renderPlans(){
  const container=document.getElementById('plansList');
  container.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${p.name}</h3>
    <p>Invest: Rs ${p.invest}</p>
    <p>Days: ${p.days}</p>
    <p>Total: Rs ${p.total}</p>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
  });
}

// Buy Plan
function buyPlan(id){
  const plan = plansData.find(p=>p.id===id);
  const amt = plan.invest;
  balance += amt;
  dailyProfit += (plan.total-plan.invest)/plan.days;
  totalProfit += (plan.total-plan.invest);
  saveData();
  alert(`${plan.name} purchased! Daily profit added.`);
  updateDashboard();
}

// Ads Data
const adsData=[];
for(let i=1;i<=7;i++){
  adsData.push({id:i,name:'Ad Plan '+i,amount:500});
}
function renderAds(){
  const container=document.getElementById('adsList');
  container.innerHTML='';
  adsData.forEach(a=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<h3>${a.name}</h3><p>Cost: Rs ${a.amount}</p><button onclick="buyAd(${a.id})">Buy Ad Plan</button>`;
    container.appendChild(div);
  });
}
function buyAd(id){
  const ad = adsData.find(a=>a.id===id);
  balance += ad.amount;
  dailyProfit += ad.amount/10; // 10 days
  totalProfit += ad.amount;
  saveData();
  alert(`${ad.name} purchased! Daily profit added.`);
  updateDashboard();
}

// Invite copy
function copyRef(){navigator.clipboard.writeText(document.getElementById('refLink').value);alert('Copied!');}

// Init
if(currentUser){updateDashboard();}
renderPlans();
renderAds();
</script>
</body>
</html>
