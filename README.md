<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root{
  --gold:#FFD700;
  --accent:#FF8C00;
  --dark:#111;
  --bg:#1a1a1a;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:#fff;overflow-x:hidden;}
header{text-align:center;padding:25px;font-size:32px;font-weight:800;background:linear-gradient(90deg,var(--gold),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:500px;margin:20px auto;padding:25px;background:rgba(255,255,255,0.03);border-radius:15px;box-shadow:0 10px 25px rgba(0,0,0,0.5);}
input,select,button{width:100%;padding:12px;margin-top:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.15);background:transparent;color:#fff;outline:none;}
button{cursor:pointer;font-weight:700;background:linear-gradient(90deg,var(--gold),var(--accent));color:#111;transition:.2s;}
button:hover{transform:translateY(-2px);box-shadow:0 6px 15px rgba(0,0,0,0.5);}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px;background:rgba(0,0,0,0.85);border-top:1px solid rgba(255,255,255,0.05);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;margin-bottom:4px;}
.hidden{display:none;}
.user-box{display:flex;justify-content:space-between;align-items:center;padding:15px;border-radius:15px;background:linear-gradient(90deg,rgba(255,215,0,0.1),rgba(255,140,0,0.1));margin-bottom:15px;font-weight:600;}
.plan{padding:15px;border-radius:15px;margin-bottom:12px;background:rgba(255,255,255,0.02);border:1px solid rgba(255,215,0,0.1);}
.plan img{width:100%;border-radius:10px;margin-bottom:8px;}
.ad-box{padding:12px;margin:12px 0;background:rgba(255,215,0,0.1);border-radius:12px;text-align:center;}
.countdown{font-weight:700;color:#FFD700;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
<input id="user" placeholder="Username" />
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<div class="user-box">
<span>Username: <b id="dashUser">-</b></span>
<span>Balance: Rs <b id="dashBalance">0</b></span>
</div>
<div class="user-box">
<span>Daily Profit: Rs <b id="dashDaily">0</b></span>
<span>Total Profit: Rs <b id="dashTotal">0</b></span>
</div>
<div class="user-box">
<span>Active Members: <b id="activeMembers">0</b></span>
</div>

<h3>Ads Watch Plans</h3>
<div id="adsPlans"></div>

<h3>Investment Plans</h3>
<div id="plansList"></div>

<button onclick="logout()">Logout</button>
</div>

<div id="deposit" class="page hidden">
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
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let totalProfit = parseFloat(localStorage.getItem('nexa_total')||'0');
let activeMembers = Math.floor(Math.random()*5000+1000);

// Dashboard
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance = parseFloat(localStorage.getItem('nexa_balance_'+u)||'0');
  dailyProfit = parseFloat(localStorage.getItem('nexa_daily_'+u)||'0');
  totalProfit = parseFloat(localStorage.getItem('nexa_total_'+u)||'0');
  updateDashboard();
}
function logout(){
  localStorage.setItem('nexa_balance_'+currentUser,balance);
  localStorage.setItem('nexa_daily_'+currentUser,dailyProfit);
  localStorage.setItem('nexa_total_'+currentUser,totalProfit);
  currentUser=null;location.reload();
}
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashTotal').innerText=totalProfit.toFixed(2);
  document.getElementById('activeMembers').innerText=activeMembers;
  showPage('dashboard');
}

// Deposit
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){
  let amt=parseFloat(document.getElementById('depositAmount').value||0);
  balance+=amt; totalProfit+=amt; dailyProfit+=amt;
  localStorage.setItem('nexa_balance_'+currentUser,balance);
  localStorage.setItem('nexa_daily_'+currentUser,dailyProfit);
  localStorage.setItem('nexa_total_'+currentUser,totalProfit);
  updateDashboard();
  alert('Deposit submitted');
}

// Investment plans
function loadPlans(){
  const plansList=document.getElementById('plansList');
  plansList.innerHTML='';
  for(let i=1;i<=50;i++){
    let multiplier=i<=5?2.4:2.2;
    let days=25+i;
    let invest=200+i*100;
    let plan=document.createElement('div');
    plan.className='plan';
    plan.innerHTML=`<img src='https://picsum.photos/400/150?random=${i}'><b>Plan ${i}</b><br>Invest: Rs ${invest}<br>Days: ${days}<br>Total Profit: Rs ${Math.round(invest*multiplier)}<br><button onclick="buyPlan(${invest})">Buy Now</button>`;
    plansList.appendChild(plan);
  }
}
function buyPlan(amount){
  showPage('deposit');
  document.getElementById('depositAmount').value=amount;
  updateDepositNumber();
}

// Ads Watch Plans + Daily Reward
function loadAdsPlans(){
  const adsPlans=document.getElementById('adsPlans');
  adsPlans.innerHTML='';
  const adsArray=[{name:'Ads Plan 1',price:500,daily:3,days:10},{name:'Ads Plan 2',price:1000,daily:5,days:15}];
  adsArray.forEach((ad,i)=>{
    let plan=document.createElement('div');
    plan.className='plan';
    plan.innerHTML=`<b>${ad.name}</b><br>Price: Rs ${ad.price}<br>Daily Ads: ${ad.daily}<br>Days: ${ad.days}<br>
    <button onclick="startAds(${i},${ad.price},${ad.daily},${ad.days})">Buy & Watch</button>
    <div id="adBox${i}" class="ad-box hidden">Time: <span id="adCountdown${i}" class="countdown">0</span>s</div>`;
    adsPlans.appendChild(plan);
  });
}

function startAds(id,price,daily,days){
  showPage('deposit');
  document.getElementById('depositAmount').value=price;
  updateDepositNumber();
  setTimeout(()=>{
    // Simulate Ads Watch
    for(let i=0;i<daily;i++){
      let reward = Math.round(price/days/daily);
      balance+=reward; totalProfit+=reward; dailyProfit+=reward;
    }
    localStorage.setItem('nexa_balance_'+currentUser,balance);
    localStorage.setItem('nexa_daily_'+currentUser,dailyProfit);
    localStorage.setItem('nexa_total_'+currentUser,totalProfit);
    updateDashboard();
    alert('Daily Ads completed. Balance updated.');
  },3000);
}

// Initialize
if(currentUser){updateDashboard(); loadPlans(); loadAdsPlans();}
else{showPage('loginPage');loadPlans();loadAdsPlans();}
</script>

</body>
</html>
