<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body,html{margin:0;padding:0;font-family:'Segoe UI',sans-serif;background:#fdf6f0;color:#333;}
header{font-size:36px;text-align:center;padding:20px;font-weight:900;color:#ff5c4d;letter-spacing:2px;}
.page{max-width:480px;margin:20px auto;padding:25px;background:#ffffffcc;border-radius:16px;box-shadow:0 12px 30px rgba(0,0,0,0.08);}
input,select,button{width:100%;padding:12px;margin:10px 0;border-radius:12px;border:1px solid #ffbfb2;font-size:16px;outline:none;}
button{background:linear-gradient(90deg,#ff5c4d,#ff9980);color:white;font-weight:700;cursor:pointer;transition:.3s;}
button:hover{opacity:.9;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 0;background:#fff4f0;border-top:1px solid #ffd1c1;}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:22px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.user-box{background:#fff0ef;padding:22px;border-radius:16px;margin-bottom:20px;text-align:center;font-weight:700;box-shadow:0 6px 20px rgba(0,0,0,0.08);}
.plan-box{border:1px solid #ffb3aa;padding:16px;margin:12px 0;border-radius:14px;background:#fff0f0;display:flex;justify-content:space-between;align-items:center;transition:.3s;}
.plan-box:hover{box-shadow:0 8px 25px rgba(0,0,0,0.08);}
.countdown{color:#ff5c4d;font-weight:700;}
.support-icon{display:flex;align-items:center;gap:8px;padding:14px;background:#fff0ef;border-radius:14px;cursor:pointer;transition:.3s;}
.support-icon:hover{box-shadow:0 6px 20px rgba(0,0,0,0.08);}
img.dashboard-img{width:100%;border-radius:14px;margin:12px 0;}
</style>
</head>
<body>

<header>NEXA EARN</header>

<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <input id="user" placeholder="Username"/>
  <input id="pass" placeholder="Password" type="password"/>
  <button id="loginBtn">Submit</button>
</div>

<div id="dashboard" class="page hidden">
  <div class="user-box">
    <div>Username: <span id="dashUser">—</span></div>
    <div>Balance: Rs <span id="dashBalance">0</span></div>
    <div>Daily Profit: Rs <span id="dashDaily">0</span></div>
    <div>Active Users: <span id="activeMembers">0</span></div>
  </div>

  <h3>About NEXA EARN</h3>
  <p>NEXA EARN has been providing reliable digital investment services since 2022. Fast, secure, and fully transparent platform with professional support for all users.</p>

  <img src="https://picsum.photos/400/200?random=1" class="dashboard-img"/>
  <img src="https://picsum.photos/400/200?random=2" class="dashboard-img"/>
  <img src="https://picsum.photos/400/200?random=3" class="dashboard-img"/>
  <img src="https://picsum.photos/400/200?random=4" class="dashboard-img"/>
</div>

<div id="plansPage" class="page hidden">
  <h2>Investment Plans</h2>
  <div id="plansListPage"></div>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center">
    <input id="depositNumber" readonly style="flex:1"/>
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount"/>
  <input id="depositTxId" placeholder="Transaction ID"/>
  <input type="file" id="depositProof"/>
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

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

<div id="about" class="page hidden">
  <h2>About NEXA EARN</h2>
  <p>Reliable investment opportunities with full transparency and continuous professional support for all users.</p>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span> Support
  </div>
</div>

<div id="bottomNav" class="nav hidden">
  <div id="homeIcon"><span class="ico">🏠</span>Home</div>
  <div id="plansIcon"><span class="ico">📦</span>Plans</div>
  <div id="depositIcon"><span class="ico">💰</span>Deposit</div>
  <div id="withdrawIcon"><span class="ico">💵</span>Withdraw</div>
  <div id="aboutIcon"><span class="ico">ℹ️</span>About</div>
  <div id="logoutIcon"><span class="ico">🚪</span>Logout</div>
</div>

<script>
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');

let plansData=[];
for(let i=1;i<=50;i++){
  let invest = 200+i*50;
  let days = 25+Math.floor(i/5)*5;
  let multiplier = i<=5?2.4:2.2;
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days,offerEnd:new Date().getTime()+24*60*60*1000});
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

document.getElementById('loginBtn').onclick=login;
document.getElementById('homeIcon').onclick=()=>showPage('dashboard');
document.getElementById('plansIcon').onclick=()=>showPage('plansPage');
document.getElementById('depositIcon').onclick=()=>showPage('deposit');
document.getElementById('withdrawIcon').onclick=()=>showPage('withdrawal');
document.getElementById('aboutIcon').onclick=()=>showPage('about');
document.getElementById('logoutIcon').onclick=()=>logout();

function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter Username & Password'); return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  if(!localStorage.getItem('nexa_balance')){balance=0;dailyProfit=0;localStorage.setItem('nexa_balance',balance);localStorage.setItem('nexa_daily',dailyProfit);}
  updateDashboard();
  renderPlans();
  randomActiveMembers();
}

function logout(){currentUser=null;localStorage.removeItem('nexa_user');location.reload();}

function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('bottomNav').classList.remove('hidden');
  showPage('dashboard');
}

function renderPlans(){
  const listPage=document.getElementById('plansListPage'); listPage.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    const now=Date.now();
    const remaining=Math.max(0, p.offerEnd-now);
    const hours=Math.floor(remaining/3600000); const minutes=Math.floor((remaining%3600000)/60000);
    div.innerHTML=`<div><b>${p.name}</b><br>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}<br><span class='countdown'>Offer Ends: ${hours}h ${minutes}m</span></div>
    <button onclick='buyNow(${p.id})'>Buy Now</button>`;
    listPage.appendChild(div);
  });
}

function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan){alert('Plan not found');return;}
  alert(`Selected ${plan.name}. Please deposit Rs ${plan.invest}`);
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){balance+=parseFloat(document.getElementById('depositAmount').value||0);localStorage.setItem('nexa_balance',balance);alert('Deposit submitted');updateDashboard();}
function submitWithdraw(){balance-=parseFloat(document.getElementById('withdrawAmount').value||0);localStorage.setItem('nexa_balance',balance);alert('Withdrawal requested');updateDashboard();}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}
function randomActiveMembers(){document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000)+1;}
setInterval(()=>{dailyProfit+=(plansData.reduce((sum,p)=>sum?p.daily:0,0)/24/60/60);localStorage.setItem('nexa_daily',dailyProfit);updateDashboard();randomActiveMembers();},1000);
</script>
</body>
</html>
