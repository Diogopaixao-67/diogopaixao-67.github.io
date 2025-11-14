<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Playmates</title>

<!-- Firebase -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.5/firebase-app.js";
import { getDatabase, ref, set, get, update, onValue, push, remove } from "https://www.gstatic.com/firebasejs/10.12.5/firebase-database.js";
import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.12.5/firebase-storage.js";

// ================= CONFIGURAÇÃO FIREBASE =================
const firebaseConfig = {
  apiKey: "AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain: "playmates-cc4f7.firebaseapp.com",
  databaseURL: "https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId: "playmates-cc4f7",
  storageBucket: "playmates-cc4f7.firebasestorage.app",
  messagingSenderId: "104004735810",
  appId: "1:104004735810:web:d3ee9a75399d6f0f222edb"
};
const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const storage = getStorage(app);

let currentUser = null;

// ================= FUNÇÕES LOGIN / CADASTRO =================
window.registerUser = function() {
  let name = document.getElementById("regName").value.trim();
  let school = document.getElementById("regSchool").value.trim();
  let phone = document.getElementById("regPhone").value.trim();
  let pass = document.getElementById("regPass").value.trim();
  let photo = document.getElementById("regPhoto").files[0];
  if(!name||!school||!phone||!pass||!photo){ alert("Preencha tudo."); return; }
  const userRef = ref(db,"users/"+phone);
  get(userRef).then(s=>{
    if(s.exists()){ alert("Número já cadastrado."); return; }
    let photoRef = sRef(storage,"profilePhotos/"+phone);
    uploadBytes(photoRef,photo).then(()=>{
      getDownloadURL(photoRef).then(url=>{
        set(userRef,{
          name, school, phone, password:pass, photo:url,
          points:0, attempts:0, blocked:false
        }).then(()=>alert("Cadastro feito! Faça login."));
      });
    });
  });
};

window.login = function() {
  let phone = document.getElementById("loginPhone").value.trim();
  let pass = document.getElementById("loginPass").value.trim();
  if(!phone||!pass){ alert("Preencha tudo."); return; }
  const userRef = ref(db,"users/"+phone);
  get(userRef).then(s=>{
    if(!s.exists()){ alert("Conta não existe."); return; }
    let u = s.val();
    if(u.blocked){ alert("Conta bloqueada!"); return; }
    if(u.password!==pass){
      let tries=(u.attempts||0)+1;
      update(userRef,{attempts:tries});
      if(tries>=4){ update(userRef,{blocked:true}); alert("Conta bloqueada."); }
      else alert("Senha errada "+tries+"/4");
      return;
    }
    update(userRef,{attempts:0});
    currentUser = phone;
    document.getElementById("loginScreen").style.display="none";
    document.getElementById("appScreen").style.display="block";
    loadProfile();
    showTab('dashboard');
    updateUserCount();
  });
};

// ================= PERFIL =================
function loadProfile(){
  if(!currentUser) return;
  const userRef = ref(db,"users/"+currentUser);
  onValue(userRef,snap=>{
    if(!snap.exists()) return;
    let u = snap.val();
    document.getElementById("pPhoto").src=u.photo;
    document.getElementById("pName").innerText=u.name;
    document.getElementById("pSchool").innerText=u.school;
    document.getElementById("pPhone").innerText=u.phone;
    document.getElementById("pPoints").innerText=u.points+" pts";
  });
}

window.editProfile = function(){
  if(!currentUser) return;
  let name = prompt("Nome:",document.getElementById("pName").innerText);
  let school = prompt("Escola:",document.getElementById("pSchool").innerText);
  let phone = prompt("Número:",document.getElementById("pPhone").innerText);
  let fileInput = document.createElement("input");
  fileInput.type="file";
  fileInput.onchange=function(){
    let file = fileInput.files[0];
    let photoRef = sRef(storage,"profilePhotos/"+phone);
    uploadBytes(photoRef,file).then(()=>getDownloadURL(photoRef).then(url=>{
      update(ref(db,"users/"+currentUser),{name,school,phone,photo:url});
      alert("Perfil atualizado!");
    }));
  };
  fileInput.click();
};

