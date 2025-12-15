<NEXA>
<html lang="ur">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA داشبورڈ</title>
<style>
:root{
  --primary:#ff4fa3;
  --secondary:#00f7ff;
  --dark:#0d0d0d;
  --light:#f1faff;
}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family: 'Noto Nastaliq Urdu', Arial, sans-serif;
  background:var(--dark);
  color:var(--light);
  overflow-x:hidden;
}
header{
  text-align:center;
  padding:24px 12px;
  font-size:30px;
  font-weight:800;
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
  animation: gradient 4s ease infinite alternate;
}
@keyframes gradient{
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}
.login-box, .page{
  max-width:460px;
  margin:18px auto;
  background:rgba(255,255,255,0.03);
  padding:20px;
  border-radius:16px;
  border:1px solid rgba(255,79,163,0.2);
  box-shadow:0 10px 30px rgba(0,0,0,0.6);
}
input,button,select{
  width:100%;
  padding:12px;
  margin-top:12px;
  border-radius:10px;
  border:1px solid rgba(255,255,255,0.1);
  background:transparent;
  color:var(--light);
  outline:none;
  font-size:15px;
  transition:0.2s;
}
input::placeholder{color:rgba(241,250,255,0.6);}
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
  box-shadow:0 10px 25px rgba(255,79,163,0.5);
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  background:rgba(255,255,255,0.02);
  display:flex;
  justify-content:space-around;
  padding:14px 6px;
  border-top:1px solid rgba(255,79,163,0.1);
  font-size:14px;
  gap:6px;
  z-index:999;
}
.nav div{text-align:center;cursor:pointer;width:60px;}
.nav div .ico{font-size:22px;margin-bottom:4px;transition:0.2s;}
.nav div:hover .ico{transform:scale(1.2);}
.hidden{display:none;}
.small{font-size:13px;color:rgba(241,250,255,0.7);}
.user-box{
  background:linear-gradient(90deg,rgba(255,79,163,0.1),rgba(0,247,255,0.05));
  padding:16px;
  border-radius:14px;
  margin-bottom:14px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  border:1px solid rgba(255,79,163,0.2);
  transition:0.3s;
}
.user-box:hover{box-shadow:0 8px 25px rgba(255,79,163,0.3);}
.plan-box{
  border:1px solid rgba(255,79,163,0.1);
  padding:14px;
  margin:12px 0;
  border-radius:14px;
  background:rgba(255,255,255,0.02);
  display:flex;
  gap:12px;
  align-items:center;
  transition:0.3s;
}
.plan-box:hover{transform:translateY(-3px);box-shadow:0 10px 25px rgba(255,79,163,0.4);}
.plan-box .meta{flex:1;}
.plan-box .actions{width:140px;text-align:right;}
.referral-box{
  background:rgba(255,255,255,0.02);
  padding:14px;
  border-radius:12px;
  margin:12px 0;
  border:1px solid rgba(255,79,163,0.1);
}
.referral-box input{background:transparent;border:1px dashed rgba(255,255,255,0.05);padding:10px;border-radius:10px;}
.support-icon{
  display:flex;
  align-items:center;
  gap:8px;
  padding:12px;
  margin-bottom:14px;
  border-radius:12px;
  background:linear-gradient(90deg, rgba(255,79,163,0.1), rgba(0,247,255,0.05));
  cursor:pointer;
  font-weight:700;
}
.support-icon:hover{box-shadow:0 6px 20px rgba(255,79,163,0.3);transform:translateY(-2px);}
@media(max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:24px}.logout-btn{padding:10px 12px;font-size:14px}}
</style>
</head>
<body>
<header>NEXA داشبورڈ</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
  <h2>لاگ ان / نیا صارف</h2>
  <select id="userOption">
    <option value="login">لاگ ان</option>
    <option value="signup">نیا صارف</option>
  </select>
  <input id="user" placeholder="صارف کا نام" />
  <input id="pass" placeholder="پاس ورڈ" type="password"/>
  <button onclick="login()">جمع کریں</button>
  <p class="small">یاد رکھیں: اسی ڈیوائس پر اکاؤنٹ محفوظ رہے گا۔</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="user-box">
    <div class="left">
      <div style="display:flex;gap:12px;align-items:center">
        <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--primary),var(--secondary));display:flex;align-items:center;justify-content:center;color:#001;font-weight:900">N</div>
        <div>
          <div id="dashUser" style="font-size:16px;font-weight:800">—</div>
          <div class="small">رکنیت کا آغاز: <span id="dashSince">—</span></div>
        </div>
      </div>
    </div>
    <div class="right">
      <div style="font-size:13px">بیلنس</div>
      <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
      <div style="margin-top:8px" class="badge">روزانہ: Rs <span id="dashDaily">0</span></div>
    </div>
  </div>
  <div id="activePlansBox"></div>
  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()">لنک کاپی کریں</button>
    </div>
    <div class="small">دوستوں کو مدعو کریں اور بونس خود بخود لگے گا۔</div>
  </div>
  <button class="logout-btn" onclick="logout()">لاگ آؤٹ</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>منصوبے</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>جمع کروائیں</h2>
  <label>طریقہ</label>
  <select id="depositMethod">
    <option value="jazzcash">جیز کیش</option>
    <option value="easypaisa">ایزی پیسہ</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()" style="width:110px">کاپی کریں</button>
  </div>
  <label>رقم</label>
  <input id="depositAmount" readonly />
  <label>ٹرانزیکشن آئی ڈی</label>
  <input id="depositTxId" placeholder="TX ID" />
  <label>ثبوت اپ لوڈ کریں</label>
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">جمع کریں</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
  <h2>رقم نکالیں</h2>
  <label>صارف کا نام</label>
  <input id="withdrawUser" readonly />
  <label>طریقہ</label>
  <select id="withdrawMethod">
    <option value="jazzcash">جیز کیش</option>
    <option value="easypaisa">ایزی پیسہ</option>
    <option value="bank">بینک</option>
  </select>
  <label>اکاؤنٹ نمبر</label>
  <input id="withdrawAccount" placeholder="اکاؤنٹ نمبر" />
  <label>رقم</label>
  <input id="withdrawAmount" placeholder="رقم" />
  <button onclick="submitWithdraw()">درخواست بھیجیں</button>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
  <h2>تاریخچہ</h2>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span>
    <div class="small">سپورٹ</div>
  </div>
  <div id="historyList"></div>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>ہوم</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>منصوبے</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>جمع</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>نکالیں</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>تاریخچہ</div>
