
<html lang="pt">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Playmates — Plataforma Oficial</title>
<link rel="icon" href="https://via.placeholder.com/16"/>
<style>
:root{
  --bg:#f6f7fb; --card:#fff; --muted:#6b7280; --accent:#075E54; --accent-2:#25D366;
  --accent-3:#ff7b00;
}
*{box-sizing:border-box}
body{margin:0;font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;background:var(--bg);color:#0b1222;min-height:100vh;padding-bottom:72px;}
header{display:flex;justify-content:space-between;align-items:center;padding:12px 16px;background:linear-gradient(90deg,var(--accent-3),#ff9a3d);color:#fff;font-weight:700;border-radius:0 0 14px 14px;position:sticky;top:0;z-index:30;}
header h1{margin:0;font-size:18px}
main{padding:12px;max-width:980px;margin:0 auto;display:grid;grid-template-columns:1fr;gap:12px;}
.card{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 20px rgba(16,24,40,0.04);}
.section{display:none;}
.section.active{display:block;}
nav{display:flex;justify-content:space-around;background:white;position:fixed;bottom:0;left:0;right:0;border-top:1px solid #ddd;padding:8px 0;}
nav button{background:none;color:#333;border:none;font-size:14px;padding:8px 6px;width:25%;display:flex;flex-direction:column;align-items:center;gap:4px;}
nav button.active{color:var(--accent-3);font-weight:700;}
small.navIcon{font-size:18px;line-height:1}

/* common */
button{cursor:pointer;border:0;border-radius:10px;padding:8px 12px;background:var(--accent-3);color:#fff;font-weight:600;}
button.ghost{background:transparent;border:1px solid rgba(0,0,0,0.06);color:var(--accent-3);}
#notification{display:none;background:var(--accent-3);color:#fff;padding:12px;border-radius:10px;margin-bottom:10px;animation:fade .4s;}
@keyframes fade{from{opacity:0;transform:translateY(-10px);}to{opacity:1;transform:translateY(0);}}
#fotoPerfil{width:100px;height:100px;border-radius:50%;object-fit:cover;margin-bottom:10px;border:3px solid #fff;}
input, label, textarea, select{display:block;margin:6px 0;width:100%;padding:8px;border-radius:8px;border:1px solid #ddd;font-size:14px;}
textarea{resize:none;}
.user-count{font-weight:600;color:var(--accent-3);}
.event-views{font-size:12px;color:var(--muted);}

/* jogos styles */
#countdownCard{display:flex;flex-direction:column;align-items:center;gap:8px;padding:14px;border-radius:10px;border:1px dashed #eee;background:linear-gradient(180deg,#fff,#fff);}
#countdownLabel{font-size:22px;font-weight:800;color:#111;}
.small-ghost{background:transparent;border:1px solid #ddd;padding:6px 8px;border-radius:8px;color:#333;cursor:pointer}
#competitorsList{display:flex;flex-direction:column;gap:10px;margin-top:6px;}
.compCard{display:flex;align-items:center;gap:12px;padding:10px;border-radius:10px;background:#fff;border:1px solid #eee;}
.compCard img{width:80px;height:80px;border-radius:8px;object-fit:cover}
.compInfo{flex:1}
.compActions{display:flex;flex-direction:column;gap:8px;align-items:flex-end}
.edit-orange{background:var(--accent-3);color:#fff;padding:6px 10px;border-radius:8px;border:0}
.vote-green{background:var(--accent-2);color:#fff;padding:8px 10px;border-radius:8px;border:0}
.photo-btn{background:#f1f1f1;border-radius:8px;padding:6px 8px;border:0}
.smallNote{font-size:12px;color:var(--muted)}

/* responsive */
@media (max-width:520px){
  header h1{font-size:16px}
  .compCard img{width:64px;height:64px}
  #countdownLabel{font-size:18px}
}
</style>
</head>
<body>
<header><h1>Playmates</h1></header>

<main>
  <div id="notification" class="card"></div>

  <!-- LOGIN / CADASTRO -->
  <div class="card section active" id="sec-sms">
    <div id="authArea">
      <h3>Bem-vindo ao Playmates</h3>
      <div id="loginForm">
        <label>Telemóvel (ex: 2449xxxxxxx)</label>
        <input id="loginPhone" type="tel" placeholder="Telemóvel"/>
        <label>Senha</label>
        <input id="loginPass" type="password"/>
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
      <img id="fotoPerfil" src="https://via.placeholder.com/100" alt="Foto"/>
      <div id="perfilInfo"></div>
      <p>Usuários cadastrados: <span id="userCount" class="user-count">0</span></p>

      <h3>Pesquisar usuários</h3>
      <input id="searchInput" placeholder="Digite nome"/>
      <div id="searchResults"></div>

      <button id="btnLogout" class="ghost">Sair</button>
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

    <div id="countdownCard">
      <div style="display:flex;align-items:center;gap:8px">
        <strong>Contagem regressiva:</strong>
        <span id="countdownLabel">00:00:00</span>
      </div>
      <div style="font-size:12px;color:#666">
        <button id="editCountdownBtn" class="small-ghost" title="Editar (senha A8)">📢 Editar contador</button>
      </div>
      <div class="smallNote">Tempo visível em tempo real para todos os utilizadores.</div>
    </div>

    <div id="competitorsList"></div>

    <!-- hidden file inputs -->
    <input type="file" id="compPhoto_1" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_2" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_3" accept="image/*" style="display:none"/>
  </div>

  <!-- SOBRE -->
  <div class="card section" id="sec-historia">
    <h2>Sobre</h2>
    <p><strong>Diogo Paixão</strong> — Fundador & CEO. Playmates é uma plataforma estudantil para eventos e votação em tempo real.</p>
  </div>
</main>

<nav>
  <button data-tab="sms" class="active"><small class="navIcon">🏠</small><span>Início</span></button>
  <button data-tab="eventos"><small class="navIcon">📅</small><span>Eventos</span></button>
  <button data-tab="jogos"><small class="navIcon">🎮</small><span>Jogos</span></button>
  <button data-tab="historia"><small class="navIcon">ℹ️</small><span>Sobre</span></button>
</nav>

<!-- SCRIPTS -->
<script type="module">
/* Firebase imports */
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js";
import { getDatabase, ref, set, get, push, onValue, remove, runTransaction } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-database.js";
import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.13.0/firebase-storage.js";

/* Replace with your Firebase config (kept same as your earlier config) */
const firebaseConfig = {
  apiKey:"AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain:"playmates-cc4f7.firebaseapp.com",
  databaseURL:"https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId:"playmates-cc4f7",
  storageBucket:"playmates-cc4f7.appspot.com",
  messagingSenderId:"104004735810",
  appId:"1:104004735810:web:d3ee9a75399d6f0f222edb"
};
const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const storage = getStorage(app);

/* Elements */
const notifBox = document.getElementById("notification");
const notifRef = ref(db,"notificacao/");
onValue(notifRef, snap => { const txt = snap.val(); if(txt){ notifBox.innerText = txt; notifBox.style.display = 'block'; }});
set(notifRef, "Olá eu sou Diogo Paixão, fundador & CEO da plataforma Playmates e espero que tenhas uma ótima experiência juvenil");

let currentUser = null;

/* NAV */
const navButtons = document.querySelectorAll("nav button");
navButtons.forEach(btn=>{
  btn.addEventListener('click', ()=>{
    const tab = btn.dataset.tab;
    document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
    document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));
    document.getElementById('sec-'+tab).classList.add('active');
    btn.classList.add('active');

    // require login for eventos/jogos
    if((tab === 'eventos' || tab === 'jogos') && !currentUser){
      alert('Faça login para acessar esta aba');
      document.getElementById('sec-sms').classList.add('active');
      document.querySelector('nav button[data-tab="sms"]').classList.add('active');
    }
  });
});

/* AUTH UI */
const loginPhone = document.getElementById('loginPhone'), loginPass = document.getElementById('loginPass'), loginSubmit = document.getElementById('loginSubmit');
const showRegisterBtn = document.getElementById('showRegister'), regCancel = document.getElementById('regCancel'), regSubmit = document.getElementById('regSubmit');
const regName = document.getElementById('regName'), regPass = document.getElementById('regPass'), regPhone = document.getElementById('regPhone'), regSchool = document.getElementById('regSchool'), regPhoto = document.getElementById('regPhoto');
const authArea = document.getElementById('authArea'), loggedArea = document.getElementById('loggedArea');
const fotoPerfil = document.getElementById('fotoPerfil'), perfilInfo = document.getElementById('perfilInfo');
const searchInput = document.getElementById('searchInput'), searchResults = document.getElementById('searchResults');
const btnLogout = document.getElementById('btnLogout'), userCountLabel = document.getElementById('userCount');

showRegisterBtn.onclick = ()=>{ document.getElementById('loginForm').style.display='none'; document.getElementById('registerForm').style.display='block'; };
regCancel.onclick = ()=>{ document.getElementById('registerForm').style.display='none'; document.getElementById('loginForm').style.display='block'; };

regSubmit.onclick = async ()=>{
  const name = (regName.value||'').trim(), pass = regPass.value, phone = (regPhone.value||'').trim(), school = (regSchool.value||'').trim();
  if(!name||!pass||!phone) return alert('Preencha campos obrigatórios');
  const userRef = ref(db, 'users/'+phone);
  const snap = await get(userRef);
  if(snap.exists()) return alert('Conta já existe');
  let photoURL = '';
  if(regPhoto.files[0]){
    const file = regPhoto.files[0];
    const sRefPath = sRef(storage, `perfilFotos/${phone}_${Date.now()}`);
    await uploadBytes(sRefPath, file);
    photoURL = await getDownloadURL(sRefPath);
  }
  await set(userRef, { name, pass, phone, school, foto:photoURL, points:0 });
  loginUser(phone);
};

loginSubmit.onclick = async ()=>{
  const phone = (loginPhone.value||'').trim(), pass = loginPass.value;
  const snap = await get(ref(db, 'users/'+phone));
  if(!snap.exists()) return alert('Conta não encontrada');
  const user = snap.val();
  if(user.pass !== pass) return alert('Senha incorreta');
  loginUser(phone);
};

function loginUser(phone){
  currentUser = phone;
  authArea.style.display = 'none';
  loggedArea.style.display = 'block';
  const userRef = ref(db, 'users/'+phone);
  onValue(userRef, snap=>{
    const u = snap.val();
    fotoPerfil.src = u.foto || 'https://via.placeholder.com/100';
    perfilInfo.innerHTML = `<p><strong>Nome:</strong> ${u.name}</p><p><strong>Telemóvel:</strong> ${u.phone}</p><p><strong>Escola:</strong> ${u.school}</p><p><strong>Pontos:</strong> ${u.points}</p>`;
  });

  const usersRef = ref(db, 'users/');
  onValue(usersRef, snap=>{
    const total = snap.size;
    userCountLabel.innerText = total >= 1000000 ? '1M+' : total;
    searchResults.innerHTML = '';
    snap.forEach(item=>{
      const u = item.val();
      if(u.phone !== currentUser){
        searchResults.innerHTML += `<div style="padding:6px;border-bottom:1px solid #eee">${u.name} - ${u.phone}</div>`;
      }
    });
  });

  searchInput.oninput = ()=>{
    const filtro = (searchInput.value||'').toLowerCase();
    searchResults.innerHTML = '';
    onValue(ref(db, 'users/'), snap=>{
      snap.forEach(item=>{
        const u = item.val();
        if(u.name && u.name.toLowerCase().includes(filtro) && u.phone !== currentUser){
          searchResults.innerHTML += `<div style="padding:6px;border-bottom:1px solid #eee">${u.name} - ${u.phone}</div>`;
        }
      });
    });
  };

  btnLogout.onclick = ()=>{
    loggedArea.style.display = 'none';
    authArea.style.display = 'block';
    currentUser = null;
  };
}

/* EVENTOS */
const eventosRef = ref(db, 'eventos/');
document.getElementById('btnNovoEvento').onclick = async ()=>{
  if(!currentUser) return alert('Faça login para criar evento.');
  const s = prompt('Senha para publicar evento:');
  if(s !== 'LEX') return alert('Senha errada!');
  const t = prompt('Título:'), c = prompt('Descrição:');
  if(!t||!c) return alert('Dados inválidos');
  push(eventosRef, { titulo: t, texto: c, views: 0 });
};

window.apagarEvento = (id)=>{
  const s = prompt('Senha para apagar:');
  if(s !== 'LEX') return alert('Senha incorreta!');
  remove(ref(db, 'eventos/'+id));
};

onValue(eventosRef, snap=>{
  const eventosDiv = document.getElementById('eventosLista');
  eventosDiv.innerHTML = '';
  snap.forEach(item=>{
    const key = item.key;
    const evt = item.val();
    if(!evt.views) evt.views = 0;
    runTransaction(ref(db, 'eventos/'+key+'/views'), current=> current===null?1:current+1);
    eventosDiv.innerHTML += `<div style="padding:10px;border-bottom:1px solid #eee"><h3>${evt.titulo}</h3><p>${evt.texto}</p><p class="event-views">👁️ ${evt.views||0} views</p><button onclick="apagarEvento('${key}')">Apagar</button></div>`;
  });
});

/* ---------- JOGOS (CONTADOR + CONCORRENTES) ---------- */
const countdownLabel = document.getElementById('countdownLabel');
const editCountdownBtn = document.getElementById('editCountdownBtn');
const competitorsList = document.getElementById('competitorsList');
const fileInputs = {
  1: document.getElementById('compPhoto_1'),
  2: document.getElementById('compPhoto_2'),
  3: document.getElementById('compPhoto_3')
};
const countdownRef = ref(db, 'jogos/countdown');
const competitorsRef = ref(db, 'jogos/competitors');

/* util */
function formatHMS(ms){
  if(ms < 0) ms = 0;
  const total = Math.floor(ms/1000);
  const h = Math.floor(total/3600).toString().padStart(2,'0');
  const m = Math.floor((total%3600)/60).toString().padStart(2,'0');
  const s = Math.floor(total%60).toString().padStart(2,'0');
  return `${h}:${m}:${s}`;
}

/* mantém target local */
let currentTarget = null;
onValue(countdownRef, snap=>{
  const val = snap.val();
  currentTarget = val && val.target ? val.target : null;
  const rem = currentTarget ? (currentTarget - Date.now()) : 0;
  countdownLabel.innerText = formatHMS(rem);
});
setInterval(()=>{
  if(currentTarget){
    const rem = currentTarget - Date.now();
    countdownLabel.innerText = formatHMS(rem);
  } else {
    countdownLabel.innerText = '00:00:00';
  }
},1000);

/* editar contador (senha A8) */
editCountdownBtn.onclick = async ()=>{
  if(!currentUser) return alert('Faça login para editar o contador.');
  const pw = prompt('Senha para editar o contador:');
  if(pw !== 'A8') return alert('Senha incorreta.');
  let h = parseInt(prompt('Horas (0-99):', '0')) || 0;
  let m = parseInt(prompt('Minutos (0-59):', '0')) || 0;
  let s = parseInt(prompt('Segundos (0-59):', '30')) || 0;
  if(h<0) h=0; if(m<0) m=0; if(s<0) s=0;
  const totalMs = ((h*3600)+(m*60)+s)*1000;
  const targetTs = Date.now() + totalMs;
  await set(countdownRef, { target: targetTs });
  alert('Contador atualizado com sucesso.');
};

/* garante concorrentes padrão */
async function ensureDefaultCompetitors(){
  const snap = await get(competitorsRef);
  if(!snap.exists()){
    const defaults = {
      1:{ name: 'Concorrente 1', school: 'Escola A', votes: 0, photo: '' },
      2:{ name: 'Concorrente 2', school: 'Escola B', votes: 0, photo: '' },
      3:{ name: 'Concorrente 3', school: 'Escola C', votes: 0, photo: '' }
    };
    await set(competitorsRef, defaults);
  }
}
ensureDefaultCompetitors().catch(()=>{});

/* render */
function renderCompetitors(listObj){
  competitorsList.innerHTML = '';
  [1,2,3].forEach(id=>{
    const c = listObj && listObj[id] ? listObj[id] : { name:`Concorrente ${id}`, school:'', votes:0, photo:'' };
    const card = document.createElement('div'); card.className = 'compCard';
    const img = document.createElement('img'); img.src = c.photo || 'https://via.placeholder.com/80';
    const info = document.createElement('div'); info.className = 'compInfo';
    info.innerHTML = `<div style="font-weight:800">${c.name}</div><div style="color:#666">${c.school||''}</div><div style="margin-top:8px;font-weight:700;color:var(--accent-3)">Votos: <span id="votes_${id}">${c.votes||0}</span></div>`;
    const actions = document.createElement('div'); actions.className = 'compActions';

    const editBtn = document.createElement('button'); editBtn.innerText = '⚫ Editar'; editBtn.className = 'edit-orange';
    editBtn.onclick = async ()=>{
      if(!currentUser) return alert('Faça login para editar concorrentes.');
      const pw = prompt('Senha para editar concorrente:');
      if(pw !== '5A') return alert('Senha incorreta.');
      const newName = prompt('Nome completo:', c.name) || c.name;
      const newSchool = prompt('Escola:', c.school) || c.school;
      const newVotes = parseInt(prompt('Número de votos:', c.votes||0)) || 0;
      await set(ref(db, `jogos/competitors/${id}`), { name: newName, school: newSchool, votes: newVotes, photo: c.photo||'' });
      alert('Concorrente atualizado.');
    };

    const photoBtn = document.createElement('button'); photoBtn.innerText = '📷 Foto'; photoBtn.className = 'photo-btn';
    photoBtn.onclick = ()=> fileInputs[id].click();

    const voteBtn = document.createElement('button'); voteBtn.innerText = 'VOTAR'; voteBtn.className = 'vote-green';
    voteBtn.onclick = async ()=>{
      if(!currentUser) return alert('Faça login para votar.');
      // obtem nome do votante
      const vsnap = await get(ref(db, `users/${currentUser}`));
      const voter = vsnap.exists() ? vsnap.val().name : 'Anon';
      const msg = `Olá eu chamo-me ${voter} e desejo votar no concorrente ${c.name}`;
      const waLink = `https://wa.me/244941530467?text=${encodeURIComponent(msg)}`;
      window.open(waLink, '_blank');
      // aumenta votos com transação
      runTransaction(ref(db, `jogos/competitors/${id}/votes`), cur => (cur||0)+1);
      push(ref(db, 'jogos/votes'), { voter: currentUser, competitor: id, ts: Date.now() });
    };

    actions.appendChild(editBtn);
    actions.appendChild(photoBtn);
    actions.appendChild(voteBtn);

    card.appendChild(img);
    card.appendChild(info);
    card.appendChild(actions);
    competitorsList.appendChild(card);
  });
}

/* listener global */
onValue(competitorsRef, snap=>{
  const val = snap.val();
  renderCompetitors(val);
  if(val){
    [1,2,3].forEach(id=>{
      const vEl = document.getElementById(`votes_${id}`);
      if(vEl) vEl.innerText = (val[id] && val[id].votes) ? val[id].votes : 0;
    });
  }
});
  

/* upload fotos */
[1,2,3].forEach(id=>{
  const inp = fileInputs[id];
  inp.onchange = async (e)=>{
    const file = e.target.files[0];
    if(!file) return;
    if(!currentUser) return alert('Faça login para enviar foto.');
    const path = `jogos/competitors/${id}/photo_${Date.now()}`;
    const sRefPath = sRef(storage, path);
    try{
      await uploadBytes(sRefPath, file);
      const url = await getDownloadURL(sRefPath);
      const compRef = ref(db, `jogos/competitors/${id}`);
      const snap = await get(compRef);
      const comp = snap.exists() ? snap.val() : { name:`Concorrente ${id}`, school:'', votes:0 };
      comp.photo = url;
      await set(compRef, comp);
      alert('Foto enviada com sucesso.');
    }catch(err){
      console.error(err); alert('Erro ao enviar foto.');
    } finally { inp.value = ''; }
  };
});

/* fim jogos block */

/* Ready note: if DB rules block writes, configure Realtime DB rules in Firebase console accordingly. */
/* If precisar, posso ajudar a criar regras seguras. */

</script>
</body>
</html>
