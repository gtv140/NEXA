<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
:root { --primary:#1E90FF; --accent:#FF69B4; --bg:#0f0f0f; --text:#fff; }
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);}
header{font-size:28px;text-align:center;padding:20px;background:linear-gradient(90deg,var(--primary),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.page{max-width:480px;margin:20px auto;padding:20px;background:rgba(255,255,255,0.03);border-radius:12px;border:1px solid rgba(255,255,255,0.05);}
input, select, button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.2);background:transparent;color:#fff;}
button{background:linear-gradient(90deg,var(--primary),var(--accent));font-weight:700;cursor:pointer;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px 0;background:rgba(0,0,0,0.9);}
.nav div{text-align:center;cursor:pointer;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.plan-box{border:1px solid rgba(255,255,255,0.1);border-radius:10px;padding:10px;margin:10px 0;display:flex;justify-content:space-between;align-items:center;}
.countdown{color:var(--accent);font-weight:700;}
.user-box{border:1px solid rgba(255,255,255,0.1);padding:12px;border-radius:12px;margin-bottom:12px;}
.photos{display:flex;gap:6px;overflow-x:auto;margin-top:10px;}
.photos img{width:100px;height:70px;border-radius:8px;object-fit:cover;}
.ads{background:rgba(255,255,255,0.05);padding:10px;border-radius:10px;margin-top:10px;font-size:14px;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px;background:rgba(255,255,255,0.05);border-radius:10px;cursor:pointer;}
</style>
</head>
<body>
<header>NEXA EARN</header>

<!-- LOGIN -->
<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
<input id="username" placeholder="Enter Username"/>
<div style="position:relative;">
<input id="password" placeholder="Enter Password" type="password"/>
<span id="togglePass" style="position:absolute;right:10px;top:10px;cursor:pointer;">👁️</span>
</div>
<button onclick="login()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="user-box">
<div>Username: <span id="dashUser">-</span></div>
<div>Balance: Rs <span id="dashBalance">0</span></div>
<div>Daily Profit: Rs <span id="dashDaily">0</span></div>
<div>Active Members: <span id="dashActive">0</span></div>
</div>

<div class="photos" id="photosBox"></div>
<div class="ads" id="adsBox"></div>

<h3>Plans</h3>
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
<div style="display:flex;gap:8px;align-items:center;margin-top:10px">
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

<!-- ABOUT -->
<div id="about" class="page hidden">
<h2>About NEXA EARN</h2>
<p>NEXA EARN is a digital platform providing easy investment plans with auto profit calculation. Trusted by thousands of members.</p>
<div class="support-icon" onclick="openSupport()"><span class="ico">🛠️</span>Support</div>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('about')"><span class="ico">ℹ️</span>About</div>
</div>

<script>
// ===== STORAGE =====
let users = JSON.parse(localStorage.getItem('nexa_users')||'{}');
let currentUser = localStorage.getItem('nexa_currentUser')||null;

// ===== PHOTOS & ADS =====
const photoUrls = ['https://picsum.photos/200/120?1','https://picsum.photos/200/120?2','https://picsum.photos/200/120?3','https://picsum.photos/200/120?4','https://picsum.photos/200/120?5'];
const adTexts = ['Special 2.4x Profit Today!','Limited Offer Plans!','Earn Daily Rs 500+','Invest Now & Get Bonus','New Users Get Extra Profit'];

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// ===== LOGIN =====
document.getElementById('togglePass').addEventListener('click',function(){
  const pass = document.getElementById('password');
  if(pass.type==='password'){ pass.type='text'; this.textContent='🙈'; }
  else{ pass.type='password'; this.textContent='👁️'; }
});

function login(){
  const option=document.getElementById('userOption').value;
  const user=document.getElementById('username').value.trim();
  const pass=document.getElementById('password').value.trim();
  if(!user||!pass){ alert('Enter Username & Password'); return; }

  if(option==='signup'){
    if(users[user]){ alert('Username already exists'); return; }
    users[user] = {password:pass,balance:0,daily:0,plans:[]};
    alert('Signup successful');
  } else {
    if(!users[user] || users[user].password!==pass){ alert('Invalid login'); return; }
  }

  currentUser=user;
  localStorage.setItem('nexa_currentUser',currentUser);
  localStorage.setItem('nexa_users',JSON.stringify(users));
  updateDashboard();
}

// ===== DASHBOARD =====
function updateDashboard(){
  if(!currentUser) return;
  const data = users[currentUser];
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=data.balance.toFixed(2);
  document.getElementById('dashDaily').innerText=data.daily.toFixed(2);
  document.getElementById('dashActive').innerText=Math.floor(Math.random()*5000+1);
  showPage('dashboard');
  showPhotosAds();
  document.getElementById('bottomNav').classList.remove('hidden');
}

// ===== PHOTOS & ADS =====
function showPhotosAds(){
  const box=document.getElementById('photosBox');
  const adsBox=document.getElementById('adsBox');
  box.innerHTML=''; adsBox.innerHTML='';
  photoUrls.forEach(url=>{ const img=document.createElement('img'); img.src=url; box.appendChild(img); });
  adTexts.forEach(text=>{ const div=document.createElement('div'); div.textContent=text; adsBox.appendChild(div); });
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Number copied');}
function submitDeposit(){alert('Deposit submitted');}

// ===== WITHDRAWAL =====
function submitWithdraw(){alert('Withdrawal requested');}

// ===== SUPPORT =====
function openSupport(){window.open('https://chat.whatsapp.com/Example','_blank');}

// ===== LOGOUT =====
function logout(){currentUser=null; localStorage.removeItem('nexa_currentUser'); showPage('loginPage');}

// AUTO LOGIN IF ALREADY LOGGED IN
if(currentUser){updateDashboard();}
</script>
</body>
</html>
