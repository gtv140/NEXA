<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#111;color:#fff;}
.page{max-width:450px;margin:20px auto;padding:20px;border-radius:12px;background:rgba(255,255,255,0.05);}
input,button{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:none;background:rgba(255,255,255,0.1);color:#fff;}
button{cursor:pointer;font-weight:700;background:#FFD700;color:#000;}
button:hover{background:#FF3C38;color:#fff;}
.hidden{display:none;}
.box{padding:12px;margin:10px 0;border-radius:12px;background:rgba(255,255,255,0.05);}
</style>
</head>
<body>

<div id="loginPage" class="page">
<h2>Login / Signup</h2>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
</div>

<div id="dashboard" class="page hidden">
<div class="box">Username: <span id="dashUser"></span></div>
<div class="box">Balance: Rs <span id="dashBalance">0</span></div>
<div class="box">Daily Profit: Rs <span id="dashDaily">0</span></div>
<div class="box">Total Profit: Rs <span id="dashTotal">0</span></div>
<div class="box">Active Members: <span id="activeMembers">0</span></div>
<button onclick="logout()">Logout</button>
</div>

<script>
let users = JSON.parse(localStorage.getItem('nexa_users')||'{}');
let currentUser = localStorage.getItem('nexa_current')||null;

function saveUsers(){localStorage.setItem('nexa_users',JSON.stringify(users));}

function login(){
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username & password'); return;}
  
  if(users[u]){
    if(users[u].pass!==p){alert('Incorrect password'); return;}
  }else{
    // new user signup
    users[u]={pass:p,balance:0,daily:0,total:0};
    saveUsers();
  }
  currentUser=u;
  localStorage.setItem('nexa_current',currentUser);
  updateDashboard();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('nexa_current');
  showPage('loginPage');
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function updateDashboard(){
  if(!currentUser) return showPage('loginPage');
  let userData = users[currentUser];
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=userData.balance;
  document.getElementById('dashDaily').innerText=userData.daily;
  document.getElementById('dashTotal').innerText=userData.total;
  document.getElementById('activeMembers').innerText=Math.floor(Math.random()*5000+100);
  showPage('dashboard');
}

// on load check if already logged in
if(currentUser && users[currentUser]){
  updateDashboard();
} else{
  showPage('loginPage');
}
</script>

</body>
</html>
