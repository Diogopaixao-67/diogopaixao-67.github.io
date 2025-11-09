
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
    <div class="muted">Clique no número para ver </div>
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

   
  
 
 
<html lang="pt">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Foto em círculo — Zoom CSS-only</title>
<style>
  :root{
    --thumb-size: 180px;      /* tamanho do círculo na página */
    --zoom-max: 90vw;        /* largura máxima do zoom (responsivo) */
    --zoom-max-h: 90vh;      /* altura máxima do zoom */
    --bg-overlay: rgba(0,0,0,0.6);
    --transition: 400ms cubic-bezier(.2,.9,.3,1);
  }

  /* reset simples */
  *{box-sizing:border-box;margin:0;padding:0}

  body{
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    background: linear-gradient(180deg,#fff,#f3f7fb);
    font-family:system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;
    padding:2rem;
  }

  .wrap{
    text-align:center;
  }

  /* checkbox escondido (hack) */
  #zoomToggle{position:absolute;left:-9999px;opacity:0;}

  /* estilo do rótulo que contém a imagem (clicável) */
  label.photo-label{
    display:inline-block;
    cursor:pointer;
    user-select:none;
    -webkit-tap-highlight-color: transparent;
  }

  .photo{
    width:var(--thumb-size);
    height:var(--thumb-size);
    border-radius:50%;
    overflow:hidden;
    display:block;
    box-shadow: 0 6px 18px rgba(0,0,0,0.15);
    transition: transform var(--transition), box-shadow var(--transition);
    transform-origin:center center;
  }

  .photo img{
    width:100%;
    height:100%;
    display:block;
    object-fit:cover; /* garante que a foto preencha o círculo sem distorcer */
    -webkit-user-drag:none;
  }

  /* pequena dica de clique */
  .hint{
    margin-top:.6rem;
    font-size:.9rem;
    color:#555;
  }

  /* quando o checkbox está checked -> ativar overlay e ampliar a foto */
  #zoomToggle:checked ~ .overlay{
    pointer-events:auto;
    opacity:1;
    visibility:visible;
  }

  /* overlay full-screen */
  .overlay{
    position:fixed;
    inset:0;
    display:flex;
    align-items:center;
    justify-content:center;
    background:linear-gradient(to bottom, rgba(0,0,0,0.55), rgba(0,0,0,0.55));
    opacity:0;
    visibility:hidden;
    transition:opacity var(--transition);
    z-index:90;
    padding:2vh;
    pointer-events:none; /* bloqueado até checked */
  }

  /* versão ampliada da imagem */
  #zoomToggle:checked ~ .overlay .photo{
    width: min(var(--zoom-max), 1200px);
    height: min(var(--zoom-max-h), 1200px);
    border-radius: 12px; /* muda de círculo para cartão arredondado quando em zoom */
    box-shadow: 0 30px 80px rgba(0,0,0,0.6);
    transition: all var(--transition);
    transform: translateY(0);
  }

  /* fechar ao clicar no fundo: fazemos um label que liga ao checkbox (mesmo for) */
  .overlay .close-area{
    position:absolute;
    inset:0;
  }

  /* botão visível de fechar (opcional) */
  .close-btn{
    position:fixed;
    top:18px;
    right:18px;
    z-index:110;
    font-size:20px;
    background: rgba(255,255,255,0.95);
    border-radius:10px;
    padding:6px 10px;
    box-shadow:0 6px 18px rgba(0,0,0,0.15);
    cursor:pointer;
  }

  /* media queries para ajustar o tamanho do thumb em telas pequenas */
  @media (max-width:420px){
    :root{--thumb-size:140px}
    .hint{font-size:.85rem}
  }
