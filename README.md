<html lang="pt"><head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Playmates — Plataforma Oficial</title>
<style>
:root{--bg:#f6f7fb;--card:#fff;--muted:#6b7280;--accent:#ff7b00;--accent-2:#ff9a3d;}
body{margin:0;font-family:Inter,sans-serif;background:var(--bg);color:#0b1222;}
header{display:flex;justify-content:space-between;align-items:center;padding:12px 16px;background:linear-gradient(90deg,var(--accent),var(--accent-2));color:#fff;font-weight:700;border-radius:0 0 14px 14px;}
header h1{margin:0;font-size:20px;}
button{cursor:pointer;border:0;border-radius:10px;padding:8px 12px;background:var(--accent);color:#fff;font-weight:600;}
button.ghost{background:transparent;border:1px solid rgba(0,0,0,0.06);color:var(--accent);}
main{padding:12px;max-width:980px;margin:0 auto;display:grid;grid-template-columns:1fr;gap:12px;}
.card{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 20px rgba(16,24,40,0.04);}
.section{display:none;}
.section.active{display:block;}
nav{display:flex;justify-content:space-around;background:white;position:fixed;bottom:0;left:0;right:0;border-top:1px solid #ddd;padding:8px 0;}
nav button{background:none;color:#333;border:none;font-size:14px;}
nav button.active{color:var(--accent);font-weight:700;}
#notification{display:none;background:var(--accent);color:#fff;padding:12px;border-radius:10px;margin-bottom:10px;animation:fade 0.5s;}
@keyframes fade{from{opacity:0;transform:translateY(-10px);}to{opacity:1;transform:translateY(0);}}
#fotoPerfil{width:100px;height:100px;border-radius:50%;object-fit:cover;margin-bottom:10px;}
input, label, textarea, select{display:block;margin:6px 0;width:100%;padding:6px;border-radius:6px;border:1px solid #ddd;}
textarea{resize:none;}
.sms-message{border-bottom:1px solid #eee;padding:8px 0;}
.sms-header{display:flex;align-items:center;margin-bottom:4px;}
.sms-header img{width:40px;height:40px;border-radius:50%;object-fit:cover;margin-right:8px;}
.user-count{font-weight:600;color:var(--accent);}
.event-views{font-size:12px;color:var(--muted);}
</style>
</head>
<body>

<header>
<h1>Playmates</h1>
</header>

<main>
<div id="notification" class="card"></div>

<!-- LOGIN / CADASTRO -->
<div class="card section active" id="sec-sms">
<div id="authArea">
<h3>Bem-vindo ao Playmates</h3>
<div id="loginForm">
<label>Telemóvel</label><input id="loginPhone" type="tel"/>
<label>Senha</label><input id="loginPass" type="password"/>
<button id="loginSubmit">Entrar</button>
<button id="showRegister" class="ghost">Criar conta</button>
</div>
<div id="registerForm" style="display:none">
<label>Nome completo</label><input id="regName" type="text"/>
<label>Senha</label><input id="regPass" type="password"/>
<label>Telemóvel</label><input id="regPhone" type="tel"/>
<label>Escola</label><input id="regSchool" type="text"/>
<label>Foto</label><input id="regPhoto" type="file" accept="image/*"/>
<button id="regSubmit">Criar conta</button>
<button id="regCancel" class="ghost">Voltar</button>
</div>
</div>

<div id="loggedArea" style="display:none">
<h3>Perfil</h3>
<img id="fotoPerfil" src="https://via.placeholder.com/100"/>
<div id="perfilInfo"></div>
<p>Usuários cadastrados: <span id="userCount" class="user-count">0</span></p>

<!-- Enviar SMS -->
<h3>Enviar SMS</h3>
<select id="smsDestinatario"></select>
<textarea id="smsTexto" maxlength="130" rows="2" placeholder="Mensagem (130 caracteres)"></textarea>
<button id="btnEnviarSMS">Enviar</button>
<h4>Mensagens recebidas</h4>
<div id="smsInbox"></div>

<!-- Pesquisa de perfis -->
<h3>Pesquisar usuários</h3>
<input type="text" id="searchInput" placeholder="Digite nome do usuário"/>
<div id="searchResults"></div>

<button id="btnLogout">Sair</button>
</div>
</div>

<!-- EVENTOS -->
<div class="card section" id="sec-eventos">
<h2>Eventos — Playmates</h2>
<button id="btnNovoEvento">Criar evento (Somente o senhor)</button>
<div id="eventosLista"></div>
</div>

<!-- JOGOS -->
<div class="card section" id="sec-jogos">
<h2>Jogos</h2>
<!-- JOGOS (SUBSTITUIR ESTA DIV) -->
<div class="card section" id="sec-jogos">
  <h2>Jogos</h2>

  <!-- Relógio central -->
  <div id="countdownCard" style="display:flex;flex-direction:column;align-items:center;gap:8px;padding:12px;border-radius:10px;border:1px dashed #eee;margin-bottom:12px;">
    <div style="display:flex;align-items:center;gap:8px;">
      <strong>Contagem regressiva:</strong>
      <span id="countdownLabel" style="font-size:20px;font-weight:800;">00:00:00</span>
    </div>
    <div style="font-size:12px;color:#666;">
      <button id="editCountdownBtn" title="Editar horário (senha A8)">📢 Editar contador</button>
    </div>
  </div>

  <!-- Concorrentes (vertical) -->
  <div id="competitorsList" style="display:flex;flex-direction:column;gap:10px;">
    <!-- Cada concorrente será renderizado dinamicamente -->
  </div>

  <!-- templates hidden: file inputs para upload de fotos (um por concorrente) -->
  <input type="file" id="compPhoto_1" accept="image/*" style="display:none"/>
  <input type="file" id="compPhoto_2" accept="image/*" style="display:none"/>
  <input type="file" id="compPhoto_3" accept="image/*" style="display:none"/>
</div>


<!-- HISTÓRIA -->
<div class="card section" id="sec-historia">
<h2>História do Fundador</h2>
<div id="historia">
  <style>
    b{color:blue};
  </style>
<b>Diogo Paixão — Fundador & CEO</b>
  <h2>Sobre o Playmates</h2>
  <p>
    O Playmates foi criado por <strong>Diogo Paixão</strong> aos 17 anos e lançado em 2025.  
    É uma plataforma pioneira em Angola que transforma votos em oportunidades financeiras.  
    Já alcançou mais de 20 escolas do ensino médio, permitindo que estudantes ganhem recompensas de forma confiável e segura.  
    É mais que uma disputa, é um caminho de empreendedorismo para jovens angolanos. Estimula liderança, comunicação e captação de apoios nas comunidades escolares.
  </p>
</div>
</div>
</main>

<nav>
<button data-tab="sms" class="active">Início</button>
<button data-tab="eventos">Eventos</button>
<button data-tab="jogos">Jogos</button>
<button data-tab="historia">Sobre</button>
</nav>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js";
import { getDatabase, ref, set, get, push, onValue, remove, runTransaction } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-database.js";
import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-storage.js";

const firebaseConfig = { apiKey:"AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI", authDomain:"playmates-cc4f7.firebaseapp.com", databaseURL:"https://playmates-cc4f7-default-rtdb.firebaseio.com", projectId:"playmates-cc4f7", storageBucket:"playmates-cc4f7.appspot.com", messagingSenderId:"104004735810", appId:"1:104004735810:web:d3ee9a75399d6f0f222edb"};
const app=initializeApp(firebaseConfig), db=getDatabase(app), storage=getStorage(app);

const notifBox=document.getElementById("notification");
const notifRef=ref(db,"notificacao/");
onValue(notifRef,snap=>{ const msg=snap.val(); if(msg){ notifBox.innerText=msg; notifBox.style.display="block"; }});
set(notifRef,"Olá eu sou Diogo Paixão, fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil");

let currentUser=null;

// MENU
const navButtons=document.querySelectorAll("nav button");
navButtons.forEach(btn=>{
  btn.onclick=()=>{
    const tab=btn.dataset.tab;
    document.querySelectorAll(".section").forEach(s=>s.classList.remove("active"));
    document.querySelectorAll("nav button").forEach(b=>b.classList.remove("active"));
    document.getElementById("sec-"+tab).classList.add("active");
    btn.classList.add("active");

    // Eventos views
    if(tab==="eventos" && currentUser){
      const eventosRef=ref(db,"eventos/");
      onValue(eventosRef,snap=>{
        const eventosDiv=document.getElementById("eventosLista");
        eventosDiv.innerHTML="";
        snap.forEach(item=>{
          const key=item.key;
          const evt=item.val();
          if(!evt.views) evt.views=0;
          runTransaction(ref(db,"eventos/"+key+"/views"), current=>current===null?1:current+1);
          eventosDiv.innerHTML+=`<div class="post"><h3>${evt.titulo}</h3><p>${evt.texto}</p><p class="event-views">👁️ ${evt.views||0} views</p><button onclick="apagarEvento('${key}')">Apagar</button></div>`;
        });
      });
    } else if((tab==="eventos"||tab==="jogos") && !currentUser){
      alert("Faça login para acessar esta aba");
      document.getElementById("sec-sms").classList.add("active");
      navButtons[0].classList.add("active");
    }
  };
});

// LOGIN / REGISTRO
const loginPhone=document.getElementById('loginPhone'), loginPass=document.getElementById('loginPass'), loginSubmit=document.getElementById('loginSubmit');
const showRegisterBtn=document.getElementById('showRegister'), regCancel=document.getElementById('regCancel'), regSubmit=document.getElementById('regSubmit');
const regName=document.getElementById('regName'), regPass=document.getElementById('regPass'), regPhone=document.getElementById('regPhone'), regSchool=document.getElementById('regSchool'), regPhoto=document.getElementById('regPhoto');
const authArea=document.getElementById('authArea'), loggedArea=document.getElementById('loggedArea');
const fotoPerfil=document.getElementById('fotoPerfil'), perfilInfo=document.getElementById('perfilInfo');
const smsDestinatario=document.getElementById('smsDestinatario'), smsTexto=document.getElementById('smsTexto'), btnEnviarSMS=document.getElementById('btnEnviarSMS');
const smsInbox=document.getElementById('smsInbox'), searchInput=document.getElementById('searchInput'), searchResults=document.getElementById('searchResults');
const btnLogout=document.getElementById('btnLogout');
const userCountLabel=document.getElementById('userCount');

showRegisterBtn.onclick=()=>{ document.getElementById('loginForm').style.display='none'; document.getElementById('registerForm').style.display='block';};
regCancel.onclick=()=>{ document.getElementById('registerForm').style.display='none'; document.getElementById('loginForm').style.display='block';};

regSubmit.onclick=async ()=>{
  const name=regName.value.trim(), pass=regPass.value, phone=regPhone.value.trim(), school=regSchool.value.trim();
  if(!name||!pass||!phone) return alert("Preencha campos obrigatórios");
  const userRef=ref(db,"users/"+phone);
  const snap=await get(userRef);
  if(snap.exists()) return alert("Conta já existe");

  let photoURL="";
  if(regPhoto.files[0]){
    const file=regPhoto.files[0];
    const storageRef=sRef(storage,"perfilFotos/"+phone);
    await uploadBytes(storageRef,file);
    photoURL=await getDownloadURL(storageRef);
  }

  await set(userRef,{name,pass,phone,school,foto:photoURL,points:0});
  loginUser(phone);
};

loginSubmit.onclick=async ()=>{
  const phone=loginPhone.value.trim(), pass=loginPass.value;
  const snap=await get(ref(db,"users/"+phone));
  if(!snap.exists()) return alert("Conta não encontrada");
  const user=snap.val();
  if(user.pass!==pass) return alert("Senha incorreta");
  loginUser(phone);
};

function loginUser(phone){
  currentUser=phone;
  authArea.style.display='none';
  loggedArea.style.display='block';

  const userRef=ref(db,"users/"+phone);
  onValue(userRef,snap=>{
    const u=snap.val();
    fotoPerfil.src=u.foto||"https://via.placeholder.com/100";
    perfilInfo.innerHTML=`<p><strong>Nome:</strong> ${u.name}</p><p><strong>Telemóvel:</strong> ${u.phone}</p><p><strong>Escola:</strong> ${u.school}</p><p><strong>Pontos:</strong> ${u.points}</p>`;
  });

  const usersRef=ref(db,"users/");
  onValue(usersRef,snap=>{
    const total=snap.size;
    userCountLabel.innerText=total>=1000000?"1M+":total;
    smsDestinatario.innerHTML="<option value=''>Selecione destinatário</option>";
    searchResults.innerHTML="";
    snap.forEach(item=>{
      const u=item.val();
      if(u.phone!==currentUser){
        smsDestinatario.innerHTML+=`<option value="${u.phone}">${u.name}</option>`;
        searchResults.innerHTML+=`<div>${u.name} - ${u.phone} <button onclick="sendToUser('${u.phone}')">Enviar SMS</button></div>`;
      }
    });
  });

  btnEnviarSMS.onclick=async ()=>{
    const dest=smsDestinatario.value;
    const msg=smsTexto.value.trim();
    if(!dest||!msg) return alert("Selecione destinatário e digite mensagem");
    const ts=Date.now()+3*3600*1000;
    push(ref(db,"sms/"+dest),{from:currentUser,text:msg,expires:ts});
    smsTexto.value="";
  };

  onValue(ref(db,"sms/"+currentUser),snap=>{
    smsInbox.innerHTML="";
    const now=Date.now();
    snap.forEach(item=>{
      const m=item.val();
      if(m.expires && m.expires<now) remove(ref(db,"sms/"+currentUser+"/"+item.key));
      else{
        get(ref(db,"users/"+m.from)).then(userSnap=>{
          const sender=userSnap.val();
          smsInbox.innerHTML+=`<div class="sms-message"><div class="sms-header"><img src="${sender.foto||'https://via.placeholder.com/40'}"/> <strong>${sender.name}</strong></div><p>${m.text}</p></div>`;
        });
      }
    });
  });

  searchInput.oninput=()=>{
    const filtro=searchInput.value.toLowerCase();
    searchResults.innerHTML="";
    onValue(ref(db,"users/"),snap=>{
      snap.forEach(item=>{
        const u=item.val();
        if(u.name.toLowerCase().includes(filtro) && u.phone!==currentUser)
          searchResults.innerHTML+=`<div>${u.name} - ${u.phone} <button onclick="sendToUser('${u.phone}')">Enviar SMS</button></div>`;
      });
    });
  };

  btnLogout.onclick=()=>{
    loggedArea.style.display='none';
    authArea.style.display='block';
    currentUser=null;
  };
}

window.sendToUser=(phone)=>{ smsDestinatario.value=phone; smsTexto.focus(); };

// EVENTOS
const eventosRef=ref(db,"eventos/");
document.getElementById("btnNovoEvento").onclick=async ()=>{
  const s=prompt("Senha para publicar evento:");
  if(s!=="LEX") return alert("Senha errada!");
  const t=prompt("Título:"), c=prompt("Descrição:");
  push(eventosRef,{titulo:t,texto:c,views:0});
};

window.apagarEvento=(id)=>{
  const s=prompt("Senha para apagar:");
  if(s!=="LEX") return alert("Senha errada!");
  remove(ref(db,"eventos/"+id));
};
  

</script>

</body>
</html>