// ================= RELÓGIO =================
let timerInterval=null;
function startCountdown() {
  onValue(ref(db,"timer"),snap=>{
    if(!snap.exists()) return;
    let t = snap.val();
    let end=t.endAt;
    clearInterval(timerInterval);
    timerInterval=setInterval(()=>{
      let now=Date.now();
      let diff=end-now;
      if(diff<=0){ document.getElementById("clock").innerText="00:00:00"; clearInterval(timerInterval); return; }
      let h=Math.floor(diff/3600000), m=Math.floor((diff%3600000)/60000), s=Math.floor((diff%60000)/1000);
      document.getElementById("clock").innerText=(h<10?"0"+h:h)+":"+(m<10?"0"+m:m)+":"+(s<10?"0"+s:s);
    },1000);
  });
}
startCountdown();

window.editClock=function(){
  let pw=prompt("Senha:");
  if(pw!=="LEX"){ alert("Senha incorreta."); return; }
  let h=prompt("Horas:"), m=prompt("Minutos:"), s=prompt("Segundos:");
  let total=Number(h)*3600000+Number(m)*60000+Number(s)*1000;
  update(ref(db,"timer"),{endAt:Date.now()+total});
  alert("Relógio atualizado.");
};

// ================= SMS =================
window.sendSMS=function(){
  if(!currentUser) return alert("Faça login.");
  let to=prompt("Número destinatário:"), msg=prompt("Mensagem até 130 caracteres:");
  if(!to||!msg||msg.length>130) return alert("Mensagem inválida.");
  let smsRef=push(ref(db,"sms"));
  let now=Date.now();
  set(smsRef,{from:currentUser,to:to,msg,createdAt:now,expiresAt:now+3*3600*1000});
  alert("SMS enviado!");
};

// ================= POSTAGENS =================
window.addPost=function(){
  if(!currentUser) return alert("Faça login.");
  let text=prompt("Texto da postagem:");
  let fileInput=document.createElement("input"); fileInput.type="file";
  fileInput.onchange=function(){
    let file=fileInput.files[0];
    let storageRef=sRef(storage,"posts/"+Date.now()+"_"+file.name);
    uploadBytes(storageRef,file).then(()=>getDownloadURL(storageRef).then(url=>{
      let postRef=push(ref(db,"posts"));
      set(postRef,{user:currentUser,text:text||"",img:url,createdAt:Date.now(),expiresAt:Date.now()+3*3600*1000});
      alert("Postagem criada!");
    }));
  };
  fileInput.click();
};

// ================= PESQUISA / TOPAR =================
function renderSearchResults(query){
  const resultsDiv=document.getElementById("searchResults");
  resultsDiv.innerHTML="";
  onValue(ref(db,"users"),snap=>{
    if(!snap.exists()) return;
    let data=snap.val();
    Object.keys(data).forEach(phone=>{
      let u=data[phone];
      if(u.name.toLowerCase().includes(query.toLowerCase())||
         u.school.toLowerCase().includes(query.toLowerCase())||
         u.phone.includes(query)){
        let div=document.createElement("div");
        div.className="box";
        div.innerHTML=`
          <img src="${u.photo}" style="width:50px;height:50px;border-radius:50%;vertical-align:middle;">
          <strong>${u.name}</strong> | ${u.school} | ${u.phone} | ${u.points} pts
          <button id="topBtn_${phone}">Topar</button>
        `;
        resultsDiv.appendChild(div);
        document.getElementById(`topBtn_${phone}`).onclick=function(){
          const topRef=ref(db,`topados/${currentUser}_${phone}`);
          get(topRef).then(s=>{
            if(s.exists()) return alert("Você já topou.");
            set(topRef,true);
            let userRef=ref(db,"users/"+phone);
            get(userRef).then(us=>{
              let points=us.val().points||0; points*=1.005;
              update(userRef,{points});
            });
            alert(`Você topou ${u.name}. Ele ganhou 0.5%!`);
          });
        };
      }
    });
  });
}
window.searchUsers=function(){ let q=document.getElementById("searchInput").value.trim(); renderSearchResults(q); };

// ================= SOS EMPREGO =================
window.addSOS=function(){
  let pw=prompt("Senha admin LEX:");
  if(pw!=="LEX"){ alert("Senha errada"); return; }
  let text=prompt("Texto SOS:");
  let fileInput=document.createElement("input"); fileInput.type="file";
  fileInput.onchange=function(){
    let file=fileInput.files[0];
    let storageRef=sRef(storage,"sos/"+Date.now()+"_"+file.name);
    uploadBytes(storageRef,file).then(()=>getDownloadURL(storageRef).then(url=>{
      let postRef=push(ref(db,"sos"));
      set(postRef,{text:text||"",img:url,createdAt:Date.now(),expiresAt:Date.now()+24*3600*1000});
      alert("SOS publicado!");
    }));
  };
  fileInput.click();
};

