<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Playmates — Final</title>

<!-- Firebase CDN (v8 classic) -->
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-storage.js"></script>

<style>
  :root{--accent:#ff7f00;--muted:#666;--bg:#fbfbfb}
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,Arial,sans-serif;background:var(--bg);color:#111}
  header{background:var(--accent);color:#fff;padding:14px;text-align:center;font-weight:800;letter-spacing:0.4px}
  main{padding:16px;padding-bottom:90px}
  .card{background:#fff;border-radius:12px;padding:12px;box-shadow:0 8px 28px rgba(10,10,10,0.05);margin-bottom:12px}
  input,textarea,button{width:100%;padding:10px;border-radius:10px;border:1px solid #eee;font-size:15px}
  button{background:var(--accent);color:#fff;border:none;font-weight:700;cursor:pointer}
  .muted{color:var(--muted);font-size:13px}
  .profile-row{display:flex;gap:12px;align-items:center}
  .avatar{width:88px;height:88px;border-radius:50%;object-fit:cover}
  .small-btn{display:inline-block;padding:8px 10px;border-radius:10px;background:transparent;color:var(--accent);border:1px solid rgba(255,127,0,0.12);cursor:pointer}
  nav.bottom-nav{position:fixed;bottom:0;left:0;right:0;background:#fff;border-top:1px solid #eee;display:flex;justify-content:space-around;padding:8px 6px}
  nav.bottom-nav button{background:transparent;border:none;color:var(--accent);font-weight:800}
  .tab{display:none}
  .visible{display:block}
  .top-stats{display:flex;justify-content:space-between;gap:10px;align-items:center}
  .badge{background:#fff;padding:8px;border-radius:10px;box-shadow:0 6px 18px rgba(0,0,0,0.04);font-weight:700}
  .clock{font-family:monospace;font-weight:800;color:var(--accent);font-size:20px}
  .orange-text{color:var(--accent);font-weight:800}
  .box-list{display:flex;flex-direction:column;gap:8px;margin-top:8px}
  .list-item{background:#fff;padding:10px;border-radius:10px;display:flex;gap:10px;align-items:center;box-shadow:0 6px 18px rgba(0,0,0,0.03)}
  .thumb{width:56px;height:56px;border-radius:50%;object-fit:cover}
  .muted-small{font-size:12px;color:#888}
  footer{height:72px}
  @media(min-width:700px){ main{max-width:720px;margin:0 auto} nav.bottom-nav{display:none} }
</style>
</head>
<body>
<header>Playmates</header>

<main>
  <!-- AUTH -->
  <section id="authSection" class="card">
    <h3>Entrar</h3>
    <input id="loginPhone" placeholder="Número de telemóvel (ex: 912345678)" />
    <input id="loginPass" placeholder="Senha" type="password" />
    <button id="btnLogin">Entrar</button>
    <p class="muted">4 tentativas erradas bloqueiam a conta.</p>

    <hr style="margin:12px 0;border:none;border-top:1px solid #f0f0f0" />

    <h3>Cadastrar</h3>
    <input id="regName" placeholder="Nome completo" />
    <input id="regSchool" placeholder="Nome da escola" />
    <input id="regPhone" placeholder="Número de telemóvel" />
    <input id="regPass" placeholder="Senha" type="password" />
    <input id="regPhoto" type="file" accept="image/*" />
    <button id="btnRegister">Cadastrar</button>
    <p class="muted">A foto é enviada ao Firebase Storage.</p>
  </section>

  <!-- APP -->
  <section id="appSection" style="display:none">
    <div class="card top-stats">
      <div>
        <div id="welcomeText" class="orange-text">Boas vindas ao Playmates</div>
        <div id="welcomeSchool" class="muted-small"></div>
      </div>
      <div style="text-align:right">
        <div class="clock" id="clockDisplay">Aguardando</div>
        <button class="small-btn" id="btnEditClock">⏱ Editar (senha)</button>
      </div>
    </div>

    <div class="card profile-row">
      <img id="mePhoto" class="avatar" src="https://via.placeholder.com/150" />
      <div style="flex:1">
        <div id="meName" style="font-weight:800;font-size:18px">-</div>
        <div id="meSchool" class="muted-small">-</div>
        <div id="mePhone" class="muted-small">-</div>
        <div style="margin-top:8px">
          <span id="mePoints" class="badge">0 pts</span>
          <button class="small-btn" id="btnEditProfile">Editar perfil</button>
          <button class="small-btn" id="btnLogout">Sair</button>
        </div>
      </div>
    </div>

    <div class="card">
      <div id="totalUsers" class="badge">Total de perfis: 0</div>
    </div>

    <!-- TABS -->
    <div id="dashboardTab" class="tab visible card">
      <h4>Dashboard</h4>
      <div id="feedRecent" class="box-list"></div>
    </div>

    <div id="smsTab" class="tab card">
      <h4>SMS (até 130 chars, expiram 3h)</h4>
      <input id="smsTo" placeholder="Número destinatário" />
      <textarea id="smsText" placeholder="Mensagem (máx 130)" maxlength="130"></textarea>
      <button id="btnSendSMS">Enviar SMS</button>
      <h4 style="margin-top:12px">Caixa de entrada</h4>
      <div id="smsInbox" class="box-list"></div>
    </div>

    <div id="searchTab" class="tab card">
      <h4>Pesquisa</h4>
      <input id="searchInput" placeholder="Nome, escola ou número" />
      <button id="btnSearch">Buscar</button>
      <div id="searchResults" class="box-list"></div>
    </div>

    <div id="postsTab" class="tab card">
      <h4>Postagens (expiram 3h)</h4>
      <textarea id="postText" placeholder="Texto"></textarea>
      <input id="postFile" type="file" accept="image/*" />
      <button id="btnPost">Publicar</button>
      <div id="postsList" class="box-list"></div>
    </div>

    <div id="sosTab" class="tab card">
      <h4>SOS Emprego (admin LEX)</h4>
      <button id="btnAddSOS">Criar SOS (LEX)</button>
      <div id="sosList" class="box-list"></div>
    </div>

    <div id="notTab" class="tab card">
      <h4>Notificações</h4>
      <div id="notList" class="box-list"></div>
    </div>
  </section>
</main>

<nav class="bottom-nav">
  <button data-target="dashboardTab">Dashboard</button>
  <button data-target="smsTab">SMS</button>
  <button data-target="searchTab">Pesquisar</button>
  <button data-target="postsTab">Postagens</button>
  <button data-target="sosTab">SOS</button>
</nav>

<footer></footer>

<script>
/* ---------------------------
   FIREBASE CONFIG (classic)
   --------------------------- */
const firebaseConfig = {
  apiKey: "AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain: "playmates-cc4f7.firebaseapp.com",
  databaseURL: "https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId: "playmates-cc4f7",
  storageBucket: "playmates-cc4f7.firebasestorage.app",
  messagingSenderId: "104004735810",
  appId: "1:104004735810:web:d3ee9a75399d6f0f222edb"
};
firebase.initializeApp(firebaseConfig);
const db = firebase.database();
const storage = firebase.storage();

/* ---------------------------
   PASSWORDS (prototype)
   --------------------------- */
const ADMIN_PASS = "LEX";
const CLOCK_PASS = "LEX";

/* ---------------------------
   DOM helpers & state
   --------------------------- */
const $ = id => document.getElementById(id);
const tabs = Array.from(document.querySelectorAll('nav.bottom-nav button'));
const state = { user: null };

/* ---------------------------
   NAVIGATION (single function)
   --------------------------- */
function showTab(tabId) {
  document.querySelectorAll('.tab').forEach(t => { t.style.display = 'none'; t.classList.remove('visible'); });
  const node = document.getElementById(tabId);
  if (node) { node.style.display = 'block'; node.classList.add('visible'); }
}
tabs.forEach(btn => btn.addEventListener('click', () => showTab(btn.dataset.target)));
showTab('dashboardTab');

/* ---------------------------
   AUTH: register / login / logout
   --------------------------- */
$('btnRegister').addEventListener('click', async () => {
  const name = $('regName').value.trim();
  const school = $('regSchool').value.trim();
  const phone = $('regPhone').value.trim();
  const pass = $('regPass').value.trim();
  const file = $('regPhoto').files[0];
  if (!name || !school || !phone || !pass || !file) { alert('Preencha tudo.'); return; }

  const userRef = db.ref('users/' + phone);
  const snap = await userRef.once('value');
  if (snap.exists()) { alert('Número já cadastrado.'); return; }

  // upload photo
  const photoRef = storage.ref().child('profilePhotos/' + phone + '_' + Date.now());
  const up = await photoRef.put(file);
  const url = await up.ref.getDownloadURL();

  await userRef.set({
    name, school, phone, password: pass, photo: url,
    points: 0, attempts: 0, blocked: false, createdAt: Date.now()
  });

  alert('Cadastro feito. Faça login.');
  $('regName').value = $('regSchool').value = $('regPhone').value = $('regPass').value = '';
  $('regPhoto').value = '';
});

$('btnLogin').addEventListener('click', async () => {
  const phone = $('loginPhone').value.trim();
  const pass = $('loginPass').value.trim();
  if (!phone || !pass) { alert('Preencha tudo.'); return; }

  const userRef = db.ref('users/' + phone);
  const snap = await userRef.once('value');
  if (!snap.exists()) { alert('Conta não existe.'); return; }
  const u = snap.val();
  if (u.blocked) { alert('Conta bloqueada.'); return; }
  if (u.password !== pass) {
    const attempts = (u.attempts || 0) + 1;
    await userRef.update({ attempts });
    if (attempts >= 4) { await userRef.update({ blocked: true }); alert('Conta bloqueada (4 tentativas).'); }
    else alert('Senha incorreta. Tentativas: ' + attempts + '/4');
    return;
  }

  await userRef.update({ attempts: 0 });
  state.user = phone;
  $('authSection').style.display = 'none';
  $('appSection').style.display = 'block';
  loadUserRealtime(phone);
  startClockListener();
  bindRealtimeLists();
  listenNotifications();
  updateTotalUsers();
  showTab('dashboardTab');
});

/* logout */
$('btnLogout').addEventListener('click', () => {
  state.user = null;
  $('authSection').style.display = 'block';
  $('appSection').style.display = 'none';
  // clear UI
  $('mePhoto').src = 'https://via.placeholder.com/150';
  $('meName').innerText = '-';
  $('meSchool').innerText = '';
  $('mePhone').innerText = '';
  $('mePoints').innerText = '0 pts';
});

/* ---------------------------
   LOAD USER REALTIME
   --------------------------- */
function loadUserRealtime(phone) {
  const uRef = db.ref('users/' + phone);
  uRef.on('value', snap => {
    if (!snap.exists()) return;
    const u = snap.val();
    $('mePhoto').src = u.photo || 'https://via.placeholder.com/150';
    $('meName').innerText = u.name || '-';
    $('meSchool').innerText = u.school || '';
    $('mePhone').innerText = u.phone || '';
    $('mePoints').innerText = (u.points || 0) + ' pts';
    $('welcomeText').innerText = 'Boas vindas ao Playmates, ' + (u.name || '');
    $('welcomeSchool').innerText = u.school || '';
  });
}

/* edit profile */
$('btnEditProfile').addEventListener('click', () => {
  if (!state.user) return alert('Faça login');
  const newName = prompt('Nome:', $('meName').innerText) || $('meName').innerText;
  const newSchool = prompt('Escola:', $('meSchool').innerText) || $('meSchool').innerText;
  const newPhone = prompt('Número (se quiser alterar):', $('mePhone').innerText) || $('mePhone').innerText;
  const fileInput = document.createElement('input'); fileInput.type = 'file'; fileInput.accept = 'image/*';
  fileInput.onchange = async () => {
    const file = fileInput.files[0];
    if (!file) return;
    const photoRef = storage.ref().child('profilePhotos/' + state.user + '_' + Date.now());
    const up = await photoRef.put(file);
    const url = await up.ref.getDownloadURL();
    await db.ref('users/' + state.user).update({ name: newName, school: newSchool, phone: newPhone, photo: url });
    alert('Perfil atualizado');
  };
  fileInput.click();
});

/* ---------------------------
   CLOCK (timer)
   --------------------------- */
let clockIntervalId = null;
function startClockListener(){
  db.ref('timer').on('value', snap => {
    if (!snap.exists()) { $('clockDisplay').innerText = 'Aguardando'; return; }
    const data = snap.val();
    const endMs = data.endAt || 0;
    if (clockIntervalId) clearInterval(clockIntervalId);
    clockIntervalId = setInterval(() => {
      const diff = endMs - Date.now();
      if (diff <= 0) { $('clockDisplay').innerText = '00:00:00'; clearInterval(clockIntervalId); return; }
      const h = Math.floor(diff / 3600000);
      const m = Math.floor((diff % 3600000) / 60000);
      const s = Math.floor((diff % 60000) / 1000);
      $('clockDisplay').innerText = `${String(h).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
    }, 1000);
  });
}

$('btnEditClock').addEventListener('click', async () => {
  const pw = prompt('Senha para editar relógio:');
  if (pw !== CLOCK_PASS) return alert('Senha incorreta');
  const h = parseInt(prompt('Horas:')) || 0;
  const m = parseInt(prompt('Minutos:')) || 0;
  const s = parseInt(prompt('Segundos:')) || 0;
  const totalMs = ((h * 3600) + (m * 60) + s) * 1000;
  await db.ref('timer').set({ endAt: Date.now() + totalMs });
  alert('Relógio atualizado e sincronizado');
});

/* ---------------------------
   TOTAL USERS
   --------------------------- */
function updateTotalUsers() {
  db.ref('users').on('value', snap => {
    const count = snap.exists() ? Object.keys(snap.val()).length : 0;
    $('totalUsers').innerText = 'Total de perfis: ' + count;
  });
}

/* ---------------------------
   SMS (3h)
   --------------------------- */
$('btnSendSMS').addEventListener('click', async () => {
  if(!state.user) return alert('Faça login');
  const to = $('smsTo').value.trim();
  const text = $('smsText').value.trim();
  if(!to || !text) return alert('Preencha destinatário e mensagem');
  if(text.length > 130) return alert('Máx 130 caracteres');
  const now = Date.now();
  await db.ref('sms').push({ from: state.user, to, text, createdAt: now, expiresAt: now + 3*3600*1000 });
  $('smsTo').value = ''; $('smsText').value = '';
  alert('SMS enviado');
});

/* inbox render */
function renderInbox() {
  db.ref('sms').on('value', snap => {
    const list = $('smsInbox'); list.innerHTML = '';
    if (!snap.exists()) { list.innerHTML = '<div class="muted-small">Sem mensagens</div>'; return; }
    const data = snap.val();
    Object.keys(data).reverse().forEach(k => {
      const m = data[k];
      if (m.to !== state.user) return;
      if (m.expiresAt && m.expiresAt < Date.now()) { db.ref('sms/' + k).remove().catch(()=>{}); return; }
      db.ref('users/' + m.from).once('value').then(uSnap => {
        const fromName = uSnap.exists() ? uSnap.val().name : m.from;
        const div = document.createElement('div'); div.className = 'list-item';
        div.innerHTML = `<img class="thumb" src="${(uSnap.exists()? uSnap.val().photo : 'https://via.placeholder.com/80')}" /><div style="flex:1"><strong>${fromName}</strong><div class="muted-small">${m.text}</div><div class="muted-small">${new Date(m.createdAt).toLocaleString()}</div></div>`;
        list.appendChild(div);
      });
    });
  });
}

/* ---------------------------
   POSTS (3h)
   --------------------------- */
$('btnPost').addEventListener('click', async () => {
  if(!state.user) return alert('Faça login');
  const text = $('postText').value.trim();
  const file = $('postFile').files[0];
  if(!text && !file) return alert('Escreva algo ou selecione uma imagem');
  const now = Date.now();
  if(file){
    const pRef = storage.ref().child('posts/' + state.user + '_' + now + '_' + file.name);
    const up = await pRef.put(file);
    const url = await up.ref.getDownloadURL();
    await db.ref('posts').push({ user: state.user, text, img: url, createdAt: now, expiresAt: now + 3*3600*1000 });
  } else {
    await db.ref('posts').push({ user: state.user, text, img: '', createdAt: now, expiresAt: now + 3*3600*1000 });
  }
  $('postText').value = ''; $('postFile').value = '';
  alert('Post criado');
});

function renderPosts() {
  db.ref('posts').on('value', snap => {
    const list = $('postsList'); list.innerHTML = '';
    if (!snap.exists()) { list.innerHTML = '<div class="muted-small">Sem postagens</div>'; return; }
    const data = snap.val();
    const arr = Object.keys(data).map(k=>({id:k, ...data[k]})).sort((a,b)=>b.createdAt - a.createdAt);
    arr.forEach(p => {
      if (p.expiresAt && p.expiresAt < Date.now()) { db.ref('posts/' + p.id).remove().catch(()=>{}); return; }
      const div = document.createElement('div'); div.className = 'list-item';
      div.innerHTML = `<img class="thumb" src="${p.img || 'https://via.placeholder.com/80'}" /><div style="flex:1"><strong>${p.user}</strong><div class="muted-small">${(p.text||'').slice(0,120)}</div><div class="muted-small">${new Date(p.createdAt).toLocaleString()}</div></div>`;
      list.appendChild(div);
    });
    // update dashboard feed
    const feed = $('feedRecent'); feed.innerHTML = '';
    arr.slice(0,5).forEach(p => {
      if (p.expiresAt && p.expiresAt < Date.now()) return;
      const f = document.createElement('div'); f.className='list-item';
      f.innerHTML = `<img class="thumb" src="${p.img||'https://via.placeholder.com/80'}" /><div style="flex:1"><div class="muted-small">${new Date(p.createdAt).toLocaleString()}</div><div>${(p.text||'').slice(0,80)}</div></div>`;
      feed.appendChild(f);
    });
  });
}

/* ---------------------------
   SEARCH + TOPAR (0.5%)
   --------------------------- */
$('btnSearch').addEventListener('click', async () => {
  const q = $('searchInput').value.trim().toLowerCase();
  const snap = await db.ref('users').once('value');
  const resultsDiv = $('searchResults'); resultsDiv.innerHTML = '';
  if(!snap.exists()){ resultsDiv.innerHTML = '<div class="muted-small">Sem perfis</div>'; return; }
  const data = snap.val();
  Object.keys(data).forEach(phone => {
    const u = data[phone];
    if ( (u.name||'').toLowerCase().includes(q) || (u.school||'').toLowerCase().includes(q) || (u.phone||'').includes(q) ) {
      const div = document.createElement('div'); div.className='list-item';
      div.innerHTML = `<img class="thumb" src="${u.photo || 'https://via.placeholder.com/80'}" /><div style="flex:1"><strong>${u.name}</strong><div class="muted-small">${u.school} • ${u.phone}</div></div><button id="top_${phone}" class="small-btn">Topar</button>`;
      resultsDiv.appendChild(div);
      document.getElementById('top_' + phone).addEventListener('click', async () => {
        if(!state.user) return alert('Faça login');
        if(state.user === phone) return alert('Não pode topar a si mesmo');
        const key = state.user + '_' + phone;
        const tSnap = await db.ref('topados/' + key).once('value');
        if (tSnap.exists()) return alert('Já topou este perfil');
        await db.ref('topados/' + key).set({ from: state.user, to: phone, at: Date.now() });
        const userRef = db.ref('users/' + phone);
        const uSnap = await userRef.once('value');
        const pts = (uSnap.val() && uSnap.val().points) ? Number(uSnap.val().points) : 0;
        const newPts = Math.round((pts * 1.005) * 100) / 100;
        await userRef.update({ points: newPts });
        await db.ref('notifications').push({ to: phone, message: `${state.user} te ajudou a ganhar dinheiro`, createdAt: Date.now() });
        alert('Topado! Perfil recebeu +0.5% pontos');
      });
    }
  });
});

/* ---------------------------
   SOS ADMIN (LEX)
   --------------------------- */
$('btnAddSOS').addEventListener('click', () => {
  const pw = prompt('Senha admin (LEX):');
  if (pw !== ADMIN_PASS) return alert('Senha incorreta');
  const text = prompt('Texto do SOS:');
  const fileInput = document.createElement('input'); fileInput.type='file'; fileInput.accept='image/*';
  fileInput.onchange = async () => {
    const file = fileInput.files[0];
    const pRef = stora
