<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:#f4f4f4;
  color:#111;
}
header{
  text-align:center;
  font-size:28px;
  font-weight:700;
  padding:20px;
  background:#fff;
  border-bottom:1px solid #ddd;
}
.page{
  max-width:500px;
  margin:20px auto;
  padding:20px;
  background:#fff;
  border-radius:12px;
  box-shadow:0 4px 10px rgba(0,0,0,0.1);
}
input, select, button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid #ccc;
  font-size:14px;
}
button{
  background:#007bff;
  color:#fff;
  border:none;
  cursor:pointer;
  font-weight:700;
}
button:hover{background:#0056b3;}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  background:#fff;
  border-top:1px solid #ddd;
}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{display:block;margin-bottom:4px;font-size:20px;}
.hidden{display:none;}
.plan-box{
  border:1px solid #ddd;
  padding:12px;
  margin:10px 0;
  border-radius:10px;
}
.plan-box .meta b{display:block;margin-bottom:6px;}
.countdown{color:red;font-weight:700;}
.support-icon{
  display:flex;
  align-items:center;
  gap:6px;
  padding:10px;
  background:#eee;
  border-radius:8px;
  cursor:pointer;
}
</style>
</head>
<body>
<header>NEXA EARN</header>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<div id="dashUser">User: —</div>
<div id="dashBalance">Balance: Rs 0</div>
<div id="dashDaily">Daily Profit: Rs 0</div>
<div id="activeMembers">Active Members: <span>0</span></div>
<div id="specialOffers"></div>
<div id="plansList"></div>
<button onclick="logout()">Logout</button>
</div>

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

<div id="about" class="page hidden">
<h2>About NEXA</h2>
<p>NEXA EARN is a premium digital investment platform providing fast and reliable profit growth. Our team is ready to support you 24/7.</p>
<div class="support-icon" onclick="openSupport()">
  <span class="ico">🛠️</span>Support
</div>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plansPage')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// STORAGE
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let savedEndTimes = JSON.parse(localStorage.getItem('nexa_offerEndTimes')||'{}');

// PLANS DATA
let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*200;
  let days=25+Math.floor((i-1)/2);
  let multiplier = i<=5?2.4:2.2;
  let endTime = savedEndTimes[i] || (i<=5?Date.now()+24*60*60*1000:Date.now()+days*24*60*60*1000);
  if(i<=5) savedEndTimes[i]=endTime;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,multiplier,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),special:i<=5,endTime});
}
localStorage.setItem('nexa_offerEndTimes',JSON.stringify(savedEndTimes));

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username'); return;}
  currentUser=u;
  localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  updateDashboard();
}
function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}
function updateDashboard(){
  document.getElementById('dashUser').innerText='User: '+currentUser;
  document.getElementById('dashBalance').innerText='Balance: Rs '+balance;
  document.getElementById('dashDaily').innerText='Daily Profit: Rs '+dailyProfit;
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
}

// PLANS
function renderPlans(){
  let offersHTML='', plansHTML='';
  plansData.forEach(p=>{
    let cd='';
    if(p.special) cd=`<div class='countdown' id='cd${p.id}'></div>`+startCountdown(p.id);
    let html=`<div class='plan-box'><div class='meta'><b>${p.name}</b>
    Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}
    ${cd}</div>
    <button onclick='buyNow(${p.id})'>Buy Now</button></div>`;
    if(p.special) offersHTML+=html; else plansHTML+=html;
  });
  document.getElementById('specialOffers').innerHTML=offersHTML;
  document.getElementById('plansList').innerHTML=plansHTML;
}

// COUNTDOWN
function startCountdown(id){
  let el=document.getElementById('cd'+id);
  if(!el) return;
  let plan=plansData.find(p=>p.id===id);
  function update(){ 
    let diff = plan.endTime-Date.now();
    if(diff<=0){ el.innerText='Offer ended'; return;}
    let h=Math.floor(diff/3600000), m=Math.floor((diff%3600000)/60000), s=Math.floor((diff%60000)/1000);
    el.innerText=`Ends in: ${h}h ${m}m ${s}s`;
    setTimeout(update,1000);
  }update();
}

// BUY NOW
function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(balance<plan.invest){alert('Insufficient balance!'); return;}
  balance-=plan.invest;
  dailyProfit+=plan.daily;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  updateDashboard();
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
}

// DEPOSIT
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}
function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

updateDashboard();
</script>
</body>
</html>
