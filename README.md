<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Playmates — Final (Email+Senha)</title>

<!-- Firebase v8 classic -->
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-storage.js"></script>

<style>
  :root{--accent:#ff7f00;--muted:#666;--bg:#fbfbfb}
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,Arial,sans-serif;background:var(--bg);color:#111}
  header{background:var(--accent);color:#fff;padding:14px;text-align:center;font-weight:800;letter-spacing:0.4px}
  main{padding:16px;padding-bottom:100px}
  .card{background:#fff;border-radius:12px;padding:12px;box-shadow:0 8px 28px rgba(10,10,10,0.05);margin-bottom:12px}
  input,textarea,button,select{width:100%;padding:10px;border-radius:10px;border:1px solid #eee;font-size:15px}
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
  #debug {position:fixed; top:70px; right:12px; background:#fff; padding:8px; border-radius:8px; box-shadow:0 6px 18px rgba(0,0,0,0.1); font-size:12px; max-width:260px; z-index:9999; display:none}
  footer{height:86px}
  @media(min-width:700px){ main{max-width:720px;margin:0 auto} nav.bottom-nav{display:none} }
</style>
</head>
<body>
<header>Playmates</header>

<main>
  <!-- AUTH -->
  <section id="authSection" class="card">
    <h3>Entrar</h3>
    <input id="loginEmail" placeholder="Email (ex: voce@mail.com)" />
    <input id="loginPass" placeholder="Senha" type="password" />
    <button id="btnLogin">Entrar</button>
    <p class="muted">4 tentativas erradas bloqueiam a conta (controlado localmente).</p>

    <hr style="margin:12px 0;border:none;border-top:1px solid #f0f0f0" />

    <h3>Cadastrar</h3>
    <input id="regName" placeholder="Nome completo" />
    <input id="regSchool" placeholder="Nome da escola" />
    <input id="regPhone" placeholder="Número de telemóvel" />
    <input id="regEmail" placeholder="Email (ex: voce@mail.com)" />
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
        <div id="meEmail" class="muted-small">-</div>
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
      <input id="smsTo" placeholder="Email ou telefone do destinatário" />
      <textarea id="smsText" placeholder="Mensagem (máx 130)" maxlength="130"></textarea>
      <button id="btnSendSMS">Enviar SMS</button>
      <h4 style="margin-top:12px">Caixa de entrada</h4>
      <div id="smsInbox" class="box-list"></div>
    </div>

    <div id="searchTab" class="tab card">
      <h4>Pesquisa</h4>
      <input id="searchInput" placeholder="Nome, escola, email ou número" />
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

<div id="debug"><strong>DEBUG</strong><div id="dbg"></div></div>
<footer></footer>

<script>
/* ======================================================
   CONFIGURAÇÃO FIREBASE (verifica que está correta)
   ====================================================== */
const firebaseConfig = {
  apiKey: "AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain: "playmates-cc4f7.firebaseapp.com",
  databaseURL: "https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId: "playmates-cc4f7",
  storageBucket: "playmates-cc4f7.appspot.com",
  messagingSenderId: "104004735810",
  appId: "1:104004735810:web:d3ee9a75399d6f0f222edb"
};
firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.database();
const storage = firebase.storage();

/* ======================================================
   SENHAS (protótipo)
   ====================================================== */
const ADMIN_PASS = "LEX";
const CLOCK_PASS = "LEX";

/* ======================================================
   HELPERS e STATE
   ====================================================== */
const $ = id => document.getElementById(id);
const tabs = Array.from(document.querySelectorAll('nav.bottom-nav button'));
const state = { user: null };
const dbgBox = $('debug'), dbgInner = $('dbg');
function dbg(msg){ dbgInner.innerHTML = (new Date()).toLocaleTimeString() + ' — ' + msg + '<br>' + dbgInner.innerHTML; dbgBox.style.display='block'; console.log('DBG:', msg); }

/* NAV */
function showTab(tabId) {
  document.querySelectorAll('.tab').forEach(t => { t.style.display = 'none'; t.classList.remove('visible'); });
  const node = document.getElementById(tabId);
  if (node) { node.style.display = 'block'; node.classList.add('visible'); dbg('showTab -> ' + tabId); }
}
tabs.forEach(btn => btn.addEventListener('click', ()=> showTab(btn.dataset.target)));
showTab('dashboardTab');

/* ======================================================
   AUTH: REGISTER (createUserWithEmailAndPassword) -> store profile in DB
   ====================================================== */
$('btnRegister').addEventListener('click', async () => {
  try {
    const name = $('regName').value.trim();
    const school = $('regSchool').value.trim();
    const phone = $('regPhone').value.trim();
    const email = $('regEmail').value.trim();
    const pass = $('regPass').value.trim();
    const file = $('regPhoto').files[0];
    if (!name || !school || !phone || !email || !pass) { alert('Preencha todos os campos.'); return; }

    // create auth user
    const userCred = await auth.createUserWithEmailAndPassword(email, pass);
    const uid = userCred.user.uid;

    // upload photo safely
    let photoUrl = 'https://via.placeholder.com/150';
    if (file) {
      try {
        const photoRef = storage.ref().child('profilePhotos/' + uid + '_' + Date.now());
        const up = await photoRef.put(file);
        photoUrl = await up.ref.getDownloadURL();
        dbg('Foto enviada: ' + photoUrl);
      } catch (err) {
        dbg('Erro upload foto: ' + err.message);
        alert('Upload da foto falhou. Será usada foto padrão.');
      }
    }

    // store profile in Realtime DB keyed by uid
    await db.ref('users/' + uid).set({
      uid, name, school, phone, email, photo: photoUrl,
      points: 0, blocked: false, attempts: 0, createdAt: Date.now()
    });

    alert('Cadastro realizado. Bem-vindo — faça login.');
    // clear register form
    $('regName').value = $('regSchool').value = $('regPhone').value = $('regEmail').value = $('regPass').value = '';
    $('regPhoto').value = '';
    dbg('Usuário cadastrado: ' + uid);
  } catch (e) {
    dbg('Erro register: ' + e.message);
    alert('Erro no cadastro: ' + e.message);
  }
});

/* ======================================================
   LOGIN (signInWithEmailAndPassword) + attempts/block logic in DB
   ====================================================== */
$('btnLogin').addEventListener('click', async () => {
  try {
    const email = $('loginEmail').value.trim();
    const pass = $('loginPass').value.trim();
    if (!email || !pass) { alert('Preencha email e senha.'); return; }

    // get user by email to check attempts/block (we need uid)
    const usersSnap = await db.ref('users').orderByChild('email').equalTo(email).once('value');
    if (!usersSnap.exists()) { alert('Conta não encontrada.'); return; }
    const users = usersSnap.val();
    const uid = Object.keys(users)[0];
    const profile = users[uid];

    if (profile.blocked) { alert('Conta bloqueada.'); return; }

    // try sign in
    try {
      const cred = await auth.signInWithEmailAndPassword(email, pass);
      // reset attempts
      await db.ref('users/' + uid).update({ attempts: 0 });

      state.user = uid;
      $('authSection').style.display = 'none';
      $('appSection').style.display = 'block';
      loadUserRealtime(uid);
      startClockListener();
      bindRealtimeLists();
      listenNotifications();
      updateTotalUsers();
      showTab('dashboardTab');
      dbg('Login OK: ' + uid);
    } catch (authErr) {
      // increment attempts in DB
      const attempts = (profile.attempts || 0) + 1;
      await db.ref('users/' + uid).update({ attempts });
      if (attempts >= 4) {
        await db.ref('users/' + uid).update({ blocked: true });
        alert('Conta bloqueada após 4 tentativas.');
      } else {
        alert('Senha incorreta. Tentativas: ' + attempts + '/4');
      }
      dbg('Login fail for ' + uid + ' - ' + authErr.message);
    }
  } catch (e) {
    dbg('Erro login: ' + e.message);
    alert('Erro no login. Ver debug.');
  }
});

/* logout */
$('btnLogout').addEventListener('click', async () => {
  try {
    await auth.signOut();
  } catch(_) {}
  state.user = null;
  $('authSection').style.display = 'block';
  $('appSection').style.display = 'none';
  $('mePhoto').src = 'https://via.placeholder.com/150';
  $('meName').innerText = '-';
  $('meSchool').innerText = '';
  $('meEmail').innerText = '';
  $('mePhone').innerText = '';
  $('mePoints').innerText = '0 pts';
  dbg('Logout');
});

/* ======================================================
   LOAD USER REALTIME
   ====================================================== */
function loadUserRealtime(uid) {
  const uRef = db.ref('users/' + uid);
  uRef.on('value', snap => {
    if (!snap.exists()) return;
    const u = snap.val();
    $('mePhoto').src = u.photo || 'https://via.placeholder.com/150';
    $('meName').innerText = u.name || '-';
    $('meSchool').innerText = u.school || '';
    $('meEmail').innerText = u.email || '';
    $('mePhone').innerText = u.phone || '';
    $('mePoints').innerText = (u.points || 0) + ' pts';
    $('welcomeText').innerText = 'Boas vindas ao Playmates, ' + (u.name || '');
    $('welcomeSchool').innerText = u.school || '';
  });
}

/* edit profile (name, school, phone, photo) */
$('btnEditProfile').addEventListener('click', () => {
  if (!state.user) return alert('Faça login');
  const newName = prompt('Nome:', $('meName').innerText) || $('meName').innerText;
  const newSchool = prompt('Escola:', $('meSchool').innerText) || $('meSchool').innerText;
  const newPhone = prompt('Número (se quiser alterar):', $('mePhone').innerText) || $('mePhone').innerText;
  const fileInput = document.createElement('input'); fileInput.type = 'file'; fileInput.accept = 'image/*';
  fileInput.onchange = async () => {
    try {
      const file = fileInput.files[0];
      let photo = $('mePhoto').src;
      if (file) {
        const photoRef = storage.ref().child('profilePhotos/' + state.user + '_' + Date.now());
        const up = await photoRef.put(file);
        photo = await up.ref.getDownloadURL();
      }
      await db.ref('users/' + state.user).update({ name: newName, school: newSchool, phone: newPhone, photo });
      alert('Perfil atualizado');
      dbg('Perfil atualizado: ' + state.user);
    } catch (e) {
      dbg('Erro atualizar perfil: ' + e.message);
      alert('Falha ao atualizar perfil. Ver debug.');
    }
  };
  fileInput.click();
});

/* ======================================================
   CLOCK (timer) - realtime
   ====================================================== */
let clockInterval = null;
function startClockListener(){
  db.ref('timer').on('value', snap => {
    if (!snap.exists()) { $('clockDisplay').innerText = 'Aguardando'; return; }
    const data = snap.val();
    const end = data.endAt || 0;
    if (clockInterval) clearInterval(clockInterval);
    clockInterval = setInterval(() => {
      const diff = end - Date.now();
      if (diff <= 0) { $('clockDisplay').innerText = '00:00:00'; clearInterval(clockInterval); return; }
      const h = Math.floor(diff / 3600000);
      const m = Math.floor((diff % 3600000) / 60000);
      const s = Math.floor((diff % 60000) / 1000);
      $('clockDisplay').innerText = `${String(h).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
    }, 1000);
    dbg('Clock listener ativo');
  });
}

$('btnEditClock').addEventListener('click', async () => {
  const pw = prompt('Senha para editar relógio:');
  if (pw !== CLOCK_PASS) return alert('Senha incorreta');
  const h = parseInt(prompt('Horas:')) || 0;
  const m = parseInt(prompt('Minutos:')) || 0;
  const s = parseInt(prompt('Segundos:')) || 0;
  const totalMs = ((h*3600)+(m*60)+s) * 1000;
  await db.ref('timer').set({ endAt: Date.now() + totalMs });
  alert('Relógio atualizado e sincronizado');
  dbg('Relógio atualizado por ' + (state.user || 'anon'));
});

/* ======================================================
   TOTAL USERS (realtime)
   ====================================================== */
function updateTotalUsers(){
  db.ref('users').on('value', snap => {
    const count = snap.exists() ? Object.keys(snap.val()).length : 0;
    $('totalUsers').innerText = 'Total de perfis: ' + count;
  });
}

/* ======================================================
   SMS (stored in DB) - expires 3h
   ====================================================== */
$('btnSendSMS').addEventListener('click', async () => {
  if (!state.user) return alert('Faça login');
  const to = $('smsTo').value.trim();
  const text = $('smsText').value.trim();
  if (!to || !text) return alert('Preencha destinatário e mensagem');
  if (text.length > 130) return alert('Máx 130 caracteres');
  const now = Date.now();
  try {
    await db.ref('sms').push({ from: state.user, to, text, createdAt: now, expiresAt: now + 3*3600*1000 });
    $('smsTo').value = ''; $('smsText').value = '';
    alert('SMS enviado');
    dbg('SMS enviado por ' + state.user + ' -> ' + to);
  } catch(e) {
    dbg('Erro enviar SMS: ' + e.message);
  }
});

/* inbox (realtime) */
function renderInbox(){
  db.ref('sms').on('value', snap => {
    const list = $('smsInbox'); list.innerHTML = '';
    if (!snap.exists()) { list.innerHTML = '<div class="muted-small">Sem mensagens</div>'; return; }
    const data = snap.val();
    Object.keys(data).reverse().forEach(async k => {
      const m = data[k];
      if (m.expiresAt && m.expiresAt < Date.now()) { db.ref('sms/' + k).remove().catch(()=>{}); return; }
      // show only messages to me (match uid or email/phone)
      if (m.to !== state.user && m.to !== $('meEmail').innerText && m.to !== $('mePhone').innerText) return;
      const fromSnap = await db.ref('users/' + m.from).once('value');
      const fromName = fromSnap.exists() ? fromSnap.val().name : m.from;
      const img = fromSnap.exists() ? fromSnap.val().photo : 'https://via.placeholder.com/80';
      const div = document.createElement('div'); div.className='list-item';
      div.innerHTML = `<img class="thumb" src="${img}" /><div style="flex:1"><strong>${fromName}</strong><div class="muted-small">${m.text}</div><div class="muted-small">${new Date(m.createdAt).toLocaleString()}</div></div>`;
      list.appendChild(div);
    });
    dbg('Inbox atualizada');
  });
}

/* ======================================================
   POSTS (3h expiration)
   ====================================================== */
$('btnPost').addEventListener('click', async () => {
  if (!state.user) return alert('Faça login');
  const text = $('postText').value.trim();
  const file = $('postFile').files[0];
  if (!text && !file) return alert('Escreva algo ou selecione imagem');
  const now = Date.now();
  try {
    let imgUrl = '';
    if (file) {
      const pRef = storage.ref().child('posts/' + state.user + '_' + now + '_' + file.name);
      const up = await pRef.put(file);
      imgUrl = await up.ref.getDownloadURL();
    }
    await db.ref('posts').push({ user: state.user, text, img: imgUrl, createdAt: now, expiresAt: now + 3*3600*1000 });
    $('postText').value = ''; $('postFile').value = '';
    alert('Post criado');
    dbg('Post criado por ' + state.user);
  } catch(e) {
    dbg('Erro criar post: ' + e.message);
    alert('Falha ao publicar. Ver console.');
  }
});

/* render posts realtime */
function renderPosts(){
  db.ref('posts').on('value', snap => {
    const list = $('postsList'); list.innerHTML = '';
    if (!snap.exists()) { list.innerHTML = '<div class="muted-small">Sem postagens</div>'; return; }
    c
