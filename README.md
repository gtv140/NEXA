<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root{--primary:#1a1a1a;--accent:#00f7ff;--accent2:#ff5cff;--text:#fff;}
body{margin:0;font-family:Arial,sans-serif;background:var(--primary);color:var(--text);}
header{text-align:center;font-size:28px;padding:20px;background:linear-gradient(90deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:500px;margin:20px auto;padding:20px;background:rgba(255,255,255,0.02);border-radius:12px;border:1px solid rgba(0,255,240,0.06);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;}
button{background:linear-gradient(90deg,var(--accent),var(--accent2));color:#001;font-weight:700;cursor:pointer;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.8);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.plan-box{border:1px solid rgba(0,255,240,0.06);padding:12px;margin:10px 0;border-radius:10px;background:rgba(0,0,0,0.1);display:flex;justify-content:space-between;align-items:center;transition:0.2s;}
.plan-box:hover{box-shadow:0 6px 20px rgba(0,255,240,0.3);}
.countdown{color:var(--accent);font-weight:700;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:rgba(0,255,240,0.06);border-radius:10px;cursor:pointer;}
.support-icon:hover{box-shadow:0 6px 20px rgba(0,255,240,0.2);transform:translateY(-2px);}
.alert-box{padding:12px;background:rgba(255,0,136,0.05);border-radius:10px;margin-bottom:12px;border:1px solid rgba(255,0,136,0.1);}
.user-box{display:flex;justify-content:space-between;align-items:center;padding:12px;border:1px solid rgba(0,255,240,0.06);border-radius:10px;margin-bottom:12px;background:rgba(0,255,240,0.02);}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<input id="user" placeholder="Username" />
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="user-box">
<div>
<b id="dashUser">-</b><br>
<span>Member since: <span id="dashSince">-</span></span>
</div>
<div>
Balance: Rs <span id="dashBalance">0</span><br>
Daily: Rs <span id="dashDaily">0</span>
</div>
</div>
<div class="alert-box">Active Members: <span id="activeMembers">0</span></div>
<div id="homePhotos" style="display:flex;gap:8px;flex-wrap:wrap;margin-top:10px;">
<img src="https://picsum.photos/100/100?random=1" style="border-radius:8px;">
<img src="https://picsum.photos/100/100?random=2" style="border-radius:8px;">
<img src="https://picsum.photos/100/100?random=3" style="border-radius:8px;">
<img src="https://picsum.photos/100/100?random=4" style="border-radius:8px;">
</div>
<button onclick="showPage('plans')">View Plans</button>
<button onclick="showPage('deposit')">Deposit</button>
<button onclick="showPage('withdrawal')">Withdraw</button>
<button onclick="showPage('about')">About</button>
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

<!-- ABOUT -->
<div id="about" class="page hidden">
<h2>About NEXA</h2>
<p>NEXA EARN is a premium digital investment platform offering fast, secure, and reliable profit opportunities. Our team ensures top support for all users.</p>
<div class="support-icon" onclick="openSupport()">
<span class="ico">🛠️</span> Support
</div>
</div>

<!-- NAV -->
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
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let totalUsers = parseInt(localStorage.getItem('nexa_totalUsers')||'1');

// ===== PLANS DATA =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*200;
  let multiplier=i<=5?2.4:2.2;
  let days=25+Math.floor((i-1)/2);
  let endTime=Date.now()+((i<=5?24:days)*60*60*1000);
  plansData.push({id:i,name:i<=5?`Special Plan ${i}`:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),days,special:i<=5,endTime});
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit);
  updateDashboard();
}
function logout(){localStorage.removeItem('nexa_user');currentUser=null;location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.querySelector('.nav').classList.remove('hidden');
  showPage('dashboard');
}
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}

// ===== RENDER PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    let countdownHTML=p.special?`<div class='countdown' id='countdown${p.id}'></div>`:'';
    div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}${countdownHTML}</div>
    <button onclick='buyNow(${p.id})'>Buy Now</button>`;
    list.appendChild(div);
    if(p.special) startCountdown(p.id,p.endTime);
  });
}

function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
}

function startCountdown(id,end){
  let countdownEl=document.getElementById(`countdown${id}`);
  if(!countdownEl) return;
  let timer=setInterval(()=>{
    let now=Date.now();
    let distance=end-now;
    if(distance<=0){clearInterval(timer);countdownEl.innerText='Offer Ended';return;}
    let h=Math.floor(distance/(1000*60*60));
    let m=Math.floor((distance%(1000*60*60))/(1000*60));
    let s=Math.floor((distance%(1000*60))/1000);
    countdownEl.innerText=`${h}h ${m}m ${s}s`;
  },1000);
}

// ===== ACTIVE MEMBERS RANDOM =====
setInterval(()=>{
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000);
},2000);

document.addEventListener('DOMContentLoaded',()=>{renderPlans(); updateDashboard();});
</script>
</body>
</html>
