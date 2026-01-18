<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN Dashboard</title>
<style>
:root{
  --neon:#00f7ff;
  --accent:#ff5cff;
  --dark:#0a0a0a;
}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:linear-gradient(-45deg,#0a0a0a,#1a1a1a,#0f0f0f,#1f1f1f);
  background-size:400% 400%;
  animation:gradientBG 20s ease infinite;
  color:#fff;
  overflow-x:hidden;
}
@keyframes gradientBG{
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}
header{
  text-align:center;
  font-size:28px;
  padding:20px;
  background:linear-gradient(90deg,var(--neon),var(--accent));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.page{
  max-width:500px;
  margin:20px auto;
  padding:20px;
  background:rgba(255,255,255,0.02);
  border-radius:12px;
  border:1px solid rgba(0,255,240,0.06);
}
input,select,button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid rgba(0,255,240,0.08);
  background:transparent;
  color:#e6f7fb;
}
button{
  background:linear-gradient(90deg,var(--neon),var(--accent));
  color:#001;
  font-weight:700;
  cursor:pointer;
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  padding:10px 0;
  background:rgba(0,0,0,0.8);
}
.nav div{
  text-align:center;
  cursor:pointer;
}
.nav div .ico{
  font-size:20px;
  display:block;
  margin-bottom:4px;
}
.hidden{display:none;}
.support-icon{
  display:flex;
  align-items:center;
  gap:6px;
  padding:10px;
  background:rgba(0,255,240,0.06);
  border-radius:10px;
  cursor:pointer;
}
.support-icon:hover{
  box-shadow:0 6px 20px rgba(0,255,240,0.2);
  transform:translateY(-2px);
}
.plan-box{
  border:1px solid rgba(0,255,240,0.06);
  padding:12px;
  margin:10px 0;
  border-radius:10px;
  background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.04));
  display:flex;
  justify-content:space-between;
  align-items:center;
}
.countdown{font-weight:700;color:var(--neon);}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div id="balanceBox">Balance: Rs <span id="dashBalance">0</span></div>
  <div id="dailyBox">Daily Profit: Rs <span id="dashDaily">0</span></div>
  <div id="activeMembers">Active Members: <span>0</span></div>
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
  <div style="display:flex; gap:8px; align-items:center; margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">Copy</button>
  </div>
  <input id="depositAmount" placeholder="Enter Amount" />
  <input id="depositTxId" placeholder="Transaction ID" />
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- ABOUT & SUPPORT -->
<div id="about" class="page hidden">
  <h2>About NEXA</h2>
  <p>NEXA EARN is a premium digital investment platform providing fast, secure, and reliable profit growth opportunities. Our support team is always ready to assist you.</p>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span> Support
  </div>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
let currentUser=localStorage.getItem('nexa_user')||null;
let balance=parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit=parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans=JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let savedEndTimes=JSON.parse(localStorage.getItem('nexa_offerEndTimes')||'{}');

let plansData=[];
for(let i=1;i<=50;i++){
  let invest=200+(i-1)*100;
  let multiplier=i<=5?2.4:2.2;
  let days=25+Math.floor(i/2);
  let special=i<=5;
  let endTime=savedEndTimes[i]|| (special?new Date().getTime()+24*60*60*1000:0);
  if(special) savedEndTimes[i]=endTime;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),daily:Math.round(invest*multiplier/days),special,endTime});
}
localStorage.setItem('nexa_offerEndTimes',JSON.stringify(savedEndTimes));

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Enter username');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  balance=0; dailyProfit=0; userPlans=[];
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
  updateDashboard();
}

function logout(){currentUser=null; localStorage.removeItem('nexa_user'); location.reload();}

function updateDashboard(){
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
  simulateActiveMembers();
  showPage('dashboard');
}

function openSupport(){window.open('https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu','_blank');}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}

function submitDeposit(){alert('Deposit submitted');}

function renderPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    let countdownHTML='';
    if(p.special){countdownHTML=`<div class='countdown' id='countdown${p.id}'></div>`; startCountdown(p.id);}
    div.innerHTML=`<div><b>${p.name}</b><div>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>${countdownHTML}</div>
    <button onclick='buyNow(${p.id})'>Buy Now</button>`;
    list.appendChild(div);
  });
}

function buyNow(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan)return;
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  updateDepositNumber();
}

function startCountdown(id){
  const cdEl=document.getElementById(`countdown${id}`);
  const interval=setInterval(()=>{
    let remaining=plansData[id-1].endTime - new Date().getTime();
    if(remaining<=0){cdEl.innerText='Offer expired';clearInterval(interval);}
    else{
      let h=Math.floor(remaining/3600000);
      let m=Math.floor((remaining%3600000)/60000);
      let s=Math.floor((remaining%60000)/1000);
      cdEl.innerText=`Offer ends in ${h}h ${m}m ${s}s`;
    }
  },1000);
}

function simulateActiveMembers(){
  document.getElementById('activeMembers').children[0].innerText=Math.floor(Math.random()*50+50);
}
</script>
</body>
</html>
