<!doctype html>
<html lang="pt-PT">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Playmates — App</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#f6f7fb; --card:#ffffff; --muted:#6b7280; --accent:#ff7b00; --accent-2:#ff9a3d; --primary:#1877f2;
}
*{box-sizing:border-box;font-family:Inter,system-ui,-apple-system,"Segoe UI",Roboto,Arial;margin:0;padding:0}
html,body{height:100%}
body{background:var(--bg);color:#0b1222;min-height:100vh;padding-bottom:86px}
.app{max-width:980px;margin:0 auto;padding:12px}
header{display:flex;align-items:center;gap:12px;padding:14px;border-radius:12px;background:linear-gradient(90deg,var(--accent),var(--accent-2));color:white;margin-bottom:12px;box-shadow:0 8px 30px rgba(255,123,0,0.12)}
header h1{font-size:18px;margin:0;font-weight:800}
.top-actions{margin-left:auto;display:flex;gap:8px}
button{cursor:pointer}
.btn { background:var(--primary); color:#fff; border:none; padding:8px 12px; border-radius:10px; font-weight:700; }
.btn.ghost { background:transparent; color:var(--primary); border:1px solid rgba(11,18,34,0.06); padding:8px 12px; border-radius:10px; font-weight:700; }
.card{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 18px rgba(16,24,40,0.04);margin-bottom:12px}
.row{display:flex;gap:8px;align-items:center}
.small{font-size:13px;color:var(--muted)}
.profile-top{display:flex;gap:12px;align-items:center}
.avatar{width:72px;height:72px;border-radius:999px;overflow:hidden;background:#f3f5f8;border:3px solid rgba(255,123,0,0.12);flex:0 0 72px}
.avatar img{width:100%;height:100%;object-fit:cover;display:block}
.section{display:none}
.section.active{display:block}
.feed-post{background:#fff;border-radius:12px;padding:12px;margin-bottom:12px;box-shadow:0 4px 14px rgba(16,24,40,0.04)}
.feed-post img{width:100%;border-radius:10px;margin-top:10px;object-fit:cover}
.menu-bottom{position:fixed;left:0;right:0;bottom:0;background:#fff;border-top:1px solid #e6e6e6;padding:8px 6px;display:flex;justify-content:space-around;gap:6px;box-shadow:0 -6px 18px rgba(0,0,0,0.06);z-index:999}
.menu-bottom button{background:none;border:0;color:#333;font-weight:700;padding:6px 8px;border-radius:8px}
.menu-bottom button.active{color:var(--accent);box-shadow:inset 0 -2px 0 rgba(0,0,0,0.03)}
.input-file{display:flex;gap:8px;align-items:center}
.preview-img{width:100%;max-height:360px;object-fit:cover;border-radius:10px;margin-top:8px}
.hidden{display:none}
.small-note{font-size:13px;color:var(--muted);margin-top:6px}
.badge{display:inline-block;padding:6px 8px;border-radius:999px;background:rgba(11,18,34,0.04);font-size:13px;color:var(--muted)}
.center{display:flex;align-items:center;justify-content:center}
</style>
</head>
<body>
  <div class="app">
    <header>
      <h1>Playmates</h1>
      <div class="top-actions">
        <button id="btnDemo" class="btn.ghost">Conta Demo</button>
        <button id="btnOpenAuth" class="btn">Entrar / Cadastrar</button>
      </div>
    </header>

    <!-- AUTH CARD (mostra login/reg ou perfil quando logado) -->
    <div id="authCard" class="card">
      <div id="loginView">
        <h3>Entrar</h3>
        <div class="small-note">Login com telemóvel + senha — 5 tentativas bloqueiam.</div>
        <div style="height:8px"></div>
        <input id="loginPhone" placeholder="Telemóvel (ex: 922000000)">
        <input id="loginPass" type="password" placeholder="Senha">
        <div class="row" style="margin-top:8px">
          <button id="loginBtn" class="btn">Entrar</button>
          <button id="toRegister" class="btn.ghost">Criar conta</button>
        </div>
      </div>

      <div id="registerView" class="hidden">
        <h3>Criar conta</h3>
        <input id="regName" placeholder="Nome completo">
        <input id="regPhone" placeholder="Telemóvel">
        <input id="regPass" type="password" placeholder="Senha">
        <input id="regSchool" placeholder="Escola (opcional)">
        <div style="margin-top:8px" class="input-file">
          <input id="regPhoto" type="file" accept="image/*">
          <button id="createBtn" class="btn">Criar conta</button>
        </div>
        <div class="small-note">Foto opcional — pode editar depois.</div>
        <div style="height:8px"></div>
        <button id="backLogin" class="btn.ghost">Voltar</button>
      </div>

      <div id="profileView" class="hidden">
        <div class="profile-top">
          <div class="avatar"><img id="profileImg" src=""></div>
          <div>
            <div id="profileName" style="font-size:16px;font-weight:800">—</div>
            <div id="profileSchool" class="small">—</div>
            <div id="profilePhone" class="small">—</div>
            <div style="height:8px"></div>
            <div class="row">
              <button id="btnEditProfile" class="btn.ghost">Editar</button>
              <button id="btnLogout" class="btn.ghost">Sair</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- TABS (visíveis após login) -->
    <div id="tabsRow" class="card hidden">
      <div class="row">
        <button class="tab active" data-target="feed">Feed</button>
        <button class="tab" data-target="eventos">Eventos</button>
        <button class="tab" data-target="jogos">Jogos</button>
        <button class="tab" data-target="historia">História</button>
      </div>
    </div>

    <!-- SECTIONS -->
    <div id="feed" class="section active card">
      <div id="notifBox" class="card hidden"></div>

      <h3>Publicar</h3>
      <textarea id="postText" rows="3" placeholder="O que estás a pensar? (opcional)"></textarea>
      <div class="row" style="margin-top:8px">
        <input id="postImage" type="file" accept="image/*">
        <button id="btnPublish" class="btn">Publicar</button>
      </div>
      <img id="preview" class="preview-img hidden" alt="preview">

      <div style="margin-top:12px"><h4>Feed</h4><div id="postsContainer"></div></div>
    </div>

    <div id="eventos" class="section card">
      <h3>Eventos</h3>
      <div class="small-note">Para publicar/apagar eventos precisa da senha <strong>LEX</strong>.</div>
      <div style="margin-top:8px" class="card">
        <input id="eventPass" type="password" placeholder="Senha (LEX)">
        <input id="eventTitle" placeholder="Título do evento">
        <textarea id="eventDesc" rows="3" placeholder="Descrição do evento"></textarea>
        <div class="row" style="margin-top:8px">
          <button id="btnPublishEvent" class="btn">Publicar Evento</button>
          <button id="btnClearExpired" class="btn.ghost">Eliminar expirados</button>
        </div>
      </div>
      <div style="margin-top:12px"><h4>Eventos publicados</h4><div id="eventsContainer"></div></div>
    </div>

    <div id="jogos" class="section card">
      <h3>Jogos</h3>
      <div class="feed-post"><strong>Playmates Runner</strong><p>Mini-jogo de corrida — desvia obstáculos e coleciona pontos.</p></div>
      <div class="feed-post"><strong>Playmates Quiz</strong><p>Quiz para treinar conhecimentos escolares.</p></div>
      <div class="feed-post"><strong>Memória Flash</strong><p>Teste de memória com imagens rápidas.</p></div>
    </div>

    <div id="historia" class="section card">
      <h3>História do Fundador</h3>
      <p><strong>Diogo Paixão</strong> é o fundador e CEO da Playmates. A sua visão é criar um espaço digital seguro para jovens expressarem criatividade, aprenderem e ganharem oportunidades online. A Playmates nasceu da vontade de unir tecnologia, educação e cultura juvenil.</p>
    </div>

    <div style="height:86px"></div>
  </div>

  <!-- MENU BOTTOM estilo WhatsApp -->
  <div class="menu-bottom">
    <button data-target="feed" class="active">Conversas</button>
    <button data-target="eventos">Eventos</button>
    <button data-target="jogos">Jogos</button>
    <button data-target="historia">História</button>
  </div>

  <!-- FIREBASE -->
  <script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js";
  import { getDatabase, ref, set, push, onValue, get, remove, update } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-database.js";

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

  /* ---------- Helpers ---------- */
  const $ = id => document.getElementById(id);
  const placeholderDataUrl = (initial) => {
    const c=document.createElement('canvas'); c.width=200; c.height=200; const ctx=c.getContext('2d');
    ctx.fillStyle='#f3f5f8'; ctx.fillRect(0,0,200,200); ctx.fillStyle='#ff7b00'; ctx.font='100px sans-serif'; ctx.textAlign='center'; ctx.textBaseline='middle';
    ctx.fillText((initial||'P').slice(0,1).toUpperCase(),100,110); return c.toDataURL();
  };
  const fileToDataUrl = (file) => new Promise((res,rej)=>{ const r=new FileReader(); r.onload=e=>res(e.target.result); r.onerror=rej; r.readAsDataURL(file); });

  /* ---------- State ---------- */
  let sessionPhone = localStorage.getItem('pm_session') || null;
  let currentUser = null;

  /* ---------- Realtime listeners ---------- */
  // accounts list (keeps in sync)
  onValue(ref(db, '/accounts'), snap=>{
    // we don't render the whole list here, but keep local copy updated implicitly via DB reads where needed
    // optionally could show total counts etc.
  });

  // notification global (founder message)
  const notifRef = ref(db, '/notificacao/global');
  onValue(notifRef, snap=>{
    const v = snap.val();
    if(!v) return;
    $('notifBox').innerHTML = v;
    $('notifBox').classList.remove('hidden');
  });
  // ensure default message exists
  set(notifRef, 'Olá eu sou Diogo Paixão, fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil.');

  // posts (feed)
  const postsRef = ref(db, '/posts');
  onValue(postsRef, snap=>{
    const container = $('postsContainer'); container.innerHTML = '';
    const val = snap.val() || {};
    const arr = Object.keys(val).map(k=>({ id:k, ...val[k]})).sort((a,b)=> (b.createdAt||0)-(a.createdAt||0));
    arr.forEach(p=>{
      const div = document.createElement('div'); div.className='feed-post';
      div.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><strong>${escapeHtml(p.author)}</strong><span class="small">${new Date(p.createdAt).toLocaleString()}</span></div>
        <div style="margin-top:8px">${escapeHtml(p.text||'')}</div>
        ${p.img?`<img src="${p.img}" alt="img">`:''}
        <div style="margin-top:8px" class="row">
          <button class="btn.ghost" onclick="likePost('${p.id}')">👍 ${p.likes||0}</button>
          ${sessionPhone===p.authorPhone?`<button class="btn.ghost" onclick="deletePost('${p.id}')">🗑️ Remover</button>`:''}
        </div>`;
      container.appendChild(div);
    });
  });

  // events
  const eventsRef = ref(db, '/eventos');
  onValue(eventsRef, snap=>{
    const container = $('eventsContainer'); container.innerHTML = '';
    const val = snap.val() || {};
    const arr = Object.keys(val).map(k=>({ id:k, ...val[k]})).sort((a,b)=> (b.createdAt||0)-(a.createdAt||0));
    arr.forEach(e=>{
      const d = document.createElement('div'); d.className='card';
      d.innerHTML = `<strong>${escapeHtml(e.title||'Evento')}</strong><div class="small-note">${new Date(e.createdAt).toLocaleString()}</div><p>${escapeHtml(e.desc||'')}</p>
        <div style="margin-top:8px"><button class="btn.ghost" onclick="attemptDeleteEvent('${e.id}')">Apagar</button></div>`;
      container.appendChild(d);
    });
  });

  /* ---------- Utility functions that call DB ---------- */
  window.escapeHtml = function(s){ if(!s) return ''; return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); };

  // create account in DB
  async function createAccountToDB(acc){
    // structure: /accounts/{phone} => { name, pass, phone, school, photoDataUrl, failedAttempts, locked, points, notifications:{ ... } }
    await set(ref(db, `/accounts/${acc.phone}`), {
      name: acc.name,
      pass: acc.pass,
      phone: acc.phone,
      school: acc.school || '—',
      photo: acc.photo || '',
      failedAttempts: 0,
      locked: false,
      points: 0
    });
  }

  // read account from DB
  async function getAccountFromDB(phone){
    const s = await get(ref(db, `/accounts/${phone}`));
    return s.exists() ? s.val() : null;
  }

  // update account in DB
  async function updateAccountDB(phone, updates){
    await update(ref(db, `/accounts/${phone}`), updates);
  }

  /* ---------- Auth: Register / Login (synchronized via DB) ---------- */
  $('toRegister').addEventListener('click', ()=>{ $('loginView').classList.add('hidden'); $('registerView').classList.remove('hidden'); });
  $('backLogin').addEventListener('click', ()=>{ $('registerView').classList.add('hidden'); $('loginView').classList.remove('hidden'); });

  $('btnDemo').addEventListener('click', async ()=>{
    const demoPhone = '999999999';
    const acc = await getAccountFromDB(demoPhone);
    if(acc) return alert('Demo já existe. Login: 999999999 / demo');
    await createAccountToDB({ name:'Conta Demo', pass:'demo', phone: demoPhone, school:'Demo', photo:'' });
    alert('Conta demo criada: 999999999 / demo');
  });

  $('createBtn').addEventListener('click', async ()=>{
    const name = $('regName').value.trim();
    const phone = $('regPhone').value.trim();
    const pass = $('regPass').value;
    const school = $('regSchool').value.trim();
    if(!name || !phone || !pass) return alert('Preenche nome, telemóvel e senha');
    const existing = await getAccountFromDB(phone);
    if(existing) return alert('Já existe conta com este telemóvel');
    let photoData = '';
    if($('regPhoto').files && $('regPhoto').files[0]){
      try{ photoData = await fileToDataUrl($('regPhoto').files[0]); } catch(e){}
    }
    await createAccountToDB({ name, pass, phone, school, photo: photoData });
    // auto-login
    sessionPhone = phone; localStorage.setItem('pm_session', sessionPhone);
    // push welcome notification to user's notifications node
    await push(ref(db, `/accounts/${phone}/notifications`), { text: `Olá ${name}, bem-vindo(a) ao Playmates!`, createdAt: Date.now() });
    // also push founder message to user's notifications
    await push(ref(db, `/accounts/${phone}/notifications`), { text: `Olá eu sou Diogo Paixão, fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil.`, createdAt: Date.now() });
    renderAfterLogin();
  });

  $('loginBtn').addEventListener('click', async ()=>{
    const phone = $('loginPhone').value.trim();
    const pass = $('loginPass').value;
    if(!phone || !pass) return alert('Preenche telemóvel e senha');
    const acc = await getAccountFromDB(phone);
    if(!acc) return alert('Conta não encontrada');
    if(acc.locked) return alert('Conta bloqueada (contacta suporte).');
    if(acc.pass === pass){
      // reset failedAttempts
      await updateAccountDB(phone, { failedAttempts: 0 });
      sessionPhone = phone; localStorage.setItem('pm_session', sessionPhone);
      // push welcome founder message to notifications for this user
      await push(ref(db, `/accounts/${phone}/notifications`), { text: `Olá ${acc.name}, bem-vindo(a) ao Playmates!`, createdAt: Date.now() });
      await push(ref(db, `/accounts/${phone}/notifications`), { text: `Olá eu sou Diogo Paixão, fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil.`, createdAt: Date.now() });
      renderAfterLogin();
    } else {
      const fa = (acc.failedAttempts||0) + 1;
      const updates = { failedAttempts: fa };
      if(fa >= 5) updates.locked = true;
      await updateAccountDB(phone, updates);
      if(fa >= 5) alert('Senha incorreta — conta bloqueada.');
      else alert(`Senha incorreta. Tentativas: ${fa}/5`);
    }
  });

  /* ---------- After login: render profile, sections, and user notifications ---------- */
  async function renderAfterLogin(){
    if(!sessionPhone) return;
    const acc = await getAccountFromDB(sessionPhone);
    if(!acc) return;
    currentUser = acc;
    // show profile view
    $('loginView').classList.add('hidden');
    $('registerView').classList.add('hidden');
    $('profileView').classList.remove('hidden');
    $('tabsRow').classList.remove('hidden');
    $('authCard').classList.add('hidden'); // hide auth card
    // fill profile
    $('profileName').textContent = acc.name;
    $('profileSchool').textContent = acc.school || '—';
    $('profilePhone').textContent = acc.phone;
    $('profileImg').src = acc.photo && acc.photo.length ? acc.photo : placeholderDataUrl(acc.name);
    // subscribe to user's notifications (real-time)
    onValue(ref(db, `/accounts/${sessionPhone}/notifications`), snap=>{
      const data = snap.val() || {};
      const list = Object.keys(data).sort((a,b)=> (data[b].createdAt||0)-(data[a].createdAt||0)).map(k=>data[k]);
      // show the latest founder message in notifBox (if any)
      if(list.length){
        // use the newest
        $('notifBox').innerHTML = escapeHtml(list[0].text || '');
        $('notifBox').classList.remove('hidden');
      }
    });
    // ensure sections show feed
    activateSection('feed');
  }

  /* ---------- Publish post (writes to /posts) ---------- */
  $('postImage').addEventListener('change', async (e)=> {
    const f = e.target.files[0];
    if(!f) { $('preview').classList.add('hidden'); $('preview').src=''; return; }
    const data = await fileToDataUrl(f);
    $('preview').src = data; $('preview').classList.remove('hidden');
  });

  $('btnPublish').addEventListener('click', async ()=>{
    if(!sessionPhone) return alert('Inicia sessão para publicar');
    const text = $('postText').value.trim();
    let img = '';
    if($('postImage').files && $('postImage').files[0]) img = await fileToDataUrl($('postImage').files[0]);
    if(!text && !img) return alert('Escreve algo ou escolhe imagem');
    const postObj = { author: currentUser.name, authorPhone: currentUser.phone, text, img, createdAt: Date.now(), likes: 0 };
    await push(postsRef, postObj);
    $('postText').value=''; $('postImage').value=''; $('preview').src=''; $('preview').classList.add('hidden');
  });

  window.likePost = async (id) => {
    const pRef = ref(db, `/posts/${id}/likes`);
    // naive transaction-like: read then set (Realtime DB has transaction support but modular SDK doesn't expose here simply; keep simple)
    const snap = await get(ref(db, `/posts/${id}`));
    if(!snap.exists()) return;
    const cur = snap.val();
    const newLikes = (cur.likes||0)+1;
    await update(ref(db, `/posts/${id}`), { likes: newLikes });
  };

  window.deletePost = async (id) => {
    if(!confirm('Remover publicação?')) return;
    const snap = await get(ref(db, `/posts/${id}`));
    if(!snap.exists()) return alert('Publicação não encontrada');
    const p = snap.val();
    if(p.authorPhone !== sessionPhone) return alert('Só podes remover as tuas publicações');
    await remove(ref(db, `/posts/${id}`));
  };

  /* ---------- Events: create/delete with LEX ---------- */
  $('btnPublishEvent').addEventListener('click', async ()=>{
    const pass = $('eventPass').value;
    if(pass !== 'LEX') return alert('Senha incorreta!');
    const title = $('eventTitle').value.trim() || 'Evento';
    const desc = $('eventDesc').value.trim() || '';
    const ev = { title, desc, createdAt: Date.now(), expireAt: Date.no
