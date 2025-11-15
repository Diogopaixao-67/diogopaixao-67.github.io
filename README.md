<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Playmates — Plataforma Oficial</title>

<!-- ====== ESTILOS ====== -->
<style>
:root{
  --bg:#f6f7fb; --card:#ffffff; --muted:#6b7280; --accent:#ff7b00; --accent-2:#ff9a3d;
}
body { margin:0; font-family: Inter, sans-serif; background:var(--bg); color:#0b1222; }
header { display:flex; justify-content: space-between; align-items:center; padding:12px 16px; background:linear-gradient(90deg,var(--accent),var(--accent-2)); color:#fff; font-weight:700; border-radius:0 0 14px 14px; }
header h1 { margin:0; font-size:20px; }
button { cursor:pointer; border:0; border-radius:10px; padding:8px 12px; background:var(--accent); color:#fff; font-weight:600; }
button.ghost { background:transparent; border:1px solid rgba(0,0,0,0.06); color:var(--accent);}
main { padding:12px; max-width:980px; margin:0 auto; display:grid; grid-template-columns:1fr; gap:12px; }
.card { background:var(--card); border-radius:12px; padding:12px; box-shadow:0 6px 20px rgba(16,24,40,0.04); }
.section { display:none; }
.section.active { display:block; }
nav { display:flex; justify-content:space-around; background:white; position:fixed; bottom:0; left:0; right:0; border-top:1px solid #ddd; padding:8px 0; }
nav button { background:none; color:#333; border:none; font-size:14px; }
nav button.active { color:var(--accent); font-weight:700; }
#notification { display:none; background:var(--accent); color:#fff; padding:12px; border-radius:10px; margin-bottom:10px; animation:fade 0.5s;}
@keyframes fade { from {opacity:0; transform:translateY(-10px);} to {opacity:1; transform:translateY(0);} }
</style>
</head>
<body>

<header>
  <h1>Playmates</h1>
  <div>
    <button id="btnOpenAuth">Entrar / Cadastrar</button>
    <button id="btnDemo" class="ghost">Conta Demo</button>
  </div>
</header>

<main>
  <!-- LOGIN / CADASTRO -->
  <div class="card section active" id="sec-feed">
    <div id="notification"></div>

    <div id="authArea">
      <h3>Bem-vindo ao Playmates</h3>
      <div class="muted-small">Faça login ou crie conta</div>
      <div id="loginForm">
        <label>Telemóvel</label><input id="loginPhone" type="tel" placeholder="ex: 922000000"/>
        <label>Senha</label><input id="loginPass" type="password" placeholder="Senha"/>
        <div><button id="loginSubmit">Entrar</button><button id="showRegister" class="ghost">Criar conta</button></div>
      </div>
      <div id="registerForm" style="display:none">
        <label>Nome completo</label><input id="regName" type="text" placeholder="Nome completo"/>
        <label>Senha</label><input id="regPass" type="password"/>
        <label>Telemóvel</label><input id="regPhone" type="tel"/>
        <label>Escola</label><input id="regSchool" type="text"/>
        <label>Foto</label><input id="regPhoto" type="file"/>
        <div><button id="regSubmit">Criar conta</button><button id="regCancel" class="ghost">Voltar</button></div>
      </div>
    </div>

    <div id="loggedArea" style="display:none">
      <h3>Perfil</h3>
      <div id="feed"></div>
    </div>
  </div>

  <!-- EVENTOS -->
  <div class="card section" id="sec-eventos">
    <h2>Eventos — Playmates</h2>
    <button id="btnNovoEvento">Criar evento</button>
    <div id="eventosLista"></div>
  </div>

  <!-- JOGOS -->
  <div class="card section" id="sec-jogos">
    <h2>Jogos</h2>
    <div class="post"><h3>Playmates Runner</h3><p>Mini-jogo em desenvolvimento...</p></div>
    <div class="post"><h3>Playmates Quiz</h3><p>Teste seus conhecimentos!</p></div>
  </div>

  <!-- HISTÓRIA -->
  <div class="card section" id="sec-historia">
    <h2>História do Fundador</h2>
    <div id="historia">
      <h3>Diogo Paixão — Fundador & CEO</h3>
      <p>Diogo criou a Playmates para jovens se expressarem e aprenderem...</p>
    </div>
  </div>
</main>

<!-- MENU -->
<nav>
  <button onclick="show('feed')" class="active">Feed</button>
  <button onclick="show('eventos')">Eventos</button>
  <button onclick="show('jogos')">Jogos</button>
  <button onclick="show('historia')">História</button>
</nav>

<!-- ===================== FIREBASE ===================== -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js";
import { getDatabase, ref, push, onValue, set, remove } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-database.js";

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

// ===================== NOTIFICAÇÃO =====================
const notifBox = document.getElementById("notification");
const notifRef = ref(db, "notificacao/");
onValue(notifRef, snap => {
  const msg = snap.val();
  if(msg){ notifBox.innerHTML = msg; notifBox.style.display="block";}
});
set(notifRef,"Olá eu sou Diogo Paixão, fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil");

// ===================== FEED =====================
const feedRef = ref(db,"feed/");
const feedDiv = document.getElementById("feed");
onValue(feedRef, snap=>{
  feedDiv.innerHTML="";
  snap.forEach(item=>{
    const p=item.val();
    feedDiv.innerHTML+=`<div class="post"><h3>${p.autor}</h3><p>${p.texto}</p></div>`;
  });
});

// ===================== EVENTOS =====================
const eventosRef = ref(db,"eventos/");
const eventosDiv = document.getElementById("eventosLista");
function carregarEventos(){
  onValue(eventosRef, snap=>{
    eventosDiv.innerHTML="";
    snap.forEach(item=>{
      const key = item.key; const evt = item.val();
      eventosDiv.innerHTML+=`<div class="post"><h3>${evt.titulo}</h3><p>${evt.texto}</p><button onclick="apagarEvento('${key}')">Apagar</button></div>`;
    });
  });
}
carregarEventos();
document.getElementById("btnNovoEvento").onclick = ()=>{
  const s=prompt("Senha para publicar evento:");
  if(s!=="LEX") return alert("Senha errada!");
  const t=prompt("Título:"); const c=prompt("Descrição:");
  push(eventosRef,{titulo:t,texto:c});
};
window.apagarEvento=(id)=>{
  const s=prompt("Senha para apagar:");
  if(s!=="LEX") return alert("Senha errada!");
  remove(ref(db,"eventos/"+id));
};

// ===================== MENU =====================
function show(tab){
  document.querySelectorAll(".section").forEach(s=>s.classList.remove("active"));
  document.querySelectorAll("nav button").forEach(b=>b.classList.remove("active"));
  document.getElementById("sec-"+tab).classList.add("active");
  [...document.querySelectorAll("nav button")].find(b=>b.innerText.toLowerCase().includes(tab)).classList.add("active");
}
window.show=show;

// ===================== LOGIN / REGISTRO LOCAL (Exemplo rápido) =====================
const loginPhone = document.getElementById('loginPhone'), loginPass=document.getElementById('loginPass'), loginSubmit=document.getElementById('loginSubmit');
const showRegisterBtn=document.getElementById('showRegister'), regCancel=document.getElementById('regCancel'), regSubmit=document.getElementById('regSubmit');
const regName=document.getElementById('regName'), regPass=document.getElementById('regPass'), regPhone=document.getElementById('regPhone'), regSchool=document.getElementById('regSchool'), regPhoto=document.getElementById('regPhoto');
const btnDemo=document.getElementById('btnDemo');
let accounts = JSON.parse(localStorage.getItem("pm_accounts")||"[]");
function saveAccounts(){localStorage.setItem("pm_accounts",JSON.stringify(accounts));}
btnDemo.onclick = ()=>{if(accounts.some(a=>a.phone==="999999999"))return alert("Demo já existe");accounts.push({name:"Demo",phone:"999999999",pass:"demo",school:"Demo",points:0,posts:[]}); saveAccounts(); alert("Demo criada: 999999999 / demo");};

showRegisterBtn.onclick=()=>{document.getElementById("loginForm").style.display="none";document.getElementById("registerForm").style.display="block";};
regCancel.onclick=()=>{document.getElementById("registerForm").style.display="none";document.getElementById("loginForm").style.display="block";};
regSubmit.onclick=()=>{
  const name=regName.value.trim(), pass=regPass.value, phone=regPhone.value.trim(), school=regSchool.value.trim();
  if(!name||!pass||!phone)return alert("Preencha campos obrigatórios");
  if(accounts.some(a=>a.phone===phone))return alert("Conta já existe");
  accounts.push({name,pass,phone,school,points:0,posts:[]});
  saveAccounts(); alert("Conta criada com sucesso");
};
loginSubmit.onclick=()=>{
  const phone=loginPhone.value.trim(), pass=loginPass.value;
  const acc=accounts.find(a=>a.phone===phone);
  if(!acc)return alert("Conta não encontrada");
  if(acc.pass!==pass)return alert("Senha incorreta");
  alert("Bem-vindo(a), "+acc.name);
};
</script>

</body>
</html>
