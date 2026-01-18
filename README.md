<nexa>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary:#1a73e8;
  --secondary:#ff9800;
  --bg:#f5f5f5;
  --dark-bg:#1f1f1f;
  --white:#fff;
}
*{box-sizing:border-box;font-family:Arial, sans-serif;margin:0;padding:0;}
body{background:var(--bg);color:#333;}
header{padding:20px;text-align:center;font-size:28px;font-weight:700;background:linear-gradient(90deg,var(--primary),var(--secondary));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;background:var(--white);padding:20px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;font-size:14px;}
button{background:linear-gradient(90deg,var(--primary),var(--secondary));color:var(--white);font-weight:700;cursor:pointer;transition:0.2s;}
button:hover{transform:translateY(-2px);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 0;background:var(--dark-bg);color:var(--white);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.box{padding:14px;background:#f0f0f0;border-radius:12px;margin-bottom:12px;box-shadow:0 4px 15px rgba(0,0,0,0.05);}
.box strong{display:block;font-size:16px;margin-bottom:6px;}
.plan-box{border:1px solid #ccc;border-radius:12px;padding:12px;margin-bottom:10px;background:#fff;display:flex;justify-content:space-between;align-items:center;}
.plan-box:hover{box-shadow:0 4px 12px rgba(0,0,0,0.1);}
.countdown{color:var(--secondary);font-weight:700;margin-top:4px;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:var(--primary);color:#fff;border-radius:10px;cursor:pointer;margin-top:10px;}
.support-icon:hover{opacity:0.9;}
.alert-box{padding:10px;background:var(--secondary);color:#fff;border-radius:10px;margin-bottom:12px;text-align:center;}
.photos{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:12px;}
.photos img{width:48%;border-radius:10px;object-fit:cover;height:120px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="alert-box">Welcome to NEXA EARN. Keep your account safe.</div>
  <div class="box"><strong>Username:</strong> <span id="dashUser">-</span></div>
  <div class="box"><strong>Balance:</strong> Rs <span id="dashBalance">0</span></div>
  <div class="box"><strong>Daily Profit:</strong> Rs <span id="dashDaily">0</span></div>
  <div class="box"><strong>Total Profit:</strong> Rs <span id="dashTotal">0</span></div>
  <div class="box"><strong>Active Members:</strong> <span id="activeMembers">0</span></div>
  
  <h3>Company Highlights</h3>
  <p>NEXA EARN provides fast, reliable profit growth. Invest with ease and track your earnings anytime.</p>
  <div class="photos" id="randomPhotos"></div>

  <button onclick="showPage('plans')">View Plans</button>
  <button onclick="showPage('deposit')">Deposit</button>
  <button onclick="showPage('withdrawal')">Withdraw</button>
  <div class="support-icon" onclick="openSupport()">🛠️ Support</div>
  <button onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>Investment Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;margin-top:10px;align-items:center">
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

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user') || null;
let balance = parseFloat(localStorage.getItem('nexa_balance') || '0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily') || '0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total') || '0');
let activeMembers = parseInt(localStorage.getItem('nexa_activeMembers') || '0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans') || '[]');

// ===== RANDOM PHOTOS =====
const photosArr = [
  'https://picsum.photos/200/120?random=1',
  'https://picsum.photos/200/120?random=2',
  'https://picsum.photos/200/120?random=3',
  'https://picsum.photos/200/120?random=4',
  'https://picsum.photos/200/120?random=5'
];
function renderRandomPhotos(){
  const container = document.getElementById('randomPhotos');
  container.innerHTML='';
  photosArr.forEach(url=>{
    const img=document.createElement('img');
    img.src=url;
    container.appendChild(img);
  });
}

// ===== DASHBOARD =====
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  document.getElementById('bottomNav').classList.remove('hidden');
  renderRandomPhotos();
  showPage('dashboard');
}

// ===== LOGIN / LOGOUT =====
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')) balance=0;
  if(!localStorage.getItem('nexa_daily')) dailyProfit=0;
  if(!localStorage.getItem('nexa_total')) totalProfit=0;
  if(!localStorage.getItem('nexa_activeMembers')) activeMembers=Math.floor(Math.random()*5000)+1;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_activeMembers',activeMembers);
  updateDashboard();
}
function logout(){
  currentUser=null;
  localStorage.removeItem('nexa_user');
  location.reload();
}

// ===== SHOW PAGE =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}

// ===== WITHDRAW =====
function submitWithdraw(){alert('Withdrawal requested');}

// ===== SUPPORT =====
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}

// ===== PLANS =====
const plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*200;
  let days=25+Math.floor(i/5)*5;
  let multiplier=i<=5?2.4:2.2;
  plansData.push({
    id:i,name:'Plan '+i,invest,days,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),
    special:i<=5
  });
}
function renderPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div><strong>${p.name}</strong>
      Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}
      ${p.special?'<div class="countdown" id="countdown'+p.id+'">24h left</div>':''}
      </div>
      <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
    if(p.special) startCountdown(p.id);
  });
}

// ===== BUY NOW =====
function buyNow(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  if(balance<plan.invest){alert('Insufficient balance!'); return;}
  balance-=plan.invest;
  dailyProfit+=plan.daily;
  totalProfit+=plan.total;
  userPlans.push({id:plan.id,name:plan.name});
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
  alert('Plan purchased!');
}

// ===== SPECIAL OFFERS COUNTDOWN =====
function startCountdown(id){
  const element=document.getElementById('countdown'+id);
  if(!element) return;
  let endTime=localStorage.getItem('nexa_offer_'+id);
  if(!endTime){endTime=new Date().getTime()+24*60*60*1000; localStorage.setItem('nexa_offer_'+id,endTime);}
  setInterval(()=>{
    const now=new Date().getTime();
    let distance=endTime-now;
    if(distance<0){element.innerText='Offer expired';return;}
    let h=Math.floor(distance/(1000*60*60));
    let m=Math.floor((distance%(1000*60*60))/(1000*60));
    let s=Math.floor((distance%(1000*60))/1000);
    element.innerText=`${h}h ${m}m ${s}s left`;
  },1000);
}

// ===== RANDOM ACTIVE MEMBERS UPDATE =====
setInterval(()=>{
  activeMembers=Math.floor(Math.random()*5000)+1;
  localStorage.setItem('nexa_activeMembers',activeMembers);
  if(document.getElementById('activeMembers')) document.getElementById('activeMembers').innerText=activeMembers;
},5000);

// ===== INITIALIZE =====
if(currentUser) updateDashboard();
</script>
</body>
</html>
