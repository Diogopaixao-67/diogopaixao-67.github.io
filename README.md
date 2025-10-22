
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Playmates - Painel de Votação</title>
<style>
  .
  .btn.small{padding:6px 8px;font-size:13px}
  .layout{display:grid;grid-template-columns:1fr 360px;gap:18px;margin-top:18px}
  /* ===== Relógio ===== */
  .clock-container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    font-size: 2rem;
    color: red;
    font-weight: bold;
  }

  .clock-container span {
    font-family: "Courier New", monospace;
    letter-spacing: 2px;
  }

  .clock-container button {
    background: black;
    border: none;
    cursor: pointer;
    font-size: 1.5rem;
    
  }
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
    color:#fff; 
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


  /* right column profile */
  .profile{display:flex;gap:12px;align-items:flex-start}
  .profile img{width:px;height:92px;border-radius:8px;object-fit:cover;background:#eef6ff}
  .profile a{display:inline-block;margin-top:8px;background:#25D366;color:#fff;padding:8px 10px;border-radius:8px;text-decoration:none;font-weight:700}
img {
    width:100%; max-height:220px; object-fit:cover;
    border-radius:8px;
    margin-bottom:6px;
  }

  body {
    font-family: "Segoe UI", sans-serif;
    margin: 0;
    background: #fff;
    color: #222;
  }
  header {
    background: #fff;
    color: orange;
    text-align: center;
    padding: 15px 10px;
    font-size: 22px;
    font-weight: bold;
    border-bottom: 2px solid #f0f0f0;
    position: sticky;
    top: 0;
    z-index: 10;
  }
  #ranking {
    font-size: 15px;
    color: #0077cc;
    margin-top: 5px;
  }
  .container {
    width: 90%;
    max-width: 420px;
    margin: 18px auto;
    background: #fff;
    border-radius: 16px;
    box-shadow: 0 3px 10px rgba(0,0,0,0.08);
    padding: 15px;
    text-align: center;
    transition: all 0.2s ease;
  }
  .container:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.1);
  }
  .photo-container {
    width: 140px;
    height: 140px;
    border-radius: 50%;
    overflow: hidden;
    margin: 0 auto 10px;
    border: 3px solid #FFA100;
    box-shadow: 0 0 8px rgba(0,0,0,0.1);
  }
  .photo-container img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    cursor: pointer;
  }
  .info {
    font-size: 14px;
    margin-bottom: 8px;
  }
  .edit-btn, .results-btn {
    font-size: 18px;
    border: none;
    background: none;
    cursor: pointer;
    margin-top: 5px;
  }
  .save-btn, .vote-btn {
    border: none;
    border-radius: 8px;
    padding: 7px 14px;
    cursor: pointer;
    color: #fff;
    font-weight: 600;
  }
  .save-btn { background:; }
  .vote-btn { background: #F2A300; }
  input {
    width: 90%;
    padding: 7px;
    border-radius: 6px;
    border: 1px solid #ccc;
    margin-top: 6px;
  }
  #zoomView {
    display: none;
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.8);
    justify-content: center;
    align-items: center;
    z-index: 100;
  }
  #zoomView img {
    max-width: 90%;
    max-height: 90%;
    border: 4px solid white;
    border-radius: 10px;
  }
  #resultsPanel {
    display: none;
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(255,255,255,0.97);
    overflow-y: auto;
    z-index: 200;
    padding: 20px;
  }
  #resultsPanel h2 {
    text-align: center;
    color: #0077cc;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 15px;
  }
  th, td {
    border-bottom: 1px solid #ddd;
    padding: 8px;
    text-align: left;
    font-size: 14px;
  }
  th {
    background: #f9f9f9;
  }
  .close-btn {
    background: #dc3545;
    color: #fff;
    border: none;
    padding: 6px 12px;
    border-radius: 6px;
    cursor: pointer;
    float: right;
  }
</style>
</head>
<body>

<header>
    
<header>
  <div class="clock-container">
    <button id="editClock">📢</button>
    <span id="countdown">00:00:00</span>
  </div>
</header>

  🎯  Playmates
  <div id="ranking"></div>
  <button class="results-btn" onclick="openResults()">📊 Resultados</button>
  <!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Painel de Pedidos</title>
