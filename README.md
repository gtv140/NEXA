<NEXA>
<html lang="ur">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA Dashboard</title>
<style>
:root{--neon1:#ff6ec7;--neon2:#1efff3;--dark:#111;}
*{box-sizing:border-box;}
body{
  margin:0;
  font-family:Arial,sans-serif;
  overflow-x:hidden;
  background:#111;
  color:#fff;
  animation:bgAnim 30s linear infinite;
}
@keyframes bgAnim{
  0%{background:linear-gradient(120deg,var(--neon1),var(--neon2));}
  25%{background:linear-gradient(120deg,#ff9a9e,#00f2fe);}
  50%{background:linear-gradient(120deg,#fbc2eb,#a18cd1);}
  75%{background:linear-gradient(120deg,#fad0c4,#ffd1ff);}
  100%{background:linear-gradient(120deg,var(--neon1),var(--neon2));}
}
header{
  text-align:center;
  font-size:28px;
  font-weight:800;
  padding:20px;
  color:#fff;
  text-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2);
}
.page{
  max-width:480px;
  margin:20px auto;
  background:rgba(255,255,255,0.05);
  padding:20px;
  border-radius:12px;
  border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 10px var(--neon1),0 0 20px var(--neon2);
}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:transparent;color:#fff;outline:none;font-size:14px;}
input::placeholder{color:rgba(255,255,255,0.7);}
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
.alert-box{background:rgba(255,0,136,0.12);color:#fff;box-shadow:0 0 10px #ff00ff inset;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;cursor:pointer;width:fit-content;font-weight:700;transition:0.15s all;}
.support-icon:hover{transform:translateY(-2px);box-shadow:0 0 20px var(--neon1),0 0 30px var(--neon2);}
.countdown{font-weight:700;color:var(--neon2);}
@media(max-width:480px){.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>NEXA</header>

<!-- DASHBOARD -->
<div id="dashboard" class="page">
  <div class="alert-box">خبردار: صرف آفیشل NEXA چینلز استعمال کریں۔ پاسورڈ شیئر نہ کریں۔</div>
  <div class="user-box">
    <div style="display:flex;gap:12px;align-items:center">
      <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--neon1),var(--neon2));display:flex;align-items:center;justify-content:center;color:#000;font-weight:900">N</div>
      <div>
        <div id="dashUser" style="font-size:16px;font-weight:800">—</div>
        <div class="small">رکن: <span id="dashSince">—</span></div>
      </div>
    </div>
    <div style="margin-top:10px">
      <div class="small">بیلنس</div>
      <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
      <div class="badge">ڈیلی: Rs <span id="dashDaily">0</span></div>
    </div>
  </div>

  <div class="alert-box">ایکٹو ممبرز: <span id="activeMembers">0</span></div>
  <div id="plansList"></div>

  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()">کاپی کریں</button>
    </div>
    <div class="small">دوستوں کو مدعو کریں، بونس خودکار طور پر شامل ہو جائے گا۔</div>
  </div>

  <button onclick="logout()">لاگ آوٹ</button>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
  <h2>ڈپازٹ</h2>
  <label>طریقہ</label>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">جیزکیش</option>
    <option value="easypaisa">ایزی پیسہ</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()">کاپی کریں</button>
  </div>
  <label>رقم</label>
  <input id="depositAmount" placeholder="رقم ڈالیں" />
  <label>TX ID</label>
  <input id="depositTxId" placeholder="TX ID" />
  <label>ثبوت اپ لوڈ کریں</label>
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">جمع کریں</button>
</div>

<!-- WITHDRAW -->
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
  <div id="historyList"></div>
</div>

<!-- ABOUT -->
<div id="about" class="page hidden">
  <h2>ہمارے بارے میں</h2>
  <p>نیکسہ ایک جدید اور محفوظ پلیٹ فارم ہے جہاں آپ چھوٹے سے بڑے پلانز میں سرمایہ کاری کر کے روزانہ منافع حاصل کر سکتے ہیں۔</p>
  <div class="support-icon" onclick="openSupport()"><span class="ico">🛠️</span> سپورٹ</div>
</div>

<!-- NAVIGATION -->
<div class="nav">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>ہوم</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>پلانز</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>ڈپازٹ</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>وڈڈراول</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>ہسٹری</div>
  <div onclick="showPage('about')"><span class="ico">ℹ️</span>ہمارے بارے میں</div>
</div>

<script>
// ===== STORAGE =====
let balance = parseFloat(localStorage.getItem('nexa_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('nexa_daily')||'0');
let referralCode = localStorage.getItem('nexa_referral')||'REF123';
let plansData=[];

// ===== PLANS 200 to 10000 =====
for(let i=1;i<=50;i++){
  let invest = 200*i;
  let days = 25+i;
  let multiplier = 2.2;
  plansData.push({id:i,name:`پلان ${i}`,invest,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),days});
}

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function copyReferral(){navigator.clipboard.writeText("https://gtv140.github.io/NEXA/"); alert('ریفرل لنک کاپی ہو گیا!');}
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=(method==='jazzcash')?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('ڈپازٹ نمبر کاپی ہو گیا!');}
function submitDeposit(){ alert('ڈپازٹ جمع ہو گیا!'); }
function submitWithdraw(){ alert('وڈڈراول ریکویسٹ ہو گیا!'); }
function openSupport(){ window.open("https://chat.whatsapp.com/yourlink","_blank"); }

// ===== DASHBOARD =====
document.getElementById('dashUser').innerText='NEXA USER';
document.getElementById('dashBalance').innerText=balance.toFixed(2);
document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
document.getElementById('dashSince').innerText=new Date().toLocaleDateString();

// ===== PLANS RENDER =====
const plansBox=document.getElementById('plansList');
plansData.forEach(p=>{
  const div=document.createElement('div'); div.className='plan-box';
  div.innerHTML=`<b>${p.name}</b> | سرمایہ: Rs ${p.invest} | کل: Rs ${p.total} | روزانہ: Rs ${p.daily} | دن: ${p.days} 
  <button onclick='buyNow(${p.id})'>اب خریدیں</button>`;
  plansBox.appendChild(div);
});
function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  document.getElementById('depositAmount').value=plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  showPage('deposit');
}

// ===== ACTIVE MEMBERS =====
function updateActiveMembers(){
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*500+50);
}
setInterval(updateActiveMembers,5000);
updateActiveMembers();
</script>
</body>
</html>
