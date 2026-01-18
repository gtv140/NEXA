<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#1e1e2f;
  --secondary:#f0f0f5;
  --accent:#4f8cff;
  --success:#00c851;
  --danger:#ff4444;
  --bg-gradient:linear-gradient(120deg,#4f8cff,#00c851);
  --box-bg:rgba(255,255,255,0.03);
}
*{box-sizing:border-box;margin:0;padding:0;font-family:'Segoe UI',sans-serif;}
body{background:var(--primary);color:var(--secondary);}
header{text-align:center;font-size:32px;font-weight:700;padding:20px;background:var(--bg-gradient);-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;background:var(--box-bg);border-radius:12px;border:1px solid rgba(255,255,255,0.1);}
input,select,button{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:none;background:rgba(255,255,255,0.1);color:#fff;font-size:14px;}
button{background:var(--accent);font-weight:700;cursor:pointer;transition:0.2s;}
button:hover{background:var(--success);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 0;background:rgba(30,30,47,0.9);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{background:rgba(255,255,255,0.05);padding:12px;margin:10px 0;border-radius:10px;}
.box b{display:block;margin-bottom:6px;}
.plan-box{display:flex;justify-content:space-between;align-items:center;padding:12px;margin:10px 0;border-radius:10px;background:rgba(255,255,255,0.05);}
.plan-box:hover{background:rgba(79,140,255,0.2);}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;margin:10px 0;border-radius:10px;background:rgba(79,140,255,0.2);cursor:pointer;}
.support-icon:hover{background:rgba(0,200,81,0.2);}
.countdown{color:#ffbb33;font-weight:700;}
img.photo{width:100%;border-radius:10px;margin:8px 0;}
.alert{padding:10px;background:var(--danger);border-radius:8px;margin-bottom:12px;color:#fff;}
</style>
</head>
<body>

<header>NEXA EARN</header>

<!-- LOGIN/SIGNUP -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="username" placeholder="Username"/>
  <input id="password" type="password" placeholder="Password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="box"><b>Username:</b> <span id="dashUser">-</span></div>
  <div class="box"><b>Balance:</b> Rs <span id="dashBalance">0</span></div>
  <div class="box"><b>Daily Profit:</b> Rs <span id="dashDaily">0</span></div>
  <div class="box"><b>Total Profit:</b> Rs <span id="dashTotal">0</span></div>
  <div class="box"><b>Active Members:</b> <span id="activeMembers">0</span></div>

  <div id="photosContainer"></div>

  <button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
  <h2>Withdraw</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number"/>
  <input id="withdrawAmount" placeholder="Amount"/>
  <button onclick="submitWithdraw()">Request Withdraw</button>
</div>

<!-- ABOUT -->
<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN is a digital investment platform offering fast growth opportunities. Invest, track your profits, and withdraw safely. Our support team is always ready to assist you.</p>
  <div class="support-icon" onclick="openSupport()">🛠️ Support</div>
</div>

<!-- NAVIGATION -->
<div class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
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
let activeMembers = parseInt(localStorage.getItem('nexa_active')||'0');

// ===== PHOTOS =====
const photos = [
'https://picsum.photos/400/200?random=1',
'https://picsum.photos/400/200?random=2',
'https://picsum.photos/400/200?random=3',
'https://picsum.photos/400/200?random=4',
'https://picsum.photos/400/200?random=5'
];

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200 + (i-1)*100;
  let multiplier = i<=5?2.4:2.2;
  let days = 25 + Math.floor((i-1)/5)*5;
  plansData.push({id:i,name:'Plan '+i,invest,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),days});
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u = document.getElementById('username').value.trim();
  const p = document.getElementById('password').value.trim();
  if(!u || !p){alert('Enter username & password'); return;}
  currentUser = u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0; activeMembers = Math.floor(Math.random()*5000+1000);
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_active',activeMembers);
  updateDashboard(); renderPhotos(); renderPlans();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=activeMembers;
  document.querySelector('.nav').classList.remove('hidden');
  showPage('dashboard');
}
function renderPhotos(){
  const container = document.getElementById('photosContainer'); container.innerHTML='';
  photos.forEach(p=>{
    const img=document.createElement('img'); img.src=p; img.className='photo';
    container.appendChild(img);
  });
}
function renderPlans(){
  const container=document.getElementById('plansList'); container.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div><b>${p.name}</b><div>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div></div>
    <button onclick="buyPlan(${p.id})">Buy Now</button>`;
    container.appendChild(div);
  });
}
function buyPlan(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan){alert('Plan not found');return;}
  balance += plan.daily; totalProfit += plan.total;
  dailyProfit += plan.daily;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
  alert(`You bought ${plan.name}`);
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Copied!');}
function submitDeposit(){alert('Deposit submitted!');}
function submitWithdraw(){alert('Withdrawal requested!');}
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}
</script>

</body>
</html>
