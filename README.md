<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root {
  --primary: #ff0080;
  --secondary: #ff8c00;
  --bg1: #fff0f5;
  --bg2: #fff5e6;
  --text: #333;
}
body {
  margin:0;
  font-family:'Segoe UI', sans-serif;
  background: linear-gradient(135deg, var(--bg1), var(--bg2));
  color: var(--text);
  overflow-x:hidden;
  transition: all 0.3s ease;
}
header{
  text-align:center;
  font-size:36px;
  font-weight:800;
  padding:20px;
  color: var(--primary);
  text-shadow: 2px 2px var(--secondary);
}
.page{
  max-width:500px;
  margin:20px auto;
  padding:20px;
  border-radius:12px;
  background:rgba(255,255,255,0.8);
  box-shadow:0 8px 20px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}
input, select, button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #ccc;
  outline:none;
  font-size:14px;
}
button{
  background: linear-gradient(90deg,var(--primary),var(--secondary));
  color:#fff;
  font-weight:700;
  cursor:pointer;
  transition: all 0.2s ease;
}
button:hover{transform:translateY(-2px); box-shadow:0 6px 20px rgba(0,0,0,0.2);}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:12px 0;
  background:rgba(255,255,255,0.9);
  border-top:1px solid var(--secondary);
}
.nav div{text-align:center; cursor:pointer;}
.nav div .ico{font-size:20px; display:block; margin-bottom:4px;}
.hidden{display:none;}
.box{
  padding:15px;
  border-radius:12px;
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  color:#fff;
  margin-bottom:15px;
  box-shadow:0 6px 15px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}
.box h3{margin:0 0 10px 0;}
.plan{
  padding:12px;
  border-radius:12px;
  background:rgba(255,255,255,0.6);
  margin-bottom:12px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  cursor:pointer;
  transition: all 0.2s ease;
}
.plan:hover{transform:translateY(-2px); box-shadow:0 6px 20px rgba(0,0,0,0.2);}
.countdown{font-weight:700; color:var(--primary);}
img.dashboard-img{width:100%; border-radius:12px; margin-bottom:12px;}

/* Animation for background */
@keyframes bgAnim{
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}
body{background-size:200% 200%; animation:bgAnim 20s linear infinite;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN/SIGNUP -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<input id="username" placeholder="Username">
<input id="password" placeholder="Password" type="password">
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="box">
<h3>Welcome, <span id="userDisplay">—</span></h3>
<p>Balance: Rs <span id="balance">0</span></p>
<p>Daily Profit: Rs <span id="daily">0</span></p>
<p>Total Profit: Rs <span id="total">0</span></p>
<p>Active Members: <span id="activeMembers">0</span></p>
</div>
<img src="https://source.unsplash.com/500x250/?finance" class="dashboard-img">
<img src="https://source.unsplash.com/500x250/?money" class="dashboard-img">
<img src="https://source.unsplash.com/500x250/?investment" class="dashboard-img">
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
<div style="display:flex; gap:8px; margin-top:10px;">
<input id="depositNumber" readonly style="flex:1;">
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Enter Amount">
<input id="depositTx" placeholder="Transaction ID">
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
<p>NEXA EARN, running since 2022, provides fast and reliable digital investment opportunities. Our support team is always ready to assist you.</p>
<div class="support-icon" onclick="openSupport()"><span class="ico">🛠️</span> Support</div>
<img src="https://source.unsplash.com/500x200/?team" class="dashboard-img">
</div>

<!-- NAV -->
<div class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plansPage')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexaUser') || null;
let balance = parseFloat(localStorage.getItem('nexaBalance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexaDaily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexaTotal')||'0');
let activeMembers = Math.floor(Math.random()*5000)+1;

// ===== SHOW PAGE =====
function showPage(id){
document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
document.getElementById(id)?.classList.remove('hidden');
}

// ===== LOGIN =====
function login(){
let user=document.getElementById('username').value.trim();
let pass=document.getElementById('password').value.trim();
if(!user||!pass){alert('Enter Username & Password'); return;}
currentUser=user;
localStorage.setItem('nexaUser',currentUser);
localStorage.setItem('nexaBalance', balance);
localStorage.setItem('nexaDaily', dailyProfit);
localStorage.setItem('nexaTotal', totalProfit);
updateDashboard();
}

// ===== LOGOUT =====
function logout(){
currentUser=null;
localStorage.removeItem('nexaUser');
updateDashboard();
showPage('loginPage');
}

// ===== DASHBOARD =====
function updateDashboard(){
document.getElementById('userDisplay').innerText=currentUser||'—';
document.getElementById('balance').innerText=balance.toFixed(2);
document.getElementById('daily').innerText=dailyProfit.toFixed(2);
document.getElementById('total').innerText=totalProfit.toFixed(2);
document.getElementById('activeMembers').innerText=activeMembers;
document.querySelector('.nav').classList.remove('hidden');
showPage('dashboard');
}

// ===== ACTIVE MEMBERS =====
setInterval(()=>{activeMembers=Math.floor(Math.random()*5000)+1; document.getElementById('activeMembers').innerText=activeMembers;},5000);

// ===== PLANS =====
let plans=[];
for(let i=1;i<=50;i++){
let invest=200+(i-1)*50;
let days=25+Math.floor(i/5)*5;
let multiplier=i<=5?2.4:2.2;
plans.push({id:i,name:'Plan '+i,invest,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),days, special:i<=5});
}
function renderPlans(){
let list=document.getElementById('plansList');
list.innerHTML='';
plans.forEach(p=>{
let div=document.createElement('div');
div.className='plan';
let cd='';
if(p.special){cd=`<span class='countdown' id='countdown${p.id}'></span>`;startCountdown(p.id);}
div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest}<br>Total: Rs ${p.total}<br>Daily: Rs ${p.daily}<br>Days: ${p.days} ${cd}</div><button onclick="buyPlan(${p.id})">Buy Now</button>`;
list.appendChild(div);
});
}
function startCountdown(id){
let endTime=Date.now()+24*60*60*1000;
let interval=setInterval(()=>{
let now=Date.now();
let diff=endTime-now;
if(diff<=0){document.getElementById('countdown'+id).innerText='Offer ended'; clearInterval(interval); return;}
let h=Math.floor(diff/3600000);
let m=Math.floor((diff%3600000)/60000);
let s=Math.floor((diff%60000)/1000);
document.getElementById('countdown'+id).innerText=`Offer ends in ${h}h ${m}m ${s}s`;
},1000);
}

// ===== BUY PLAN =====
function buyPlan(id){
let plan=plans.find(p=>p.id===id);
if(!plan){alert('Plan not found'); return;}
document.getElementById('deposit').classList.remove('hidden');
document.getElementById('depositAmount').value=plan.invest;
showPage('deposit');
}

// ===== DEPOSIT =====
function updateDepositNumber(){
const m=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=m==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit Submitted'); balance+=parseFloat(document.getElementById('depositAmount').value||0); dailyProfit+=5; totalProfit+=5; localStorage.setItem('nexaBalance',balance); localStorage.setItem('nexaDaily',dailyProfit); localStorage.setItem('nexaTotal',totalProfit); updateDashboard();}

// ===== WITHDRAW =====
function submitWithdraw(){alert('Withdrawal Requested');}

// ===== SUPPORT =====
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}

// ===== INIT =====
if(currentUser){updateDashboard(); renderPlans();} 
</script>
</body>
</html>