// ================= NOTIFICAÇÃO PADRÃO =================
const notifRef = ref(db,"notifications/default");
set(notifRef,"Olá eu sou Diogo paixão fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil nesta plataforma");

// ================= CONTADOR TOTAL DE USUÁRIOS =================
function updateUserCount(){
  onValue(ref(db,"users"),snap=>{
    let count = snap.exists()? Object.keys(snap.val()).length : 0;
    document.getElementById("totalUsers").innerText = "Total de usuários: "+count;
  });
}

// ================= LIMPEZA AUTOMÁTICA =================
setInterval(()=>{
  let now=Date.now();
  ["sms","posts","sos"].forEach(path=>{
    onValue(ref(db,path),snap=>{
      if(!snap.exists()) return;
      snap.forEach(child=>{
        if(child.val().expiresAt && child.val().expiresAt<now)
          remove(ref(db,path+"/"+child.key));
      });
    });
  });
},60*1000);

// ================= ABAS =================
function showTab(tab){
  ["dashboard","smsTab","searchTab","postTab","sosTab"].forEach(t=>{
    document.getElementById(t).style.display=(t===tab)?"block":"none";
  });
}
</script>

<style>
body{margin:0;font-family:Arial;background:#f5f5f5;}
header{background:#ff7f00;padding:12px;text-align:center;color:white;font-size:22px;font-weight:bold;}
nav{display:flex;justify-content:space-around;background:#ffa500;position:fixed;bottom:0;width:100%;padding:8px 0;}
nav button{background:none;color:white;font-weight:bold;font-size:14px;cursor:pointer;border:none;}
.screen{padding:20px;padding-bottom:60px;}
input,button{width:100%;padding:12px;margin-top:10px;border-radius:8px;border:none;font-size:15px;}
button{background:#ff7f00;color:white;font-weight:bold;cursor:pointer;}
#pPhoto{width:120px;height:120px;border-radius:50%;object-fit:cover;}
.box{background:white;padding:15px;border-radius:12px;margin-top:15px;}
#clock{font-size:30px;font-weight:bold;margin-top:15px;}
#totalUsers{background:#ffcc80;padding:10px;border-radius:8px;text-align:center;margin-top:10px;}
</style>
</head>

<body>

<header>Playmates</header>

<!-- LOGIN / CADASTRO -->
<div id="loginScreen" class="screen">
<h2>Login</h2>
<input id="loginPhone" placeholder="Número de telefone">
<input id="loginPass" placeholder="Senha" type="password">
<button onclick="login()">Login</button>

<h2>Cadastro</h2>
<input id="regName" placeholder="Nome completo">
<input id="regSchool" placeholder="Escola">
<input id="regPhone" placeholder="Número de telefone">
<input id="regPass" placeholder="Senha" type="password">
<input id="regPhoto" type="file">
<button onclick="registerUser()">Cadastrar</button>
</div>

<!-- APLICAÇÃO PRINCIPAL -->
<div id="appScreen" class="screen" style="display:none;">

<div id="dashboard">
<img id="pPhoto">
<p id="pName"></p>
<p id="pSchool"></p>
<p id="pPhone"></p>
<p id="pPoints"></p>
<button onclick="editProfile()">Editar Perfil</button>
<div id="clock">00:00:00 <button onclick="editClock()">⏰</button></div>
<div id="totalUsers">Total de usuários: 0</div>
</div>

<div id="smsTab" style="display:none;">
<h2>Enviar SMS</h2>
<button onclick="sendSMS()">Enviar SMS</button>
</div>

<div id="searchTab" style="display:none;">
<h2>Pesquisa de Usuários</h2>
<input id="searchInput" placeholder="Nome, Escola ou Número">
<button onclick="searchUsers()">Buscar</button>
<div id="searchResults"></div>
</div>

<div id="postTab" style="display:none;">
<h2>Postagens</h2>
<button onclick="addPost()">Nova Postagem</button>
</div>

<div id="sosTab" style="display:none;">
<h2>SOS Emprego (Admin)</h2>
<button onclick="addSOS()">Postar SOS</button>
</div>

<nav>
<button onclick="showTab('dashboard')">Dashboard</button>
<button onclick="showTab('smsTab')">SMS</button>
<button onclick="showTab('searchTab')">Pesquisa</button>
<button onclick="showTab('postTab')">Postagens</button>
<button onclick="showTab('sosTab')">SOS</button>
</nav>

</body>
</html>
