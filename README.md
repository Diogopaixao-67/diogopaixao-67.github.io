<html lang="pt">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Playmates — Protótipo (WhatsApp style)</title>
<style>
:root{
  --bg:#e9f5ee; --card:#fff; --muted:#6b7280; --accent:#075E54; --accent-2:#25D366; --accent-3:#ff7b00;
  --shadow: 0 6px 18px rgba(16,24,40,0.06);
}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,-apple-system,"Segoe UI",Roboto,Helvetica,Arial;background:var(--bg);color:#0b1222;min-height:100vh;padding-bottom:76px}
header{display:flex;align-items:center;gap:12px;padding:12px 16px;background:linear-gradient(90deg,var(--accent-3),#ff9a3d);color:#fff;font-weight:700;border-radius:0 0 14px 14px;position:sticky;top:0;z-index:40}
header h1{margin:0;font-size:18px}
main{padding:12px;max-width:980px;margin:0 auto;display:grid;grid-template-columns:1fr;gap:12px}
.card{background:var(--card);border-radius:12px;padding:12px;box-shadow:var(--shadow)}
.section{display:none}
.section.active{display:block}
nav{display:flex;justify-content:space-around;background:white;position:fixed;bottom:0;left:0;right:0;border-top:1px solid #ddd;padding:8px 0;z-index:50}
nav button{background:none;color:#333;border:none;font-size:13px;padding:6px 4px;width:25%;display:flex;flex-direction:column;align-items:center;gap:4px}
nav button.active{color:var(--accent-3);font-weight:700}
small.navIcon{font-size:18px}

/* common */
button{cursor:pointer;border:0;border-radius:10px;padding:8px 12px;background:var(--accent-3);color:#fff;font-weight:600}
button.ghost{background:transparent;border:1px solid rgba(0,0,0,0.06);color:var(--accent-3)}
input, label, textarea, select{display:block;margin:6px 0;width:100%;padding:8px;border-radius:8px;border:1px solid #ddd;font-size:14px}
textarea{resize:none}
.user-count{font-weight:600;color:var(--accent-3)}
.event-views{font-size:12px;color:var(--muted)}

/* jogos */
#countdownCard{display:flex;flex-direction:column;align-items:center;gap:8px;padding:14px;border-radius:10px;border:1px dashed #eee;background:#fff}
#countdownLabel{font-size:22px;font-weight:800;color:#111}
.small-ghost{background:transparent;border:1px solid #ddd;padding:6px 8px;border-radius:8px;color:#333;cursor:pointer}
#competitorsList{display:flex;flex-direction:column;gap:10px;margin-top:6px}
.compCard{display:flex;align-items:center;gap:12px;padding:10px;border-radius:10px;background:#fff;border:1px solid #eee}
.compCard img{width:80px;height:80px;border-radius:8px;object-fit:cover}
.compInfo{flex:1}
.compActions{display:flex;flex-direction:column;gap:8px;align-items:flex-end}
.edit-orange{background:var(--accent-3);color:#fff;padding:6px 10px;border-radius:8px;border:0}
.vote-green{background:var(--accent-2);color:#fff;padding:8px 10px;border-radius:8px;border:0}
.photo-btn{background:#f1f1f1;border-radius:8px;padding:6px 8px;border:0}
.smallNote{font-size:12px;color:var(--muted)}

/* helper modal */
.modal-back{position:fixed;inset:0;background:rgba(0,0,0,0.35);display:flex;align-items:center;justify-content:center;z-index:60}
.modal{background:#fff;padding:14px;border-radius:10px;max-width:520px;width:94%}
.modal h3{margin:0 0 8px 0}
.modal .row{margin:8px 0}
.modal input{width:100%}
.pill{display:inline-block;padding:6px 8px;border-radius:999px;background:#f7f7f7;margin-right:6px;font-size:12px}

/* small UI */
.flex{display:flex;gap:8px;align-items:center}
.requestsList{margin-top:10px}
.requestRow{display:flex;align-items:center;justify-content:space-between;padding:8px;border-radius:8px;border:1px solid #eee;background:#fff;margin-top:6px}
.requestRow .meta{font-size:13px;color:#444}

/* responsive */
@media (max-width:520px){
  .compCard img{width:64px;height:64px}
  #countdownLabel{font-size:18px}
}
</style>
</head>
<body>
<header><h1>Playmates</h1></header>

<main>
  <div id="notification" class="card" style="display:none"></div>

  <!-- LOGIN / CADASTRO -->
  <div id="sec-sms" class="card section active">
    <h3>Bem-vindo ao Playmates</h3>

    <div id="authArea">
      <div id="loginForm">
        <label>Telemóvel (ex: 2449xxxxxxx)</label>
        <input id="loginPhone" type="tel" placeholder="Telemóvel"/>
        <label>Senha</label>
        <input id="loginPass" type="password"/>
        <div style="display:flex;gap:8px">
          <button id="loginSubmit">Entrar</button>
          <button id="showRegister" class="ghost">Criar conta</button>
        </div>
      </div>

      <div id="registerForm" style="display:none">
        <label>Nome completo</label><input id="regName" type="text"/>
        <label>Senha</label><input id="regPass" type="password"/>
        <label>Telemóvel</label><input id="regPhone" type="tel"/>
        <label>Escola</label><input id="regSchool" type="text"/>
        <label>Foto (poderás enviar ou colar link depois)</label><input id="regPhoto" type="file" accept="image/*"/>
        <div style="display:flex;gap:8px">
          <button id="regSubmit">Criar conta</button>
          <button id="regCancel" class="ghost">Voltar</button>
        </div>
      </div>
    </div>

    <div id="loggedArea" style="display:none; margin-top:12px">
      <h3>Perfil</h3>
      <img id="fotoPerfil" src="https://via.placeholder.com/100" style="width:100px;height:100px;border-radius:50%;object-fit:cover"/>
      <div id="perfilInfo"></div>
      <div style="display:flex;gap:8px;margin-top:6px">
        <button id="btnEditProfile" class="ghost">Editar Perfil</button>
      </div>
      <p>Usuários cadastrados: <span id="userCount" class="user-count">0</span></p>
      <h4>Pesquisar usuários</h4>
      <input id="searchInput" placeholder="Digite nome ou telefone"/>
      <div id="searchResults"></div>
      <div style="margin-top:8px">
        <button id="btnLogout" class="ghost">Sair</button>
      </div>
    </div>
  </div>

  <!-- EVENTOS -->
  <div id="sec-eventos" class="card section">
    <h2>Eventos — Playmates</h2>
    <button id="btnNovoEvento" class="primary">Criar evento (Somente o senhor)</button>
    <div id="eventosLista" style="margin-top:12px"></div>
  </div>

  <!-- JOGOS -->
  <div id="sec-jogos" class="card section">
    <h2>Jogos</h2>

    <div id="countdownCard">
      <div style="display:flex;align-items:center;gap:8px">
        <strong>Contagem regressiva:</strong>
        <span id="countdownLabel">00:00:00</span>
      </div>
      <div style="font-size:12px;color:#666"><button id="editCountdownBtn" class="small-ghost" title="Editar (senha A8)">📢 Editar contador</button></div>
      <div class="smallNote">Tempo visível em tempo real para todos os utilizadores.</div>
    </div>

    <div id="competitorsList"></div>

    <!-- pedido panel (novo) -->
    <div class="card" style="margin-top:10px">
      <h3>Painel de pedidos — Packs de votos</h3>
      <div style="display:grid;gap:8px">
        <input id="req_name" placeholder="Nome"/>
        <input id="req_school" placeholder="Escola"/>
        <input id="req_whatsapp" placeholder="Número WhatsApp (ex: 2449...)"/>
        <select id="req_pack">
          <option value="free_400">Grátis — ganha 400kz</option>
          <option value="500_2000">Pack 500kz — ganha 2000kz</option>
        </select>
        <div style="display:flex;gap:8px">
          <button id="reqSubmit">Enviar pedido</button>
          <button id="simPlus" class="ghost">+1 (Simulador)</button>
          <div style="margin-left:auto" class="pill">Pedidos: <span id="reqCount">0</span></div>
        </div>
        <div id="requestsList" class="requestsList"></div>
      </div>
    </div>

    <!-- hidden file inputs -->
    <input type="file" id="compPhoto_1" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_2" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_3" accept="image/*" style="display:none"/>
  </div>
    <!-- SOBRE -->
  <div id="sec-historia" class="card section">
    <h2>Sobre</h2>
    <p><strong>Diogo Paixão</strong> — Fundador &amp; CEO.</p>
  <p>
    O Playmates foi criado por <strong>Diogo Paixão</strong> aos 17 anos e lançado em 2025.  
    É uma plataforma pioneira em Angola que transforma votos em oportunidades financeiras.  
    Já alcançou mais de 20 escolas do ensino médio, permitindo que estudantes ganhem recompensas de forma confiável e segura.  
    É mais que uma disputa, é um caminho de empreendedorismo para jovens angolanos. Estimula liderança, comunicação e captação de apoios nas comunidades escolares.
  </p>
  </div>
</main>

<nav>
  <button data-tab="sms" class="active"><small class="navIcon">🏠</small><span>Início</span></button>
  <button data-tab="eventos"><small class="navIcon">📅</small><span>Eventos</span></button>
  <button data-tab="jogos"><small class="navIcon">🎮</small><span>Jogos</span></button>
  <button data-tab="historia"><small class="navIcon">ℹ️</small><span>Sobre</span></button>
</nav>

<!-- modal container (for nicer UX instead of many prompts) -->
<div id="modalRoot" style="display:none"></div>

<script type="module">
/* -----------------------
   Firebase imports
   ----------------------- */
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js";
import { getDatabase, ref, set, get, push, onValue, remove, runTransaction, update } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-database.js";
import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-storage.js";

/* ============ CONFIG ============ */
/* Usa o teu config */
const firebaseConfig = {
  apiKey: "AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain: "playmates-cc4f7.firebaseapp.com",
  databaseURL: "https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId: "playmates-cc4f7",
  storageBucket: "playmates-cc4f7.appspot.com",
  messagingSenderId: "104004735810",
  appId: "1:104004735810:web:d3ee9a75399d6f0f222edb"
};
/* ================================= */

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const storage = getStorage(app);

/* Helpers */
const $ = id => document.getElementById(id);
const showNotification = (txt, timeout=5000) => {
  const n = $('notification');
  n.innerText = txt; n.style.display = 'block';
  if(timeout>0) setTimeout(()=>{ n.style.display='none'; }, timeout);
};

/* NAV behaviour */
document.querySelectorAll('nav button').forEach(btn=>{
  btn.addEventListener('click', ev=>{
    const tab = ev.currentTarget.dataset.tab;
    document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
    document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));
    const sec = document.getElementById('sec-'+tab);
    if(sec) sec.classList.add('active');
    ev.currentTarget.classList.add('active');
    // require login for eventos/jogos
    if((tab==='eventos' || tab==='jogos') && !currentUser){
      alert('Faça login para acessar esta aba');
      document.getElementById('sec-sms').classList.add('active');
      document.querySelector('nav button[data-tab="sms"]').classList.add('active');
    }
  });
});

/* Elements */
const loginPhone = $('loginPhone'), loginPass = $('loginPass'), loginSubmit = $('loginSubmit');
const showRegisterBtn = $('showRegister'), regCancel = $('regCancel'), regSubmit = $('regSubmit');
const regName = $('regName'), regPass = $('regPass'), regPhone = $('regPhone'), regSchool = $('regSchool'), regPhoto = $('regPhoto');
const fotoPerfil = $('fotoPerfil'), perfilInfo = $('perfilInfo'), searchInput = $('searchInput'), searchResults = $('searchResults');
const btnLogout = $('btnLogout'), userCountLabel = $('userCount');
const btnEditProfile = $('btnEditProfile');

const editCountdownBtn = $('editCountdownBtn'), countdownLabel = $('countdownLabel');
const competitorsList = $('competitorsList');
const fileInputs = {1: $('compPhoto_1'), 2: $('compPhoto_2'), 3: $('compPhoto_3')};

/* Requests elements */
const reqName = $('req_name'), reqSchool = $('req_school'), reqWhats = $('req_whatsapp'), reqPack = $('req_pack');
const reqSubmit = $('reqSubmit'), simPlus = $('simPlus'), reqCountLabel = $('reqCount'), requestsList = $('requestsList');

/* DB refs */
const notifRef = ref(db, 'notificacao/');
const usersRef = ref(db, 'users/');
const eventosRef = ref(db, 'eventos/');
const countdownRef = ref(db, 'jogos/countdown');
const competitorsRef = ref(db, 'jogos/competitors');
const messagesRootRef = ref(db, 'messages/'); // messages/{recipientPhone}/{msgId}
const requestsRef = ref(db, 'jogos/requests');
const requestsCountRef = ref(db, 'jogos/requestsCount');

/* default message */
set(notifRef, "Bem-vindo ao Playmates!").catch(()=>{});

/* state */
let currentUser = null;
let currentUserObj = null;

/* UI helpers for modal (we use a simple modal for editing instead of many prompts) */
function openModal(html, onOk){
  const root = $('modalRoot');
  root.innerHTML = `<div class="modal-back"><div class="modal">${html}<div style="display:flex;gap:8px;margin-top:10px"><button id="modalOk">OK</button><button id="modalCancel" class="ghost">Cancelar</button></div></div></div>`;
  root.style.display = 'block';
  $('modalCancel').onclick = ()=>{ root.style.display='none'; root.innerHTML=''; };
  $('modalOk').onclick = ()=>{ try{ onOk(root); } catch(e){ console.error(e); } root.style.display='none'; root.innerHTML=''; };
}

/* AUTH: show/hide register */
showRegisterBtn.onclick = ()=>{ $('loginForm').style.display='none'; $('registerForm').style.display='block'; };
regCancel.onclick = ()=>{ $('registerForm').style.display='none'; $('loginForm').style.display='block'; };

/* REGISTER */
regSubmit.onclick = async ()=>{
  try{
    const name = (regName.value||'').trim(), pass = regPass.value, phone = (regPhone.value||'').trim(), school = (regSchool.value||'').trim();
    if(!name || !pass || !phone) return alert('Preencha campos obrigatórios');
    const uRef = ref(db, 'users/'+phone);
    const snap = await get(uRef);
    if(snap.exists()) return alert('Conta já existe com esse número');
    let photoURL = '';
    if(regPhoto.files && regPhoto.files[0]){
      const file = regPhoto.files[0];
      const sRefPath = sRef(storage, `perfilFotos/${phone}_${Date.now()}`);
      await uploadBytes(sRefPath, file);
      photoURL = await getDownloadURL(sRefPath);
    }
    await set(uRef, { name, pass, phone, school, foto:photoURL, points:0 });
    loginUser(phone);
    showNotification('Conta criada e logado',3000);
  }catch(err){
    console.error(err); alert('Erro ao criar conta: '+ (err.message||err));
  }
};

/* LOGIN */
loginSubmit.onclick = async ()=>{
  try{
    const phone = (loginPhone.value||'').trim(), pass = loginPass.value;
    if(!phone || !pass) return alert('Digite número e senha');
    const snap = await get(ref(db, 'users/'+phone));
    if(!snap.exists()) return alert('Conta não encontrada');
    const u = snap.val();
    if(u.pass !== pass) return alert('Senha incorreta');
    loginUser(phone);
    showNotification('Login bem-sucedido',2000);
  }catch(err){
    console.error(err); alert('Erro login: '+ (err.message||err));
  }
};

/* login UI */
function loginUser(phone){
  currentUser = phone;
  $('loginForm').style.display = 'none';
  $('registerForm').style.display = 'none';
  $('loggedArea').style.display = 'block';

  const userRef = ref(db, 'users/'+phone);
  onValue(userRef, snap=>{
    const u = snap.val()||{};
    currentUserObj = u;
    fotoPerfil.src = u.foto || 'https://via.placeholder.com/100';
    perfilInfo.innerHTML = `<p><strong>Nome:</strong> ${u.name||''}</p><p><strong>Telemóvel:</strong> ${u.phone||''}</p><p><strong>Escola:</strong> ${u.school||''}</p><p><strong>Pontos:</strong> ${u.points||0}</p>`;
  });

  onValue(usersRef, snap=>{
    userCountLabel.innerText = snap.size >= 1000000 ? '1M+' : snap.size;
    renderSearchResults(''); // initial render
  });
}

/* logout */
btnLogout.onclick = ()=>{ currentUser = null; currentUserObj = null; $('loggedArea').style.display='none'; $('loginForm').style.display='block'; };

/* EDIT PROFILE button */
btnEditProfile.onclick = ()=>{
  if(!currentUser) return alert('Faça login para editar o perfil.');
  openModal(`<h3>Editar perfil</h3>
    <div class="row"><label>Nome</label><input id="m_name" value="${(currentUserObj && currentUserObj.name)||''}" /></div>
    <div class="row"><label>Escola</label><input id="m_school" value="${(currentUserObj && currentUserObj.school)||''}" /></div>
    <div class="row"><label>Foto (cole link)</label><input id="m_foto" value="${(currentUserObj && currentUserObj.foto)||''}" /></div>`, async (root)=>{
      const name = root.querySelector('#m_name').value.trim();
      const school = root.querySelector('#m_school').value.trim();
      const foto = root.querySelector('#m_foto').value.trim();
      await update(ref(db, `users/${currentUser}`), { name, school, foto });
      showNotification('Perfil atualizado',2000);
  });
};

/* Search: filter users and open profile modal */
searchInput.addEventListener('input', e=>{
  renderSearchResults(e.target.value || '');
});

async function renderSearchResults(q){
  const snap = await get(usersRef);
  searchResults.innerHTML = '';
  if(!snap.exists()) return;
  const lower = (q||'').toLowerCase();
  snap.forEach(item=>{
    const u = item.val();
    if(!u) return;
    const label = `${u.name||''} — ${u.phone||''}`;
    if(!lower || label.toLowerCase().includes(lower) || (u.phone||'').includes(lower)){
      const div = document.createElement('div');
      div.style.padding = '8px';
      div.style.borderBottom = '1px solid #eee';
      div.style.cursor = 'pointer';
      div.innerHTML = `<strong>${u.name||''}</strong><div style="font-size:12px;color:#666">${u.school||''} • ${u.phone||''}</div>`;
      div.onclick = ()=> openProfileModal(u);
      searchResults.appendChild(div);
    }
  });
}

/* Profile modal: show info and allow sending internal SMS that expire in 3h; list messages from currentUser to this user with edit/delete */
async function openProfileModal(userObj){
  const phone = userObj.phone;
  // load messages to this recipient
  const msgsSnap = await get(ref(db, `messages/${phone}`));
  const msgs = [];
  if(msgsSnap.exists()){
    msgsSnap.forEach(m=> msgs.push({ id: m.key, ...m.val() }));
  }
  const now = Date.now();
  const validMsgs = msgs.filter(m => (m.expiresAt||0) > now);
  let html = `<h3>${userObj.name || ''}</h3><div><img src="${userObj.foto||'https://via.placeholder.com/100'}" style="width:80px;height:80px;border-radius:8px;object-fit:cover;margin-right:8px;float:left"/></div>
    <div style="margin-top:6px"><strong>Escola:</strong> ${userObj.school||''}</div>
    <div><strong>Telemóvel:</strong> ${userObj.phone||''}</div>
    <div style="clear:both"></div>
    <hr/>
    <h4>Enviar SMS interna (expira em 3h)</h4>
    <textarea id="msg_text" placeholder="Escreve a mensagem" style="height:80px"></textarea>
    <div style="display:flex;gap:8px;margin-top:8px"><button id="sendMsgBtn">Enviar</button><button id="cancelMsg" class="ghost">Fechar</button></div>
    <hr/>
    <h4>Mensagens válidas para este perfil</h4>
    <div id="modalMsgsList">${renderMsgsListHTML(validMsgs)}</div>
  `;
  openModal(html, (root)=>{
    // send button handlers are attached after modal shows
  });
  // attach send/cancel/edit/delete handlers
  setTimeout(()=>{ // small delay until modal elements exist
    const root = $('modalRoot');
    if(!root) return;
    const sendBtn = root.querySelector('#sendMsgBtn');
    const cancelBtn = root.querySelector('#cancelMsg');
    const textEl = root.querySelector('#msg_text');

    cancelBtn.onclick = ()=>{ root.style.display='none'; root.innerHTML=''; };

    sendBtn.onclick = async ()=>{
      if(!currentUser) return alert('Faça login para enviar mensagem interna.');
      const txt = (textEl.value||'').trim();
      if(!txt) return alert('Escreve algo');
      const expiresAt = Date.now() + (3*60*60*1000); // 3 horas
      const payload = { 
