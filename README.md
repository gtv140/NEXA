<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{--primary:#1a1a1a;--accent:#00cfff;--accent2:#ff4fff;}
body{margin:0;font-family:Arial,sans-serif;background:var(--primary);color:#fff;overflow-x:hidden;}
header{text-align:center;font-size:32px;font-weight:800;padding:20px;background:linear-gradient(90deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;background:rgba(255,255,255,0.02);border-radius:12px;}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;font-size:14px;}
button{background:linear-gradient(90deg,var(--accent),var(--accent2));border:none;color:#001;font-weight:700;cursor:pointer;transition:0.2s;}
button:hover{transform:translateY(-2px);box-shadow:0 8px 20px rgba(0,255,240,0.2);}
.nav{position:fixed;bottom:0;left:0;right:0;background:rgba(0,0,0,0.85);display:flex;justify-content:space-around;padding:10px 0;}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;margin-bottom:4px;}
.hidden{display:none;}
.ad-photo{width:100%;margin:8px 0;border-radius:12px;}
.box{padding:12px;margin:10px 0;background:rgba(0,255,240,0.05);border-radius:12px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="box">Username: <span id="dashUser">-</span></div>
<div class="box">Balance: Rs <span id="dashBalance">0</span></div>
<div class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
<div class="box">Total Profit: Rs <span id="dashTotal">0</span></div>
<div class="box">Active Members: <span id="activeMembers">0</span></div>

<div id="adsPhotos"></div>

<div id="plansList"></div>

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

<!-- ABOUT -->
<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN is a fast, secure, and professional investment platform with attractive profit plans and daily updates. Join thousands of active members and maximize your profit.</p>
</div>

<!-- NAV -->
<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plansList')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let activeMembers = parseInt(localStorage.getItem('nexa_activeUsers')||Math.floor(Math.random()*5000));

const adsPhotos = [
"https://picsum.photos/400/120?random=1",
"https://picsum.photos/400/120?random=2",
"https://picsum.photos/400/120?random=3",
"https://picsum.photos/400/120?random=4",
"https://picsum.photos/400/120?random=5"
];

const plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*200;
  let multiplier = i<=5 ? 2.4 : 2.2;
  let days = 25+((i-1)%10);
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),days});
}

// ===== FUNCTIONS =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  if(id==='plansList'){renderPlans();id='dashboard';}
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u || !p){alert('Enter username and password'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
}

function logout(){ currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=activeMembers;
  renderAdsPhotos();
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

function renderAdsPhotos(){
  let container=document.getElementById('adsPhotos'); container.innerHTML='';
  adsPhotos.forEach(url=>{
    const img=document.createElement('img');
    img.src=url; img.className='ad-photo';
    container.appendChild(img);
  });
}

function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='box';
    div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan) return; 
  if(balance < plan.invest){ alert('Insufficient balance!'); return;}
  balance-=plan.invest; dailyProfit+=plan.daily; totalProfit+=plan.total;
  userPlans.push({id:plan.id,name:plan.name,daily:plan.daily,total:plan.total,start:new Date().getTime(),days:plan.days});
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  alert(`Plan ${plan.name} bought successfully`);
  updateDashboard();
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = method==='jazzcash' ? '03705519562' : '03379827882';
}

function copyDepositNumber(){ navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){ alert('Deposit submitted');}
function submitWithdraw(){ alert('Withdrawal requested');}

// ===== AUTO UPDATE =====
setInterval(()=>{
  if(currentUser){
    let randomDaily=Math.floor(Math.random()*100); 
    balance+=randomDaily; dailyProfit+=randomDaily; totalProfit+=randomDaily*2;
    localStorage.setItem('nexa_balance',balance);
    localStorage.setItem('nexa_daily',dailyProfit);
    localStorage.setItem('nexa_total',totalProfit);
    updateDashboard();
  }
},60000);
</script>
</body>
</html>
