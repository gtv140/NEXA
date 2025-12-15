<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Dashboard</title>
<style>
:root{
  --primary:#00f7ff;
  --secondary:#ff5cff;
  --dark:#0b0b0b;
  --light:#e6f7fb;
}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family:Inter, Arial, sans-serif;
  background:var(--dark);
  color:var(--light);
  overflow-x:hidden;
  transition:0.3s;
}
header{
  text-align:center;
  padding:24px 12px;
  font-size:28px;
  font-weight:800;
  letter-spacing:2px;
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
  animation: gradient 3s ease infinite alternate;
}
@keyframes gradient{
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}
.login-box,.page{
  max-width:450px;
  margin:18px auto;
  background:rgba(255,255,255,0.02);
  padding:20px;
  border-radius:14px;
  border:1px solid rgba(0,255,240,0.06);
  box-shadow:0 8px 30px rgba(0,0,0,0.6);
  transition:0.3s;
}
input,button,select{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:1px solid rgba(0,255,240,0.08);
  background:transparent;
  color:var(--light);
  outline:none;
  font-size:14px;
  transition:0.2s;
}
input::placeholder{color:rgba(230,247,251,0.5);}
button{
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  border:none;
  color:#001;
  font-weight:700;
  cursor:pointer;
  padding:12px;
  border-radius:12px;
  transition:0.2s;
}
button:hover{
  transform:translateY(-3px);
  box-shadow:0 12px 25px rgba(0,255,240,0.5);
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  background:rgba(255,255,255,0.02);
  display:flex;
  justify-content:space-around;
  padding:12px 6px;
  border-top:1px solid rgba(0,255,240,0.04);
  font-size:14px;
  gap:6px;
  z-index:999;
}
.nav div{
  text-align:center;
  cursor:pointer;
  width:60px;
}
.nav div .ico{font-size:22px;display:block;margin-bottom:4px;transition:0.2s;}
.nav div:hover .ico{transform:scale(1.2);}
.hidden{display:none;}
.small{font-size:13px;color:rgba(230,247,251,0.7);}
.user-box{
  background:linear-gradient(90deg,rgba(0,255,240,0.06),rgba(255,92,255,0.02));
  padding:14px;
  border-radius:12px;
  margin-bottom:12px;
  font-weight:700;
  display:flex;
  justify-content:space-between;
  align-items:center;
  gap:10px;
  border:1px solid rgba(0,255,240,0.06);
  transition:0.3s;
}
.user-box:hover{box-shadow:0 10px 30px rgba(0,255,240,0.3);}
.user-box .left{display:flex;flex-direction:column;gap:6px;}
.user-box .right{text-align:right;font-size:13px;}
.badge{background:rgba(0,255,240,0.06);padding:6px 10px;border-radius:999px;color:var(--primary);font-weight:800;}
.alert-box{background:linear-gradient(90deg, rgba(255,0,136,0.04), rgba(0,0,0,0.02));padding:12px;border-radius:10px;margin-bottom:12px;color:var(--secondary);border:1px solid rgba(255,0,136,0.06);box-shadow:0 6px 20px rgba(255,0,136,0.03) inset;}
.plan-box{
  border:1px solid rgba(0,255,240,0.06);
  padding:12px;
  margin:10px 0;
  border-radius:12px;
  background:rgba(255,255,255,0.01);
  display:flex;
  gap:12px;
  align-items:center;
  transition:0.3s;
}
.plan-box:hover{transform:translateY(-3px);box-shadow:0 12px 25px rgba(0,255,240,0.4);}
.plan-box .meta{flex:1;}
.plan-box .meta b{display:block;margin-bottom:6px;font-size:15px;}
.plan-box .actions{width:140px;text-align:right;}
.referral-box{background:rgba(255,255,255,0.01);padding:12px;border-radius:12px;margin:10px 0;border:1px solid rgba(0,255,240,0.04);}
.referral-box input{background:transparent;border:1px dashed rgba(255,255,255,0.03);padding:8px;border-radius:8px;}
.support-icon{
  display:flex;
  align-items:center;
  gap:6px;
  padding:10px 12px;
  margin-bottom:12px;
  border-radius:12px;
  background:linear-gradient(90deg, rgba(0,255,240,0.06), rgba(255,92,255,0.02));
  cursor:pointer;
  font-weight:700;
}
.support-icon:hover{box-shadow:0 6px 20px rgba(0,255,240,0.3);transform:translateY(-2px);}
@media(max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}.logout-btn{padding:10px 12px;font-size:14px}}
</style>
</head>
<body>
<header>NEXA</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
  <h2>Login / Signup</h2>
  <select id="userOption">
    <option value="login">Login</option>
    <option value="signup">Naya User</option>
  </select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
  <p class="small">Tip: Same device pe account save rahega.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="alert-box">Sirf official NEXA channels use karein.</div>
  <div class="user-box">
    <div class="left">
      <div style="display:flex;gap:12px;align-items:center">
        <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;color:#001;font-weight:900">N</div>
        <div>
          <div id="dashUser" style="font-size:16px;font-weight:800">—</div>
          <div class="small">Member since: <span id="dashSince">—</span></div>
        </div>
      </div>
    </div>
    <div class="right">
      <div style="font-size:13px">Balance</div>
      <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
      <div style="margin-top:8px" class="badge">Daily: Rs <span id="dashDaily">0</span></div>
    </div>
  </div>
  <div id="activePlansBox"></div>
  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()">Copy Link</button>
    </div>
    <div class="small">Invite friends aur bonus automatic.</div>
  </div>
  <button class="logout-btn" onclick="logout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <label>Method</label>
  <select id="depositMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()" style="width:110px">Copy</button>
  </div>
  <label style="margin-top:12px">Amount</label>
  <input id="depositAmount" readonly />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
  <h2>History</h2>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span>
    <div class="small">Support</div>
  </div>
  <div id="historyList"></div>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>History</div>
</div>

<script>
let currentUser=null, balance=0, dailyProfit=0, userPlans=[], referralCode='', historyData=[];
let plansData=[];
for(let i=1;i<=25;i++){
  let invest=200*i; if(invest>10000) invest=10000;
  let days=25 + i;
  plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*2.5),daily:Math.round(invest*2.5/days)});
}
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}
function login(){
  const u=document.getElementById('user').value.trim();
  if(!u){alert('Username daalein'); return;}
  currentUser=u; balance=0; dailyProfit=0; referralCode=Math.random().toString(36).substring(2,10);
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('refLink').value=`https://nexa.example.com/?ref=${referralCode}`;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
}
function logout(){ location.reload(); }
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert('Copied!');}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Copied!');}
function openSupport(){ window.open("https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu","_blank"); }
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div class='meta'><b>${p.name}</b><div class='small'>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div></div>
      <div class='actions'><button onclick='selectPlan(${p.id})'>Buy Now</button></div>`;
    list.appendChild(div);
  });
}
function selectPlan(id){
  const plan=plansData.find(p=>p.id===id);
  if(!plan) return;
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositNumber').value='03705519562';
}
</script>
</body>
</html>
