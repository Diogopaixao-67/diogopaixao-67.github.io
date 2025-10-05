# diogopaixao-67.github.io
#<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Playmates - Plataforma de Votos</title>
  <style>
  
    :root{
      --fb-blue:#3b5998;
      --accent:#ff7a00;
      --danger:#e10600;
      --bg:#f0f2f5;
      --card:#fff;
    }
    *{box-sizing:border-box}
    body {margin:0; font-family:Arial, Helvetica, sans-serif;background:var(--bg)}
    header {background:#FF6600;color:#fff;padding:10px 12px;display:flex;flex-wrap:wrap;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:30}
    header h1{margin:0;font-size:20px}
    nav{display:flex;gap:8px;align-items:center;flex-wrap:wrap;margin-top:6px}
    nav a{color:#fff;text-decoration:none;font-weight:bold;padding:6px 10px;border-radius:6px;cursor:pointer;flex:1 1 auto;text-align:center}
    nav a:hover{background:rgba(255,255,255,.17)}
    .search-wrap{display:flex;align-items:center;gap:8px;margin-top:6px;flex:1 1 100%}
    .search-input{padding:6px 8px;border-radius:8px;border:none;flex:1}
    .container{padding:16px;max-width:1100px;margin:0 auto}
    .grid{display:grid;gap:12px}
    .btn{background:var(--accent);border:none;color:#fff;padding:10px 14px;border-radius:8px;cursor:pointer;font-weight:bold}
    .btn:disabled{opacity:.6;cursor:not-allowed}
    .btn-red{background:var(--danger)}
    .btn-outline{background:#fff;color:#333;border:1px solid #ccc}
    .hidden{display:none}
    .card{background:var(--card);border-radius:12px;box-shadow:0 2px 6px rgba(0,0,0,.08);padding:14px;width:100%}
    .list{display:flex;flex-direction:column;gap:10px}
    table{width:100%;border-collapse:collapse;display:block;overflow-x:auto}
    th,td{padding:8px;border-bottom:1px solid #e9ecef;text-align:center;white-space:nowrap}
    thead th{background:#f7f7f7}
    img.avatar{width:50px;height:50px;object-fit:cover;border-radius:8px;background:#eee}
    .circle{width:18px;height:18px;border-radius:50%;display:inline-block;margin-right:8px;background:var(--danger);cursor:pointer}
    #countdown{position:fixed;right:16px;bottom:16px;background:#000;color:#fff;padding:8px 10px;border-radius:10px;font-size:14px;box-shadow:0 2px 6px rgba(0,0,0,.25);z-index:25;cursor:pointer}
    .editor-row{display:grid;grid-template-columns:60px 1fr 1fr 90px auto;gap:8px;align-items:center}
    .editor-row input[type="text"], .editor-row input[type="number"], select{width:100%;padding:6px 8px;border:1px solid #ccc;border-radius:6px}
    .editor-row input[type="file"]{font-size:12px}
    .event-img{width:100%;max-height:180px;object-fit:cover;border-radius:10px;background:#eee}
    .feed-central{background:#fff3cd;color:#8a6d3b;border:1px solid #ffeeba;padding:10px 12px;border-radius:10px;margin-bottom:10px}
    .feed-actions{display:flex;gap:8px;flex-wrap:wrap}
    .profile-card{display:flex;gap:12px;align-items:center}
    .small-tabs{display:flex;gap:6px;flex-wrap:wrap}
    .small-tab{background:#fff;padding:6px 10px;border-radius:8px;border:1px solid #ddd;box-shadow:0 1px 4px rgba(0,0,0,.06)}
    .overlay{position:fixed;inset:0;background:rgba(0,0,0,.6);display:flex;align-items:center;justify-content:center;z-index:60}
    .winner-card{background:#fff;padding:20px;border-radius:14px;text-align:center;width:90%;max-width:520px;animation:pop .6s}
    @keyframes pop{from{transform:scale(.9);opacity:0}to{transform:scale(1);opacity:1}}
    .winner-name{font-size:28px;font-weight:900;margin-top:6px}
    .winner-info{margin-top:8px;font-size:18px}
    .profile-photo{width:110px;height:110px;border-radius:12px;object-fit:cover;background:#eee;border:2px solid #fff;box-shadow:0 2px 6px rgba(0,0,0,.12)}

    /* 📱 Responsividade */
    @media (max-width:768px){
      header{flex-direction:column;align-items:flex-start}
      nav{width:100%;justify-content:space-around}
      .editor-row{grid-template-columns:1fr;gap:6px}
      .feed-actions{flex-direction:column}
      .btn, .btn-outline{width:100%;text-align:center}
      .grid{grid-template-columns:1fr}
      body{font-size:15px}
    }
    @media (max-width:480px){
      header h1{font-size:18px}
      nav a{font-size:14px;padding:6px}
      .search-input{font-size:14px}
      .winner-name{font-size:22px}
      .winner-info{font-size:16px}
    }
    
    /* Puzzle styles */
    .puzzle-grid{display:grid;grid-template-columns:repeat(3,80px);gap:8px;justify-content:center}
    .puzzle-cell{width:80px;height:80px;display:flex;align-items:center;justify-content:center;background:#fff;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,.08);font-weight:900;font-size:22px;cursor:pointer}
    .puzzle-cell.empty{background:transparent;box-shadow:none;cursor:default}
  </style>
</head>
    <body>
  <header>
    <h1>Playmates</h1>
    <nav>
      <a onclick="showSection('home')">Home</a>
      <a onclick="showSection('about')">About</a>
      <a onclick="showSection('services')">Services</a>
      <a onclick="showSection('direto')">DiretoVots</a>
      <a onclick="showSection('ranking')">Ranking Escola</a>
      <a onclick="showSection('events')">Eventos</a>
      <a onclick="showSection('feed')">Feed</a>
      
      <a onclick="showSection('profile')">Perfil</a><a onclick="showSection('jogos')">Jogos</a>
      <a onclick="showSection
    </nav>

    <div class="search-wrap">
      <input id="searchInput" class="search-input" placeholder="🔎 Buscar por nome..." oninput="doSearch()" />
      <div id="searchCount" style="color:#fff;font-weight:bold;margin-right:8px"></div>
    </div>
  </header>

  <div class="container">
    <!-- HOME -->
    <section id="home" class="grid">
      <div class="card">
        <h2>Painel de Inscrição - Disputa de Votos</h2>
        <p>Playmates é uma plataforma que transforma votos em dinheiro para estudantes! Inscreva-se e participe para ganhar recompensas em sua escola.</p>
        <form id="registerForm" class="grid" onsubmit="return false;">
          <input type="text" id="nome" placeholder="Nome Completo" required>
          <input type="tel" id="telefone" placeholder="Número WhatsApp" required>
          <input type="text" id="escola" placeholder="Nome da Escola" required>
          <select id="pack" required>
            <option value="">-- Escolha um Pack de Votos --</option>
            <option value="Pack Grátis">Pack grátis - ganho de 1000kz (20s)</option>
            <option value="Pack Mater">Pack Mater - ganho de 5.000kz (10s)</option>
            <option value="Pack Premium">Pack Premium - muito mais!</option>
          </select>
          <button class="btn" id="btnCadastrar">Cadastrar</button>
        </form>
        <div class="feed-actions">
          <button class="btn-outline" onclick="showPedidos()">Pedidos Pendentes</button>
          <strong id="pendingCount"></strong>
        </div>
        <div id="pedidos" class="list hidden"></div>
      </div>

      <div class="card">
        <h3 style="display:flex;align-items:center;gap:10px">
          <span class="btn btn-red" style="pointer-events:none">DiretoVots</span>
          <button class="btn" onclick="showSection('direto')">Abrir</button>
        </h3>
        <p>Gerencie votos, selecione concorrentes e envie o voto ao CEO via WhatsApp.</p>
      </div>
    </section>
<!-- ABOUT -->
<section id="about" class="hidden card" style="text-align:center;">
  <!-- Foto de perfil em círculo com animação -->
  <div class="profile-pic-container">
    <img id="profilePic" src="HTML/Playmates.png" alt="Foto de perfil" class="profile-pic"/>
    <div class="star-animation"></div>
  </div>

  <!-- Modal para visualizar imagem -->
  <div id="imageModal" class="modal hidden">
    <span class="close" onclick="closeModal()">&times;</span>
    <img id="modalImg" class="modal-content"/>
  </div>

  <h2>Sobre o Playmates</h2>
  <p>
    O Playmates foi criado por <strong>Diogo Paixão</strong> aos 17 anos e lançado em 2025.  
    É uma plataforma pioneira em Angola que transforma votos em oportunidades financeiras.  
    Já alcançou mais de 20 escolas do ensino médio, permitindo que estudantes ganhem recompensas de forma confiável e segura.  
    É mais que uma disputa, é um caminho de empreendedorismo para jovens angolanos. Estimula liderança, comunicação e captação de apoios nas comunidades escolares.
  </p>

  <!-- Reações -->
  <div class="reactions">
    <button onclick="addReaction('like')">👍</button>
    <button onclick="addReaction('love')">♥️</button>
    <div id="reactionCounts"></div>
  </div>

  <!-- Galeria horizontal -->
  <div class="gallery">
    <img src="HTML/Playmates.png" alt="Foto 1"/>
    <img src="HTML/Playmates.png" alt="Foto 2"/>
    <img src="HTML/Playmates.png" alt="Foto 3"/>
  </div>

  <!-- Contato -->
  <div class="contact-card">
    <h3>Contacto</h3>
    <p>📞 +244 941 530 467</p>
    <a href="https://wa.me/244941530467" target="_blank" class="btn-whatsapp">Marcar Reunião no WhatsApp</a>
  </div>
</section>


<style>
  .profile-pic-container {
    position: relative;
    display: inline-block;
    margin-bottom: 10px;
  }
  .profile-pic {
    width: 150px; height: 150px;
    border-radius: 50%;
    cursor: pointer;
    border: 3px solid #3b5998;
    object-fit: cover;
  }
  .star-animation {
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 100%;
    border-radius: 50%;
    pointer-events: none;
    background: radial-gradient(circle, rgba(255,255,0,0.5) 10%, transparent 60%);
    animation: twinkle 2s infinite alternate;
  }
  @keyframes twinkle {
    from { transform: scale(1); opacity: 0.5; }
    to { transform: scale(1.2); opacity: 1; }
  }

  /* Modal */
  .modal {
    display:flex; justify-content:center; align-items:center;
    position:fixed; z-index:999; left:0; top:0; width:100%; height:100%;
    background:rgba(0,0,0,0.8);
  }
  .modal.hidden { display:none; }
  .modal-content {
    max-width:80%; max-height:80%;
    border-radius: 10px;
  }
  .close {
    position:absolute; top:20px; right:40px;
    color:white; font-size:40px; cursor:pointer;
  }

  /* Reações */
  .reactions { margin: 15px 0; }
  .reactions button {
    background:none; border:none; font-size:25px;
    cursor:pointer; margin:0 5px;
  }
  #reactionCounts { margin-top: 5px; font-size:14px; color:#555; }

  /* Galeria */
  .gallery {
    display:flex; justify-content:center; gap:10px;
    margin-top:15px;
  }
  .gallery img {
    width:80px; height:80px;
    object-fit:cover; border-radius:8px;
    border:2px solid #ccc;
  }

  /* Contato */
  .contact-card {
    margin-top:20px; padding:15px;
    border:1px solid #ddd; border-radius:10px;
    background:#f9f9f9; text-align:center;
  }
  .btn-whatsapp {
    display:inline-block; margin-top:10px;
    padding:8px 15px; background:#25D366;
    color:white; border-radius:5px; text-decoration:none;
  }
</style>

<script>
  // ===== Modal de imagem =====
  const profilePic = document.getElementById('profilePic');
  const modal = document.getElementById('imageModal');
  const modalImg = document.getElementById('modalImg');
  profilePic.onclick = () => {
    modal.classList.remove('hidden');
    modalImg.src = profilePic.src;
  }
  function closeModal(){ modal.classList.add('hidden'); }

  // ===== Reações =====
  function loadReactions(){
    return JSON.parse(localStorage.getItem("LS.reactions")) || { like:0, love:0 };
  }
  function saveReactions(data){
    localStorage.setItem("LS.reactions", JSON.stringify(data));
  }
  function updateReactionUI(){
    const r = loadReactions();
    document.getElementById("reactionCounts").innerText =
      `👍 ${r.like} · ♥️ ${r.love}`;
  }
  function addReaction(type){
    const r = loadReactions();
    r[type]++; 
    saveReactions(r);
    updateReactionUI();
  }
  // inicializar
  updateReactionUI();
</script>

    <!-- ABOUT -->
    <section id="about" class="hidden card">
                    <img src="HTML/Playmates.png"/>
            
      <h2>Sobre o Playmates</h2>
      <p>O Playmates foi criado por <strong>Diogo Paixão</strong> aos 17 anos e lançado em 2025. É uma plataforma pioneira em Angola que transforma votos em oportunidades financeiras. Já alcançou mais de 20 escolas do ensino médio, permitindo que estudantes ganhem recompensas de forma confiável e segura. É mais que uma disputa, é um caminho de empreendedorismo para jovens angolanos. Estimula liderança, comunicação e captação de apoios nas comunidades escolares.</p>
    </section>

    <!-- SERVICES -->
    <section id="services" class="hidden card">
      <h2>Serviços</h2>
      <p>Conectamos escolas e estudantes em competições interativas que convertem votos em ganhos reais. Gestão de inscrições, acompanhamento de votos em tempo real e pagamentos confiáveis.</p>
    </section>

    <!-- DIRETOVOTS -->
    <section id="direto" class="hidden card">
      <h2>DiretoVots</h2>
      <div class="list">
        <div style="display:flex;justify-content:space-between;align-items:center;gap:8px">
          <div>
            <button class="btn" onclick="openCompetitorEditor()">Editar Concorrentes (senha 777)</button>
            <button class="btn-outline" onclick="requestNotifications()">Ativar Notificações</button>
          </div>
          <div id="inCountdownStatus"></div>
        </div>

        <div style="margin-top:10px" id="liveTabs" class="small-tabs"></div>

        <table id="competitorsTable">
          <thead>
            <tr><th>Foto</th><th>Nome</th><th>Escola</th><th>Votos</th><th>Ações</th></tr>
          </thead>
          <tbody></tbody>
        </table>
        <button class="btn" onclick="openCompetitorEditor()">Editar Concorrentes (senha 777)</button>
        <div id="competitorEditor" class="hidden card"></div>
      </div>
    </section>

    <!-- RANKING ESCOLA -->
    <section id="ranking" class="hidden card">
      <h2>Ranking por Escola</h2>
      <div id="rankingResumo" class="list"></div>
      <hr>
      <div id="rankingDetalhes" class="list"></div>
    </section>

    <!-- EVENTOS -->
    <section id="events" class="hidden card">
      <h2>Eventos</h2>
      <div id="eventToolbar" class="feed-actions">
        <button class="btn" onclick="toggleEventEditor()">Adicionar/Editar (senha A4)
        
        
        </button>
      </div>
      <div id="eventEditor" class="hidden card">
        <div class="grid">
          <input type="text" id="evData" placeholder="Dia/Mês/Ano">
          <textarea id="evConteudo" placeholder="Descrição do evento" rows="3" style="width:100%"></textarea>
          <input type="file" id="evFoto" accept="image/*">
          <div class="feed-actions">
            <button class="btn" onclick="saveNewEvent()">Salvar Evento</button>
            <button class="btn-outline" onclick="cancelEventEditor()">Cancelar</button><!-- Exemplo de post no Feed -->
<div class="post">
  Postagem de exemplo
  <button class="btn-excluir" onclick="excluirPost(this)">x</button>
</div>
          </div>
        </div>
      </div>
      <!-- Botão de excluir na sessão Eventos -->
<button onclick="excluirEvento()">Excluir Evento</button>

<script>
  function excluirEvento(){
    let senha = prompt("Digite a senha para excluir este evento:");
    if(senha === "ER"){
      document.getElementById("eventos").remove();
      alert("Evento excluído com sucesso!");
    } else {
      alert("Senha incorreta. Exclusão cancelada.");
    }
  }
</script>
      <div id="eventList" class="grid"></div>
    </section>

    <!-- FEED -->
    <section id="feed" class="hidden card">
      <h2>Feed de Notícias</h2>
      <div class="feed-central">Sabias que 1 voto pode valér para vc 5.000kz</div>
      <div class="feed-actions">
        <button class="btn" onclick="openFeedAuto()">Auto (admin)</button>
      </div>
      <div id="feedAuto" class="hidden card">
        <textarea id="feedText" rows="3" placeholder="Escreva a publicação..." style="width:100%"></textarea>
        <div class="feed-actions">
          <button class="btn" onclick="postToFeed()">Publicar</button>
          <button class="btn-outline" onclick="closeFeedAuto()">Fechar</button>
        </div>
      </div>
      <div id="feedList" class="list"></div>
    </section>

    <!-- PROFILE -->
    <section id="profile" class="hidden card">
      <h2>Perfil</h2>
      <div id="profileArea"></div>
    </section>
    
  <!-- JOGOS -->
  <section id="jogos" class="card hidden">
    <h2>Jogos - PLAYHOU</h2>
    <div class="card">
      <div style="text-align:center;margin-bottom:10px">
        <div id="puzzleTimer">02:00</div>
        <div id="puzzleStatus" class="small"></div>
      </div>
      <div id="puzzleBoard" class="puzzle-grid"></div>
      <div style="display:flex;gap:8px;justify-content:center;margin-top:12px">
        <button class="btn" id="startPuzzleBtn">Iniciar Jogo</button>
        <button class="btn-outline" id="shuffleBtn">Embaralhar</button>
      </div>
      <div style="margin-top:12px">Ranking (melhores tempos): <ul id="puzzleRank"></ul> 
      
      
    <!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Painel Love & Sexo</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      margin: 0;
      padding: 0;
    }

    #painel {
      display: none;
      padding: 20px;
    }
    .card {
      background: #fff;
      padding: 15px;
      margin: 10px 0;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }
    label {
      font-weight: bold;
    }
    input, select, button {
      margin-top: 8px;
      width: 100%;
      padding: 8px;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    button {
      background: #e91e63;
      color: #fff;
      border: none;
      cursor: pointer;
      font-weight: bold;
      transition: 0.3s;
    }
    button:hover {
      background: #c2185b;
    }
    .perfil {
      margin: 10px 0;
      padding: 10px;
      border: 1px solid #eee;
      border-radius: 6px;
      background: #fafafa;
    }
    #acoes {
      display: flex;
      gap: 10px;
      margin-top: 10px;
    }
    #acoes button {
      flex: 1;
    }
    footer {
      margin-top: 20px;
      padding: 15px;
      background: #fff3f3;
      border-top: 1px solid #ddd;
      font-size: 14px;
    }
  </style>
</head>
<body>
    <button id="btnLoveSexo">Love & Sexo</button>
  </div>
  
  <div id="painel">
    <div class="card" id="formCard">
      <label>Nome:</label>
      <input type="text" id="nome">
      
      <label>Sexo:</label>
      <select id="sexo">
        <option value="">Selecione</option>
        <option value="homem">Homem</option>
        <option value="mulher">Mulher</option>
      </select>
      
      <label>Estado Civil:</label>
      <select id="estadoCivil">
        <option value="">Selecione</option>
        <option value="solteiro">Solteiro(a)</option>
        <option value="casado">Casado(a)</option>
      </select>
      
      <label>Profissão:</label>
      <select id="profissao">
        <option value="">Selecione</option>
        <option>Barbeiro</option>
        <option>Ministra</option>
        <option>Polícia</option>
        <option>Bombeiro</option>
        <option>Engenheiro</option>
        <option>Desempregado</option>
        <option>Taxista</option>
        <option>Outro</option>
      </select>
      
      <button onclick="salvarDados()">Salvar Dados</button>
    </div>

    <div id="acoes" style="display:none;">
      <button onclick="editarDados()">Editar Dados</button>
      <button onclick="apagarDados()">Apagar Dados</button>
    </div>
    
    <div id="conteudoExtra"></div>
    
    <footer>
      💰 Se queres ganhar dinheiro com o <b>Playmates</b> siga os passos abaixo:<br>
      ➡️ Convide 20 pessoas e ganhe <b>5$</b>.
    </footer>
  </div>
  
  <script>
    const btnLoveSexo = document.getElementById("btnLoveSexo");
    const painel = document.getElementById("painel");
    const conteudoExtra = document.getElementById("conteudo
