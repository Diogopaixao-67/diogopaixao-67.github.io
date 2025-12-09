
<html lang="pt">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Playmates — Protótipo (WhatsApp style)</title>
<style>
:root{
  --bg:#e9f5ee; --card:#fff; --muted:#6b7280; --accent-3:#ff7b00;
  --shadow: 0 6px 18px rgba(16,24,40,0.06);
}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,-apple-system,"Segoe UI",Roboto,Helvetica,Arial;background:var(--bg);color:#0b1222;min-height:100vh;padding-bottom:100px}
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
.vote-green{background:#25D366;color:#fff;padding:8px 10px;border-radius:8px;border:0}
.photo-btn{background:#f1f1f1;border-radius:8px;padding:6px 8px;border:0}
.smallNote{font-size:12px;color:var(--muted)}

/* modal */
.modal-back{position:fixed;inset:0;background:rgba(0,0,0,0.35);display:flex;align-items:center;justify-content:center;z-index:60}
.modal{background:#fff;padding:14px;border-radius:10px;max-width:720px;width:96%;max-height:88vh;overflow:auto}
.modal h3{margin:0 0 8px 0}
.modal .row{margin:8px 0}
.pill{display:inline-block;padding:6px 8px;border-radius:999px;background:#f7f7f7;margin-right:6px;font-size:12px}

/* posts */
.postCard{padding:10px;border-radius:10px;background:#fff;border:1px solid #eee;margin-top:8px}
.postImg{max-width:100%;border-radius:8px;margin-top:8px;cursor:pointer}

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
        <button id="btnNewPost" class="ghost">Nova Postagem (3h)</button>
      </div>

      <p>Usuários cadastrados: <span id="userCount" class="user-count">0</span></p>

      <h4>Mensagens (Inbox)</h4>
      <div id="inboxList" style="margin-bottom:10px"></div>

      <h4>Pesquisar usuários</h4>
      <input id="searchInput" placeholder="Digite nome ou telefone"/>
      <div id="searchResults"></div>
      <div style="margin-top:8px">
        <button id="btnLogout" class="ghost">Sair</button>
      </div>

      <h4 style="margin-top:12px">Postagens ativas</h4>
      <div id="postsList"></div>
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

    <div style="margin-top:10px" class="card">
      <h3>IBAN da Playmates</h3>
      <div style="display:flex;align-items:center;gap:8px">
        <div id="ibanText" style="font-weight:800"> AO06005500007150984310146</div>
        <button id="copyIban" class="ghost">Copiar</button>
      </div>
      <div style="font-size:12px;color:#666;margin-top:6px">IBAN fixo (opção A). Desejando apoiar um ato inovador.</div>
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
    <!-- Seção Sobre -->
<div style="text-align:center; margin:20px 0;">
  <div style="
    width:150px; 
    height:150px; 
    margin:0 auto; 
    border-radius:50%; 
    overflow:hidden; 
    border:3px solid #FFA500;"> <!-- borda laranja -->
    <img src="https://diogopaixao-67.github.io/Imagens/IMG-20251022-WA0007.jpg" 
         alt="Minha Foto" 
         style="width:100%; height:100%; object-fit:cover;">
  </div>
  <p style="margin-top:10px; font-size:16px; color:#333;">Meu story aqui</p>
</div>
    
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

<!-- modal container -->
<div id="modalRoot" style="display:none"></div>

<script type="module">
/* Firebase imports */
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js";
import { getDatabase, ref, set, get, push, onValue, remove, runTransaction, update } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-database.js";
import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-storage.js";

/* ============ CONFIG ============ */
const firebaseConfig = {
  apiKey: "AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain: "playmates-cc4f7.firebaseapp.com",
  databaseURL: "https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId: "playmates-cc4f7",
  storageBucket: "playmates-cc4f7.firebasestorage.app",
  messagingSenderId: "104004735810",
  appId: "1:104004735810:web:d3ee9a75399d6f0f222edb"
};
/* ================================= */

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const storage = getStorage(app);

/* Helpers */
const $ = id => document.getElementById(id);
const showNotification = (txt, timeout=4000) => {
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

const postsList = $('postsList');
const btnNewPost = $('btnNewPost');

const reqName = $('req_name'), reqSchool = $('req_school'), reqWhats = $('req_whatsapp'), reqPack = $('req_pack');
const reqSubmit = $('reqSubmit'), simPlus = $('simPlus'), reqCountLabel = $('reqCount'), requestsList = $('requestsList');

const notifRef = ref(db, 'notificacao/');
const usersRef = ref(db, 'users/');
const eventosRef = ref(db, 'eventos/');
const countdownRef = ref(db, 'jogos/countdown');
const competitorsRef = ref(db, 'jogos/competitors');
const messagesRootRef = ref(db, 'messages/');
const requestsRef = ref(db, 'jogos/requests');
const requestsCountRef = ref(db, 'jogos/requestsCount');
const postsRef = ref(db, 'posts/');

set(notifRef, "Bem-vindo ao Playmates!").catch(()=>{});

let currentUser = null;
let currentUserObj = null;
let inboxData = {};
let currentTarget = null;

/* modal helper */
function openModal(html, onOk){
  const root = $('modalRoot');
  root.innerHTML = `<div class="modal-back"><div class="modal">${html}<div style="display:flex;gap:8px;margin-top:10px"><button id="modalOk">OK</button><button id="modalCancel" class="ghost">Cancelar</button></div></div></div>`;
  root.style.display = 'block';
  $('modalCancel').onclick = ()=>{ root.style.display='none'; root.innerHTML=''; };
  $('modalOk').onclick = ()=>{ try{ onOk(root); } catch(e){ console.error(e); } root.style.display='none'; root.innerHTML=''; };
}

/* AUTH toggles */
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
    await set(uRef, { name, pass, phone, school, foto:photoURL, points:0, votes:0 });
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
    perfilInfo.innerHTML = `<p><strong>Nome:</strong> ${u.name||''}</p><p><strong>Telemóvel:</strong> ${u.phone||''}</p><p><strong>Escola:</strong> ${u.school||''}</p><p><strong>Pontos:</strong> ${u.points||0}</p><p><strong>Votos:</strong> ${(u.votes||0)}</p>`;
  });

  onValue(usersRef, snap=>{
    userCountLabel.innerText = snap.size >= 1000000 ? '1M+' : snap.size;
    renderSearchResults(''); // initial render
  });

  // subscribe to inbox (messages where currentUser is recipient)
  const myInboxRef = ref(db, `messages/${currentUser}`);
  onValue(myInboxRef, snap=>{
    inboxData = {};
    if(snap.exists()){
      snap.forEach(m=>{
        const o = m.val();
        o.id = m.key;
        const partner = o.sender || 'unknown';
        if(!inboxData[partner]) inboxData[partner] = { last: null };
        // pick last by timestamp
        if(!inboxData[partner].last || (o.ts && o.ts > inboxData[partner].last.ts)) inboxData[partner].last = o;
      });
    }
    renderInbox();
  });

  // listen to posts
  onValue(postsRef, snap=>{
    const arr = [];
    if(snap.exists()){
      snap.forEach(p => { const o = p.val(); o.id = p.key; arr.push(o); });
    }
    renderPosts(arr.filter(p => (p.expiresAt||0) > Date.now()));
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
    <div class="row"><label>Foto (cole link)</label><input id="m_foto" value="${(currentUserObj && currentUserObj.foto)||''}" /></div>
    <div class="row"><label> editar foto </label><input id="editPwd" /></div>`, async (root)=>{
      const name = root.querySelector('#m_name').value.trim();
      const school = root.querySelector('#m_school').value.trim();
      const foto = root.querySelector('#m_foto').value.trim();
      const pwd = root.querySelector('#editPwd').value.trim();
      if(pwd !== '5A') return alert('Senha incorreta para editar foto/perfil');
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

/* Profile modal: conversation view + send/edit/delete */
async function openProfileModal(userObj){
  const otherPhone = userObj.phone;
  if(!otherPhone) return alert('Telefone inválido');
  // collect messages from both paths
  const [snapOther, snapMe] = await Promise.all([ get(ref(db, `messages/${otherPhone}`)), currentUser ? get(ref(db, `messages/${currentUser}`)) : Promise.resolve(null) ]);
  const msgs = [];
  if(snapOther && snapOther.exists()){
    snapOther.forEach(m=>{ const o = m.val(); o.id = m.key; o.recipient = otherPhone; msgs.push(o); });
  }
  if(snapMe && snapMe.exists()){
    snapMe.forEach(m=>{ const o = m.val(); o.id = m.key; o.recipient = currentUser; msgs.push(o); });
  }
  const conv = msgs.filter(m=>{
    const s = m.sender || 'unknown';
    const r = m.recipient || otherPhone;
    return (currentUser && ((s===currentUser && r===otherPhone) || (s===otherPhone && r===currentUser))) || (!currentUser && (s===otherPhone && r===otherPhone));
  });
  const validMsgs = conv.filter(m => (m.expiresAt||0) > Date.now());

  const votes = (userObj.votes||userObj.points||0);

  let html = `<h3>${escapeHtml(userObj.name || '')}</h3>
    <div style="display:flex;gap:12px;align-items:center">
      <img src="${userObj.foto||'https://via.placeholder.com/100'}" style="width:80px;height:80px;border-radius:8px;object-fit:cover"/>
      <div>
        <div><strong>Escola:</strong> ${escapeHtml(userObj.school||'')}</div>
        <div><strong>Telemóvel:</strong> ${escapeHtml(userObj.phone||'')}</div>
        <div style="margin-top:6px"><strong>Votos / Pontos:</strong> ${votes}</div>
        <div style="margin-top:6px"><button id="btnChatFounder" class="ghost">Falar com o Fundador/CEO</button><button id="btnOpenChat" class="ghost">Iniciar conversa</button></div>
      </div>
    </div>
    <hr/>
    <h4>Enviar SMS interna (expira em 3h)</h4>
    <textarea id="msg_text" placeholder="Escreve a mensagem" style="height:80px"></textarea>
    <div style="display:flex;gap:8px;margin-top:8px"><button id="sendMsgBtn">Enviar</button><button id="cancelMsg" class="ghost">Fechar</button></div>
    <hr/>
    <h4>Conversação recente</h4>
    <div id="modalMsgsList">${renderMsgsListHTML(validMsgs, otherPhone)}</div>
  `;
  openModal(html, (root)=>{ /* noop */ });

  setTimeout(()=>{ 
    const root = $('modalRoot');
    if(!root) return;
    const sendBtn = root.querySelector('#sendMsgBtn');
    const cancelBtn = root.querySelector('#cancelMsg');
    const textEl = root.querySelector('#msg_text');
    const chatFounder = root.querySelector('#btnChatFounder');
    const openChatBtn = root.querySelector('#btnOpenChat');

    cancelBtn.onclick = ()=>{ root.style.display='none'; root.innerHTML=''; };

    chatFounder.onclick = ()=> {
      openQuickComposer({ phone: '244941530467', name: 'Diogo Paixão' });
    };

    openChatBtn.onclick = ()=> {
      openQuickComposer({ phone: otherPhone, name: userObj.name || otherPhone });
    };

    sendBtn.onclick = async ()=>{
      if(!currentUser) return alert('Faça login para enviar mensagem interna.');
      const txt = (textEl.value||'').trim();
      if(!txt) return alert('Escreve algo');
      await sendInternalMessage(currentUser, otherPhone, txt);
      showNotification('Mensagem interna enviada (expira em 3h)',2000);
      // refresh list
      const [s1, s2] = await Promise.all([ get(ref(db, `messages/${otherPhone}`)), get(ref(db, `messages/${currentUser}`)) ]);
      const all = [];
      if(s1.exists()) s1.forEach(m=>{ const o=m.val(); o.id=m.key; o.recipient = otherPhone; all.push(o); });
      if(s2.exists()) s2.forEach(m=>{ const o=m.val(); o.id=m.key; o.recipient = currentUser; all.push(o); });
      const conv2 = all.filter(m=>{
        const s = m.sender || 'unknown'; const r = m.recipient || otherPhone;
        return (s===currentUser && r===otherPhone) || (s===otherPhone && r===currentUser);
      });
      const valid2 = conv2.filter(m => (m.expiresAt||0) > Date.now());
      const listDiv = root.querySelector('#modalMsgsList');
      if(listDiv) listDiv.innerHTML = renderMsgsListHTML(valid2, otherPhone);
      textEl.value = '';
      attachMsgButtons(root, currentUser, otherPhone);
    };

    attachMsgButtons(root, currentUser, otherPhone);
  }, 80);
}

function renderMsgsListHTML(msgs, otherPhone){
  if(!msgs || msgs.length===0) return '<div style="color:#666;">Sem mensagens válidas.</div>';
  msgs.sort((a,b)=>b.ts - a.ts);
  return msgs.map(m=>{
    const sender = m.sender || '??';
    const time = new Date(m.ts).toLocaleString();
    const isMine = (sender === currentUser);
    const expiresIn = Math.max(0, (m.expiresAt||0) - Date.now());
    const expText = expiresIn>0 ? `Expira em ${Math.ceil(expiresIn/60000)} min` : 'Expirada';
    return `<div style="padding:8px;border-radius:8px;border:1px solid #eee;background:#fff;margin-top:6px" data-id="${m.id}">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div><strong>${escapeHtml(sender)}</strong> <div style="font-size:12px;color:#666">${time} • ${expText}</div></div>
        <div style="display:flex;gap:6px">
          ${isMine ? `<button class="ghost editMsg" data-id="${m.id}" data-rec="${escapeHtml(m.recipient||otherPhone)}">Editar</button><button class="ghost delMsg" data-id="${m.id}" data-rec="${escapeHtml(m.recipient||otherPhone)}">Apagar</button>` : ''}
        </div>
      </div>
      <div style="margin-top:8px">${escapeHtml(m.text||'')}</div>
    </div>`;
  }).join('');
}

function escapeHtml(s){ return (s||'').toString().replace(/[&<>"']/g, (c)=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[c])); }

function attachMsgButtons(root, viewerPhone, otherPhone){
  if(!root) return;
  root.querySelectorAll('.delMsg').forEach(btn=>{
    btn.onclick = async ()=>{
      const id = btn.dataset.id;
      const recipientPath = btn.dataset.rec || otherPhone;
      const snap = await get(ref(db, `messages/${recipientPath}/${id}`));
      if(!snap.exists()) return alert('Mensagem não encontrada');
      const m = snap.val();
      if(m.sender !== viewerPhone) return alert('Só quem enviou pode apagar esta mensagem.');
      if(!confirm('Apagar esta mensagem?')) return;
      await remove(ref(db, `messages/${recipientPath}/${id}`));
      await remove(ref(db, `messages/${viewerPhone}/${id}`));
      showNotification('Mensagem apagada',2000);
      // refresh modal
      const [s1, s2] = await Promise.all([ get(ref(db, `messages/${otherPhone}`)), get(ref(db, `messages/${viewerPhone}`)) ]);
      const all = [];
      if(s1.exists()) s1.forEach(m2=>{ const o=m2.val(); o.id=m2.key; o.recipient = otherPhone; all.push(o); });
      if(s2.exists()) s2.forEach(m2=>{ const o=m2.val(); o.id=m2.key; o.recipient = viewerPhone; all.push(o); });
      const conv2 = all.filter(m=>{
        const s = m.sender || 'unknown'; const r = m.recipient || otherPhone;
        return (s===viewerPhone && r===otherPhone) || (s===otherPhone && r===viewerPhone);
      });
      const valid2 = conv2.filter(m => (m.expiresAt||0) > Date.now());
      const listDiv = root.querySelector('#modalMsgsList');
      if(listDiv) listDiv.innerHTML = renderMsgsListHTML(valid2, otherPhone);
      attachMsgButtons(root, viewerPhone, otherPhone);
    };
  });

  root.querySelectorAll('.editMsg').forEach(btn=>{
    btn.onclick = async ()=>{
      const id = btn.dataset.id;
      const recipientPath = btn.dataset.rec || otherPhone;
      const snap = await get(ref(db, `messages/${recipientPath}/${id}`));
      if(!snap.exists()) return alert('Mensagem não encontrada');
      const m = snap.val();
      if(m.sender !== viewerPhone) return alert('Só quem enviou pode editar esta mensagem.');
      openModal(`<h3>Editar mensagem</h3><textarea id="edit_text" style="height:100px">${escapeHtml(m.text||'')}</textarea>`, async (r)=>{
        const newText = r.querySelector('#edit_text').value.trim();
        if(!newText) return alert('Mensagem vazia');
        const newExpires = Date.now() + (3*60*60*1000);
        await update(ref(db, `messages/${recipientPath}/${id}`), { text: newText, expiresAt: newExpires, ts: Date.now() });
        await update(ref(db, `messages/${viewerPhone}/${id}`), { text: newText, expiresAt: newExpires, ts: Date.now() });
        showNotification('Mensagem editada e expirará em 3h',2000);
      });
    };
  });
}

/* Mirror message to sender and recipient */
async function sendInternalMessage(senderPhone, recipientPhone, text){
  const keyRef = push(ref(db, `messages/${recipientPhone}`));
  const id = keyRef.key;
  const payload = { sender: senderPhone, recipient: recipientPhone, text, ts: Date.now(), expiresAt: Date.now() + (3*60*60*1000) };
  await set(ref(db, `messages/${recipientPhone}/${id}`), payload);
  await set(ref(db, `messages/${senderPhone}/${id}`), payload);
  return id;
}

function openQuickComposer({ phone, name }){
  openModal(`<h3>Enviar para ${escapeHtml(name||phone)}</h3><textarea id="composer_text" style="height:120px" placeholder="Mensagem"></textarea>`, async (root)=>{
    const txt = root.querySelector('#composer_text').value.trim();
    if(!txt) return alert('Mensagem vazia');
    if(!currentUser) return alert('Faça login para enviar.');
    await sendInternalMessage(currentUser, phone, txt);
    showNotification('Mensagem enviada',2000);
  });
}

/* EVENTOS */
$('btnNovoEvento').onclick = async ()=>{
  if(!currentUser) return alert('Faça login para criar evento.');
  const s = prompt('Senha para publicar evento:');
  if(s !== 'LEX') return alert('Senha errada!');
  const t = prompt('Título:'), c = prompt('Descrição:');
  if(!t||!c) return alert('Dados inválidos');
  await push(eventosRef, { titulo:t, texto:c, views:0, createdBy: currentUser, ts: Date.now() });
};

onValue(eventosRef, snap=>{
  const div = $('eventosLista'); if(!div) return;
  div.innerHTML = '';
  snap.forEach(item=>{
    const k = item.key; const e = item.val();
    div.innerHTML += `<div style="padding:10px;border-bottom:1px solid #eee">
      <h3>${e.titulo||''}</h3><p>${e.texto||''}</p><p class="event-views">👁️ ${e.views||0}</p>
      <div style="display:flex;gap:8px;margin-top:6px">
        <button class="ghost viewEvent" data-id="${k}">Ver</button>
        <button class="ghost delEvent" data-id="${k}">Eliminar</button>
      </div>
    </div>`;
  });
  document.querySelectorAll('.delEvent').forEach(btn=>{
    btn.onclick = async ()=>{
      const id = btn.dataset.id;
      const pw = prompt('Senha  para eliminar:');
      if(pw !== 'LEX') return alert('Senha incorreta.');
      await remove(ref(db, `eventos/${id}`));
      showNotification('Evento eliminado',2000);
    };
  });
});

/* JOGOS - countdown */
function formatHMS(ms){
  if(ms < 0) ms = 0;
  const total = Math.floor(ms/1000);
  const h = Math.floor(total/3600).toString().padStart(2,'0');
  const m = Math.floor((total%3600)/60).toString().padStart(2,'0');
  const s = Math.floor(total%60).toString().padStart(2,'0');
  return `${h}:${m}:${s}`;
}

onValue(countdownRef, snap=>{
  const v = snap.val();
  currentTarget = v && v.target ? v.target : null;
  countdownLabel.innerText = formatHMS(currentTarget?currentTarget - Date.now():0);
});
setInterval(()=>{ countdownLabel.innerText = formatHMS(currentTarget?currentTarget - Date.now():0); }, 1000);

editCountdownBtn.onclick = async ()=>{
  if(!currentUser) return alert('Faça login para editar o contador.');
  const pw = prompt('Senha para editar o contador:');
  if(pw !== 'A8') return alert('Senha incorreta.');
  let h = parseInt(prompt('Horas (0-99):','0')) || 0;
  let m = parseInt(prompt('Minutos (0-59):','0')) || 0;
  let s = parseInt(prompt('Segundos (0-59):','30')) || 0;
  if(h<0) h=0; if(m<0) m=0; if(s<0) s=0;
  const totalMs = ((h*3600)+(m*60)+s)*1000;
  await set(countdownRef, { target: Date.now() + totalMs });
  showNotification('Contador atualizado',2000);
};

/* Competitors */
async function ensureDefaultCompetitors(){
  const snap = await get(competitorsRef);
  if(!snap.exists()){
    const defaults = {
      1:{ name:'Concorrente 1', school:'Escola A', votes:0, photo:'' },
      2:{ name:'Concorrente 2', school:'Escola B', votes:0, photo:'' },
      3:{ name:'Concorrente 3', school:'Escola C', votes:0, photo:'' }
    };
    await set(competitorsRef, defaults);
  }
}
ensureDefaultCompetitors();

function renderCompetitors(listObj){
  competitorsList.innerHTML = '';
  [1,2,3].forEach(id=>{
    const c = listObj && listObj[id] ? listObj[id] : { name:`Concorrente ${id}`, school:'', votes:0, photo:'' };
    const card = document.createElement('div'); card.className='compCard';
    const img = document.createElement('img'); img.src = c.photo || 'https://via.placeholder.com/80';
    const info = document.createElement('div'); info.className='compInfo';
    info.innerHTML = `<div style="font-weight:800">${c.name}</div><div style="color:#666">${c.school||''}</div><div style="margin-top:8px;font-weight:700;color:var(--accent-3)">Votos: <span id="votes_${id}">${c.votes||0}</span></div>`;
    const actions = document.createElement('div'); actions.className='compActions';

    const editBtn = document.createElement('button'); editBtn.innerText='⚫ Editar'; editBtn.className='edit-orange';
    editBtn.onclick = async ()=>{
      if(!currentUser) return alert('Faça login para editar concorrentes.');
      const pw = prompt('Senha para editar concorrente:');
      if(pw !== '5A') return alert('Senha incorreta.');
      openModal(`<h3>Editar concorrente ${id}</h3>
        <div class="row"><label>Nome</label><input id="m_name" value="${(c.name||'')}" /></div>
        <div class="row"><label>Escola</label><input id="m_school" value="${(c.school||'')}" /></div>
        <div class="row"><label>Votos</label><input id="m_votes" type="number" value="${(c.votes||0)}" /></div>
        <div class="row"><label>Imagem: (cole link)</label><input id="m_link" placeholder="https://..." /></div>`, async (root)=>{
          const newName = root.querySelector('#m_name').value.trim();
          const newSchool = root.querySelector('#m_school').value.trim();
          const newVotes = parseInt(root.querySelector('#m_votes').value) || 0;
          const link = root.querySelector('#m_link').value.trim();
          const updates = { name: newName, school: newSchool, votes: newVotes };
          if(link) updates.photo = link;
          await update(ref(db, `jogos/competitors/${id}`), updates);
          showNotification('Concorrente atualizado',2000);
        });
    };

    const photoBtn = document.createElement('button'); photoBtn.innerText='📷 Foto'; photoBtn.className='photo-btn';
    photoBtn.onclick = ()=> { if(fileInputs[id]) fileInputs[id].click(); };

    const voteBtn = document.createElement('button'); voteBtn.innerText='VOTAR'; voteBtn.className='vote-green';
    voteBtn.onclick = async ()=>{
      if(!currentUser) return alert('Faça login para votar.');
      const vsnap = await get(ref(db, `users/${currentUser}`));
      const voter = vsnap.exists() ? vsnap.val().name : 'Anon';
      const msg = `Olá eu chamo-me ${voter} e desejo votar no concorrente ${c.name}`;
      const waLink = `https://wa.me/244941530467?text=${encodeURIComponent(msg)}`;
      window.open(waLink, '_blank');
      await runTransaction(ref(db, `jogos/competitors/${id}/votes`), cur => (cur||0)+1);
      await push(ref(db, 'jogos/votes'), { voter: currentUser, competitor: id, ts: Date.now() });
    };

    actions.appendChild(editBtn); actions.appendChild(photoBtn); actions.appendChild(voteBtn);
    card.appendChild(img); card.appendChild(info); card.appendChild(actions);
    competitorsList.appendChild(card);
  });
}

onValue(competitorsRef, snap=>{
  const v = snap.val() || {};
  renderCompetitors(v);
  [1,2,3].forEach(id=>{
    const el = document.getElementById(`votes_${id}`);
    if(el) el.innerText = (v[id] && v[id].votes) ? v[id].votes : 0;
  });
});

[1,2,3].forEach(id=>{
  const inp = fileInputs[id];
  if(!inp) return;
  inp.onchange = async (e)=>{
    try{
      const file = e.target.files[0];
      if(!file) return;
      if(!currentUser) return alert('Faça login para enviar foto.');
      const path = `jogos/competitors/${id}/photo_${Date.now()}`;
      const sRefPath = sRef(storage, path);
      await uploadBytes(sRefPath, file);
      const url = await getDownloadURL(sRefPath);
      await update(ref(db, `jogos/competitors/${id}`), { photo: url });
      showNotification('Foto enviada com sucesso',2000);
    }catch(err){
      console.error(err); alert('Erro ao enviar foto: '+(err.message||err));
    } finally { inp.value=''; }
  };
});

/* Requests */
reqSubmit.onclick = async ()=>{
  if(!reqName.value || !reqWhats.value) return alert('Preencha nome e número WhatsApp');
  const payload = {
    name: reqName.value.trim(),
    school: reqSchool.value.trim(),
    whatsapp: reqWhats.value.trim(),
    pack: reqPack.value,
    status: 'pending',
    user: currentUser || null,
    ts: Date.now()
  };
  const p = await push(requestsRef, payload);
  await runTransaction(requestsCountRef, cur => (cur||0)+1);
  showNotification('Pedido enviado',2000);
  reqName.value=''; reqSchool.value=''; reqWhats.value=''; reqPack.value='free_400';
};

onValue(requestsCountRef, snap=>{
  const v = snap.val() || 0;
  reqCountLabel.innerText = v;
});

onValue(requestsRef, snap=>{
  const arr = [];
  if(snap.exists()){
    snap.forEach(item => {
      const o = item.val(); o.id = item.key;
      if(!o) return;
      arr.push(o);
    });
  }
  const pending = arr.filter(r => r.status === 'pending');
  requestsList.innerHTML = '';
  pending.forEach(r=>{
    const row = document.createElement('div'); row.className='requestRow';
    row.innerHTML = `<div><div style="font-weight:700">${r.name}</div><div class="meta">${r.school||''} • ${r.whatsapp||''} • ${r.pack||''}</div></div>
      <div style="display:flex;gap:8px;align-items:center">
        <button class="ghost viewReq" data-id="${r.id}">Ver</button>
        <button class="ghost delReq" data-id="${r.id}" title="Eliminar pedido">❌</button>
      </div>`;
    requestsList.appendChild(row);
  });
  document.querySelectorAll('.delReq').forEach(btn=>{
    btn.onclick = async ()=>{
      const id = btn.dataset.id;
      if(!currentUser) return alert('Faça login para eliminar pedido.');
      const snap = await get(ref(db, `jogos/requests/${id}`));
      if(!snap.exists()) return alert('Pedido não encontrado');
      const o = snap.val();
      if(o.user === currentUser){
        await remove(ref(db, `jogos/requests/${id}`));
        await runTransaction(requestsCountRef, cur => (cur||1)-1);
        showNotification('Pedido eliminado',2000);
        return;
      }
      const pw = prompt('Senha para eliminar pedido (ou cancela):');
      if(pw !== 'LEX') return alert('Senha incorreta.');
      await remove(ref(db, `jogos/requests/${id}`));
      await runTransaction(requestsCountRef, cur => (cur||1)-1);
      showNotification('Pedido eliminado (admin)',2000);
    };
  });

  document.querySelectorAll('.viewReq').forEach(btn=>{
    btn.onclick = async ()=>{
      const id = btn.dataset.id;
      const snap = await get(ref(db, `jogos/requests/${id}`));
      if(!snap.exists()) return alert('Pedido não encontrado');
      const o = snap.val();
      openModal(`<h3>Pedido</h3>
        <div><strong>Nome:</strong> ${o.name||''}</div>
        <div><strong>Escola:</strong> ${o.school||''}</div>
        <div><strong>WhatsApp:</strong> ${o.whatsapp||''}</div>
        <div><strong>Pack:</strong> ${o.pack||''}</div>`, ()=>{});
    };
  });
});

simPlus.onclick = async ()=>{
  const pw = prompt('Senha  para ver simulador:');
  if(pw !== 'LEX') return alert('Senha incorreta.');
  const snap = await get(requestsRef);
  const arr = []; if(snap.exists()) snap.forEach(s=>arr.push({ id: s.key, ...s.val() }));
  const html = `<h3>Registos de pedidos</h3>${arr.length ? arr.map(a=>`<div style="padding:8px;border-bottom:1px solid #eee"><strong>${a.name}</strong> • ${a.school||''} • ${a.pack||''} • ${a.whatsapp||''}</div>`).join('') : '<div style="color:#666">Sem registos</div>'}`;
  openModal(html, ()=>{});
};

/* POSTS */
btnNewPost.onclick = ()=>{
  if(!currentUser) return alert('Faça login para postar.');
  openModal(`<h3>Nova Postagem (expira em 3h)</h3>
    <div class="row"><label>Texto</label><textarea id="post_text" style="height:120px"></textarea></div>
    <div class="row"><label>Imagem (colar link)</label><input id="post_img" placeholder="https://..."/></div>`, async (root)=>{
      const txt = root.querySelector('#post_text').value.trim();
      const img = root.querySelector('#post_img').value.trim();
      if(!txt && !img) return alert('A postagem está vazia');
      const payload = { author: currentUser, text: txt, img: img||'', ts: Date.now(), expiresAt: Date.now() + (3*60*60*1000), views: 0 };
      await push(postsRef, payload);
      showNotification('Post criado (expira em 3h)',2000);
  });
};

function renderPosts(postsArr){
  postsList.innerHTML = '';
  postsArr.sort((a,b)=>b.ts - a.ts);
  postsArr.forEach(p=>{
    const div = document.createElement('div'); div.className = 'postCard';
    div.innerHTML = `<div style="font-size:13px;color:#666"><strong>${escapeHtml(p.author||'Anon')}</strong> • ${new Date(p.ts).toLocaleString()}</div>
      <div style="margin-top:6px">${escapeHtml(p.text||'')}</div>
      ${p.img ? `<img class="postImg" src="${p.img}" alt="img" />` : ''}
      <div style="display:flex;align-items:center;gap:12px;margin-top:8px"><div class="event-views">👁️ <span id="postViews_${p.id}">${p.views||0}</span></div><button class="ghost viewPostBtn" data-id="${p.id}">Ver / Contar visão</button></div>`;
    postsList.appendChild(div);
  });

  document.querySelectorAll('.viewPostBtn').forEach(btn=>{
    btn.onclick = async ()=>{
      const id = btn.dataset.id;
      await runTransaction(ref(db, `posts/${id}/views`), cur => (cur||0)+1);
    };
  });

  document.querySelectorAll('.postImg').forEach(img=>{
    img.onclick = async (ev)=>{
      const parent = img.closest('.postCard');
      if(!parent) return;
      const btn = parent.querySelector('.viewPostBtn');
      if(btn) btn.click();
    };
  });
}

/* Inbox */
function renderInbox(){
  const list = $('inboxList');
  list.innerHTML = '';
  const partners = Object.keys(inboxData||{});
  if(partners.length===0){ list.innerHTML = '<div style="color:#666">Sem mensagens</div>'; return; }
  partners.forEach(p=>{
    const last = inboxData[p].last;
    const preview = last && last.text ? (last.text.length>50 ? last.text.slice(0,50)+'...' : last.text) : '';
    const div = document.createElement('div');
    div.style.padding = '8px'; div.style.borderBottom = '1px solid #eee'; div.style.cursor='pointer';
    div.innerHTML = `<strong>${p}</strong><div style="font-size:12px;color:#666">${preview}</div>`;
    div.onclick = async ()=> {
      const snap = await get(ref(db, `users/${p}`));
      const u = snap.exists() ? snap.val() : { phone: p, name: p };
      openProfileModal(u);
    };
    list.appendChild(div);
  });
}

/* IBAN copy */
$('copyIban').onclick = ()=>{
  const text = $('ibanText').innerText;
  navigator.clipboard?.writeText(text).then(()=> showNotification('IBAN copiado para a área de transferência',2000)).catch(()=> alert('Não foi possível copiar'));
};

/* Ensure UI defaults */
document.addEventListener('DOMContentLoaded', ()=> {
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.getElementById('sec-sms').classList.add('active');
});

/* Final note: DB rules must allow read/write during testing.
   For production, tighten security rules and remove plaintext passwords from DB.
*/
</script>

<footer style="width:100%; text-align:center; padding:15px; margin-top:20px;
background:#f2f2f2; color:#333; font-size:14px; border-top:1px solid #ddd;">
  © 2023–2025 Playmates • Todos os direitos reservados  
  | <a href="#" style="color:#333; text-decoration:underline;">Termos de Uso</a>
</footer>

</body>
</html>
