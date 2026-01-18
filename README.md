<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root {
  --primary:#00aaff;
  --secondary:#ff55aa;
  --bg:#f4f4f4;
  --white:#fff;
  --text:#111;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{background:var(--bg);color:var(--text);}
header{padding:20px;text-align:center;font-size:28px;font-weight:700;color:var(--primary);}
.page{max-width:500px;margin:20px auto;padding:20px;background:var(--white);border-radius:12px;box-shadow:0 4px 15px rgba(0,0,0,0.2);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;font-size:14px;}
button{background:linear-gradient(90deg,var(--primary),var(--secondary));color:#fff;font-weight:700;cursor:pointer;border:none;}
button:hover{opacity:0.9;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:#eee;box-shadow:0 -3px 10px rgba(0,0,0,0.1);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.plan-box{border-radius:12px;padding:12px;margin:12px 0;background:linear-gradient(135deg,#00aaff,#ff55aa);color:#fff;display:flex;justify-content:space-between;align-items:center;transition:transform 0.2s;}
.plan-box:hover{transform:scale(1.02);}
.plan-meta b{display:block;font-size:16px;margin-bottom:6px;}
.plan-meta .small{font-size:13px;}
.countdown{font-weight:700;margin-top:6px;font-size:14px;}
.support-icon{display:flex;align-items:center;gap:8px;padding:10px;background:#eee;border-radius:10px;cursor:pointer;margin-top:10px;}
.support-icon:hover{background:#ddd;}
.photo-grid{display:flex;flex-wrap:wrap;gap:10px;margin-top:12px;}
.photo-grid img{width:48%;border-radius:12px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN PAGE -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div style="margin-bottom:12px;">Welcome, <b id="dashUser">-</b></div>
  <div style="margin-bottom:12px;"><b>About NEXA EARN:</b> NEXA EARN is a premium digital investment platform providing fast, secure, and reliable profit growth. Trusted by thousands of users worldwide.</div>
  <div class="photo-grid">
    <img src="https://via.placeholder.com/200x120?text=Photo+1"/>
    <img src="https://via.placeholder.com/200x120?text=Photo+2"/>
    <img src="https://via.placeholder.com/200x120?text=Photo+3"/>
    <img src="https://via.placeholder.com/200x120?text=Photo+4"/>
    <img src="https://via.placeholder.com/200x120?text=Photo+5"/>
  </div>
  <div style="margin-top:12px;">Your Balance: Rs <span id="dashBalance">0</span></div>
  <div>Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div>Active Members: <span id="activeMembers">0</span></div>
  <button onclick="logout()">Logout</button>
</div>

<!-- PLANS PAGE -->
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
  <h2>About NEXA EARN</h2>
  <p>NEXA EARN is a premium digital investment platform offering fast, secure, and reliable profit growth. Our support team is always ready to assist you.</p>
  <div class="support-icon" onclick="openSupport()">
    🛠️ Support
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
// STORAGE
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let savedEndTimes = JSON.parse(localStorage.getItem('nexa_offerEndTimes')||'{}');

// PLANS DATA
let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*200;
  let days=25+Math.floor((i-1)/2);
  let multiplier = i<=5?2.4:2.2;
  let special = i<=5;
  let endTime = savedEndTimes[i]||(special? new Date().getTime()+24*60*60*1000:new Date().getTime()+days*24*60*60*1000);
  if(special) savedEndTimes[i]=endTime;
  plansData.push({id:i,name:special?`Special Plan ${i}`:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),special,endTime});
}
localStorage.setItem('nexa_offerEndTimes',JSON.stringify(savedEndTimes));

// FUNCTIONS
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; userPlans=[];
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.querySelector('.nav').classList.remove('hidden');
  showPage('dashboard');
}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}

// RENDER PLANS
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    let countdownHTML='';
    if(p.special) countdownHTML=`<div class="countdown" id="countdown${p.id}"></div>`;
    div.innerHTML=`<div class="plan-meta"><b>${p.name}</b>
      <div class="small">Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
      ${countdownHTML}
    </div>
    <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
    if(p.special) startCountdown(p.id);
  });
}
function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
}
function startCountdown(id){
  const countdownEl=document.getElementById('countdown'+id);
  if(!countdownEl) return;
  let endTime = plansData.find(p=>p.id===id).endTime;
  let timer=setInterval(()=>{
    let now=new Date().getTime();
    let distance=endTime-now;
    if(distance<0){clearInterval(timer); countdownEl.innerText='Offer Ended'; return;}
    let h=Math.floor(distance/(1000*60*60));
    let m=Math.floor((distance%(1000*60*60))/(1000*60));
    let s=Math.floor((distance%(1000*60))/1000);
    countdownEl.innerText=`Ends in ${h}h ${m}m ${s}s`;
  },1000);
}

// INIT
renderPlans();
</script>
</body>
</html>