</style>
</head>
<body>
  <div class="wrap">
    <!-- input checkbox (controla o estado: normal / zoom) -->
    <input type="checkbox" id="zoomToggle" aria-hidden="true">

    <!-- rótulo que exibe o círculo (clicar alterna o checkbox) -->
    <label for="zoomToggle" class="photo-label" aria-hidden="false" aria-controls="zoomToggle" title="Clique para ampliar">
      <div class="photo" role="img" aria-label="Minhas fotos">
        <!-- <-- Substitua o src abaixo pelo caminho da sua imagem.
             Exemplo: src="./IMG-20251022-WA0007.jpg" ou o caminho no GitHub sem espaços. -->
        <img src="./Imagens/IMG-20251022-WA0007.jpg" alt="Foto dentro do círculo">
      </div>
    </label>

    <div class="hint">Clique na foto para ampliar — clique novamente para fechar</div>

    <!-- overlay (aparece quando #zoomToggle está checked) -->
    <div class="overlay" aria-hidden="true">
      <!-- área de fundo (clicar fecha porque é um label ligado ao mesmo input) -->
      <label for="zoomToggle" class="close-area" title="Fechar"></label>

      <!-- versão ampliada (mantemos a mesma estrutura .photo com a mesma <img>) -->
      <div style="z-index:100;position:relative;">
        <div class="photo" role="img" aria-label="Foto ampliada">
          <!-- a mesma imagem: é carregada duas vezes pelo browser, evita-se JS. -->
          <img src="./Imagens/IMG-20251022-WA0007.jpg" alt="Foto ampliada">
        </div>
      </div>

      <!-- botão visível de fechar (opcional): também é um label que desmarca o checkbox -->
      <label for="zoomToggle" class="close-btn" aria-hidden="false" title="Fechar">✕</label>
    </div>
  </div>
</body>
</html>

   

      
  <h2>Sobre o Playmates</h2>

  <p>
    O Playmates foi criado por <strong>Diogo Paixão</strong> aos 17 anos e lançado em 2025.  
    É uma plataforma pioneira em Angola que transforma votos em oportunidades financeiras.  
    Já alcançou mais de 20 escolas do ensino médio, permitindo que estudantes ganhem recompensas de forma confiável e segura.  
    É mais que uma disputa, é um caminho de empreendedorismo para jovens angolanos. Estimula liderança, comunicação e captação de apoios nas comunidades escolares.
  </p>
  
<html lang="pt">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Ranking - Plataforma</title>
  <style>
    :root{
      --bg:#0b1220;
      --card:#071028;
      --muted:rgba(255,255,255,0.06);
      --accent:#06b6d4;
      --accent-2:#7c3aed;
      --glass: rgba(255,255,255,0.03);
      --text:#e6eef8;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
   
          margin:0; padding:18px; font-family:Inter, system-ui, Arial, sans-serif;
    background: linear-gradient(180deg,#eef6ff 0%, #fbfdff 100%);
    color:#0b1220;
    }

    .card{
      width:100%;
      max-width:860px;
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:14px;
      padding:18px;
      border:1px solid var(--glass);
    }

    .header{display:flex;align-items:center;gap:12px;margin-bottom:14px}
    .icon{font-size:22px;display:inline-grid;place-items:center;width:46px;height:46px;border-radius:10px;background:linear-gradient(180deg,var(--accent-2),#5b21b6);box-shadow:0 8px 24px rgba(124,58,237,0.14)}
    .title{display:flex;flex-direction:column}
    .title h3{margin:0;font-size:18px}
    .title small{opacity:0.75}

    .controls{margin-left:auto;display:flex;gap:8px;align-items:center}
    .pwd-btn{background:transparent;border:1px dashed var(--muted);padding:8px 12px;border-radius:10px;color:var(--text);cursor:pointer;font-weight:600}
    .pwd-btn:hover{border-style:solid}
    .edit-indicator{padding:6px 10px;border-radius:10px;background:linear-gradient(90deg,var(--accent),#0ea5a2);font-weight:700}

    .list{display:grid;grid-template-columns:1fr;gap:12px;margin-top:8px}
    .row{display:flex;align-items:center;gap:12px;padding:12px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.007));border:1px solid rgba(255,255,255,0.02)}

    .photo{width:60px;height:60px;border-radius:50%;overflow:hidden;flex-shrink:0;cursor:pointer;border:2px solid var(--accent-2);transition:transform .2s}
    .photo img{width:100%;height:100%;object-fit:cover}
    .photo:hover{transform:scale(1.05)}

    .place{width:40px;height:40px;border-radius:10px;display:grid;place-items:center;font-weight:800;font-size:18px}
    .place.gold{background:linear-gradient(180deg,#ffd54a,#f59e0b);color:#10221a}
    .place.silver{background:linear-gradient(180deg,#e6e6e6,#9ca3af);color:#071224}
    .place.bronze{background:linear-gradient(180deg,#f3cc9a,#c084fc);color:#071224}

    .info{flex:1;display:flex;flex-direction:column}
    .name{font-weight:700}
    .school{opacity:0.8;font-size:13px}
    .score{min-width:86px;text-align:right;font-weight:800}

    .edit-mode{margin-top:12px;padding:12px;border-radius:10px;border:1px dashed var(--muted);background:linear-gradient(180deg, rgba(124,58,237,0.04), rgba(6,182,212,0.03));display:flex;flex-direction:column;gap:8px}
    .edit-row{display:flex;gap:8px;align-items:center}
    input[type=text], input[type=number]{flex:1;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:var(--text)}
    .save-btn{padding:10px 14px;border-radius:10px;border:0;background:linear-gradient(90deg,var(--accent),#0ea5a2);cursor:pointer;color:#042022;font-weight:800}
    .cancel-btn{padding:10px 14px;border-radius:10px;border:0;background:transparent;color:var(--text);cursor:pointer}

    .pwd-overlay{position:fixed;inset:0;display:grid;place-items:center;background:rgba(0,0,0,0.8);visibility:hidden;opacity:0;transition:opacity .2s ease,visibility .2s;z-index:999}
    .pwd-overlay.show{visibility:visible;opacity:1}

    /* Zoom circular */
    .zoom-box{
      position:relative;
      border-radius:50%;
      overflow:hidden;
      width:300px;
      height:300px;
      display:flex;
      align-items:center;
      justify-content:center;
      background:rgba(255,255,255,0.05);
      box-shadow:0 0 30px rgba(0,0,0,0.7)
    }
    .zoom-box img{
      width:100%;
      height:100%;
      object-fit:cover;
      border-radius:50%;
    }
    .close-btn{
      position:absolute;
      top:-10px;
      right:-10px;
      background:#ff4444;
      color:white;
      border:none;
      border-radius:50%;
      width:32px;
      height:32px;
      font-size:18px;
      font-weight:bold;
      cursor:pointer;
      box-shadow:0 2px 6px rgba(0,0,0,0.3)
    }

    @media (max-width:560px){
      .row{padding:10px}
      .photo{width:50px;height:50px}
      .zoom-box{width:220px;height:220px}
    }
  </style>
</head>
<body>
  <div class="card">
    <div class="header">
 
      <div class="title">
        <h3>Ranking Oficial</h3>
<mark>Aqui você encontra os alunos de diferentes escolas que estão no ranking atualizado de hoje considerados os populares do momento no <b>Playmates</b></mark>
    <div class="list" id="rankingList">
      <div class="row">
        <div class="place gold">1</div>
        <div class="photo" onclick="showZoom('HTML/IMG-20251022-WA0007.jpg')">
          <img src="HTML/IMG-20251022-WA0007.jpg">
        </div>
        <div class="info">
          <div class="name">Diogo paixao</div>
          <div class="school">Escola Central</div>
        </div>
        <div class="score">985</div>
      </div>

      <div class="row">
        <div class="place silver">2</div>
        <div class="photo" onclick="showZoom('HTML/IMG-20251022-WA0008.jpg')">
          <img src="HTML/IMG-20251022-WA0008.jpg">
        </div>
        <div class="info">
          <div class="name">Bruno Costa</div>
          <div class="school">Colégio São José</div>
        </div>
        <div class="score">942</div>
      </div>

      <div class="row">
        <div class="place bronze">3</div>
        <div class="photo" onclick="showZoom('HTML/IMG-20251022-WA0009.jpg')">
          <img src="HTML/IMG-20251022-WA0009.jpg">
        </div>
        <div class="info">
          <div class="name">Carla Silva</div>
          <div class="school">Liceu Sul</div>
        </div>
        <div class="score">912</div>
      </div>
    </div>

    <div id="editPanel" class="edit-mode" style="display:none">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <strong>Modo de edição</strong>
        <div>
          <button id="saveAll" class="save-btn">Guardar</button>
          <button id="cancelEdit" class="cancel-btn">Cancelar</button>
        </div>
      </div>
      <div id="editRows"></div>
    </div>
  </div>

  <!-- Zoom -->
  <div class="pwd-overlay" id="zoomOverlay">
    <div class="zoom-box" id="zoomBox"></div>
  </div>

  <!-- Senha -->
  <div class="pwd-overlay" id="pwdOverlay">
    <div class="zoom-box" style="background:#07162a;padding:20px;border-radius:12px;max-width:400px;width:auto;height:auto">
      <h3>Senha de edição</h3>
      <input id="pwdInput" type="password" placeholder="Senha" style="width:100%;padding:10px;border-radius:8px;margin:10px 0;background:transparent;color:var(--text)">
      <div id="pwdMsg" style="color:#fca5a5;height:18px"></div>
      <button id="pwdSubmit" class="save-btn">Confirmar</button>
    </div>
  </div>

  <script>
    const PASS='AX';
    const editPanel=document.getElementById('editPanel');
    const editBadge=document.getElementById('editBadge');
    const pwdOverlay=document.getElementById('pwdOverlay');
    const pwdInput=document.getElementById('pwdInput');
    const pwdSubmit=document.getElementById('pwdSubmit');
    const pwdMsg=document.getElementById('pwdMsg');

    document.getElementById('openPwd').onclick=()=>{
      pwdOverlay.classList.add('show');
      pwdInput.value='';
      pwdMsg.textContent='';
    };

    pwdSubmit.onclick=()=>{
      if(pwdInput.value===PASS){
        pwdOverlay.classList.remove('show');
        enterEdit();
      } else {
        pwdMsg.textContent='Senha incorreta';
      }
   };
    function enterEdit(){
      editPanel.style.display='block';
      editBadge.style.display='block';
    }

    document.getElementById('cancelEdit').onclick=()=>{
      editPanel.style.display='none';
      editBadge.style.display='none';
    };

    // Zoom circular com botão X
    const zoomOverlay=document.getElementById('zoomOverlay');
    const zoomBox=document.getElementById('zoomBox');
    function showZoom(src){
      zoomOverlay.classList.add('show');
      zoomBox.innerHTML=`
        <button class='close-btn' id='closeZoom'>×</button>
        <img src='${src}' alt='Foto'>
      `;
      document.getElementById('closeZoom').onclick=()=>zoomOverlay.classList.remove('show');
      zoomOverlay.onclick=(e)=>{if(e.target===zoomOverlay)zoomOverlay.classList.remove('show');};
    }
  </script>
  <!doctype html>
<html lang="pt-PT">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Playmates — Plataforma</title>
<!-- Lucide icons CDN -->
<link rel="preload" href="https://unpkg.com/lucide@0.263.0/dist/lucide.min.js" as="script">
<style>
  :root{
    --bg:#f6f7fb; --card:#ffffff; --muted:#6b7280; --accent:#ff7b00; --accent-2:#ff9a3d;
    --glass: rgba(255,255,255,0.8);
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
  .card{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 20px rgba(16,24,40,0.04) }
  /* big counter */
  .counter-box{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:18px;border-radius:12px;background:linear-gradient(180deg,#fff,#fff);text-align:center}
  .counter-box h2{font-size:20px;color:var(--accent);margin-bottom:6px}
  .counter-box p{color:var(--muted);font-size:13px}
  /* profile */
  .profile-top{display:flex;gap:12px;align-items:center}
  .avatar{width:88px;height:88px;border-radius:999px;overflow:hidden;flex:0 0 88px;background:#f3f5f8;border:3px solid rgba(255,123,0,0.12)}
  .avatar img{width:100%;height:100%;object-fit:cover;display:block}
  .pinfo h2{font-size:16px;margin-bottom:4px}
  .small{color:var(--muted);font-size:13px}
  .tabs{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap}
  .tab{padding:8px 10px;border-radius:10px;cursor:pointer;font-weight:600;color:var(--muted);background:transparent}
  .tab.active{background:linear-gradient(90deg,rgba(255,123,0,0.12),rgba(255,154,61,0.06));color:var(--accent)}
  /* search */
  .searchbar{display:flex;gap:8px;align-items:center}
  .searchbar input{flex:1;padding:10px;border-radius:10px;border:1px solid rgba(11,18,34,0.06);background:transparent}
  .results{margin-top:12px;display:grid;gap:8px}
  .result{display:flex;gap:10px;align-items:center;padding:10px;border-radius:10px;background:linear-gradient(180deg,#fff,#fbfcff);cursor:pointer}
  .result .mini{width:56px;height:56px;border-radius:999px;overflow:hidden;background:#f2f6ff;flex:0 0 56px}
  .result .mini img{width:100%;height:100%;object-fit:cover}
  .rinfo{flex:1}
  .badge{font-size:12px;padding:6px 8px;border-radius:999px;background:rgba(11,18,34,0.04);color:var(--muted);display:inline-block}
  /* view panel */
  .view-panel{margin-top:12px;padding:12px;border-radius:10px;background:linear-gradient(180deg,#fff,#fbfcff)}
  .progress-wrap{height:12px;background:rgba(11,18,34,0.06);border-radius:10px;overflow:hidden}
  .progress{height:100%;width:0%;background:linear-gradient(90deg,var(--accent),var(--accent-2));transition:width 1s ease}
  .notify-list{margin-top:10px;display:flex;flex-direction:column;gap:8px}
  .notify{padding:10px;border-radius:10px;background:linear-gradient(90deg,rgba(255,123,0,0.08),rgba(255,154,61,0.04));font-size:13px}
  /* forms */
  label{display:block;font-size:13px;margin-bottom:6px;color:var(--muted)}
  input[type="text"],input[type="password"],input[type="tel"],select,textarea{padding:10px;border-radius:10px;border:1px solid rgba(11,18,34,0.06);width:100%;display:block}
  input[type="file"]{padding:6px}
  textarea{min-height:80px;resize:vertical}
  .muted-small{font-size:12px;color:var(--muted);margin-top:6px}
  /* post list */
  .post{display:flex;gap:10px;align-items:flex-start;padding:10px;border-radius:10px;background:#fff}
  .post img{width:86px;height:86px;border-radius:10px;object-fit:cover}
  .post .meta{flex:1}
  /* small */
  .row{display:flex;gap:8px;align-items:center}
  .tiny{font-size:12px;color:var(--muted)}
  .center{display:flex;align-items:center;justify-content:center}
</style>
</head>
<body>
  <div class="app">
    <header>
      <h1>Playmates</h1>
      <div class="top-actions">
        <button id="btnOpenAuth"><i data-feather="log-in"></i> Entrar / Cadastrar</button>
        <button id="btnDemo" class="ghost"><i data-feather="user"></i> Conta Demo</button>
      </div>
    </header>

    <main>
      <!-- left column -->
      <div class="card" id="leftCol">
        <div id="authArea">
          <h3>Bem-vindo ao Playmates</h3>
          <div class="muted-small">Faça login ou crie conta — fotos guardadas localmente no seu navegador.</div>
          <div style="height:10px"></div>

          <div id="loginForm">
            <label>Telemóvel</label>
            <input id="loginPhone" type="tel" placeholder="ex: 922000000" />
            <label>Senha</label>
            <input id="loginPass" type="password" placeholder="Senha" />
            <div class="row" style="margin-top:10px">
              <button id="loginSubmit"><i data-feather="log-in"></i> Entrar</button>
              <button id="showRegister" class="ghost"><i data-feather="user-plus"></i> Criar conta</button>
            </div>
            <div class="muted-small" style="margin-top:8px">Se errar 3 vezes a conta fica bloqueada.</div>
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
              <button id="regSubmit"><i data-feather="check-circle"></i> Criar conta</button>
              <button id="regCancel" class="ghost"><i data-feather="chevrons-left"></i> Voltar</button>
            </div>
          </div>
        </div>

        <!-- when logged -->
        <div id="loggedArea" style="display:none">
          <div class="profile-top">
            <div class="avatar" id="leftAvatar"><img src="" alt="avatar"/></div>
            <div class="pinfo">
              <h2 id="leftName">—</h2>
              <div class="small" id="leftSchool">—</div>
              <div class="small" id="leftPhone">—</div>
              <div style="height:8px"></div>
              <div class="row">
                <button id="btnEditProfile" class="ghost"><i data-feather="edit-3"></i> Editar</button>
                <button id="btnLogout" class="ghost"><i data-feather="log-out"></i> Sair</button>
              </div>
            </div>
          </div>

          <div style="height:12px"></div>
          <div class="tabs">
            <div class="tab active" data-tab="perfil"><i data-feather="user"></i> Perfil</div>
            <div class="tab" data-tab="pesquisa"><i data-feather="search"></i> Pesquisa</div>
            <div class="tab" data-tab="notifs"><i data-feather="bell"></i> Notificações <span id="notifCount" class="tiny"></span></div>
            <div class="tab" data-tab="jogos"><i data-feather="gamepad"></i> Jogos</div>
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
              <div class="muted-small">Notificações aparecem nesta aba quando outros te Topam ou te enviam SMS.</div>
            </div>

            <div data-content="pesquisa" style="display:none">
              <div class="searchbar">
                <input id="searchInput" placeholder="Pesquisar por nome (Enter)" />
                <button id="btnSearch" class="ghost"><i data-feather="search"></i> Pesquisar</button>
              </div>
              <div class="results card" id="results"></div>

              <div id="viewPanel" style="display:none" class="card view-panel">
                <div style="display:flex;gap:12px;align-items:center">
                  <div class="mini" style="width:72px;height:72px;border-radius:999px;overflow:hidden"><img id="viewMini" src="" style="width:100%;height:100%;object-fit:cover" /></div>
                  <div style="flex:1">
                    <div id="viewName" style="font-weight:700"></div>
                    <div class="small" id="viewSchool"></div>
                    <div class="small">Pontos: <span id="viewPoints"></span></div>
                    <div style="height:8px"></div>
                    <div class="row">
                      <button id="btnTopar"><i data-feather="thumbs-up"></i> Topar</button>
                      <button id="btnSendSMS" class="ghost"><i data-feather="message-circle"></i> Enviar SMS</button>
                    </div>
                  </div>
                </div>
                <div style="margin-top:12px">
                  <div class="progress-wrap"><div class="progress" id="viewProgress"></div></div>
                  <div class="tiny muted-small" id="viewProgressText" style="margin-top:6px">Progresso: <span id="viewPercent">0%</span></div>
                </div>
              </div>
            </div>

            <div data-content="notifs" style="display:none" class="view-panel">
              <div class="small">Notificações recentes</div>
              <div id="notifList" class="notify-list"></div>
            </div>

            <div data-content="jogos" style="display:none" class="view-panel">
              <div class="small">Aba Jogos</div>
              <div style="margin-top:8px">🎮 Espaço reservado — podes colar/implementar aqui o teu jogo mais tarde.</div>
            </div>
          </div>
        </div>
      </div>

      <!-- right column -->
      <div>
        <div class="card" id="postsCard">
          <h3><i data-feather="image"></i> Postagens (3 horas)</h3>
          <label>Texto (opcional)</label>
          <textarea id="postText" placeholder="Escreve algo... (max 300)"></textarea>
          <label>Foto (opcional)</label>
          <input id="postImg" type="file" accept="image/*" />
          <div style="height:8px"></div>
          <div class="row">
            <button id="btnPost"><i data-feather="upload-cloud"></i> Publicar</button>
            <button id="btnClearPosts" class="ghost"><i data-feather="trash-2"></i> Limpar expirados</button>
          </div>
          <div id="postsFeed" style="margin-top:12px"></div>
        </div>

        <div class="card" id="smsCard">
          <h3><i data-feather="send"></i> Enviar SMS (130 chars - 3h)</h3>
          <label>Para (número)</label>
          <input id="smsTo" type="tel" placeholder="Número do destinatário" />
          <label>Mensagem (máx 130)</label>
          <textarea id="smsText" maxlength="130" placeholder="Mensagem curta..."></textarea>
          <div style="height:8px"></div>
          <div class="row">
            <button id="btnSendSMSGlobal"><i data-feather="send"></i> Enviar</button>
            <button id="btnClearSMS" class="ghost"><i data-feather="trash"></i> Limpar expirados</button>
          </div>
          <div id="smsFeed" style="margin-top:12px"></div>
        </div>

        <div class="card" id="searchCard">
          <h3><i data-feather="users"></i> Pesquisar usuários</h3>
          <label>Pesquisar por nome</label>
          <input id="quickSearch" placeholder="Digite nome..." />
          <div id="quickResults" style="margin-top:10px"></div>
        </div>
      </div>
    </main>
  </div>

  <!-- Lucide icons -->
  <script src="https://unpkg.com/lucide@0.263.0/dist/lucide.min.js"></script>
  <script>lucide.replace()</script>

<script>
/*
Playmates consolidated single-file app
Storage keys:
 - pm_accounts: array of accounts {name,phone,school,photo,pass,points:Number,progressPercent:int,failedAttempts:int,locked:boolean,notifications:[],posts:[],smsReceived:[]}
 - pm_topados: object {viewerPhone: [targetPhones...]}
 - pm_session: phone of logged in user
*/
(() => {
  // storage helpers
  const load = (k) => { try { return JSON.parse(localStorage.getItem(k) || 'null') } catch(e){ return null } };
  const save = (k,v) => localStorage.setItem(k, JSON.stringify(v));
  if(!load('pm_accounts')) save('pm_accounts', []);
  if(!load('pm_topados')) save('pm_topados', {});
  if(!load('pm_session')) save('pm_session', null);

  // DOM refs
  const loginPhone = document.getElementById('loginPhone'), loginPass = document.getElementById('loginPass'), loginSubmit = document.getElementById('loginSubmit');
  const showRegisterBtn = document.getElementById('showRegister'), regCancel = document.getElementById('regCancel'), regSubmit = document.getElementById('regSubmit');
  const regName = document.getElementById('regName'), regPass = document.getElementById('regPass'), regPhone = document.getElementById('regPhone'), regSchool = document.getElementById('regSchool'), regPhoto = document.getElementById('regPhoto');
  const btnOpenAuth = document.getElementById('btnOpenAuth'), btnDemo = document.getElementById('btnDemo');
  const leftAvatar = document.getElementById('leftAvatar'), leftName = document.getElementById('leftName'), leftSchool = document.getElementById('leftSchool'), leftPhone = document.getElementById('leftPhone'), leftPoints = document.getElementById('leftPoints');
  const loggedArea = document.getElementById('loggedArea'), authArea = document.getElementById('authArea');
  const profilesCount = document.getElementById('profilesCount');
  const tabs = document.querySelectorAll('.tab');
  const tabContainers = document.querySelectorAll('[data-content]');
  const searchInput = document.getElementById('searchInput'), btnSearch = document.getElementById('btnSearch');
  const resultsBox = document.getElementById('results'), viewPanel = document.getElementById('viewPanel');
  const viewMini = document.getElementById('viewMini'), viewName = document.getElementById('viewName'), viewSchool = document.getElementById('viewSchool'), viewPoints = document.getElementById('viewPoints'), viewProgress = document.getElementById('viewProgress'), viewPercent = document.getElementById('viewPercent');
  const btnTopar = document.getElementById('btnTopar'), btnSendSMS = document.getElementById('btnSendSMS');
  const notifList = document.getElementById('notifList'), notifCount = document.getElementById('notifCount');
  const btnEditProfile = document.getElementById('btnEditProfile'), btnLogout = document.getElementById('btnLogout');
  const postText = document.getElementById('postText'), postImg = document.getElementById('postImg'), btnPost = document.getElementById('btnPost'), postsFeed = document.getElementById('postsFeed'), btnClearPosts = document.getElementById('btnClearPosts');
  const smsTo = document.getElementById('smsTo'), smsText = document.getElementById('smsText'), btnSendSMSGlobal = document.getElementById('btnSendSMSGlobal'), smsFeed = document.getElementById('smsFeed'), btnClearSMS = document.getElementById('btnClearSMS');
  const quickSearch = document.getElementById('quickSearch'), quickResults = document.getElementById('quickResults');
  const loginForm = document.getElementById('loginForm'), registerForm = document.getElementById('registerForm');

  // state
  let accounts = load('pm_accounts') || [];
  let topados = load('pm_topados') || {};
  let sessionPhone = load('pm_session') || null;
  let currentViewAccount = null;
  let currentUser = null;
  // constants
  const EXPIRE_MS = 3 * 60 * 60 * 1000; // 3 hours

  // init demo accounts if none
  if(accounts.length === 0){
    accounts = [
      {name:'Ana Silva', phone:'910000001', pass:'1', school:'Escola A', photo: placeholderDataUrl('A'), points:2.5, progressPercent:5, failedAttempts:0, locked:false, notifications:[], posts:[], smsReceived:[]},
      {name:'Bruno Costa', phone:'910000002', pass:'1', school:'Escola B', photo: placeholderDataUrl('B'), points:1.0, progressPercent:2, failedAttempts:0, locked:false, notifications:[], posts:[], smsReceived:[]}
    ];
    save('pm_accounts', accounts);
  }

  // onboarding: wire events
  btnOpenAuth.addEventListener('click', ()=> { window.scrollTo({top:0,behavior:'smooth'}); });
  btnDemo.addEventListener('click', ()=>{
    if(accounts.some(a=>a.phone==='999999999')) return alert('Demo já existe. Login: 999999999 / demo');
    accounts.push({name:'Conta Demo',phone:'999999999',pass:'demo',school:'Demo',photo:placeholderDataUrl('D'),points:0,progressPercent:0,failedAttempts:0,locked:false,notifications:[],posts:[],smsReceived:[]});
    save('pm_accounts',accounts); renderCounts(); alert('Demo criada: 999999999 / demo');
  });

  // show register form
  showRegisterBtn.addEventListener('click', ()=>{ loginForm.style.display='none'; registerForm.style.display='block'; });
  regCancel.addEventListener('click', ()=>{ registerForm.style.display='none'; loginForm.style.display='block'; });

  // register
  regSubmit.addEventListener('click', async ()=>{
    const name = regName.value.trim(), pass = regPass.value, phone = regPhone.value.trim(), school = regSchool.value.trim();
    if(!name || !pass || !phone){ return alert('Preencha nome, senha e telemóvel'); }
    if(accounts.some(a=>a.phone===phone)) return alert('Já existe conta com esse telemóvel');
    // photo
    let photoData = placeholderDataUrl(name);
    if(regPhoto.files && regPhoto.files[0]){ try { photoData = await fileToDataUrl(regPhoto.files[0]); } catch(e){} }
    const welcomeMsg = "Olá eu sou Diogo paixão CEO & fundador do Playmates espero que tenhas uma ótima experiência juvenil na plataforma e consigas fazer dinheiro online.";
    const newAcc = {name, pass, phone, school: school||'—', photo: photoData, points:0, progressPercent:0, failedAttempts:0, locked:false, notifications:[welcomeMsg], posts:[], smsReceived:[]};
    accounts.push(newAcc);
    save('pm_accounts', accounts);
    // auto login
    sessionPhone = phone; save('pm_session', sessionPhone);
    renderCounts();
    alert('Conta criada. Bem-vindo(a)!');
    loadSession();
  });

  // login
  loginSubmit.addEventListener('click', ()=>{
    const phone = loginPhone.value.trim(), pass = loginPass.value;
    const acc = accounts.find(a=>a.phone===phone);
    if(!acc) return alert('Conta não encontrada');
    if(acc.locked) return alert('Conta bloqueada por demasiadas tentativas.');
    if(acc.pass === pass){
      acc.failedAttempts = 0; save('pm_accounts',accounts);
      sessionPhone = acc.phone; save('pm_session',sessionPhone);
      alert(`Bem-vindo(a), ${acc.name}`);
      loadSession();
    } else {
      acc.failedAttempts = (acc.failedAttempts||0) + 1;
      if(acc.failedAttempts >= 3){ acc.locked = true; alert('Senha incorreta — conta bloqueada.'); }
      else alert(`Senha incorreta. Tentativas: ${acc.failedAttempts}/3`);
      save('pm_accounts', accounts);
    }
  });

  // session loader
  function loadSession(){
    accounts = load('pm_accounts') || [];
    topados = load('pm_topados') || {};
    sessionPhone = load('pm_session') || sessionPhone;
    currentUser = accounts.find(a=>a.phone === sessionPhone) || null;
    if(currentUser){
      authArea.style.display = 'none';
      loggedArea.style.display = 'block';
      leftAvatar.querySelector('img').src = currentUser.photo || placeholderDataUrl(currentUser.name);
      leftName.textContent = currentUser.name;
      leftSchool.textContent = currentUser.school || '—';
      leftPhone.textContent = currentUser.phone;
      leftPoints.textContent = (currentUser.points||0).toFixed(1);
      renderCounts();
      renderNotifList();
      renderPostsFeed();
      renderSMSFeed();
    } else {
      authArea.style.display = 'block';
      loggedArea.style.display = 'none';
    }
  }
  loadSession();

  // edit & logout
  btnEditProfile.addEventListener('click', async ()=>{
    if(!currentUser) return;
    const newName = prompt('Nome completo', currentUser.name);
    const newPhone = prompt('Telemóvel', currentUser.phone);
    const newSchool = prompt('Escola', currentUser.school);
    if(!newName || !newPhone) return;
    // if phone changed, ensure uniqueness
    if(newPhone !== currentUser.phone && accounts.some(a=>a.phone === newPhone)) return alert('Telemóvel em uso');
    // handle optionally new photo
    const f = confirm('Quer alterar a foto? OK para escolher ficheiro local (em seguida). Cancel para manter.');
    if(f){
      // create an <input type=file> dynamically to choose
      const input = document.createElement('input'); input.type='file'; input.accept='image/*';
      input.onchange = async (e)=> {
        if(input.files && input.files[0]) currentUser.photo = await fileToDataUrl(input.files[0]);
        finalizeEdit();
      };
      input.click();
    } else finalizeEdit();

    function finalizeEdit(){
      const oldPhone = currentUser.phone;
      currentUser.name = newName; currentUser.phone = newPhone; currentUser.school = newSchool;
      // update topados keys and notifications if phone changed
      if(oldPhone !== newPhone){
        const newTop = {};
        Object.entries(topados).forEach(([viewer, targets])=>{
          const v = viewer === oldPhone ? newPhone : viewer;
          newTop[v] = (targets || []).map(t => t === oldPhone ? newPhone : t);
        });
        topados = newTop;
        // update references in accounts notifications/smsReceived
        accounts.forEach(a=>{
          if(a.notifications) a.notifications = a.notifications.map(n => typeof n === 'object' ? n : n);
          if(a.smsReceived) a.smsReceived = a.smsReceived.map(s => ({...s, to: s.to === oldPhone ? newPhone : s.to, from: s.from === oldPhone ? newPhone : s.from}));
        });
        if(sessionPhone === oldPhone) sessionPhone = newPhone;
      }
      save('pm_topados', topados);
      save('pm_accounts', accounts);
      save('pm_session', sessionPhone);
      alert('Perfil atualizado');
      loadSession();
    }
  });

  btnLogout.addEventListener('click', ()=>{
    sessionPhone = null; currentUser = null; save('pm_session', null);
    loadSession();
    window.scrollTo({top:0,behavior:'smooth'});
  });

  // render counts
  function renderCounts(){ accounts = load('pm_accounts') || []; profilesCount.textContent = accounts.length; }

  // search
  btnSearch.addEventListener('click', ()=> doSearch(searchInput.value));
  searchInput.addEventListener('keydown', e=>{ if(e.key === 'Enter') doSearch(searchInput.value) });
  quickSearch.addEventListener('input', ()=> quickSearchUsers(quickSearch.value));

  function doSearch(q){
    resultsBox.innerHTML = '';
    const s = (q||'').trim().toLowerCase();
    if(!s) { resultsBox.innerHTML = '<div class="tiny">Digite algo para pesquisar</div>'; return; }
    const found = accounts.filter(a => a.name.toLowerCase().includes(s));
    if(found.length === 0){ resultsBox.innerHTML = '<div class="tiny">Nenhum resultado</div>'; return; }
    found.forEach(acc => {
      const r = document.createElement('div'); r.className = 'result';
      r.innerHTML = `
        <div class="mini"><img src="${acc.photo}" alt="${escapeHtml(acc.name)}"/></div>
        <div class="rinfo">
          <div style="font-weight:700">${escapeHtml(acc.name)}</div>
          <div class="small">${escapeHtml(acc.school||'—')}</div>
          <div style="margin-top:6px"><span class="badge">Pontos: ${(acc.points||0).toFixed(1)}</span> <span class="badge">Progresso: ${acc.progressPercent||0}%</span></div>
        </div>
      `;
      r.addEventListener('click', ()=> openViewPanel(acc.phone));
      resultsBox.appendChild(r);
    });
  }

  function quickSearchUsers(q){
    quickResults.innerHTML = '';
    const s = (q||'').trim().toLowerCase();
    if(!s) return;
    const list = accounts.filter(a => a.name.toLowerCase().includes(s) && a.phone !== sessionPhone);
    quickResults.innerHTML = list.map(u=>`<div class="result" style="padding:8px"><div class="mini"><img src="${u.photo}"></div><div style="flex:1;margin-left:8px"><strong>${u.name}</strong><div class="small">${u.school}</div></div><div style="margin-left:8px"><button onclick="openFromQuick('${u.phone}')">Ver</button></div></div>`).join('');
  }
  window.openFromQuick = (phone) => openViewPanel(phone);

  // open view panel for a target
  function openViewPanel(phone){
    const acc = accounts.find(a=>a.phone===phone);
    if(!acc) return;
    currentViewAccount = acc;
    viewMini.src = acc.photo || placeholderDataUrl(acc.name);
    viewName.textContent = acc.name;
    viewSchool.textContent = acc.school || '—';
    viewPoints.textContent = (acc.points||0).toFixed(1);
    viewProgress.style.width = (acc.progressPercent||0) + '%';
    viewPercent.textContent = (acc.progressPercent||0) + '%';
    viewPanel.style.display = 'block';
    // disable Topar if not logged or own profile
    if(!currentUser) { btnTopar.disabled = true; btnTopar.innerText = 'Login para Topar'; }
    else if(currentUser.phone === acc.phone) { btnTopar.disabled = true; btnTopar.innerText = 'É você'; }
    else { btnTopar.disabled = false; btnTopar.innerHTML = '<i data-feather=\"thumbs-up\"></i> Topar'; lucide.replace(); }
  }

  // Topar logic
  btnTopar.addEventListener('click', ()=>{
    if(!currentUser) return alert('Faça login para Topar');
    const viewer = currentUser.phone;
    const target = currentViewAccount && currentViewAccount.phone;
    if(!target) return;
    topados = load('pm_topados') || {};
    const vList = topados[viewer] || [];
    if(vList.includes(target)) return alert('Já topaste este perfil');
    vList.push(target); topados[viewer] = vList; save('pm_topados', topados);
    const targ = accounts.find(a=>a.phone === target);
    if(!targ) return;
    targ.points = (Number(targ.points) || 0) + 0.5;
    targ.progressPercent = Math.min(100, (targ.progressPercent||0) + 1);
    const notifText = `${currentUser.name} ${currentUser.school} ${currentUser.phone} está te ajudando a ganhar dinheiro.`;
    targ.notifications = targ.notifications || []; targ.notifications.unshift(notifText);
    save('pm_accounts', accounts);
    // update UI
    viewProgress.style.width = (targ.progressPercent) + '%';
    viewPercent.textContent = targ.progressPercent + '%';
    viewPoints.textContent = targ.points.toFixed(1);
    alert('Topado! O dono do perfil será notificado.');
    if(sessionPhone === targ.phone) renderNotifList();
  });

  // send SMS to viewed profile (quick)
  btnSendSMS.addEventListener('click', ()=> {
    if(!currentUser) return alert('Login necessário');
    const target = currentViewAccount && currentViewAccount.phone;
    if(!target) return alert('Nenhum perfil selecionado');
    const text = prompt('Escreva SMS (max 130 chars):');
    if(!text) return;
    if(text.length > 130) return alert('Máx 130 caracteres');
    const targ = accounts.find(a=>a.phone === target);
    if(!targ) return;
    const sms = {from: currentUser.phone, to: target, text, time: Date.now()};
    targ.smsReceived = targ.smsReceived || []; targ.smsReceived.unshift(sms);
    save('pm_accounts', accounts);
    alert('SMS enviado (guardado localmente por 3h).');
    if(sessionPhone === targ.phone) renderSMSFeed();
  });

  // Posts: publish and cleanup expired
  btnPost.addEventListener('click', async ()=>{
    if(!currentUser) return alert('Faça login para publicar');
    const text = postText.value.trim();
    const f = postImg.files[0];
    if(!text && !f) return alert('Escreva algo ou selecione imagem');
    let imgData = null;
    if(f) imgData = await fileToDataUrl(f);
    const post = {text, img: imgData, time: Date.now()};
    const me = accounts.find(a=>a.phone === currentUser.phone);
    me.posts = me.posts || []; me.posts.unshift(post);
    save('pm_accounts', accounts);
    postText.value = ''; postImg.value = '';
    renderPostsFeed();
  });
  btnClearPosts.addEventListener('click', ()=> { cleanupPosts(); renderPostsFeed(); alert('Postagens expiradas removidas.'); });

  function renderPostsFeed(){
    const me = accounts.find(a => a.phone === sessionPhone);
    postsFeed.innerHTML = '';
    if(!me || !me.posts || me.posts.length === 0) { postsFeed.innerHTML = '<div class="tiny">Sem postagens recentes.</div>'; return; }
    const now = Date.now();
    // filter valid only
    me.posts = (me.posts||[]).filter(p => now - p.time < EXPIRE_MS);
    save('pm_accounts', accounts);
    postsFeed.innerHTML = me.posts.map(p => `
      <div class="post">
        ${p.img ? `<img src="${p.img}" alt="post">` : ''}
        <div class="meta">
          <div style="font-weight:700">${escapeHtml(currentUser.name)}</div>
          <div class="small">${escapeHtml(p.text || '')}</div>
          <div class="tiny" style="margin-top:6px">Expira em até 3h</div>
        </div>
      </div>
    `).join('');
  }
  function cleanupPosts(){
    const now = Date.now();
    accounts.forEach(a => { if(a.posts) a.posts = a.posts.filter(p => now - p.time < EXPIRE_MS); });
    save('pm_accounts', accounts);
  }

  // SMS global send
  btnSendSMSGlobal.addEventListener('click', ()=>{
    if(!currentUser) return alert('Login necessário');
    const to = smsTo.value.trim(), text = smsText.value.trim();
    if(!to || !text) return alert('Preencha número e mensagem');
    if(text.length > 130) return alert('Máx 130 caracteres');
    const targ = accounts.find(a => a.phone === to);
    if(!targ) return alert('Usuário destino não encontrado');
    const sms = {from: currentUser.phone, to, text, time: Date.now()};
    targ.smsReceived = targ.smsReceived || []; targ.smsReceived.unshift(sms);
    save('pm_accounts', accounts);
    smsTo.value=''; smsText.value='';
    renderSMSFeed();
    alert('SMS enviado (expires em 3h)');
  });
  btnClearSMS.addEventListener('click', ()=> { cleanupSMS(); renderSMSFeed(); alert('SMS expirados limpos'); });

  function renderSMSFeed(){
    const me = accounts.find(a => a.phone === sessionPhone);
    smsFeed.innerHTML = '';
    if(!me || !me.smsReceived || me.smsReceived.length === 0) { smsFeed.innerHTML = '<div class="tiny">Sem SMS recentes.</div>'; return; }
    const now = Date.now();
    me.smsReceived = me.smsReceived.filter(s => now - s.time < EXPIRE_MS);
    save('pm_accounts', accounts);
    smsFeed.innerHTML = me.smsReceived.map(s => `<div class="post"><div class="meta"><div class="small">De: ${s.from}</div><div>${escapeHtml(s.text)}</div><div class="tiny">Recebido há ${(Math.round((now - s.time)/60000))} min</div></div></div>`).join('');
  }
  function cleanupSMS(){
    const now = Date.now();
    accounts.forEach(a => { if(a.smsReceived) a.smsReceived = a.smsReceived.filter(s => now - s.time < EXPIRE_MS); });
    save('pm_accounts', accounts);
  }

  // Notificações
  function renderNotifList(){
    notifList.innerHTML = '';
    const me = accounts.find(a => a.phone === sessionPhone);
    if(!me || !me.notifications || me.notifications.length === 0) { notifList.innerHTML = '<div class="tiny">Sem notificações.</div>'; notifCount.textContent=''; return; }
    notifCount.textContent = `(${me.notifications.length})`;
    notifList.innerHTML = me.notifications.map(n => `<div class="notify">${escapeHtml(n)}</div>`).join('');
  }

  // Tabs switching
  tabs.forEach(t => t.addEventListener('click', ()=>{
    tabs.forEach(x=>x.classList.remove('active'));
    t.classList.add('active');
    const key = t.dataset.tab;
    tabContainers.forEach(c => { c.style.display = (c.dataset.content === key) ? (key==='perfil' ? 'block' : '') : 'none'; });
    if(key === 'notifs') renderNotifList();
  }));

  // utility: file -> dataURL
  function fileToDataUrl(file){
    return new Promise((res,rej) => {
      const fr = new FileReader();
      fr.onload = ()=> res(fr.result);
      fr.onerror = rej;
      fr.readAsDataURL(file);
    });
  }
  function placeholderDataUrl(text){
    const t = (text||'P').charAt(0).toUpperCase();
    const bg = '#fff7ed';
    const fg = '#ff7b00';
    const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='400' height='400'><rect width='100%' height='100%' fill='${bg}' rx='30'/><text x='50%' y='52%' dominant-baseline='middle' text-anchor='middle' font-family='Arial' font-size='180' fill='${fg}'>${t}</text></svg>`;
    return 'data:image/svg+xml;base64,' + btoa(svg);
  }

  function escapeHtml(s){ return (s+'').replace(/[&<>"']/g, m=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[m])); }

  // initial render
  renderCounts();

  // expose some helpers for debugging (optional)
  window._pm = {
    accounts: () => load('pm_accounts'),
    topados: () => load('pm_topados'),
    session: () => load('pm_session')
  };

  // keep UI reactive when storage changed elsewhere
  window.addEventListener('storage', ()=> { accounts = load('pm_accounts') || []; renderCounts(); loadSession(); });

  // small helper functions for global onclick from generated HTML (safe)
  window.openProfileByPhone = (phone) => {
    if(!phone) return;
    openViewPanel(phone);
    // switch to Pesquisa tab programmatically
    document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
    document.querySelector('.tab[data-tab="pesquisa"]').classList.add('active');
    tabContainers.forEach(c => c.style.display = (c.dataset.content === 'pesquisa') ? '' : 'none');
  };

})();

</script>
</body>
</html
