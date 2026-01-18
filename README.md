<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f4f4f4;color:#111;}
header{background:#1a73e8;color:#fff;text-align:center;padding:20px;font-size:28px;font-weight:700;}
.page{max-width:500px;margin:20px auto;padding:20px;background:#fff;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;}
button{background:#1a73e8;color:#fff;font-weight:700;cursor:pointer;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:#eee;}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.plan-box{border:1px solid #ccc;padding:12px;margin:10px 0;border-radius:10px;background:#fafafa;display:flex;justify-content:space-between;align-items:center;}
.plan-box:hover{box-shadow:0 6px 15px rgba(0,0,0,0.1);}
.countdown{color:red;font-weight:700;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:#eee;border-radius:10px;cursor:pointer;}
</style>
</head>
<body>
<header>NEXA EARN</header><!-- LOGIN --><div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div><!-- DASHBOARD --><div id="dashboard" class="page hidden">
  <h3>Welcome, <span id="dashUser">—</span></h3>
  <div>Balance: Rs <span id="dashBalance">0</span></div>
  <div>Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div>Active Members: <span id="activeMembers">0</span></div>
  <div id="plansList"></div>
  <div style="margin-top:20px">
    <h4>About NEXA EARN</h4>
    <p>We provide fast, secure digital investment with verified support and instant profit updates. Join our premium community today!</p>
    <img src="https://via.placeholder.com/150" alt="Company Image 1" style="width:100%;margin-bottom:10px;border-radius:8px;">
    <img src="https://via.placeholder.com/150" alt="Company Image 2" style="width:100%;margin-bottom:10px;border-radius:8px;">
  </div>
  <button onclick="logout()">Logout</button>
</div><!-- DEPOSIT --><div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount" />
  <input id="depositTxId" placeholder="Transaction ID" />
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div><!-- WITHDRAWAL --><div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div><div id="about" class="page hidden">
  <h2>Support & Company Info</h2>
  <p>NEXA EARN offers verified, premium digital investment options with 24/7 support.</p>
  <div class="support-icon" onclick="openSupport()">🛠️ Contact Support</div>
</div><div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div><script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let activeMembers = parseInt(localStorage.getItem('nexa_activeMembers')||'0');

window.onload = function(){
  if(currentUser){
    document.getElementById('dashUser').innerText = currentUser;
    document.getElementById('dashBalance').innerText = balance;
    document.getElementById('dashDaily').innerText = dailyProfit;
    document.getElementById('activeMembers').innerText = activeMembers;
    document.getElementById('bottomNav').classList.remove('hidden');
    renderPlans();
    showPage('dashboard');
  } else {
    showPage('loginPage');
  }
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser = u;
  localStorage.setItem('nexa_user', currentUser);
  if(!localStorage.getItem('nexa_balance')){
    balance=0; dailyProfit=0; activeMembers=Math.floor(Math.random()*5000)+1;
    localStorage.setItem('nexa_balance', balance);
    localStorage.setItem('nexa_daily', dailyProfit);
    localStorage.setItem('nexa_activeMembers', activeMembers);
  }
  document.getElementById('dashUser').innerText = currentUser;
  document.getElementById('dashBalance').innerText = balance;
  document.getElementById('dashDaily').innerText = dailyProfit;
  document.getElementById('activeMembers').innerText = activeMembers;
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
  showPage('dashboard');
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); showPage('loginPage');}
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value);alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function submitWithdraw(){alert('Withdrawal requested');}

// ===== PLANS =====
let plans=[];
for(let i=1;i<=50;i++){
  let invest=200+i*200;
  let multiplier=i<=5?2.4:2.2;
  let days=25+Math.floor(i/2);
  plans.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),days:days,special:i<=5});
}
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plans.forEach(p=>{
    let div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
    <div><button onclick='buyNow(${p.id})'>Buy Now</button>${p.special?`<div class='countdown' id='countdown${p.id}'>24:00:00</div>`:''}</div>`;
    list.appendChild(div);
    if(p.special){startCountdown(p.id);}
  });
}
function buyNow(id){let plan=plans.find(p=>p.id===id);document.getElementById('depositAmount').value=plan.invest; showPage('deposit');}
function startCountdown(id){
  let endTime=Date.now()+24*60*60*1000;
  let interval=setInterval(()=>{
    let diff=endTime-Date.now();
    if(diff<=0){clearInterval(interval);document.getElementById('countdown'+id).innerText='Offer ended'; return;}
    let h=Math.floor(diff/3600000); let m=Math.floor((diff%3600000)/60000); let s=Math.floor((diff%60000)/1000);
    document.getElementById('countdown'+id).innerText=`${h}:${m}:${s}`;
  },1000);
}
</script></body>
</html>
