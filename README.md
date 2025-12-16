<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Earn</title>
<style>
body{margin:0;font-family:Arial;background:#111;color:#fff;overflow-x:hidden;}
header{text-align:center;padding:14px;font-size:28px;font-weight:900;background:#000;}
.box{max-width:430px;margin:12px auto;padding:14px;background:#222;border-radius:12px}
.hidden{display:none}
input,select,button{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid #333;background:#111;color:#fff}
button{background:#00ffe7;color:#000;font-weight:800;border:none;cursor:pointer}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;background:#000;padding:8px 0}
.nav div{font-size:12px;cursor:pointer;text-align:center}
.nav div .ico{font-size:20px;display:block;margin-bottom:2px}
.card{border:1px solid #333;padding:10px;border-radius:10px;margin-bottom:8px}
.plan-box{border:1px solid #333;padding:8px;margin-bottom:8px;border-radius:8px}
small{opacity:.7}
</style>
</head>
<body>
<header>NEXA Earn</header>

<!-- LOGIN -->
<div id="login" class="box">
<h2>Login / Signup</h2>
<input id="username" placeholder="Username">
<input id="password" type="password" placeholder="Password">
<select id="loginOption">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<button onclick="loginUser()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dash" class="box hidden">
<div class="card">
<b>User:</b> <span id="du"></span><br>
<b>Balance:</b> Rs <span id="bal">0</span><br>
<b>Daily Profit:</b> Rs <span id="dprofit">0</span><br>
<b>Total Profit:</b> Rs <span id="tprofit">0</span><br>
<b>Active Users:</b> <span id="active"></span>
</div>

<div class="card">
<b>Company Rules:</b><br>
Deposit/Withdrawal issue ho to <b>Administration</b> se contact karein.  
Deposit ya withdrawal request ke baad <b>Screenshot admin ko bhejna zaroori</b>.
</div>

<div class="card">
<b>Plans:</b>
<div id="plansList"></div>
</div>

<div class="card">
<b>Deposit:</b>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<div style="display:flex;gap:8px;margin-top:8px">
<input id="depositNumber" readonly style="flex:1"/>
<button onclick="copyDepositNumber()">Copy</button>
</div>
<input id="depositAmount" placeholder="Amount">
<input id="depositTxId" placeholder="Transaction ID">
<input type="file" id="depositProof">
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div class="card">
<b>Withdrawal:</b>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" placeholder="Amount">
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div class="card">
<b>History:</b>
<div id="historyList"></div>
</div>

<div class="card">
<b>Support:</b><br>
Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a><br>
WhatsApp: <a href="https://chat.whatsapp.com/" target="_blank">Join Group</a>
</div>
</div>

<!-- NAVIGATION -->
<div class="nav" id="nav">
<div onclick="show('dash')"><span class="ico">🏠</span>Dashboard</div>
<div onclick="show('plansDiv')"><span class="ico">📦</span>Plans</div>
<div onclick="show('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="show('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="show('history')"><span class="ico">📜</span>History</div>
<div onclick="show('support')"><span class="ico">ℹ️</span>Support</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let bal = parseFloat(localStorage.getItem('nexa_balance')||'0');
let daily = parseFloat(localStorage.getItem('nexa_daily')||'10');
let total = parseFloat(localStorage.getItem('nexa_total')||'0');
let lastProfit = parseInt(localStorage.getItem('nexa_lastProfit')||Date.now());
let history = JSON.parse(localStorage.getItem('nexa_history')||'[]');

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=25;i++){
  let invest=200*i;
  let days=25+i*2;
  let multiplier=(i<=5)?2.4:2.2;
  plansData.push({id:i,name:'Plan '+i,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days,endTime:Date.now()+days*24*60*60*1000});
}

// ===== LOGIN =====
function loginUser(){
  const user=document.getElementById('username').value.trim();
  const pass=document.getElementById('password').value.trim();
  const option=document.getElementById('loginOption').value;
  if(!user||!pass){alert("Fill username & password");return;}
  currentUser=user;
  if(option==='signup'){bal=0;total=0;daily=10;lastProfit=Date.now(); save();}
  save(); showDash();
}

// ===== SAVE =====
function save(){
  localStorage.setItem('nexa_user',currentUser);
  localStorage.setItem('nexa_balance',bal);
  localStorage.setItem('nexa_daily',daily);
  localStorage.setItem('nexa_total',total);
  localStorage.setItem('nexa_lastProfit',lastProfit);
  localStorage.setItem('nexa_history',JSON.stringify(history));
}

// ===== DASHBOARD =====
function showDash(){
  document.getElementById('login').classList.add('hidden');
  document.getElementById('dash').classList.remove('hidden');
  document.getElementById('du').innerText=currentUser;
  tick();
  renderPlans();
  renderHistory();
  updateDepositNumber();
}

// ===== TICK DAILY PROFIT =====
function tick(){
  let now=Date.now();
  if(now-lastProfit>=86400000){bal+=daily;total+=daily;lastProfit=now; save();}
  document.getElementById('bal').innerText=bal;
  document.getElementById('dprofit').innerText=daily;
  document.getElementById('tprofit').innerText=total;
  document.getElementById('active').innerText=Math.floor(50+Math.random()*150);
  setTimeout(tick,5000);
}

// ===== PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<b>${p.name}</b> | Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} <br>
    <span id="timer${p.id}"></span><br>
    <button onclick='buyPlan(${p.id})'>Buy Now</button>`;
    list.appendChild(div);
    startCountdown(p.id,p.endTime);
  });
}

function buyPlan(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  show('deposit');
}

// ===== COUNTDOWN =====
function startCountdown(id,endTime){
  const interval=setInterval(()=>{
    const now=Date.now();
    const diff=endTime-now;
    if(diff<=0){document.getElementById('timer'+id).innerText='Offer ended';clearInterval(interval);return;}
    const days=Math.floor(diff/1000/60/60/24);
    const hrs=Math.floor((diff/1000/60/60)%24);
    const min=Math.floor((diff/1000/60)%60);
    const sec=Math.floor((diff/1000)%60);
    document.getElementById('timer'+id).innerText=`Ends in: ${days}d ${hrs}h ${min}m ${sec}s`;
  },1000);
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=(method==='jazzcash')?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Deposit number copied!');}
function submitDeposit(){
  const amt=document.getElementById('depositAmount').value;
  if(!amt){alert('Enter amount'); return;}
  history.push(`Deposit: Rs ${amt} Method: ${document.getElementById('depositMethod').value}`);
  save(); renderHistory(); alert('Deposit request submitted. Admin ko screenshot bhejein.');
}

// ===== WITHDRAW =====
function submitWithdraw(){
  const amt=document.getElementById('withdrawAmount').value;
  const acc=document.getElementById('withdrawAccount').value;
  if(!amt||!acc){alert('Enter details');return;}
  history.push(`Withdrawal: Rs ${amt} Account: ${acc} Method: ${document.getElementById('withdrawMethod').value} (Pending)`);
  save(); renderHistory();
  setTimeout(()=>{
    history[history.length-1]=`Withdrawal: Rs ${amt} Account: ${acc} Method: ${document.getElementById('withdrawMethod').value} (Approved)`;
    save(); renderHistory();
  },15000);
  alert('Withdrawal request submitted. Admin ko screenshot bhejein.');
}

// ===== HISTORY =====
function renderHistory(){
  const h=document.getElementById('historyList');
  h.innerHTML='';
  history.forEach(entry=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerText=entry;
    h.appendChild(div);
  });
}

// ===== SHOW FUNCTION =====
function show(id){
  document.getElementById('dash').classList.add('hidden');
  document.getElementById('deposit').classList.add('hidden');
  document.getElementById('plansDiv')?.classList.add('hidden');
  document.getElementById('withdrawal').classList.add('hidden');
  document.getElementById('history')?.classList.add('hidden');
  document.getElementById('support')?.classList.add('hidden');
  document.getElementById(id).classList.remove('hidden');
}

window.onload=()=>{
  if(currentUser){showDash();}
}
</script>
</body>
</html>
