
<html lang="pt-PT">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Playmates — Plataforma</title>
<style>
:root{
  --bg:#f6f7fb; --card:#ffffff; --muted:#6b7280; --accent:#ff7b00; --accent-2:#ff9a3d;
}
*{box-sizing:border-box;font-family:Inter,system-ui,-apple-system,"Segoe UI",Roboto,Arial;margin:0;padding:0}
body{background:var(--bg);color:#0b1222;min-height:100vh;padding:14px}
.app{max-width:980px;margin:0 auto}
header{display:flex;gap:12px;align-items:center;padding:16px;border-radius:14px;background:linear-gradient(90deg,var(--accent),var(--accent-2));color:white;box-shadow:0 8px 30px rgba(255,123,0,0.12)}
header h1{font-size:20px}
.top-actions{margin-left:auto;display:flex;gap:8px;align-items:center}
button{background:var(--accent);color:#fff;border:0;padding:10px 12px;border-radius:10px;font-weight:700;display:inline-flex;gap:8px;align-items:center;cursor:pointer}
button.ghost{background:transparent;color:var(--accent);border:1px solid rgba(0,0,0,0.06)}
main{display:grid;grid-template-columns:1fr;gap:12px;margin-top:14px}
@media(min-width:920px){ main{grid-template-columns:340px 1fr} }
.card{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 20px rgba(16,24,40,0.04)}
.counter-box{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:18px;border-radius:12px;background:linear-gradient(180deg,#fff,#fff);text-align:center}
.counter-box h2{font-size:20px;color:var(--accent);margin-bottom:6px}
.counter-box p{color:var(--muted);font-size:13px}
.profile-top{display:flex;gap:12px;align-items:center}
.avatar{width:88px;height:88px;border-radius:999px;overflow:hidden;flex:0 0 88px;background:#f3f5f8;border:3px solid rgba(255,123,0,0.12)}
.avatar img{width:100%;height:100%;object-fit:cover;display:block}
.pinfo h2{font-size:16px;margin-bottom:4px}
.small{color:var(--muted);font-size:13px}
.tabs{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap}
.tab{padding:8px 10px;border-radius:10px;cursor:pointer;font-weight:600;color:var(--muted);background:transparent}
.tab.active{background:linear-gradient(90deg,rgba(255,123,0,0.12),rgba(255,154,61,0.06));color:var(--accent)}
.view-panel{margin-top:12px;padding:12px;border-radius:10px;background:linear-gradient(180deg,#fff,#fbfcff)}
.muted-small{font-size:12px;color:var(--muted);margin-top:6px}
.searchbar{display:flex;gap:8px;align-items:center}
.searchbar input{flex:1;padding:10px;border-radius:10px;border:1px solid rgba(11,18,34,0.06);background:transparent}
.results{margin-top:12px;display:grid;gap:8px}
.result{display:flex;gap:10px;align-items:center;padding:10px;border-radius:10px;background:linear-gradient(180deg,#fff,#fbfcff);cursor:pointer}
.result .mini{width:56px;height:56px;border-radius:999px;overflow:hidden;background:#f2f6ff;flex:0 0 56px}
.result .mini img{width:100%;height:100%;object-fit:cover}
.rinfo{flex:1}
.badge{font-size:12px;padding:6px 8px;border-radius:999px;background:rgba(11,18,34,0.04);color:var(--muted);display:inline-block}
.row{display:flex;gap:8px;align-items:center}
.tiny{font-size:12px;color:var(--muted)}
.center{display:flex;align-items:center;justify-content:center}
textarea{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(11,18,34,0.06)}
input[type="text"],input[type="password"],input[type="tel"]{padding:10px;border-radius:10px;border:1px solid rgba(11,18,34,0.06);width:100%;display:block}
</style>
</head>
<body>
<div class="app">
<header>
  <h1>Playmates</h1>
  <div class="top-actions">
    <button id="btnOpenAuth">Entrar / Cadastrar</button>
    <button id="btnDemo" class="ghost">Conta Demo</button>
  </div>
</header>

<main>
<div class="card" id="leftCol">
  <div id="authArea">
    <h3>Bem-vindo ao Playmates</h3>
    <div class="muted-small">Faça login ou crie conta</div>
    <div style="height:10px"></div>

    <div id="loginForm">
      <label>Telemóvel</label>
      <input id="loginPhone" type="tel" placeholder="ex: 922000000" />
      <label>Senha</label>
      <input id="loginPass" type="password" placeholder="Senha" />
      <div class="row" style="margin-top:10px">
        <button id="loginSubmit">Entrar</button>
        <button id="showRegister" class="ghost">Criar conta</button>
      </div>
    </div>

    <div id="registerForm" style="display:none">
      <label>Nome completo</label>
      <input id="regName" type="text" placeholder="Nome completo" />
      <label>Senha</label>
      <input id="regPass" type="password" placeholder="Senha" />
      <label>Telemóvel</label>
      <input id="regPhone" type="tel" placeholder="ex: 922000000" />
      <label>Nome da escola</label>
      <input id="regSchool" type="text" placeholder="Escola" />
      <label>Foto (upload)</label>
      <input id="regPhoto" type="file" accept="image/*" />
      <div class="row" style="margin-top:10px">
        <button id="regSubmit">Criar conta</button>
        <button id="regCancel" class="ghost">Voltar</button>
      </div>
    </div>
  </div>

  <div id="loggedArea" style="display:none">
    <div class="profile-top">
      <div class="avatar" id="leftAvatar"><img src="" alt="avatar"/></div>
      <div class="pinfo">
        <h2 id="leftName">—</h2>
        <div class="small" id="leftSchool">—</div>
        <div class="small" id="leftPhone">—</div>
        <div style="height:8px"></div>
        <div class="row">
          <button id="btnEditProfile" class="ghost">Editar</button>
          <button id="btnLogout" class="ghost">Sair</button>
        </div>
      </div>
    </div>
    <div class="muted-small">OBS: Perfil em tempo real via Firebase</div>

    <div style="height:12px"></div>
    <div class="tabs">
      <div class="tab active" data-tab="perfil">Perfil</div>
      <div class="tab" data-tab="pesquisa">Pesquisa</div>
      <div class="tab" data-tab="notifs">Notificações</div>
      <div class="tab" data-tab="eventos">Eventos</div>
      <div class="tab" data-tab="jogos">Jogos</div>
      <div class="tab" data-tab="historia">História</div>
    </div>

    <div id="tabContainers">
      <div data-content="perfil" class="view-panel" style="display:block">
        <div class="counter-box card">
          <h2 id="profilesCount">0</h2>
          <p>Perfis registados no Playmates</p>
        </div>
        <div style="height:10px"></div>
        <div class="small">Pontos</div>
        <div style="font-weight:800;font-size:20px" id="leftPoints">0</div>
      </div>

      <div data-content="pesquisa" class="view-panel" style="display:none">
        <div class="searchbar">
          <input id="searchInput" placeholder="Pesquisar por nome (Enter)" />
        </div>
        <div class="results card" id="results"></div>
      </div>

      <div data-content="notifs" class="view-panel" style="display:none">
        <div class="small">Notificações recentes</div>
        <div id="notifList" class="notify-list"></div>
      </div>

      <div data-content="eventos" class="view-panel" style="display:none">
        <label>Senha LEX necessária para publicar ou apagar:</label>
        <input type="password" id="eventPass"/>
        <textarea id="eventText" placeholder="Digite seu evento aqui"></textarea>
        <button id="btnAddEvent">Publicar</button>
        <button id="btnClearEvent">Apagar todos</button>
        <div id="eventsFeed"></div>
      </div>

      <div data-content="jogos" class="view-panel" style="display:none">
        <div class="small">Informações breves sobre jogos:</div>
        <ul>
          <li>🎮 Jogo 1: Sistema de votação interno</li>
          <li>🎮 Jogo 2: Desafios de criatividade</li>
          <li>🎮 Jogo 3: Pontos e ranking em tempo real</li>
        </ul>
      </div>

      <div data-content="historia" class="view-panel" style="display:none">
        <p><strong>História do fundador:</strong> Diogo Paixão fundou a plataforma Playmates com o objetivo de conectar estudantes, criar um espaço seguro e divertido e permitir que os jovens ganhem pontos e reconhecimento através de interações positivas.</p>
      </div>
    </div>
  </div>
</div>

<div>
  <div class="card" id="postsCard">
    <h3>Postagens</h3>
    <textarea id="postText" placeholder="Escreve algo..."></textarea>
    <input id="postImg" type="file" accept="image/*" />
    <div class="row">
      <button id="btnPost">Publicar</button>
      <button id="btnClearPosts" class="ghost">Limpar expirados</button>
    </div>
    <div id="postsFeed" style="margin-top:12px"></div>
  </div>
</div>
</main>
</div>

<!-- Firebase SDK -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.15.0/firebase-app.js";
import { getDatabase, ref, set, push, onValue, remove } from "https://www.gstatic.com/firebasejs/10.15.0/firebase-database.js";
import { getStorage, ref as sref, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.15.0/firebase-storage.js";

const firebaseConfig = {
  apiKey: "AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain: "playmates-cc4f7.firebaseapp.com",
  databaseURL: "https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId: "playmates-cc4f7",
  storageBucket: "playmates-cc4f7.appspot.com",
  messagingSenderId: "104004735810",
  appId: "1:104004735810:web:d3ee9a75399d6f0f222edb"
};

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const storage = getStorage(app);

// DOM refs
const loginPhone = document.getElementById('loginPhone');
const loginPass = document.getElementById('loginPass');
const loginSubmit = document.getElementById('loginSubmit');
const showRegisterBtn = document.getElementById('showRegister');
const regCancel = document.getElementById('regCancel');
const regSubmit = document.getElementById('regSubmit');
const regName = document.getElementById('regName');
const regPass = document.getElementById('regPass');
const regPhone = document.getElementById('regPhone');
const regSchool = document.getElementById('regSchool');
const regPhoto = document.getElementById('regPhoto');
const authArea = document.getElementById('authArea');
const loggedArea = document.getElementById('loggedArea');
const leftAvatar = document.getElementById('leftAvatar');
const leftName = document.getElementById('leftName');
const leftSchool = document.getElementById('leftSchool');
const leftPhone = document.getElementById('leftPhone');
const leftPoints = document.getElementById('leftPoints');
const tabs = document.querySelectorAll('.tab');
const tabContainers = document.querySelectorAll('[data-content]');
const notifList = document.getElementById('notifList');
const profilesCount = document.getElementById('profilesCount');
const postText = document.getElementById('postText');
const postImg = document.getElementById('postImg');
const btnPost = document.getElementById('btnPost');
const postsFeed = document.getElementById('postsFeed');
const btnClearPosts = document.getElementById('btnClearPosts');
const eventPass = document.getElementById('eventPass');
const eventText = document.getElementById('eventText');
const btnAddEvent = document.getElementById('btnAddEvent');
const btnClearEvent = document.getElementById('btnClearEvent');
const eventsFeed = document.getElementById('eventsFeed');

let sessionPhone = null;
let currentUser = null;

// Helpers
function placeholderDataUrl(name){
  const canvas=document.createElement('canvas');canvas.width=88;canvas.height=88;const ctx=canvas.getContext('2d');ctx.fillStyle='#ccc';ctx.fillRect(0,0,88,88);ctx.fillStyle='#000';ctx.font='30px Arial';ctx.fillText(name[0]||'P',20,55);return canvas.toDataURL();
}

// Tabs
tabs.forEach(t=>{
  t.addEventListener('click',()=>{tabs.forEach(x=>x.classList.remove('active'));t.classList.add('active');tabContainers.forEach(c=>c.style.display=c.dataset.content===t.dataset.tab?'block':'none');});
});

// Cadastro
async function fileToDataUrl(file){
  return new Promise((res,rej)=>{
    const reader = new FileReader();
    reader.onload = e=>res(e.target.result);
    reader.onerror=err=>rej(err);
    reader.readAsDataURL(file);
  });
}

regSubmit.addEventListener('click', async ()=>{
  const name = regName.value.trim(), pass = regPass.value, phone = regPhone.value.trim(), school = regSchool.value.trim();
  if(!name || !pass || !phone) return alert('Preencha todos os campos');
  const userRef = ref(db,'users/'+phone);
  const snap = await userRef.get();
  if(snap.exists()) return alert('Usuário já existe');
  let photoData = placeholderDataUrl(name);
  if(regPhoto.files[0]){
    photoData = await fileToDataUrl(regPhoto.files[0]);
    const storageRef = sref(storage,'avatars/'+phone);
    await uploadBytes(storageRef,regPhoto.files[0]);
    photoData = await getDownloadURL(storageRef);
  }
  const newUser = {name,phone,school:school||'—',pass,photo:photoData,points:0,notifications:["Olá eu sou Diogo paixão fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil"],posts:[]};
  set(userRef,newUser);
  sessionPhone = phone; loadSessionFromFirebase();
});

// Login
loginSubmit.addEventListener('click',async()=>{
  const phone = loginPhone.value.trim(), pass = loginPass.value.trim();
  const userRef = ref(db,'users/'+phone);
  const snap = await userRef.get();
  if(!snap.exists()) return alert('Usuário não encontrado');
  const u = snap.val();
  if(u.pass!==pass) return alert('Senha incorreta');
  sessionPhone = phone; loadSessionFromFirebase();
});

// Load Session
function loadSessionFromFirebase(){
  if(!sessionPhone) return;
  const userRef = ref(db,'users/'+sessionPhone);
  onValue(userRef,snap=>{
    currentUser = snap.val();
    if(!currentUser) return;
    leftAvatar.querySelector('img').src=currentUser.photo;
    leftName.textContent=currentUser.name;
    leftSchool.textContent=currentUser.school;
    leftPhone.textContent=currentUser.phone;
    leftPoints.textContent=currentUser.points||0;
    authArea.style.display='none';
    loggedArea.style.display='block';

    // Atualiza notificações
    notifList.innerHTML='';
    if(currentUser.notifications) currentUser.notifications.forEach(n=>{
      const d = document.createElement('div'); d.className='notify'; d.textContent=n; notifList.appendChild(d);
    });
  });
}

// Publicar post
btnPost.addEventListener('click',async()=>{
  if(!postText.value && !postImg.files[0]) return alert('Digite algo ou selecione imagem');
  let imgUrl='';
  if(postImg.files[0]){
    const storageRef = sref(storage,'posts/'+Date.now());
    await uploadBytes(storageRef,postImg.files[0]);
    imgUrl = await getDownloadURL(storageRef);
  }
  const post = {author:currentUser.name,text:postText.value,img:imgUrl,time:Date.now()};
  const postsRef = ref(db,'posts'); push(postsRef,post);
  postText.value=''; postImg.value='';
});

// Mostrar posts
const postsRef = ref(db,'posts');
onValue(postsRef,snap=>{
  postsFeed.innerHTML='';
  snap.forEach(s=>{
    const p = s.val();
    const div=document.createElement('div'); div.className='post';
    div.innerHTML=`${p.img?`<img src="${p.img}"/>`:''}<div class="meta"><b>${p.author}</b><p>${p.text}</p></div>`;
    postsFeed.appendChild(div);
  });
});

// Eventos com senha LEX
btnAddEvent.addEventListener('click',()=>{
  if(eventPass.value!=='LEX') return alert('Senha incorreta');
  const evRef = ref(db,'events'); push(evRef,{text:eventText.value,time:Date.now()});
  eventText.value='';
});
btnClearEvent.addEventListener('click',()=>{
  if(eventPass.value!=='LEX') return alert('Senha incorreta');
  remove(ref(db,'events'));
});
// Mostrar eventos
const evRef = ref(db,'events');
onValue(evRef,snap=>{
  eventsFeed.innerHTML='';
  snap.forEach(s=>{
    const e = s.val();
    const div = document.createElement('div'); div.textContent=e.text; eventsFeed.appendChild(div);
  });
});
</script>
</body>
</html>
