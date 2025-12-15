<NEXA>
<html lang="ur">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Dashboard</title>
<style>
:root{
  --primary:#ff4081;
  --secondary:#18ffff;
  --dark:#0a0a0a;
  --light:#ffffff;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:'Segoe UI',sans-serif;}
body{
  background:#0a0a0a;
  color:var(--light);
  overflow-x:hidden;
  animation:bgAnim 30s linear infinite;
}
@keyframes bgAnim{
  0%{background:linear-gradient(120deg,#ff4081,#18ffff);}
  25%{background:linear-gradient(120deg,#ff9e18,#ff4081);}
  50%{background:linear-gradient(120deg,#18ffff,#ff9e18);}
  75%{background:linear-gradient(120deg,#ff4081,#18ffff);}
  100%{background:linear-gradient(120deg,#ff4081,#18ffff);}
}
header{
  text-align:center;
  font-size:28px;
  font-weight:800;
  padding:20px;
  color:var(--light);
  text-shadow:0 0 10px var(--primary);
}
.page, .login-box{
  max-width:450px;
  margin:20px auto;
  background:rgba(0,0,0,0.6);
  border-radius:12px;
  padding:18px;
  box-shadow:0 8px 20px rgba(0,0,0,0.8);
}
input, select, button{
  width:100%;
  padding:10px;
  margin-top:10px;
  border-radius:8px;
  border:none;
  outline:none;
  font-size:14px;
}
button{
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  color:#000;
  font-weight:700;
  cursor:pointer;
  transition:0.2s all;
}
button:hover{
  transform:translateY(-2px);
  box-shadow:0 5px 15px rgba(0,0,0,0.5);
}
.nav{
  position:fixed;
  bottom:0;
  left:0;
  right:0;
  display:flex;
  justify-content:space-around;
  background:rgba(0,0,0,0.6);
  padding:12px 0;
  border-top:1px solid var(--secondary);
}
.nav div{
  text-align:center;
  cursor:pointer;
}
.nav div .ico{
  font-size:20px;
}
.hidden{display:none;}
.user-box{
  display:flex;
  justify-content:space-between;
  padding:12px;
  margin-bottom:12px;
  border-radius:12px;
  background:rgba(255,255,255,0.05);
}
.plan-box{
  background:rgba(255,255,255,0.05);
  padding:12px;
  margin:10px 0;
  border-radius:10px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  transition:0.2s all;
}
.plan-box:hover{
  transform:translateY(-3px);
  box-shadow:0 8px 25px rgba(0,0,0,0.5);
}
.referral-box, .help-box{
  background:rgba(255,255,255,0.05);
  padding:10px;
  border-radius:10px;
  margin:10px 0;
}
.support-icon{
  display:flex;
  align-items:center;
  gap:8px;
  padding:10px;
  border-radius:10px;
  background:rgba(0,255,255,0.1);
  cursor:pointer;
  margin-bottom:12px;
}
.support-icon:hover{
  box-shadow:0 5px 20px rgba(0,255,255,0.3);
}
#popup{
  position:fixed;
  top:20%;
  left:50%;
  transform:translateX(-50%);
  background:linear-gradient(90deg,var(--primary),var(--secondary));
  padding:20px;
  border-radius:12px;
  color:#000;
  font-weight:700;
  z-index:9999;
  text-align:center;
}
#popup button{
  margin-top:10px;
  padding:8px 12px;
  border:none;
  border-radius:8px;
  background:#000;
  color:#fff;
  cursor:pointer;
}
</style>
</head>
<body>
<header>NEXA ڈیش بورڈ</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
  <h2>لاگ ان / رجسٹریشن</h2>
  <select id="userOption"><option value="login">لاگ ان</option><option value="signup">نیا صارف</option></select>
  <input id="user" placeholder="صارف نام" />
  <input id="pass" type="password" placeholder="پاس ورڈ"/>
  <button onclick="login()">جمع کریں</button>
  <p class="small">مشورہ: ایک ہی براؤزر استعمال کریں تاکہ اکاؤنٹ محفوظ رہے۔</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="user-box">
    <div>
      <b>صارف:</b> <span id="dashUser">—</span><br>
      <b>بیلنس:</b> Rs <span id="dashBalance">0</span><br>
      <b>روزانہ منافع:</b> Rs <span id="dashDaily">0</span>
    </div>
    <div>
      <b>تاریخ رجسٹریشن:</b> <span id="dashSince">—</span>
    </div>
  </div>
  <div id="activeMembersBox">فعال ممبرز: <span id="activeMembers">0</span></div>
  <div id="plansList"></div>
  <div class="referral-box">
    <input id="refLink" readonly />
    <button onclick="copyReferral()">لنک کاپی کریں</button>
  </div>
  <div class="help-box">
    <div class="support-icon" onclick="openSupport()"><span>🛠️</span> سپورٹ</div>
    <div>
      <h3>کمپنی کے بارے میں</h3>
      <p>نیکسا ایک جدید سرمایہ کاری پلیٹ فارم ہے جو محفوظ اور شفاف سرمایہ کاری کی سہولت دیتا ہے۔ ہمارا مقصد صارفین کو بہترین منافع کے مواقع فراہم کرنا ہے۔</p>
    </div>
  </div>
  <button onclick="logout()">لاگ آؤٹ</button>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>جمع کریں</h2>
  <label>طریقہ</label>
  <select id="depositMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <input id="depositAmount" placeholder="رقم درج کریں" />
  <input id="depositTxId" placeholder="ٹرانزیکشن ID" />
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">جمع کریں</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
  <h2>رقم نکالیں</h2>
  <label>طریقہ</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
    <option value="bank">بینک</option>
  </select>
  <input id="withdrawAccount" placeholder="اکاؤنٹ نمبر / فون نمبر" />
  <input id="withdrawAmount" placeholder="رقم" />
  <button onclick="submitWithdraw()">درخواست بھیجیں</button>
</div>

<!-- POPUP -->
<div id="popup" class="hidden">
  <span id="popupText"></span><br>
  <button onclick="closePopup()">بند کریں</button>
</div>

<!-- NAVIGATION -->
<div class="nav">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span> ہوم</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span> جمع کریں</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span> نکالیں</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user') || null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let referralCode = localStorage.getItem('nexa_referral')||'';
let totalUsers = parseInt(localStorage.getItem('nexa_totalUsers')||'1');

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const option = document.getElementById('userOption').value;
  const u = document.getElementById('user').value.trim();
  const p = document.getElementById('pass').value.trim();
  if(!u||!p){alert('صارف نام اور پاس ورڈ درج کریں'); return;}
  currentUser = u; localStorage.setItem('nexa_user',currentUser);
  referralCode = referralCode || Math.random().toString(36).substring(2,10); localStorage.setItem('nexa_referral',referralCode);
  if(option==='signup'){ balance=0; dailyProfit=0; userPlans=[]; totalUsers++; 
    localStorage.setItem('nexa_balance',balance); 
    localStorage.setItem('nexa_daily',dailyProfit);
    localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans));
    localStorage.setItem('nexa_totalUsers',totalUsers);
  }
  updateDashboard();
}
function logout(){ location.reload(); }
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert('لنک کاپی ہوگیا!');}

// ===== DASHBOARD =====
function updateDashboard(){
  document.getElementById('dashUser').innerText = currentUser;
  document.getElementById('dashBalance').innerText = balance.toFixed(2);
  document.getElementById('dashDaily').innerText = dailyProfit.toFixed(2);
  document.getElementById('dashSince').innerText = new Date().toLocaleDateString();
  document.getElementById('refLink').value = `https://example.com/?ref=${referralCode}`;
  renderPlans();
  updateActiveMembers();
}

function renderPlans(){
  const list=document.getElementById('plansList');
  list.innerHTML='';
  for(let i=1;i<=25;i++){
    let invest=200*i;
    let daily=Math.round(invest/25);
    const div = document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div><b>پلان ${i}</b><br>سرمایہ: Rs ${invest}<br>روزانہ: Rs ${daily}<br>مدت: 25 دن</div><button onclick="buyPlan(${invest},${daily})">خریدیں</button>`;
    list.appendChild(div);
  }
}

function buyPlan(amount,daily){
  document.getElementById('depositAmount').value=amount;
  dailyProfit += daily;
  balance -= amount;
  localStorage.setItem('nexa_balance',balance);
  localStorage.setItem('nexa_daily',dailyProfit);
  showPage('deposit');
}

function updateActiveMembers(){
  document.getElementById('activeMembers').innerText = Math.floor(Math.random()*500 + 50);
  setTimeout(updateActiveMembers,5000);
}

// ===== SUPPORT =====
function openSupport(){ window.open("https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu","_blank"); }

function submitDeposit(){ alert('جمع کر دیا گیا!'); }
function submitWithdraw(){ alert('درخواست بھیج دی گئی!'); }
function closePopup(){ document.getElementById('popup').classList.add('hidden'); }
</script>
</body>
</html>
