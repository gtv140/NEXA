<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root {
  --primary:#ff5f00;
  --secondary:#ffc200;
  --bg:#f8f9fa;
  --text:#222;
  --card:#fff;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif;}
body{background:var(--bg);color:var(--text);overflow-x:hidden;}
header{padding:20px;text-align:center;font-size:28px;font-weight:700;background:linear-gradient(90deg,var(--primary),var(--secondary));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:500px;margin:20px auto;padding:20px;background:var(--card);border-radius:12px;box-shadow:0 4px 20px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid #ccc;outline:none;font-size:14px;}
button{background:var(--primary);color:#fff;font-weight:700;cursor:pointer;transition:0.2s;}
button:hover{opacity:0.9;}
.nav{position:fixed;bottom:0;left:0;right:0;background:#fff;display:flex;justify-content:space-around;padding:10px 0;border-top:1px solid #ddd;}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.card-box{padding:12px;margin:10px 0;border-radius:10px;background:#fff;box-shadow:0 2px 12px rgba(0,0,0,0.08);}
.card-box b{font-size:16px;}
.plan-box{padding:12px;margin:10px 0;border-radius:10px;background:#fff;box-shadow:0 2px 12px rgba(0,0,0,0.08);display:flex;justify-content:space-between;align-items:center;}
.countdown{color:var(--primary);font-weight:700;}
.support-icon{display:flex;align-items:center;gap:8px;padding:10px;background:var(--secondary);border-radius:10px;cursor:pointer;}
.support-icon:hover{opacity:0.9;}
img{max-width:100%;border-radius:10px;margin-top:10px;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="card-box">Username: <b id="dashUser">—</b></div>
  <div class="card-box">Balance: Rs <b id="dashBalance">0</b></div>
  <div class="card-box">Daily Profit: Rs <b id="dashDaily">0</b></div>
  <div class="card-box">Total Profit: Rs <b id="dashTotal">0</b></div>
  <div class="card-box">Active Members: <b id="activeMembers">0</b></div>
  
  <div class="card-box">
    <h3>About NEXA EARN</h3>
    <p>Since 2022, NEXA EARN provides fast, safe and reliable investment opportunities. Join thousands of satisfied users growing their profits daily.</p>
    <img src="https://source.unsplash.com/400x200/?finance,office" alt="Company Photo">
    <img src="https://source.unsplash.com/400x200/?money,cash" alt="Company Photo">
  </div>

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
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px;">
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

<!-- SUPPORT -->
<div id="about" class="page hidden">
  <h2>Support</h2>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span> Contact Support
  </div>
</div>

<!-- NAVIGATION -->
<div class="nav hidden" id="bottomNav">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>Support</div>
</div>

<script>
let currentUser=localStorage.getItem('nexa_user')||null;
let balance=parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit=parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit=parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers=parseInt(localStorage.getItem('nexa_active')||Math.floor(Math.random()*5000)+1);
let userPlans=JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');

const plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*50;
  let days=25+Math.floor(i/5)*5;
  let multiplier=(i<=5)?2.4:2.2;
  plansData.push({
    id:i,
    name:'Plan '+i,
    invest,
    days,
    total:Math.round(invest*multiplier),
    daily:Math.round((invest*multiplier)/days),
    special:(i<=5),
    endTime:Date.now()+(i<=5?24*60*60*1000:days*24*60*60*1000)
  });
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; totalProfit=0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_total',totalProfit);
  updateDashboard();
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); showPage('loginPage');}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashTotal').innerText=totalProfit;
  document.getElementById('activeMembers').querySelector('b').innerText=activeMembers;
  renderPlans();
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

function renderPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days} ${p.special?'<br><span class="countdown" id="count'+p.id+'"></span>':''}</div>
      <button onclick="buyNow(${p.id})">Buy Now</button>`;
    list.appendChild(div);
    if(p.special) startCountdown(p.id,p.endTime);
  });
}

function startCountdown(id,endTime){
  const el=document.getElementById('count'+id);
  const interval=setInterval(()=>{
    let diff=Math.floor((endTime-Date.now())/1000);
    if(diff<=0){el.innerText='Offer ended';clearInterval(interval);}
    else{
      let h=Math.floor(diff/3600),m=Math.floor((diff%3600)/60),s=diff%60;
      el.innerText=`Special Offer: ${h}h ${m}m ${s}s`;
    }
  },1000);
}

function buyNow(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan){return;}
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}

function submitDeposit(){
  const amt=parseFloat(document.getElementById('depositAmount').value)||0;
  balance+=amt;
  totalProfit+=amt; dailyProfit+=Math.round(amt/10);
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_total',totalProfit);
  localStorage.setItem('nexa_daily',dailyProfit);
  alert('Deposit successful!');
  updateDashboard();
}

function submitWithdraw(){
  const amt=parseFloat(document.getElementById('withdrawAmount').value)||0;
  if(amt>balance){alert('Insufficient balance'); return;}
  balance-=amt;
  localStorage.setItem('nexa_balance',balance);
  alert('Withdrawal requested!');
  updateDashboard();
}

function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}

// Auto-update active members randomly
setInterval(()=>{
  activeMembers=Math.floor(Math.random()*5000)+1;
  localStorage.setItem('nexa_active',activeMembers);
  if(document.getElementById('activeMembers')) document.getElementById('activeMembers').querySelector('b').innerText=activeMembers;
},5000);

</script>
</body>
</html>
