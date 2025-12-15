<NEXA>
<html lang="ur">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Dashboard</title>
<style>
:root{--neon1:#ff00ff;--neon2:#00ffff;--dark:#111;}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family:Arial,sans-serif;
  overflow-x:hidden;
  animation:bgAnim 20s linear infinite;
  background:#111;
  color:#000;
}
@keyframes bgAnim{
  0%{background:linear-gradient(120deg,var(--neon1),var(--neon2));}
  25%{background:linear-gradient(120deg,var(--neon2),#ff4081);}
  50%{background:linear-gradient(120deg,#ff4081,#18ffff);}
  75%{background:linear-gradient(120deg,#18ffff,var(--neon1));}
  100%{background:linear-gradient(120deg,var(--neon1),var(--neon2));}
}
header{
  text-align:center;
  font-size:28px;
  font-weight:800;
  padding:20px;
  color:#fff;
  text-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2),0 0 30px var(--neon1),0 0 40px var(--neon2);
}
.login-box,.page{
  max-width:430px;
  margin:18px auto;
  background:rgba(255,255,255,0.1);
  padding:18px;
  border-radius:12px;
  border:1px solid rgba(255,255,255,0.05);
  box-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2);
}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:transparent;color:#fff;outline:none;font-size:14px;}
input::placeholder{color:rgba(255,255,255,0.6);}
button{
  background:linear-gradient(90deg,var(--neon1),var(--neon2));
  color:#000;
  font-weight:700;
  cursor:pointer;
  transition:0.2s all;
  box-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2);
}
button:hover{
  transform:translateY(-2px);
  box-shadow:0 0 20px var(--neon1),0 0 30px var(--neon2);
}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:12px 6px;font-size:14px;background:rgba(255,255,255,0.1);}
.nav div{text-align:center;cursor:pointer;width:64px;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(255,255,255,0.7);}
.user-box,.plan-box,.referral-box,.help-box,.alert-box,.support-icon{border-radius:10px;padding:12px;margin-bottom:12px;}
.user-box,.plan-box,.referral-box,.help-box{background:rgba(255,255,255,0.05);box-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2) inset;}
.alert-box{background:rgba(255,0,136,0.1);color:#fff;box-shadow:0 0 10px #ff00ff inset;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;cursor:pointer;width:fit-content;font-weight:700;transition:0.15s all;}
.support-icon:hover{transform:translateY(-2px);box-shadow:0 0 20px var(--neon1),0 0 30px var(--neon2);}
.countdown{font-weight:700;color:var(--neon2);}
#popup{position:fixed;top:20%;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,var(--neon1),var(--neon2));padding:20px;border-radius:12px;color:#000;font-weight:800;z-index:9999;text-align:center;}
#popup button{margin-top:10px;padding:8px 12px;border:none;border-radius:8px;background:#000;color:#fff;cursor:pointer;}
@media(max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>NEXA</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
  <h2>لاگ ان / سائن اپ</h2>
  <select id="userOption"><option value="login">لاگ ان</option><option value="signup">نیا یوزر</option></select>
  <input id="user" placeholder="یوزر نیم" />
  <input id="pass" placeholder="پاسورڈ" type="password"/>
  <button onclick="login()">سبمٹ</button>
  <p class="small">نوٹ: اسی ڈیوائس / براؤزر کا استعمال کریں تاکہ اکاؤنٹ محفوظ رہے۔</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="alert-box">خبردار: صرف آفیشل NEXA چینلز استعمال کریں۔ پاسورڈ شیئر نہ کریں۔</div>
  <div class="user-box">
    <div class="left">
      <div style="display:flex;gap:12px;align-items:center">
        <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--neon1),var(--neon2));display:flex;align-items:center;justify-content:center;color:#000;font-weight:900">N</div>
        <div>
          <div id="dashUser" style="font-size:16px;font-weight:800">—</div>
          <div class="small">رکن: <span id="dashSince">—</span></div>
        </div>
      </div>
    </div>
    <div class="right">
      <div class="small">بیلنس</div>
      <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
      <div class="badge">ڈیلی: Rs <span id="dashDaily">0</span></div>
    </div>
  </div>

  <div class="alert-box">ایکٹو ممبرز: <span id="activeMembers">0</span></div>
  <div id="activePlansBox"></div>

  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()">کاپی کریں</button>
    </div>
    <div class="small">دوستوں کو مدعو کریں، بونس خودکار طور پر شامل ہو جائے گا۔</div>
  </div>

  <button class="logout-btn" onclick="logout()">لاگ آوٹ</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>پلانز</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>ڈپازٹ</h2>
  <label>طریقہ</label>
  <select id="depositMethod">
    <option value="jazzcash">جیزکیش</option>
    <option value="easypaisa">ایزی پیسہ</option>
  </select>
  <input id="depositNumber" readonly />
  <label>رقم</label>
  <input id="depositAmount" placeholder="رقم ڈالیں" />
  <label>ٹرانزیکشن آئی ڈی</label>
  <input id="depositTxId" placeholder="TX ID" />
  <label>ثبوت اپ لوڈ کریں</label>
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">جمع کریں</button>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
  <h2>رقم نکالیں</h2>
  <label>طریقہ</label>
  <select id="withdrawMethod">
    <option value="jazzcash">جیزکیش</option>
    <option value="easypaisa">ایزی پیسہ</option>
  </select>
  <input id="withdrawAccount" placeholder="اکاؤنٹ نمبر" />
  <input id="withdrawAmount" placeholder="رقم" />
  <button onclick="submitWithdraw()">ریکویسٹ کریں</button>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
  <h2>ہسٹری</h2>
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span>
    <div class="small">سپورٹ</div>
  </div>
  <div id="historyList"></div>
</div>

<!-- ABOUT -->
<div id="about" class="page hidden">
  <h2>ہمارے بارے میں</h2>
  <p>نیکسہ ایک جدید پلیٹ فارم ہے جہاں آپ چھوٹے سے بڑے پلانز میں سرمایہ کاری کر کے روزانہ منافع حاصل کر سکتے ہیں۔ ہمارا مقصد محفوظ، شفاف اور جدید سروس فراہم کرنا ہے۔</p>
</div>

<!-- POPUP -->
<div id="popup" class="hidden">
  <span id="popupText"></span><br>
  <button onclick="closePopup()">بند کریں</button>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>ہوم</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>پلانز</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>ڈپازٹ</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>وڈڈراول</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>ہسٹری</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>ہمارے بارے میں</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('nexa_user')||null;
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('nexa_userPlans')||'[]');
let referralCode = localStorage.getItem('nexa_referral')||'';
let history = JSON.parse(localStorage.getItem('nexa_history')||'[]');
let totalUsers = parseInt(localStorage.getItem('nexa_totalUsers')||'1');

