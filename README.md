<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body {
  margin: 0; font-family: Arial, sans-serif; background: #f5f5f5; color: #111;
}
header { text-align:center; font-size:32px; font-weight:700; padding:20px; color:#2c3e50; }
.page { max-width:500px; margin:20px auto; padding:20px; background:#fff; border-radius:12px; box-shadow:0 5px 20px rgba(0,0,0,0.1);}
input, select, button { width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid #ccc; font-size:14px;}
button { background:#3498db; color:#fff; font-weight:700; cursor:pointer; transition:0.2s; }
button:hover { background:#2980b9; }
.nav { display:flex; justify-content:space-around; margin-top:10px; }
.nav div { text-align:center; cursor:pointer; font-weight:700; }
.nav div .ico { font-size:20px; display:block; margin-bottom:4px; }
.hidden{display:none;}
.card-box{margin-bottom:20px; padding:15px; background:#ecf0f1; border-radius:12px;}
.card-box img{width:100%; border-radius:10px; margin-top:10px;}
.plan-box{border:1px solid #3498db; padding:10px; margin:10px 0; border-radius:10px; display:flex; justify-content:space-between; align-items:center;}
.countdown{color:#e74c3c; font-weight:700;}
.alert-box{padding:10px; background:#f1c40f; border-radius:8px; margin-bottom:10px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="card-box">
    <h3>Welcome, <span id="dashUser">-</span></h3>
    <p>Balance: Rs <span id="dashBalance">0</span></p>
    <p>Daily Profit: Rs <span id="dashDaily">0</span></p>
    <p>Total Profit: Rs <span id="dashTotal">0</span></p>
    <p>Active Members: <span id="activeMembers">0</span></p>
  </div>

  <div class="card-box">
    <h3>About NEXA EARN</h3>
    <p>Since 2022, NEXA EARN provides fast, secure, and reliable investment growth. Trusted by thousands of users worldwide.</p>
    <ul>
      <li>✅ Secure deposits & withdrawals</li>
      <li>✅ Transparent profit tracking</li>
      <li>✅ Special offers with countdown</li>
      <li>✅ 24/7 Support</li>
    </ul>
    <img src="https://source.unsplash.com/400x200/?finance" alt="Finance">
    <img src="https://source.unsplash.com/400x200/?team" alt="Team">
    <img src="https://source.unsplash.com/400x200/?money" alt="Money">
  </div>

  <div id="plansList"></div>

  <button onclick="showPage('deposit')">Deposit</button>
  <button onclick="showPage('withdrawal')">Withdraw</button>
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
  <input id="depositAmount" placeholder="Amount"/>
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

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u || !p){alert('Enter username & password');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){balance=0; dailyProfit=0; totalProfit=0; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_total',totalProfit);}
  updateDashboard(); renderPlans();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000);
  showPage('dashboard');
}

// ===== PLANS =====
let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+i*50;
  let days=25+Math.floor(i/2);
  let multiplier=i<=5?2.4:2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days,special:i<=5});
}

function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    let countdownHTML='';
    if(p.special){countdownHTML=`<div class="countdown" id="countdown${p.id}"></div>`; startCountdown(p.id);}
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div>
      <b>${p.name}</b><br/>
      Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}<br/>
      ${countdownHTML}
    </div>
    <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
  });
}

function startCountdown(id){
  const endTime=new Date().getTime()+24*60*60*1000;
  const el=document.getElementById('countdown'+id);
  setInterval(()=>{ 
    let now=new Date().getTime(); let distance=endTime-now;
    if(distance<0){el.innerText="Expired"; return;}
    let h=Math.floor((distance%(1000*60*60*24))/(1000*60*60));
    let m=Math.floor((distance%(1000*60*60))/(1000*60));
    let s=Math.floor((distance%(1000*60))/1000);
    el.innerText=`Special Offer Ends: ${h}h ${m}m ${s}s`;
  },1000);
}

function buyNow(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan){alert('Plan not found'); return;}
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

// ===== DEPOSIT / WITHDRAW =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted'); balance+=parseFloat(document.getElementById('depositAmount').value||0); dailyProfit+=Math.round(balance*0.01); totalProfit+=dailyProfit; localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_total',totalProfit); updateDashboard(); showPage('dashboard');}
function submitWithdraw(){alert('Withdrawal requested'); balance-=parseFloat(document.getElementById('withdrawAmount').value||0); localStorage.setItem('nexa_balance',balance); updateDashboard(); showPage('dashboard');}
</script>
</body>
</html>
