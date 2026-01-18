<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --primary:#FFD700;
  --secondary:#1e1e1e;
  --accent:#FF69B4;
  --text:#fff;
}
body{
  margin:0;
  font-family:'Segoe UI',sans-serif;
  background:var(--secondary);
  color:var(--text);
  overflow-x:hidden;
}
header{
  padding:20px;
  font-size:28px;
  font-weight:700;
  text-align:center;
  background: linear-gradient(90deg,var(--primary),var(--accent));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.page{max-width:480px;margin:20px auto;padding:20px;background: rgba(255,255,255,0.05);border-radius:12px; border:1px solid rgba(255,191,0,0.2);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(255,191,0,0.2);background:transparent;color:#fff;}
button{background:linear-gradient(90deg,var(--primary),var(--accent));font-weight:700;cursor:pointer;transition:0.3s;}
button:hover{opacity:0.85;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.85);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.box{background:rgba(255,255,255,0.05);padding:15px;margin:10px 0;border-radius:12px;box-shadow:0 4px 8px rgba(0,0,0,0.3);transition:0.3s;}
.box:hover{transform:translateY(-4px);box-shadow:0 8px 20px rgba(0,0,0,0.4);}
img.dashboard-img{width:100%;border-radius:12px;margin:10px 0;transition:0.5s;}
img.dashboard-img:hover{transform:scale(1.05);}
.back-btn{margin-top:10px;background:#ff4c4c;color:#fff;}
.timer{margin-top:10px;font-weight:700;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="box">
    <strong>Username:</strong> <span id="dashUser"></span><br>
    <strong>Balance:</strong> Rs <span id="dashBalance">0</span>
  </div>
  <div class="box">
    <strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span><br>
    <strong>Active Users:</strong> <span id="dashActive">0</span>
  </div>
  <div class="box" id="companyDetails">
    <h3>About NEXA EARN</h3>
    <p>Since 2022, NEXA EARN offers safe digital investments. Earn by plans or watching ads daily.</p>
    <img class="dashboard-img" src="https://picsum.photos/400/150?random=1"/>
    <img class="dashboard-img" src="https://picsum.photos/400/150?random=2"/>
    <img class="dashboard-img" src="https://picsum.photos/400/150?random=3"/>
  </div>
  <div id="plansList"></div>
  <div id="adsList"></div>
  <button class="back-btn" onclick="logout()">Logout</button>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
  <button class="back-btn" onclick="showPage('dashboard')">Back</button>
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
  <button class="back-btn" onclick="showPage('dashboard')">Back</button>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
  <h2>Support</h2>
  <div class="box" onclick="openSupport()">🛠️ WhatsApp Support</div>
  <button class="back-btn" onclick="showPage('dashboard')">Back</button>
</div>

<!-- NAV -->
<div class="nav hidden" id="bottomNav">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plansList')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('adsList')"><span class="ico">🎬</span>Watch Ads</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('support')"><span class="ico">🛠️</span>Support</div>
</div>

<script>
// Local storage
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let activeUsers = parseInt(localStorage.getItem('nexa_active')||Math.floor(Math.random()*5000));
let adsWatchedToday = parseInt(localStorage.getItem('nexa_ads_watched')||0);

// Plans
let plansData=[];
for(let i=1;i<=50;i++){
  plansData.push({id:i,name:'Plan '+i,invest:200+(i-1)*100,days:25+Math.floor((i-1)*(50/49)),multiplier:i<=5?2.4:2.2,offer:i<=5});
}

// Ads Plans
let adsData=[];
for(let i=1;i<=10;i++){
  adsData.push({id:i,name:'Ads Plan '+i,invest:500+(i-1)*100,days:10,dailyAds:3,profit:10});
}

// Countdown timer for special offers
function startCountdown(){
  setInterval(()=>{
    const now=Date.now();
    plansData.forEach((p,i)=>{
      if(p.offer){
        if(!p.endTime) p.endTime=now+24*60*60*1000;
        const diff=p.endTime-now;
        const h=Math.floor(diff/3600000);
        const m=Math.floor((diff%3600000)/60000);
        const s=Math.floor((diff%60000)/1000);
        const timerEl=document.querySelector(`#planTimer${i}`);
        if(timerEl) timerEl.innerText=`Offer ends in ${h}h ${m}m ${s}s`;
      }
    });
  },1000);
}

// Functions
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  if(id==='plansList')renderPlans();
  if(id==='adsList')renderAds();
  if(id==='dashboard')updateDashboard();
  document.getElementById(id).classList.remove('hidden');
}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')) balance=0;
  localStorage.setItem('nexa_balance',balance);
  if(!localStorage.getItem('nexa_daily')) dailyProfit=0;
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_active',activeUsers);
  updateDashboard();
  startCountdown();
}
function logout(){currentUser=null;localStorage.removeItem('nexa_user');location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashActive').innerText=activeUsers;
  document.getElementById('bottomNav').classList.remove('hidden');
  renderAdsTask();
}

// Render Plans
function renderPlans(){
  const container=document.getElementById('plansList');container.innerHTML='';
  plansData.forEach((p,i)=>{
    const card=document.createElement('div');
    card.className='box';
    card.innerHTML=`<h3>${p.name} ${p.offer?'🔥Special Offer':''}</h3>
    <p>Invest: Rs ${p.invest}</p>
    <p>Days: ${p.days}</p>
    <p>Total: Rs ${Math.round(p.invest*p.multiplier)}</p>
    ${p.offer?`<div class="timer" id="planTimer${i}"></div>`:''}
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(card);
  });
}

// Render Ads Plans
function renderAds(){
  const container=document.getElementById('adsList');container.innerHTML='';
  adsData.forEach(a=>{
    const card=document.createElement('div');
    card.className='box';
    card.innerHTML=`<h3>${a.name}</h3>
    <p>Invest: Rs ${a.invest}</p>
    <p>Days: ${a.days} | Daily Ads: ${a.dailyAds}</p>
    <button onclick="buyAds(${a.id})">Buy Now</button>`;
    container.appendChild(card);
  });
}

// Ads Daily Task
function renderAdsTask(){
  const container=document.getElementById('adsList');
  if(balance===0){container.innerHTML='<p>Deposit first to unlock Ads plans.</p>'; return;}
  adsData.forEach(a=>{
    const card=document.createElement('div');
    card.className='box';
    card.innerHTML=`<h3>${a.name} Task</h3>
    <p>Watch ${a.dailyAds} Ads daily to earn Rs ${a.profit} per day</p>
    <button onclick="watchAd(${a.id})" class="ad-task-btn">Watch Ad</button>`;
    container.appendChild(card);
  });
}

function watchAd(id){
  alert('Ad is playing... wait 5 sec');
  setTimeout(()=>{
    alert('Ad watched! Rs 10 added to balance.');
    balance+=10;
    dailyProfit+=10;
    localStorage.setItem('nexa_balance',balance);
    localStorage.setItem('nexa_daily',dailyProfit);
    updateDashboard();
  },5000);
}

function buyPlan(id){alert('Plan '+id+' selected. Proceed to deposit.');showPage('deposit');document.getElementById('depositAmount').value=plansData[id-1].invest;}
function buyAds(id){alert('Ads Plan '+id+' selected. Proceed to deposit.');showPage('deposit');document.getElementById('depositAmount').value=adsData[id-1].invest;}

function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');balance+=parseFloat(document.getElementById('depositAmount').value);dailyProfit+=Math.floor(document.getElementById('depositAmount').value*0.02);localStorage.setItem('nexa_balance',balance);localStorage.setItem('nexa_daily',dailyProfit);updateDashboard();}
function submitWithdraw(){alert('Withdrawal requested');balance-=parseFloat(document.getElementById('withdrawAmount').value);localStorage.setItem('nexa_balance',balance);updateDashboard();}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

// Active Users random update
setInterval(()=>{activeUsers+=Math.floor(Math.random()*5);localStorage.setItem('nexa_active',activeUsers);if(document.getElementById('dashActive'))document.getElementById('dashActive').innerText=activeUsers;},5000);
</script>
</body>
</html>