<style>
  * {
    box-sizing: border-box;
  }
  body {
    font-family: "Poppins", sans-serif;
    margin: 0;
    background: #f3f6f9;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    min-height: 100vh;
    padding: 20px;
  }
  .card {
    background: #fff;
    width: 100%;
    max-width: 430px;
    border-radius: 16px;
    box-shadow: 0 5px 18px rgba(0,0,0,0.1);
    padding: 20px;
  }
  h3 {
    margin-top: 0;
    color: #333;
    text-align: center;
  }
  .form-row {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  input {
    padding: 10px;
    font-size: 16px;
    border: 1.5px solid #ccc;
    border-radius: 8px;
    width: 100%;
    outline: none;
    transition: border 0.3s;
  }
  input:focus {
    border-color: #4CAF50;
  }
  .pack-choices {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .pack-option {
    padding: 10px;
    border: 2px solid #ccc;
    border-radius: 10px;
    cursor: pointer;
    text-align: center;
    transition: all 0.3s;
  }
  .pack-option:hover {
    border-color: #4CAF50;
  }
  .pack-option.selected {
    background: Orange;
    color: #fff;
    border-color: #00F8FF;
  }
  .btn {
    background: Orange;
    color: #fff;
    border: none;
    flex: 1;
    padding: 10px;
    border-radius: 10px;
    font-size: 16px;
    cursor: pointer;
    transition: 0.3s;
  }
  .btn:hover {
    background: #43a047;
  }
  .simulator {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #e8f5e9;
    padding: 10px 14px;
    border-radius: 10px;
  }
  .sim-number {
    font-size: 24px;
    font-weight: bold;
    color: #2e7d32;
    cursor: pointer;
  }
  .muted {
    font-size: 12px;
    color: #666;
  }
  #adminPanel {
    display: none;
    background: #ffffff;
    border-radius: 12px;
    margin-top: 20px;
    padding: 15px;
    box-shadow: 0 5px 18px rgba(0,0,0,0.1);
  }
  .pedido-card {
    background: #f9f9f9;
    border-radius: 10px;
    padding: 10px 14px;
    margin-bottom: 10px;
    border-left: 4px solid #4CAF50;
    position: relative;
  }
  .pedido-card h4 {
    margin: 0;
    color: #333;
  }
  .pedido-card small {
    color: #666;
  }
  .delete-btn {
    position: absolute;
    right: 10px;
    top: 8px;
    background: none;
    border: none;
    color: #d32f2f;
    font-size: 20px;
    cursor: pointer;
  }
  .close-admin {
    background: #d32f2f;
    color: #fff;
    border: none;
    padding: 8px 12px;
    border-radius: 8px;
    cursor: pointer;
    margin-top: 10px;
  }
  .close-admin:hover {
    background: #b71c1c;
  }
  @media (max-width: 480px) {
    .card {
      padding: 16px;
    }
    input, .btn, .pack-option {
      font-size: 15px;
    }
  }
</style>
</head>
<body>

<div class="card">
  <h3>Enviar Pedido de Participação</h3>
  <div class="form-row">
    <input id="inputName" type="text" placeholder="Nome completo" />
    <input id="inputWhats" type="tel" placeholder="WhatsApp (ex: 941530467)" />
  </div>
  <div style="margin-top:8px">
    <div class="pack-choices">
      <div class="pack-option selected" data-pack="Começar gratuito 20S e ganhar 450kz">1. Começar gratuito → Win 450kz</div>
      <div class="pack-option" data-pack="Começar 500 e ganhar 2000">2. Começar com 500kz → Win 2000</div>
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

<!-- Painel Admin -->
<div id="adminPanel">
  <h3>📋 Lista de Pedidos</h3>
  <div id="listaPedidos"></div>
  <button class="close-admin" id="closeAdminBtn">Fechar Painel</button>
</div>

<script>
  const inputName = document.getElementById("inputName");
  const inputWhats = document.getElementById("inputWhats");
  const sendRequestBtn = document.getElementById("sendRequestBtn");
  const simNumber = document.getElementById("simNumber");
  const packOptions = document.querySelectorAll(".pack-option");
  const adminPanel = document.getElementById("adminPanel");
  const listaPedidos = document.getElementById("listaPedidos");
  const closeAdminBtn = document.getElementById("closeAdminBtn");

  // Recuperar pedidos anteriores
  let pedidos = JSON.parse(localStorage.getItem("pedidos")) || [];
  simNumber.textContent = pedidos.length;

  // Seleção de pacotes
  packOptions.forEach(opt => {
    opt.addEventListener("click", () => {
      packOptions.forEach(o => o.classList.remove("selected"));
      opt.classList.add("selected");
    });
  });

  // Enviar pedido
  sendRequestBtn.addEventListener("click", () => {
    const nome = inputName.value.trim();
    const whats = inputWhats.value.trim();
    const pack = document.querySelector(".pack-option.selected").dataset.pack;

    if (!nome || !whats) {
      alert("Por favor, preencha todos os campos.");
      return;
    }

    if (pedidos.some(p => p.nome.toLowerCase() === nome.toLowerCase())) {
      alert("Este nome já enviou um pedido!");
      return;
    }

    const novoPedido = {
      nome,
      whats,
      pack,
      hora: new Date().toLocaleString()
    };

    pedidos.push(novoPedido);
    localStorage.setItem("pedidos", JSON.stringify(pedidos));

    simNumber.textContent = pedidos.length;

    inputName.value = "";
    inputWhats.value = "";

    alert("✅ Pedido enviado com sucesso!");
  });

  // Mostrar painel admin (senha A3)
  simNumber.addEventListener("click", () => {
    const senha = prompt("Digite a senha de acesso:");
    if (senha === "A3") {
      atualizarLista();
      adminPanel.style.display = "block";
      window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
    } else if (senha) {
      alert("Senha incorreta!");
    }
  });

  // Fechar painel admin
  closeAdminBtn.addEventListener("click", () => {
    adminPanel.style.display = "none";
  });

  // Atualizar lista visual dos pedidos
  function atualizarLista() {
    listaPedidos.innerHTML = "";
    if (pedidos.length === 0) {
      listaPedidos.innerHTML = "<p style='color:#777;text-align:center;'>Nenhum pedido registrado.</p>";
      return;
    }

    pedidos.forEach((p, index) => {
      const card = document.createElement("div");
      card.classList.add("pedido-card");
      card.innerHTML = `
        <button class="delete-btn" title="Eliminar" data-index="${index}">❌</button>
        <h4>${p.nome}</h4>
        <small>📱 ${p.whats}</small><br>
        <small>🎁 ${p.pack}</small><br>
        <small>🕒 ${p.hora}</small>
      `;
      listaPedidos.appendChild(card);
    });

    // Botões de eliminar
    document.querySelectorAll(".delete-btn").forEach(btn => {
      btn.addEventListener("click", () => {
        const idx = btn.getAttribute("data-index");
        if (confirm(`Deseja eliminar o pedido de "${pedidos[idx].nome}"?`)) {
          pedidos.splice(idx, 1);
          localStorage.setItem("pedidos", JSON.stringify(pedidos));
          simNumber.textContent = pedidos.length;
          atualizarLista();
        }
      });
    });
  }
</script>

</body>
</html>

</header>

<div id="contestants"></div>

<div id="zoomView" onclick="this.style.display='none'">
  <img id="zoomImg" src="">
</div>

<div id="resultsPanel">
  <button class="close-btn" onclick="closeResults()">Fechar</button>
  <h2>🗂️ Painel de Resultados</h2>
  <table id="resultsTable">
    <thead><tr><th>Nome</th><th>Concorrente</th><th>Hora</th></tr></thead>
    <tbody></tbody>
  </table>
</div>

<script>


const contestants = [
  {id:1, name:"Concorrente 1", votes:0, school:"Escola A"},
  {id:2, name:"Concorrente 2", votes:0, school:"Escola B"},
  {id:3, name:"Concorrente 3", votes:0, school:"Escola C"}
];

// --- CRIA CARTÕES ---
function createCard(c){
  const div=document.createElement("div");
  div.className="container";
  div.innerHTML=`
    <div class="photo-container">
      <img id="photo${c.id}" src="" alt="foto" onclick="zoomPhoto(${c.id})">
    </div>
    <div class="info">
      <strong id="name${c.id}">${c.name}</strong><br>
      <span id="school${c.id}">${c.school}</span><br>
      <b>Votos:</b> <span id="votes${c.id}">${c.votes}</span>
    </div>
    <button class="edit-btn" onclick="editAccess(${c.id})">⚫</button>
    <div id="edit${c.id}" style="display:none;">
      <input type="text" id="ename${c.id}" placeholder="Nome">
      <input type="text" id="eschool${c.id}" placeholder="Escola">
      <input type="number" id="evotes${c.id}" placeholder="Votos">
      <input type="text" id="eimg${c.id}" placeholder="Caminho da imagem ex: html/DP.png">
      <button class="save-btn" onclick="saveData(${c.id})">Salvar</button>
    </div>
    <button class="vote-btn" onclick="vote(${c.id})">Votar</button>
  `;
  document.getElementById("contestants").appendChild(div);
}

// --- SENHA DE EDIÇÃO ---
function editAccess(id){
  const senha=prompt("Digite a senha para editar:");
  if(senha==="5A") toggleEdit(id);
  else alert("Senha incorreta!");
}

// --- MOSTRA/OCULTA EDIÇÃO ---
function toggleEdit(id){
  const box=document.getElementById("edit"+id);
  box.style.display=box.style.display==="none"?"block":"none";
  document.getElementById("ename"+id).value=document.getElementById("name"+id).innerText;
  document.getElementById("eschool"+id).value=document.getElementById("school"+id).innerText;
  document.getElementById("evotes"+id).value=document.getElementById("votes"+id).innerText;
  document.getElementById("eimg"+id).value=document.getElementById("photo"+id).src;
}

// --- SALVAR DADOS ---
function saveData(id){
  const n=document.getElementById("ename"+id).value.trim();
  const s=document.getElementById("eschool"+id).value.trim();
  const v=parseInt(document.getElementById("evotes"+id).value)||0;
  const imgSrc=document.getElementById("eimg"+id).value.trim();

  if(n) document.getElementById("name"+id).innerText=n;
  if(s) document.getElementById("school"+id).innerText=s;
  document.getElementById("votes"+id).innerText=v;

  if(imgSrc){
    document.getElementById("photo"+id).src=imgSrc;
    localStorage.setItem("photo"+id,imgSrc);
  }

  localStorage.setItem("name"+id,n);
  localStorage.setItem("school"+id,s);
  localStorage.setItem("votes"+id,v);

  toggleEdit(id);
  updateRanking();
}

// --- AMPLIAR FOTO ---
function zoomPhoto(id){
  const src=document.getElementById("photo"+id).src;
  if(src){
    document.getElementById("zoomImg").src=src;
    document.getElementById("zoomView").style.display="flex";
  }
}

// --- REGISTRAR VOTO ---
function vote(id){
  const nome=prompt("Digite o seu nome:");
  if(!nome) return;
  const codigo=prompt("Digite o seu código de votação:");
  if(!codigo) return;
  const concorrente=document.getElementById("name"+id).innerText;
  const hora=new Date().toLocaleString();
  const registro={nome,concorrente,hora};
  const votos=JSON.parse(localStorage.getItem("registros")||"[]");
  votos.push(registro);
  localStorage.setItem("registros",JSON.stringify(votos));
  const atual=parseInt(localStorage.getItem("votes"+id)||0)+1;
  localStorage.setItem("votes"+id,atual);
  document.getElementById("votes"+id).innerText=atual;
  updateRanking();
  alert(`Voto registado para ${concorrente} às ${hora}`);
}

// --- RANKING ---
function updateRanking(){
  let top={name:"",school:"",votes:-1};
  contestants.forEach(c=>{
    const name=localStorage.getItem("name"+c.id)||c.name;
    const votes=parseInt(localStorage.getItem("votes"+c.id)||c.votes);
    if(votes>top.votes){ top={name,school:c.school,votes}; }
  });
  document.getElementById("ranking").innerText=
    top.votes>=0 ? `Mais votado: ${top.name} (${top.votes} votos)` : "";
}

// --- PAINEL DE RESULTADOS ---
function openResults(){
  const senha=prompt("Digite a senha para abrir resultados:");
  if(senha!=="5A"){ alert("Senha incorreta!"); return; }
  const tbody=document.querySelector("#resultsTable tbody");
  tbody.innerHTML="";
  const registros=JSON.parse(localStorage.getItem("registros")||"[]");
  registros.forEach(r=>{
    const tr=document.createElement("tr");
    tr.innerHTML=`<td>${r.nome}</td><td>${r.concorrente}</td><td>${r.hora}</td>`;
    tbody.appendChild(tr);
  });
  document.getElementById("resultsPanel").style.display="block";
}
function closeResults(){ document.getElementById("resultsPanel").style.display="none"; }

// --- CARREGAR ---
function loadData(){
  contestants.forEach(c=>{
    createCard(c);
    const n=localStorage.getItem("name"+c.id);
    const s=localStorage.getItem("school"+c.id);
    const v=localStorage.getItem("votes"+c.id);
    const p=localStorage.getItem("photo"+c.id);
    if(n) document.getElementById("name"+c.id).innerText=n;
    if(s) document.getElementById("school"+c.id).innerText=s;
    if(v) document.getElementById("votes"+c.id).innerText=v;
    if(p) document.getElementById("photo"+c.id).src=p;
  });
  updateRanking();
}
loadData();
/* ======= LOCALSTORAGE SETUP ======= */

let countdownData = JSON.parse(localStorage.getItem("countdownData")) || {time: 0, start: null};

/* ======= RELÓGIO ======= */
let countdownEl = document.getElementById("countdown");
let timer;

function atualizarRelogio() {
  if (!countdownData.start || countdownData.time <= 0) {
    countdownEl.textContent = "GAME OVER";
    return;
  }
  let now = Date.now();
  let diff = Math.max(0, countdownData.time - (now - countdownData.start));
  if (diff <= 0) {
    countdownEl.textContent = "GAME OVER";
    localStorage.setItem("countdownData", JSON.stringify({time: 0, start: null}));
    clearInterval(timer);
    return;
  }
  let h = Math.floor(diff / 3600000);
  let m = Math.floor((diff % 3600000) / 60000);
  let s = Math.floor((diff % 60000) / 1000);
  countdownEl.textContent = `${String(h).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
}

function iniciarContagem() {
  clearInterval(timer);
  timer = setInterval(atualizarRelogio, 1000);
}

document.getElementById("editClock").onclick = function() {
  const senha = prompt("Digite a senha do relógio:");
  if (senha !== "A8") return alert("Senha incorreta!");
  let horas = parseInt(prompt("Horas:")) || 0;
  let minutos = parseInt(prompt("Minutos:")) || 0;
  let segundos = parseInt(prompt("Segundos:")) || 0;
  let totalMs = ((horas * 3600) + (minutos * 60) + segundos) * 1000;
  countdownData = {time: totalMs, start: Date.now()};
  localStorage.setItem("countdownData", JSON.stringify(countdownData));
  iniciarContagem();
};

if (countdownData.start && countdownData.time > 0) iniciarContagem();

</script>

 <!-- RIGHT -->
  <aside>
   
   
        <div style="font-weight:800">Diogo paixão</div>
        <div class="muted" style="margin-top:4px"><b>Cargo: Fundador & <b>CEO</b></b></div>
        <style>
            b{color:Blue;}
        </style>
        <div style="margin-top:8px;font-size:14px">Eu faço 25$ por mês na internet aqui em Angola .</div>
        <a id="waBtn" class="" href="https://wa.me/244941530467" target="_blank" style="display:inline-block;margin-top:8px;background:#25D366;color:#fff;padding:8px 10px;border-radius:8px;text-decoration:none;font-weight:700">Iniciar WhatsApp — 941530467</a>
        <div style="margin-top:8px;display:flex;gap:8px">
            <style>  
</style>
  <h2>Sobre o Playmates</h2>
  <p>
    O Playmates foi criado por <strong>Diogo Paixão</strong> aos 17 anos e lançado em 2025.  
    É uma plataforma pioneira em Angola que transforma votos em oportunidades financeiras.  
    Já alcançou mais de 20 escolas do ensino médio, permitindo que estudantes ganhem recompensas de forma confiável e segura.  
    É mais que uma disputa, é um caminho de empreendedorismo para jovens angolanos. Estimula liderança, comunicação e captação de apoios nas comunidades escolares.
  </p>
