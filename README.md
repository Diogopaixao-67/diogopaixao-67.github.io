<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Playmates — WhatsApp-style Tabs</title>

<!-- Firebase v8 classic -->
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-storage.js"></script>

<style>
:root{--accent:#ff7f00;--muted:#666;--bg:#fbfbfb}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,Arial,sans-serif;background:var(--bg);color:#111}
.header{background:var(--accent);color:#fff;padding:14px 12px;display:flex;align-items:center;justify-content:space-between}
.header h1{margin:0;font-size:18px;font-weight:800}
.tab-bar{display:flex;background:#fff;padding:6px;border-bottom:1px solid #eee;gap:6px;overflow:auto}
.tab-bar button{flex:1;padding:10px;border:none;background:transparent;font-weight:700;color:var(--muted);cursor:pointer;border-bottom:3px solid transparent}
.tab-bar button.active{color:var(--accent);border-bottom-color:var(--accent)}
main{padding:14px;padding-bottom:100px;max-width:720px;margin:0 auto}
.card{background:#fff;border-radius:12px;padding:12px;box-shadow:0 8px 28px rgba(10,10,10,0.04);margin-bottom:12px}
.input,textarea,button,select{width:100%;padding:10px;border-radius:10px;border:1px solid #eee;font-size:15px}
button.primary{background:var(--accent);color:#fff;border:none;font-weight:700;cursor:pointer}
.muted{color:var(--muted);font-size:13px}
.profile-row{display:flex;gap:12px;align-items:center}
.avatar{width:88px;height:88px;border-radius:50%;object-fit:cover}
.small-btn{display:inline-block;padding:8px 10px;border-radius:10px;background:transparent;color:var(--accent);border:1px solid rgba(255,127,0,0.12);cursor:pointer}
.clock{font-family:monospace;font-weight:800;color:var(--accent);font-size:20px}
.box-list{display:flex;flex-direction:column;gap:8px;margin-top:8px}
.list-item{background:#fff;padding:10px;border-radius:10px;display:flex;gap:10px;align-items:center;box-shadow:0 6px 18px rgba(0,0,0,0.03)}
.thumb{width:56px;height:56px;border-radius:50%;object-fit:cover}
.muted-small{font-size:12px;color:#888}
#debug {position:fixed; top:72px; right:12px; background:#fff; padding:8px; border-radius:8px; box-shadow:0 6px 18px rgba(0,0,0,0.1); font-size:12px; max-width:300px; z-index:9999; display:none}
@media(min-width:700px){body{background:#f2f2f2}} 
</style>
</head>
<body>

<div class="header">
  <h1>Playmates</h1>
  <div style="display:flex;gap:8px;align-items:center">
    <div id="clockTop" class="clock">Aguardando</div>
    <button class="small-btn" id="btnEditClock">⏱</button>
  </div>
</div>

<div class="tab-bar" id="tabBar">
  <button data-tab="tabProfiles" class="active">Perfis</button>
  <button data-tab="tabSMS">SMS</button>
  <button data-tab="tabSearch">Pesquisa</button>
  <button data-tab="tabPosts">Postagens</button>
  <button data-tab="tabSOS">SOS</button>
  <button data-tab="tabNot">Notificações</button>
</div>

<main>
  <!-- AUTH -->
  <section id="authSection" class="card">
    <h3>Entrar</h3>
    <input id="loginEmail" class="input" placeholder="Email (ex: voce@mail.com)" />
    <input id="loginPass" class="input" placeholder="Senha" type="password" />
    <button id="btnLogin" class="primary">Entrar</button>
    <p class="muted">4 tentativas erradas bloqueiam a conta (controlado no perfil).</p>

    <hr style="margin:12px 0;border:none;border-top:1px solid #f0f0f0" />

    <h3>Cadastrar</h3>
    <input id="regName" class="input" placeholder="Nome completo" />
    <input id="regSchool" class="input" placeholder="Nome da escola" />
    <input id="regPhone" class="input" placeholder="Número de telemóvel" />
    <input id="regEmail" class="input" placeholder="Email (ex: voce@mail.com)" />
    <input id="regPass" class="input" placeholder="Senha" type="password" />
    <input id="regPhoto" type="file" accept="image/*" />
    <button id="btnRegister" class="primary">Cadastrar</button>
  </section>

  <!-- APP -->
  <section id="appSection" style="display:none">
    <div class="card profile-row">
      <img id="mePhoto" class="avatar" src="https://via.placeholder.com/150" />
      <div style="flex:1">
        <div id="meName" style="font-weight:800;font-size:18px">-</div>
        <div id="meSchool" class="muted-small">-</div>
        <div id="meEmail" class="muted-small">-</div>
        <div id="mePhone" class="muted-small">-</div>
        <div style="margin-top:8px">
          <span id="mePoints" class="small-btn" style="background:#fff;color:var(--accent);border:1px solid rgba(255,127,0,0.12)">0 pts</span>
          <button class="small-btn" id="btnEditProfile">Editar perfil</button>
          <button class="small-btn" id="btnLogout">Sair</button>
        </div>
      </div>
    </div>

    <div id="tabProfiles" class="card tab visible">
      <h3>Dashboard</h3>
      <div id="totalUsers" class="muted-small">Total de perfis: 0</div>
      <div id="feedRecent" class="box-list" style="margin-top:12px"></div>
    </div>

    <div id="tabSMS" class="card tab" style="display:none">
      <h3>Enviar SMS (130 chars, expiram 3h)</h3>
      <input id="smsTo" class="input" placeholder="Email ou UID do destinatário" />
      <textarea id="smsText" class="input" placeholder="Mensagem (máx 130)" maxlength="130"></textarea>
      <button id="btnSendSMS" class="primary">Enviar SMS</button>
      <h4 style="margin-top:12px">Caixa de entrada</h4>
      <div id="smsInbox" class="box-list"></div>
    </div>

    <div id="tabSearch" class="card tab" style="display:none">
      <h3>Pesquisar usuários</h3>
      <input id="searchInput" class="input" placeholder="Nome, escola, email ou número" />
      <button id="btnSearch" class="primary">Buscar</button>
      <div id="searchResults" class="box-list" style="margin-top:12px"></div>
    </div>

    <div id="tabPosts" class="card tab" style="display:none">
      <h3>Postagens (expiram 3h)</h3>
      <textarea id="postText" class="input" placeholder="Texto"></textarea>
      <input id="postFile" type="file" accept="image/*" />
      <button id="btnPost" class="primary">Publicar</button>
      <div id="postsList" class="box-list" style="margin-top:12px"></div>
    </div>

    <div id="tabSOS" class="card tab" style="display:none">
      <h3>SOS Emprego (admin LEX)</h3>
      <button id="btnAddSOS" class="primary">Criar SOS (LEX)</button>
      <div id="sosList" class="box-list" style="margin-top:12px"></div>
    </div>

    <div id="tabNot" class="card tab" style="display:none">
      <h3>Notificações</h3>
      <div id="notList" class="box-list"></div>
    </div>
  </section>
</main>

<div id="debug" style="display:none"><strong>DEBUG</strong><div id="dbg"></div></div>

<script>
/* ===========================
   FIREBASE CONFIG (v8 classic)
   =========================== */
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

/* ----------------
   PROTÓTIPOS / SENHAS
   ---------------- */
const ADMIN_PASS = "LEX";
const CLOCK_PASS = "LEX";

/* ----------------
   HELPERS & STATE
   ---------------- */
const $ = id => document.getElementById(id);
const tabs = Array.from(document.querySelectorAll('.tab-bar button'));
const state = { uid: null };
const dbgBox = $('debug'), dbgInner = $('dbg');
function dbg(msg){ dbgInner.innerHTML = (new Date()).toLocaleTimeString() + ' — ' + msg + '<br>' + dbgInner.innerHTML; dbgBox.style.display='block'; console.log('DBG:', msg); }

/* ----------------
   Tabs top (WhatsApp style)
   ---------------- */
function showTab(name){
  document.querySelectorAll('.tab').forEach(t=>t.style.display='none');
  document.querySelectorAll('.tab-bar button').forEach(b=>b.classList.remove('active'));
  const btn = document.querySelector('.tab-bar button[data-tab="'+name+'"]');
  if(btn) btn.classList.add('active');
  const node = $(name);
  if(node) node.style.display='block';
}
tabs.forEach(b=> b.addEventListener('click', ()=> showTab(b.dataset.tab) ));
showTab('tabProfiles');

/* ===========================
   AUTH: register + store profile
   =========================== */
$('btnRegister').addEventListener('click', async ()=>{
  try{
    const name = $('regName').value.trim();
    const school = $('regSchool').value.trim();
    const phone = $('regPhone').value.trim();
    const email = $('regEmail').value.trim();
    const pass = $('regPass').value.trim();
    const file = $('regPhoto').files[0];
    if(!name||!school||!phone||!email||!pass){ alert('Preencha todos os campos'); return; }

    const cred = await auth.createUserWithEmailAndPassword(email, pass);
    const uid = cred.user.uid;

    // upload photo
    let photo = 'https://via.placeholder.com/150';
    if(file){
      try{
        const refPhoto = storage.ref().child('profilePhotos/'+uid+'_'+Date.now());
        const up = await refPhoto.put(file);
        photo = await up.ref.getDownloadURL();
      }catch(e){ dbg('Upload foto falhou: '+e.message); alert('Upload foto falhou; imagem padrão usada'); }
    }

    await db.ref('users/'+uid).set({
      uid, name, school, phone, email, photo,
      points:0, attempts:0, blocked:false, createdAt:Date.now()
    });

    alert('Cadastro feito! Faça login.');
    // clear
    $('regName').value=''; $('regSchool').value=''; $('regPhone').value=''; $('regEmail').value=''; $('regPass').value=''; $('regPhoto').value='';
    dbg('Usuario cadastrado: '+uid);
  }catch(e){
    dbg('Erro register: '+e.message);
    alert('Erro no cadastro: '+(e.message||e));
  }
});

/* ===========================
   LOGIN (email+senha) w/ attempts
   =========================== */
$('btnLogin').addEventListener('click', async ()=>{
  try{
    const email = $('loginEmail').value.trim();
    const pass = $('loginPass').value.trim();
    if(!email||!pass){ alert('Preencha email e senha'); return; }

    // find user profile by email in DB to check attempts/status
    const snap = await db.ref('users').orderByChild('email').equalTo(email).once('value');
    if(!snap.exists()){ alert('Conta não encontrada'); return; }
    const users = snap.val();
    const uid = Object.keys(users)[0];
    const profile = users[uid];

    if(profile.blocked){ alert('Conta bloqueada'); return; }

    try{
      await auth.signInWithEmailAndPassword(email, pass);
      await db.ref('users/'+uid).update({ attempts: 0 });

      state.uid = uid;
      $('authSection').style.display='none'; $('appSection').style.display='block';
      loadUserRealtime(uid);
      startClockListener();
      bindRealtimeLists();
      listenNotifications();
      updateTotalUsers();
      showTab('tabProfiles');
      dbg('Login OK: '+uid);
    }catch(authErr){
      const attempts = (profile.attempts||0)+1;
      await db.ref('users/'+uid).update({ attempts });
      if(attempts>=4){ await db.ref('users/'+uid).update({ blocked:true }); alert('Conta bloqueada após 4 tentativas'); }
      else alert('Senha incorreta. Tentativas: '+attempts+'/4');
      dbg('Auth fail: '+authErr.message);
    }

  }catch(e){
    dbg('Erro login: '+e.message);
    alert('Erro no login. Ver debug.');
  }
});

/* logout */
$('btnLogout').addEventListener('click', async ()=>{
  try{ await auth.signOut(); }catch(e){}
  state.uid = null;
  $('authSection').style.display='block'; $('appSection').style.display='none';
  $('mePhoto').src='https://via.placeholder.com/150'; $('meName').innerText='-'; $('meSchool').innerText=''; $('meEmail').innerText=''; $('mePhone').innerText=''; $('mePoints').innerText='0 pts';
  dbg('Logout');
});

/* ===========================
   Load user realtime UI
   =========================== */
function loadUserRealtime(uid){
  const refU = db.ref('users/'+uid);
  refU.on('value', snap=>{
    if(!snap.exists()) return;
    const u = snap.val();
    $('mePhoto').src = u.photo || 'https://via.placeholder.com/150';
    $('meName').innerText = u.name || '-';
    $('meSchool').innerText = u.school || '';
    $('meEmail').innerText = u.email || '';
    $('mePhone').innerText = u.phone || '';
    $('mePoints').innerText = (u.points||0) + ' pts';
    dbg('Perfil carregado: '+uid);
  });
}

/* edit profile */
$('btnEditProfile').addEventListener('click', ()=>{
  if(!state.uid) return alert('Faça login');
  const newName = prompt('Nome:', $('meName').innerText) || $('meName').innerText;
  const newSchool = prompt('Escola:', $('meSchool').innerText) || $('meSchool').innerText;
  const newPhone = prompt('Número:', $('mePhone').innerText) || $('mePhone').innerText;
  const fileInput = document.createElement('input'); fileInput.type='file'; fileInput.accept='image/*';
  fileInput.onchange = async ()=>{
    try{
      const f = fileInput.files[0];
      let photo = $('mePhoto').src;
      if(f){
        const pRef = storage.ref().child('profilePhotos/'+state.uid+'_'+Date.now());
        const up = await pRef.put(f);
        photo = await up.ref.getDownloadURL();
      }
      await db.ref('users/'+state.uid).update({ name:newName, school:newSchool, phone:newPhone, photo });
      alert('Perfil atualizado');
      dbg('Perfil atualizado: '+state.uid);
    }catch(e){ dbg('Erro atualizar perfil: '+e.message); alert('Falha ao atualizar perfil'); }
  };
  fileInput.click();
});

/* ===========================
   CLOCK (timer) realtime
   =========================== */
let clockInterval = null;
function startClockListener(){
  db.ref('timer').on('value', snap=>{
    if(!snap.exists()){ $('clockTop').innerText='Aguardando'; return; }
    const data = snap.val(); const end = data.endAt || 0;
    if(clockInterval) clearInterval(clockInterval);
    clockInterval = setInterval(()=>{
      const diff = end - Date.now();
      if(diff<=0){ $('clockTop').innerText='00:00:00'; clearInterval(clockInterval); return; }
      const h = Math.floor(diff/3600000), m = Math.floor((diff%3600000)/60000), s = Math.floor((diff%60000)/1000);
      $('clockTop').innerText = `${String(h).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
    },1000);
    dbg('Clock listener ativo');
  });
}
$('btnEditClock').addEventListener('click', async ()=>{
  const pw = prompt('Senha (para editar relógio):');
  if(pw!==CLOCK_PASS) return alert('Senha incorreta');
  const h = parseInt(prompt('Horas:'))||0, m = parseInt(prompt('Minutos:'))||0, s = parseInt(prompt('Segundos:'))||0;
  const total = ((h*3600)+(m*60)+s)*1000;
  await db.ref('timer').set({ endAt: Date.now()+total });
  alert('Relógio atualizado');
  dbg('Relógio atualizado por '+(state.uid||'anon'));
});

/* ===========================
   TOTAL USERS listener
   =========================== */
function updateTotalUsers(){
  db.ref('users').on('value', snap=>{
    const count = snap.exists()? Object.keys(snap.val()).length : 0;
    $('totalUsers').innerText = 'Total de perfis: ' + count;
  });
}
updateTotalUsers();

/* ===========================
   SMS (3h) send & inbox realtime
   =========================== */
$('btnSendSMS').addEventListener('click', async ()=>{
  if(!state.uid) return alert('Faça login');
  const to = $('smsTo').value.trim(); const text = $('smsText').value.trim();
  if(!to||!text) return alert('Preencha destinatário e mensagem');
  if(text.length>130) return alert('Máx 130 caracteres');
  const now = Date.now();
  await db.ref('sms').push({ from: state.uid, to, text, createdAt: now, expiresAt: now + 3*3600*1000 });
  $('smsTo').value=''; $('smsText').value='';
  alert('SMS enviado');
  dbg('SMS enviado por '+state.uid+' -> '+to);
});
function renderInbox(){
  db.ref('sms').on('value', snap=>{
    const list = $('smsInbox'); list.innerHTML='';
    if(!snap.exists()){ list.innerHTML='<div class="muted-small">Sem mensagens</div>'; return; }
    const data = snap.val();
    Object.keys(data).reverse().forEach(k=>{
      const m = data[k];
      if(m.expiresAt && m.expiresAt < Date.now()){ db.ref('sms/'+k).remove().catch(()=>{}); return; }
      // show messages that match my uid, email or phone
      const mineEmails = [$('meEmail').innerText, $('mePhone').innerText, state.uid];
      if(!mineEmails.includes(m.to)) return;
      db.ref('users/'+m.from).once('value').then(s=>{
        const n = s.exists()? s.val().name : m.from;
        const img = s.exists()? s.val().photo : 'https://via.placeholder.com/80';
        const div = document.createElement('div'); div.className='list-item';
        div.innerHTML = `<img class="thumb" src="${img}" /><div style="flex:1"><strong>${n}</strong><div class="muted-small">${m.text}</div><div class="muted-small">${new Date(m.createdAt).toLocaleString()}</div></div>`;
        list.appendChild(div);
      });
    });
    dbg('Inbox atualizada');
  });
}

/* ===========================
   POSTS (3h) create & render
   =========================== */
$('btnPost').addEventListener('click', async ()=>{
  if(!state.uid) return alert('Faça login');
  const text = $('postText').value.trim(); const file = $('postFile').files[0]; const now = Date.now();
  if(!text && !file) return alert('Escreva algo ou selecione uma imagem');
  try{
    let img = '';
    if(file){
      const pRef = storage.ref().child('posts/'+state.uid+'_'+now+'_'+file.name);
      const up = await pRef.put(file);
      img = await up.ref.getDownloadURL();
    }
    await db.ref('posts').push({ user: state.uid, text, img, createdAt: now, expiresAt: now + 3*3600*1000 });
    $('postText').value=''; $('postFile').value='';
    alert('Post criado');
    dbg('Post criado por '+state.uid);
  }catch(e){ dbg('Erro post: '+e.message); alert('Falha ao criar post'); }
});
function renderPosts(){
  db.ref('posts').on('value', snap=>{
    const list = $('postsList'); list.innerHTML='';
    if(!snap.exists()){ list.innerHTML='<div class="muted-small">Sem postagens</div>'; return; }
    const data = snap.val();
    const arr = Object.keys(data).map(k=>({ id:k, ...data[k] })).sort((a,b)=>b.createdAt - a.createdAt);
    arr.forEach(p=>{
      if(p.expiresAt && p.expiresAt < Date.now()){ db.ref('posts/'+p.id).remove().catch(()=>{}); return; }
      const div = document.createElement('div'); div.className='list-item';
      div.innerHTML = `<img class="thumb" src="${p.img||'https://via.placeholder.com/80'}" /><div style="flex:1"><strong>${p.user}</strong><div class="muted-small">${(p.text||'').slice(0,120)}</div><div class="muted-small">${new Date(p.createdAt).toLocaleString()}</div></div>`;
      list.appendChild(div);
    });
    // feedRecent
    const feed = $('feedRecent'); feed.innerHTML='';
    arr.slice(0,5).forEach(p=>{
      if(p.expiresAt && p.expiresAt < Date.now()) return;
      const f = document.createElement('div'); f.className='list-item';
      f.innerHTML = `<img class="thumb" src="${p.img||'https://via.placeholder.com/80'}" /><div style="flex:1"><div class="muted-small">${new Date(p.createdAt).toLocaleString()}</div><div>${(p.text||'').slice(0,80)}</div></div>`;
      feed.appendChild(f);
    });
    dbg('Posts renderizados');
  });
}

/* ===========================
   SEARCH + TOPAR (+0.5%)
   =========================== */
$('btnSearch').addEventListener('click', async ()=>{
  const q = $('searchInput').value.trim().toLowerCase();
  const snap = await db.ref('users').once('value');
  const out = $('searchResults'); out.innerHTML='';
  if(!snap.exists()){ out.innerHTML='<div class="muted-small">Sem perfis</div>'; return;
