<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Playmates - Gestor de Votos (Light)</title>
<style>
  :root{
    --bg: #f6f9fc;
    --card: #ffffff;
    --muted: #6b7280;
    --accent: #0b6bff;
    --success:#16a34a;
    --danger:#ef4444;
    --glass: rgba(255,255,255,0.9);
    --shadow: 0 8px 28px rgba(11,107,255,0.06);
    --radius: 12px;
  }
  *{box-sizing:border-box}
  body{
    margin:0; padding:18px; font-family:Inter, system-ui, Arial, sans-serif;
    background: linear-gradient(180deg,#eef6ff 0%, #fbfdff 100%);
    color:#0b1220;
  }
  header{
    display:flex; align-items:center; justify-content:space-between;
    padding:14px; border-radius:var(--radius); background:linear-gradient(90deg,#0b4ea3,#0b6bff);
    color:#fff; box-shadow:0 12px 40px rgba(11,107,255,0.12);
  }
  .brand-left{display:flex; gap:12px; align-items:center;}
  .brand-circle{width:48px;height:48px;border-radius:50%;background:black;display:flex;align-items:center;justify-content:center;color:orange;font-weight:800}
  #timer{font-weight:800;font-size:20px}
  .btn{background:var(--card);border:none;padding:8px 10px;border-radius:10px;cursor:pointer;box-shadow:var(--shadow);font-weight:700}
  .btn.small{padding:6px 8px;font-size:13px}
  .layout{display:grid;grid-template-columns:1fr 360px;gap:18px;margin-top:18px}
  @media(max-width:980px){ .layout{grid-template-columns:1fr} header{flex-direction:column;align-items:flex-start;gap:10px} }

  /* Cards */
  .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:var(--shadow)}
  .card h3{margin:0 0 8px 0}
  /* contestants */
  .contestant{display:flex;gap:12px;align-items:center;padding:8px;border-radius:10px;border:1px solid #eef3ff;margin-bottom:10px}
  .contestant img{width:84px;height:84px;border-radius:10px;object-fit:cover;background:#eef6ff}
  .c-info{flex:1;text-align:left}
  .c-info strong{display:block;font-size:16px}
  .c-meta{color:var(--muted);font-size:13px;margin-top:6px}
  .c-actions{display:flex;flex-direction:column;gap:8px;align-items:flex-end}
  .edit-dot{width:36px;height:36;border-radius:50%;background:#0b1220;color:#fff;display:flex;align-items:center;justify-content:center;cursor:pointer}

  /* edit inline */
  .edit-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
  .edit-row input{padding:8px;border-radius:8px;border:1px solid #e6eef8;font-size:14px}

  /* prize box */
  .prize-box{display:flex;justify-content:space-between;align-items:center;padding:12px;border-radius:10px;background:linear-gradient(180deg,#fff,#fbfdff);border:1px solid #eef3ff}
  .prize-amount{font-weight:800;color:#92400e;font-size:20px}

  /* participant form */
  .form-row{display:flex;gap:8px;margin-top:10px}
  input[type="text"], input[type="tel"], select{padding:10px;border-radius:10px;border:1px solid #e6eef8;font-size:14px;width:100%}
  .pack-choices{display:flex;gap:8px;margin-top:8px}
  .pack-option{flex:1;padding:10px;border-radius:10px;border:1px dashed #e6eef8;cursor:pointer}
  .pack-option.selected{border-style:solid;background:#f8fbff;box-shadow:0 8px 24px rgba(11,107,255,0.04)}

  /* simulator */
  .simulator{display:flex;align-items:center;justify-content:space-between;padding:10px;border-radius:10px;border:1px solid #eef3ff;margin-top:10px}
  .sim-number{font-weight:800;font-size:20px;padding:6px 10px;border-radius:8px;background:#eef6f3;cursor:pointer}

  /* modal (password) */
  .modal-back{position:fixed;inset:0;background:rgba(2,6,23,0.35);display:flex;align-items:center;justify-content:center;z-index:9999;padding:18px}
  .modal-card{background:var(--card);padding:18px;border-radius:12px;max-width:420px;width:100%;box-shadow:0 18px 60px rgba(2,6,23,0.18)}
  .modal-card h4{margin:0 0 8px 0}
  .modal-actions{display:flex;gap:8px;justify-content:flex-end;margin-top:12px}
  .danger{background:var(--danger);color:#fff}
  .success{background:var(--success);color:#fff}

  /* participants modal */
  .participants-list{max-height:360px;overflow:auto;margin-top:10px}
  .p-row{display:flex;justify-content:space-between;align-items:center;padding:8px;border-bottom:1px solid #f1f5f9}

  /* winner modal */
  .winner-modal{:0;background:linear-gradient(180deg,rgba(3,7,18,0.6),rgba(2,6,23,0.8));display:flex;align-items:center;justify-content:center;z-index:10000}
  .winner-card{background:linear-gradient(180deg,#fff,#fbfdff);padding:22px;border-radius:14px;text-align:center;max-width:520px;width:100%;box-shadow:0 20px 60px rgba(2,6,23,0.4)}
  .winner-pulse{width:140px;height:140px;border-radius:50%;display:flex;align-items:center;justify-content:center;margin:14px auto;font-weight:800;font-size:20px;background:linear-gradient(90deg,#ffedd5,#fff7ed);animation:pulse 1s infinite}
  @keyframes pulse{0%{transform:scale(1)}50%{transform:scale(1.06)}100%{transform:scale(1)}}

  /* right column profile */
  .profile{display:flex;gap:12px;align-items:flex-start}
  .profile img{width:92px;height:92px;border-radius:10px;object-fit:cover;background:#eef6ff}
  .profile a{display:inline-block;margin-top:8px;background:#25D366;color:#fff;padding:8px 10px;border-radius:8px;text-decoration:none;font-weight:700}

  /* ranking */
  .ranking{margin-top:10px}
  .rank-row{display:flex;justify-content:space-between;padding:8px;border-radius:8px;border:1px solid #eef3ff;margin-bottom:6px}
  .muted{color:var(--muted);font-size:13px}
</style>
</head>
<body>

<header>
  <div class="brand-left">
    <div class="brand-circle">P</div>
    <div>
      <div style="font-weight:800">Playmates — Gestor de Votos</div>
      <div style="font-size:13px;color:rgba(255,255,255,0.9)">Painel de votação & prémios</div>
    </div>
  </div>
  <div style="display:flex;gap:10px;align-items:center">
    <div id="timer">00:00:00</div>
    <button id="editTimeBtn" class="btn small">📢 Editar tempo (A6)</button>
  </div>
</header>

<div class="layout">

  <!-- MAIN -->
  <main>
    <div class="card prize-box">
      <div>
        <div class="muted">Prémio para o mais votado</div>
        <div class="prize-amount" id="prizeAmount">5.000,00 Kz</div>
        <div class="muted" style="font-size:13px">Clique em editar para modificar (senha AE)</div>
      </div>
      <div style="display:flex;flex-direction:column;gap:8px;align-items:flex-end">
        <button id="btnEditPrize" class="btn small">✏️ Editar Prémio</button>
        <div class="muted" style="font-size:12px">Persistente</div>
      </div>
    </div>

    <div class="card" id="contestantsCard">
      <h3>Concorrentes</h3>
      <div id="contestantsList"></div>
      <div style="margin-top:12px">
        <h4 style="margin:0 0 8px 0">Modo de edição</h4>
        <div class="muted" style="font-size:13px">Clique no botão ⚫ em qualquer concorrente para autenticar e editar.</div>
      </div>
      <div id="ranking" class="ranking card" style="margin-top:12px">
        <h4 style="margin:0 0 8px 0">Ranking (em tempo real)</h4>
        <div id="rankingList"></div>
      </div>
    </div>

    <div class="card">
      <h3>Enviar Pedido de Participação</h3>
      <div class="form-row">
        <input id="inputName" type="text" placeholder="Nome completo" />
        <input id="inputWhats" type="tel" placeholder="WhatsApp (ex: 941530467)" />
      </div>
      <div style="margin-top:8px">
        <div class="pack-choices">
          <div class="pack-option selected" data-pack="Começar gratuito 20S e ganhar 450kz">1. Começar gratuito → 450kz</div>
          <div class="pack-option" data-pack="Começar 2.000 e ganhar 4.000">2. Começar 2.000 → 4.000</div>
        </div>
      </div>
      <div style="margin-top:12px;display:flex;gap:8px">
        <button id="sendRequestBtn" class="btn">Enviar Pedido</button>
      </div>
      <div class="simulator" style="margin-top:12px">
        <div>
          <div class="muted">Pedidos totais</div>
          <div id="simNumber" class="sim-number">0</div>
        </div>
        <div class="muted">Clique no número para ver (senha A3)</div>
      </div>
    </div>
  </main>

  <!-- RIGHT -->
  <aside>
    <div class="card profile">
      <img id="profilePhoto" src="" alt="Foto Diogo" />
      <div>
        <div style="font-weight:800">Diogo paixão</div>
        <div class="muted" style="margin-top:4px">Estado civil: Solteiro</div>
        <div style="margin-top:8px;font-size:14px">Empreendedor e especialista em marketing digital. Ajudo a aumentar vendas com estratégias práticas.</div>
        <a id="waBtn" class="" href="https://wa.me/244941530467" target="_blank" style="display:inline-block;margin-top:8px;background:#25D366;color:#fff;padding:8px 10px;border-radius:8px;text-decoration:none;font-weight:700">Iniciar WhatsApp — 941530467</a>
        <div style="margin-top:8px;display:flex;gap:8px">
          <button id="uploadProfile" class="btn small">Carregar foto</button>
        </div>
      </div>
    </div>

    <div class="card" style="margin-top:12px">
      <h4 style="margin:0 0 8px 0">Observações</h4>
      <div class="muted">Senhas importantes: A6 (tempo) · AE (prémio) · A3 (ver pedidos) · 5A (editar concorrentes)</div>
    </div>
  </aside>
</div>

<div id="modalRoot"></div>

<script>
/* -------------------------------
   HELPERS
------------------------------- */
const qs = id => document.getElementById(id);
const setLS = (k,v) => localStorage.setItem(k, JSON.stringify(v));
const getLS = (k, fallback=null) => { const s=localStorage.getItem(k); if(!s) return fallback; try{return JSON.parse(s);}catch(e){return s;} }
const escapeHtml = str=>str.replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const placeholderDataURL=name=>`https://via.placeholder.com/84x84.png?text=${encodeURIComponent(name)}`;

/* -------------------------------
   DADOS INICIAIS
------------------------------- */
const defaultContestants=[
  {id:1,name:"Concorrente 1",votes:0,course:"Curso A",school:"Escola X",photo:""},
  {id:2,name:"Concorrente 2",votes:0,course:"Curso B",school:"Escola Y",photo:""},
  {id:3,name:"Concorrente 3",votes:0,course:"Curso C",school:"Escola Z",photo:""}
];

function loadContestants(){
  const arr=[];
  for(const d of defaultContestants){
    const key="contestant_"+d.id;
    const saved=getLS(key,null);
    if(saved) arr.push(saved);
    else{ setLS(key,d); arr.push(d);}
  }
  return arr;
}
function saveContestant(c){ setLS("contestant_"+c.id,c);}
function getParticipants(){ return getLS("participants",[]);}
function setParticipants(arr){ setLS("participants",arr);}
function getRequestsCount(){ return getParticipants().length;}

/* -------------------------------
   RENDER
------------------------------- */
function renderContestants(){
  const list=loadContestants();
  const wrap=document.createElement("div");
  for(const c of list){
    const el=document.createElement("div");
    el.className="contestant";
    el.id="contestant_"+c.id;
    el.innerHTML=`
      <img id="photo_${c.id}" src="${c.photo||placeholderDataURL(c.name)}" alt="">
      <div class="c-info">
        <strong id="cname_${c.id}">${escapeHtml(c.name)}</strong>
        <div class="c-meta">Votos: <span id="cvotes_${c.id}">${c.votes}</span> · ${escapeHtml(c.course)} · ${escapeHtml(c.school)}</div>
        <div class="edit-row" id="editRow_${c.id}" style="display:none;">
          <input id="inName_${c.id}" type="text" value="${escapeHtml(c.name)}" placeholder="Nome" />
          <input id="inVotes_${c.id}" type="number" value="${c.votes}" style="width:100px" />
          <input id="inCourse_${c.id}" type="text" value="${escapeHtml(c.course)}" placeholder="Curso" />
          <input id="inSchool_${c.id}" type="text" value="${escapeHtml(c.school)}" placeholder="Escola" />
          <input id="inPhoto_${c.id}" type="file" accept="image/*"/>
          <div style="width:100%;display:flex;gap:8px;margin-top:8px">
            <button class="btn small" id="saveBtn_${c.id}">Salvar</button>
            <button class="btn small" id="cancelBtn_${c.id}">Cancelar</button>
          </div>
        </div>
      </div>
      <div class="c-actions">
        <div title="Editar concorrente (pede senha 5A)" class="edit-dot" id="dot_${c.id}">⚫</div>
        <button class="btn small" id="voteBtn_${c.id}">➕ Votar</button>
      </div>
    `;
    wrap.appendChild(el);
  }
  const container=qs("contestantsList");
  container.innerHTML="";
  container.appendChild(wrap);
  // listeners
  for(const c of list){
    qs("dot_"+c.id).addEventListener("click",()=>onEditDot(c.id));
    qs("voteBtn_"+c.id).addEventListener("click",()=>onVote(c.id));
    qs("saveBtn_"+c.id).addEventListener("click",()=>onSaveEdit(c.id));
    qs("cancelBtn_"+c.id).addEventListener("click",()=>onCancelEdit(c.id));
    const fileInput=qs("inPhoto_"+c.id);
    fileInput.addEventListener("change", (e)=>onUploadPhoto(c.id,e));
  }
  renderRanking();
}

function renderRanking(){
  const list=loadContestants().slice().sort((a,b)=>Number(b.votes)-Number(a.votes));
  const rwrap=qs("rankingList");
  rwrap.innerHTML="";
  list.forEach((c,idx)=>{
    const row=document.createElement("div");
    row.className="rank-row";
    row.innerHTML=`<div><strong>${idx+1}. ${escapeHtml(c.name)}</strong><div class="muted">${escapeHtml(c.course)} · ${escapeHtml(c.school)}</div></div><div style="font-weight:800">${c.votes}</div>`;
    rwrap.appendChild(row);
  });
}

/* -------------------------------
   EDIT CONTESTANT
------------------------------- */
function onEditDot(id){
  promptPassword("5A","Senha necessária para editar concorrente",()=>{
    qs("editRow_"+id).style.display="flex";
  });
}
function onSaveEdit(id){
  const c=loadContestants().find(c=>c.id===id);
  c.name=qs("inName_"+id).value;
  c.votes=Number(qs("inVotes_"+id).value);
  c.course=qs("inCourse_"+id).value;
  c.school=qs("inSchool_"+id).value;
  saveContestant(c);
  renderContestants();
}
function onCancelEdit(id){
  qs("editRow_"+id).style.display="none";
}
function onUploadPhoto(id,e){
  const file=e.target.files[0];
  if(file){
    const reader=new FileReader();
    reader.onload=function(ev){
      const c=loadContestants().find(c=>c.id===id);
      c.photo=ev.target.result;
      saveContestant(c);
      renderContestants();
    }
    reader.readAsDataURL(file);
  }
}

/* -------------------------------
   VOTAR via WhatsApp
------------------------------- */
function onVote(id){
  const c=loadContestants().find(c=>c.id===id);
  const name=prompt("Digite seu nome para enviar SMS via WhatsApp");
  if(!name) return;
  const waURL=`https://wa.me/244941530467?text=${encodeURIComponent(`Olá eu chamo-me ${name} e pretendo votar no ${c.name} Diogo paixão`)}`;
  window.open(waURL,"_blank");
  // esperar o usuário confirmar (não dá para detectar envio real no WhatsApp)
  setTimeout(()=>{
    c.votes+=1;
    saveContestant(c);
    renderContestants();
  },5000); // 5s simula confirmação
}

/* -------------------------------
   PARTICIPANTES / PEDIDOS
------------------------------- */
const simNumber=qs("simNumber");
function updateSimNumber(){
  simNumber.textContent=getRequestsCount();
}
simNumber.addEventListener("click",()=>{
  promptPassword("A3","Senha necessária para ver pedidos",()=>{
    showParticipantsModal();
  });
});
function showParticipantsModal(){
  const participants=getParticipants();
  const modal=createModal(`<h4>Pedidos de Participação</h4>
    <div class="participants-list">
      ${participants.map((p,idx)=>`<div class="p-row">
        <div>${escapeHtml(p.name)} · ${escapeHtml(p.whats)} · ${escapeHtml(p.pack)}</div>
        <div><button class="btn small danger" onclick="removeParticipant(${idx})">Eliminar</button></div>
      </div>`).join("")}
    </div>`);
}
function removeParticipant(idx){
  const arr=getParticipants();
  arr.splice(idx,1);
  setParticipants(arr);
  updateSimNumber();
  renderContestants();
  document.querySelector(".modal-back").remove();
}

/* -------------------------------
   ENVIO DE PEDIDO
------------------------------- */
const sendRequestBtn=qs("sendRequestBtn");
const packOptions=document.querySelectorAll(".pack-option");
let selectedPack="Começar gratuito 20S e ganhar 450kz";
packOptions.forEach(po=>{
  po.addEventListener("click",()=>{
    packOptions.forEach(p=>p.classList.remove("selected"));
    po.classList.add("selected");
    selectedPack=po.dataset.pack;
  });
});
sendRequestBtn.addEventListener("click",()=>{
  const name=qs("inputName").value.trim();
  const whats=qs("inputWhats").value.trim();
  if(!name||!whats){alert("Preencha nome e WhatsApp"); return;}
  const arr=getParticipants();
  arr.push({name,whats,pack:selectedPack,date:new Date().toLocaleString()});
  setParticipants(arr);
  updateSimNumber();
  qs("inputName").value=""; qs("inputWhats").value="";
});

/* -------------------------------
   MODAL / SENHA
------------------------------- */
function createModal(content){
  const modal=document.createElement("div");
  modal.className="modal-back";
  modal.innerHTML=`<div class="modal-card">${content}</div>`;
  document.getElementById("modalRoot").appendChild(modal);
  return modal;
}
function promptPassword(correct,promptText,callback){
  const modal=createModal(`<h4>${promptText}</h4>
    <input type="password" id="pwInput" placeholder="Digite a senha" style="width:100%;padding:8px;border-radius:8px;border:1px solid #e6eef8;margin-top:6px"/>
    <div class="modal-actions">
      <button class="btn small success" id="pwOk">Confirmar</button>
      <button class="btn small danger" id="pwCancel">Cancelar</button>
    </div>
  `);
  qs("pwOk").onclick=()=>{
    const val=qs("pwInput").value;
    if(val===correct){ modal.remove(); callback(); }
    else{ alert("Senha incorreta"); }
  };
  qs("pwCancel").onclick=()=>modal.remove();
}

/* -------------------------------
   CONTAGEM REGRESSIVA
------------------------------- */
let countdownTimer;
let remainingTime=getLS("countdownTime",300); // segundos default
const timerEl=qs("timer");

function updateTimerDisplay(){
  const h=Math.floor(remainingTime/3600);
  const m=Math.floor((remainingTime%3600)/60);
  const s=remainingTime%60;
  timerEl.textContent=`${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
}
function startCountdown(){
  clearInterval(countdownTimer);
  countdownTimer=setInterval(()=>{
    if(remainingTime>0){ remainingTime--; updateTimerDisplay(); setLS("countdownTime",remainingTime);}
    else clearInterval(countdownTimer);
  },1000);
}
updateTimerDisplay();
startCountdown();

qs("editTimeBtn").addEventListener("click",()=>{
  promptPassword("A6","Senha para editar contagem regressiva",()=>{
    const h=prompt("Horas",Math.floor(remainingTime/3600));
    const m=prompt("Minutos",Math.floor((remainingTime%3600)/60));
    const s=prompt("Segundos",remainingTime%60);
    remainingTime=Number(h)*3600 + Number(m)*60 + Number(s);
    updateTimerDisplay();
    setLS("countdownTime",remainingTime);
    startCountdown();
  });
});

/* -------------------------------
   INIT
------------------------------- */
renderContestants();
updateSimNumber();

</script>

</body>
</html>