</div>

<script>
let currentUser=null, balance=0, dailyProfit=0, plansData=[];
for(let i=1;i<=25;i++){
  let invest=200*i; if(invest>10000) invest=10000;
  let days=25 + i;
  plansData.push({id:i,name:`منصوبہ ${i}`,invest,days,total:Math.round(invest*2.5),daily:Math.round(invest*2.5/days)});
}
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const u=document.getElementById('user').value.trim(); if(!u){alert('صارف کا نام درج کریں'); return;}
  currentUser=u; balance=0; dailyProfit=0;
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('refLink').value=`https://nexa.example.com/?ref=${Math.random().toString(36).substring(2,10)}`;
  document.getElementById('withdrawUser').value=currentUser;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
}
function logout(){ location.reload(); }
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert('کاپی ہو گیا!');}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('کاپی ہو گیا!');}
function openSupport(){ window.open("https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu","_blank"); }
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div class='meta'><b>${p.name}</b><div class='small'>رقم: Rs ${p.invest} | مکمل: Rs ${p.total} | روزانہ: Rs ${p.daily} | دن: ${p.days}</div></div>
      <div class='actions'><button onclick='selectPlan(${p.id})'>اب خریدیں</button></div>`;
    list.appendChild(div);
  });
}
function selectPlan(id){
  const plan=plansData.find(p=>p.id===id); if(!plan) return;
  showPage('deposit');
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositNumber').value='03705519562';
}
function submitDeposit(){alert('جمع کروانے کی درخواست بھیج دی گئی!');}
function submitWithdraw(){alert('رقم نکالنے کی درخواست بھیج دی گئی!');}
</script>
</body>
</html>