// ===== PLANS DATA =====
let plansData=[];
for(let i=1;i<=25;i++){
  let invest = 200*i;
  let days = 25 + i*2;
  let multiplier = 2 + i*0.05;
  plansData.push({id:i,name:`پلان ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days});
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const option=document.getElementById('userOption').value;
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('تمام فیلڈز بھریں');return;}
  currentUser=u; localStorage.setItem('nexa_user',currentUser);
  referralCode=referralCode||Math.random().toString(36).substring(2,10); localStorage.setItem('nexa_referral',referralCode);
  if(option==='signup'){ balance=0; dailyProfit=0; userPlans=[]; totalUsers++; localStorage.setItem('nexa_totalUsers',totalUsers);
    localStorage.setItem('nexa_balance',balance); localStorage.setItem('nexa_daily',dailyProfit); localStorage.setItem('nexa_userPlans',JSON.stringify(userPlans)); }
  updateDashboard();
}
function logout(){ currentUser=null; localStorage.removeItem('nexa_user'); location.reload(); }
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert('ریفرل لنک کاپی ہو گیا!');}

// ===== DASHBOARD =====
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('refLink').value=`https://example.com/?ref=${referralCode}`;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans(); renderHistory(); updateActiveMembers();
}

// ===== PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<b>${p.name}</b> | سرمایہ: Rs ${p.invest} | کل: Rs ${p.total} | روزانہ: Rs ${p.daily} | دن: ${p.days}
    <button onclick='buyNow(${p.id})'>اب خریدیں</button>`;
    list.appendChild(div);
  });
}
function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  document.getElementById('depositAmount').value=plan.invest;
  showPage('deposit');
}

// ===== DEPOSIT & WITHDRAW =====
function submitDeposit(){ alert('Deposit submitted!'); }
function submitWithdraw(){ alert('Withdrawal requested!'); }

// ===== HISTORY =====
function renderHistory(){
  const list=document.getElementById('historyList'); list.innerHTML='';
  history.forEach(h=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`${h}`;
    list.appendChild(div);
  });
}

// ===== SUPPORT =====
function openSupport(){ window.open("https://chat.whatsapp.com/yourlink","_blank"); }

// ===== ACTIVE MEMBERS =====
function updateActiveMembers(){
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*500+50);
}

// ===== POPUP =====
function closePopup(){ document.getElementById('popup').classList.add('hidden'); }

</script>
</body>
</html>
