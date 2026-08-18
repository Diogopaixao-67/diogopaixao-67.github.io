<html lang="pt">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Playmates — Protótipo (WhatsApp style)</title>
<style>
:root{
  --bg:#fffaf5; --card:#ffffff; --muted:#667085; --accent-3:#f97316; --accent-2:#ea580c; --gold:#f59e0b;
  --shadow: 0 6px 18px rgba(16,24,40,0.06);
}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,-apple-system,"Segoe UI",Roboto,Helvetica,Arial;background:var(--bg);color:#0b1222;min-height:100vh;padding-bottom:100px}
header{display:flex;align-items:center;gap:12px;padding:13px 16px;background:linear-gradient(135deg,#f97316,#ea580c);color:#fff;font-weight:700;border-radius:0 0 14px 14px;position:sticky;top:0;z-index:40}
header h1{margin:0;font-size:18px}
main{padding:12px;max-width:980px;margin:0 auto;display:grid;grid-template-columns:1fr;gap:12px}
.card{background:var(--card);border-radius:12px;padding:12px;box-shadow:var(--shadow)}
.section{display:none}
.section.active{display:block}
nav{display:flex;justify-content:space-around;align-items:center;background:rgba(255,255,255,.97);backdrop-filter:blur(12px);position:fixed;bottom:0;left:0;right:0;border-top:1px solid #dbe2ea;padding:8px 4px;z-index:50;box-shadow:0 -8px 24px rgba(15,23,42,.08)}
nav button{background:none;color:#64748b;border:none;font-size:12px;padding:7px 4px;width:25%;display:flex;flex-direction:column;align-items:center;gap:4px;transition:.2s}
nav button.active{color:#f97316;font-weight:800;transform:translateY(-1px)}
small.navIcon{font-size:18px;line-height:1}

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
/*=============================
CARTEIRA
=============================*/

.walletCard{

margin-top:15px;

padding:15px;

background:#ffffff;

border-radius:15px;

box-shadow:0 4px 15px rgba(0,0,0,.12);

cursor:pointer;

transition:.25s;

}

.walletCard:hover{

transform:scale(1.02);

}

.walletTitulo{

font-size:18px;

font-weight:bold;

color:#ff7b00;

margin-bottom:8px;

}

#walletSaldo{

font-size:30px;

font-weight:bold;

color:#16a34a;

}
/*==========================
EVENTOS
==========================*/

.eventoBanner{

width:100%;

overflow:hidden;

border-radius:12px;

}

.eventoBanner img{

width:100%;

display:block;

border-radius:12px;

object-fit:cover;

}

.eventoInfo{

padding:15px 0;

text-align:center;

}

.eventoInfo h2{

color:#ff7b00;

margin-bottom:10px;

}

.eventoInfo p{

color:#555;

font-size:15px;

line-height:1.5;

}

.areaComprar{

margin-top:20px;

}

.areaComprar button{

width:100%;

padding:18px;

font-size:22px;

background:#25D366;

border:none;

border-radius:12px;

color:white;

font-weight:bold;

cursor:pointer;

transition:.25s;

}

.areaComprar button:hover{

transform:scale(1.02);

background:#1ea952;

}

/* PLAYMATES — refinamento visual */
.section{animation:sectionIn .22s ease}
@keyframes sectionIn{from{opacity:.4;transform:translateY(5px)}to{opacity:1;transform:none}}
.marketplaceProduct{border:1px solid #e2e8f0;border-radius:18px;overflow:hidden;background:#fff;box-shadow:0 12px 30px rgba(15,23,42,.08)}
.marketplaceProductImage{width:100%;aspect-ratio:1/1;display:block;object-fit:contain;background:linear-gradient(145deg,#eef2ff,#f8fafc);padding:10px}
.marketplaceBody{padding:16px}
.marketplaceBadge{display:inline-flex;padding:5px 9px;border-radius:999px;background:#eff6ff;color:#1d4ed8;font-size:11px;font-weight:800}
.marketplacePrice{font-size:24px;font-weight:900;color:#0f172a;margin:8px 0}
.marketplaceBuy{width:100%;padding:14px!important;border-radius:12px!important;background:linear-gradient(135deg,#2563eb,#1d4ed8)!important}
.jogosGrid{display:grid;gap:12px}
@media(min-width:700px){.jogosGrid{grid-template-columns:1fr 1fr}}
.competitorsGrid{display:grid;grid-template-columns:1fr;gap:12px}
@media(min-width:700px){.competitorsGrid{grid-template-columns:1fr 1fr}}
.compCard{box-shadow:0 8px 22px rgba(15,23,42,.05);border:1px solid #e5eaf1}
.compCard img{background:#f1f5f9}
.vote-green{background:#2563eb!important}
.edit-orange{background:#0f172a!important}
.photo-btn{color:#334155}
#countdownCard{border:1px solid #dbe4f0;background:linear-gradient(145deg,#ffffff,#f8fafc)}
#countdownLabel{color:#0f172a}
.walletTitulo{color:#2563eb}
#userCount.user-count{color:#2563eb}


/* PLAYMATES FINAL ORANGE/WHITE THEME */
body{background:#fffaf5}
header h1{letter-spacing:.4px}
.card{border:1px solid #f1f1f1}
.marketGrid{display:grid;grid-template-columns:1fr;gap:14px;margin-top:14px}
.marketCard{overflow:hidden;background:#fff;border:1px solid #eee;border-radius:16px;box-shadow:0 8px 24px rgba(23,32,51,.07)}
.marketCard img{width:100%;aspect-ratio:1/1;object-fit:contain;background:#fff7ed;display:block;padding:8px}
.marketBody{padding:13px}.marketBody h3{margin:8px 0 5px}.marketPrice{font-size:21px;font-weight:900;margin:8px 0 12px;color:#172033}
.marketBadge{display:inline-block;background:#fff1e8;color:#c2410c;border-radius:99px;padding:5px 8px;font-size:10px;font-weight:900}
.marketBtn{width:100%;background:linear-gradient(135deg,#f97316,#ea580c);color:#fff;font-weight:900}
.v2Modal{position:relative;max-width:720px;width:96%;max-height:90vh;overflow:auto}
.modalCloseV2{position:absolute;right:10px;top:10px;width:38px;height:38px;border-radius:50%;background:#f8fafc;color:#334155;font-size:22px;padding:0}
.copyRowV2{display:flex;gap:7px;align-items:center}.copyRowV2 input{margin:0;flex:1}
.commissionV2{padding:13px;background:#fff7ed;border:1px solid #fed7aa;border-radius:12px;margin:12px 0;display:grid;gap:3px}
.commissionV2 strong{color:#c2410c}
.affRowV2{padding:11px;border:1px solid #e5e7eb;border-radius:11px;margin:7px 0}
.platformMenuV2{display:grid;gap:8px;margin-top:12px}
.platformMenuV2 button{width:100%;display:flex;align-items:center;gap:12px;text-align:left;background:#fff;color:#172033;border:1px solid #e5e7eb;padding:13px}
.platformMenuV2 button span{display:grid;gap:2px;flex:1}.platformMenuV2 small{color:#667085;font-weight:500}
.mariaChatV2{height:270px;overflow:auto;background:#f8fafc;border-radius:12px;padding:8px;margin:10px 0}
.mariaMsgV2{max-width:88%;padding:9px 11px;border-radius:12px;margin:6px 0}.mariaMsgV2.bot{background:#fff;border:1px solid #e5e7eb}.mariaMsgV2.user{background:#ffedd5;margin-left:auto}
.sponsorPlansV2{display:grid;grid-template-columns:1fr;gap:8px}.sponsorPlansV2 div{padding:12px;background:#fff7ed;border:1px solid #fed7aa;border-radius:12px;display:flex;justify-content:space-between}
@media(min-width:700px){.marketGrid{grid-template-columns:1fr 1fr}.sponsorPlansV2{grid-template-columns:1fr 1fr 1fr}}


/* ===== FINAL ORANGE / WHITE THEME ===== */
:root{--accent-3:#f97316!important;--accent-2:#ea580c!important;--bg:#fffaf5!important}
body{background:#fffaf5!important;color:#172033!important}
header{background:linear-gradient(135deg,#f97316,#ea580c)!important}
button{background:#f97316;color:#fff}
button:hover{background:#ea580c}
nav button{background:transparent!important;color:#667085!important}
nav button.active{color:#f97316!important}
.marketplaceBadge,.marketBadge{background:#fff1e8!important;color:#c2410c!important}
.marketplaceProductImage,.marketCard img{background:#fff7ed!important}
.marketplaceBuy,.marketBtn{background:linear-gradient(135deg,#f97316,#ea580c)!important}
.vote-green{background:#25D366!important}
.edit-orange{background:#f97316!important}
.walletTitulo{color:#f97316!important}
#userCount.user-count{color:#f97316!important}
.v5PlatformModal{display:grid;gap:8px;margin-top:12px}
.v5PlatformModal button{width:100%;display:flex;align-items:center;gap:12px;text-align:left;background:#fff!important;color:#172033!important;border:1px solid #fed7aa!important;padding:14px}
.v5PlatformModal small{display:block;color:#667085;margin-top:2px}
.v5Close{position:absolute;right:10px;top:10px;width:38px!important;height:38px!important;border-radius:50%!important;padding:0!important}
.v5Reel{background:#111;color:#fff;border-radius:16px;overflow:hidden;margin:12px 0}
.v5ReelVideo{width:100%;height:min(78vh,720px);object-fit:cover;display:block}
.v5ReelMeta{padding:12px;background:linear-gradient(180deg,#111,#222)}
.v5CommentBox{background:#fff;color:#172033;padding:10px}
.v5Comment{padding:7px 0;border-bottom:1px solid #eee}
.v5Chat{height:420px;overflow:auto;background:#f8fafc;border-radius:12px;padding:10px;margin:10px 0}
.v5Bubble{padding:9px 11px;background:#fff;border:1px solid #eee;border-radius:12px;margin:7px 0;max-width:86%}
.v5Bubble.me{margin-left:auto;background:#fff7ed;border-color:#fed7aa}
.v5Rank{display:grid;grid-template-columns:56px 1fr auto;gap:10px;align-items:center;padding:11px 2px;border-bottom:1px solid #eee}
.v5Rank img{width:52px;height:52px;border-radius:50%;object-fit:cover;border:3px solid #f97316}
.v5RankNo{font-weight:900;color:#f97316}.v5RankName{font-weight:900}.v5RankSchool{font-size:12px;color:#667085}.v5RankScore{font-weight:900;text-align:right}.v5RankPrize{font-size:12px;color:#15803d}
.v5AdminRow{border:1px solid #eee;border-radius:12px;padding:10px;margin:8px 0}
.v5AdminRow input{margin:4px 0}
@media(max-width:600px){.v5ReelVideo{height:70vh}.v5Rank{grid-template-columns:50px 1fr}.v5RankScore{grid-column:2;text-align:left}}

</style>
</head>
<body>
<header><div style="font-size:22px">◈</div><h1>PLAYMATES</h1><button type="button" id="platformMenuBtn" style="margin-left:auto;background:rgba(255,255,255,.16);border:1px solid rgba(255,255,255,.3);color:#fff">PLATFORM ▾</button></header>

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
      <div id="walletCard" class="walletCard">

<div class="walletTitulo">

💰 Carteira Digital

</div>

<div id="walletSaldo">

0,00 Kz

</div>

</div>
      <div style="display:flex;gap:8px;margin-top:6px">
        <button id="btnEditProfile" class="ghost">Editar Perfil</button>
        <button id="btnNewPost" class="ghost">Nova Postagem (3h)</button>
      </div>

      <p>Usuários cadastrados: <span id="userCount" class="user-count">0</span></p>

      <h4>Mensagens</h4>
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

  <!-- MARKETPLACE -->
  
<div id="sec-eventos" class="card section">
  <div class="marketplaceProduct">
    <img id="eventoBannerImg" class="marketplaceProductImage"
      src="https://diogopaixao-67.github.io/importação.png"
      alt="E-book de Importação Playmates"
      onerror="this.src='https://via.placeholder.com/800x800/f97316/ffffff?text=PLAYMATES'">
    <div class="marketplaceBody">
      <span class="marketplaceBadge">MARKETPLACE • PRODUTO DIGITAL</span>
      <h2 style="margin:10px 0 6px">Marketplace PLAYMATES</h2>
      <p style="color:#667085;line-height:1.55;margin:0">Escolha um produto e clique em AFILIAR-SE para consultar descrição, comissão de 50% e link de divulgação.</p>
    </div>
  </div>

  <div id="marketplaceProductsV2" class="marketGrid"></div>

  <div class="card" style="margin-top:14px;text-align:center">
    <h3>Administração de Afiliados</h3>
    <p class="smallNote">Área reservada ao administrador.</p>
    <button type="button" id="affiliateAdminBtnV2">ADMIN — AFILIADOS</button>
  </div>

  <!-- Mantido para o JavaScript original do Marketplace -->
  <button id="btnComprarEvento" style="display:none" type="button">Comprar</button>
</div>

<!-- JOGOS -->
  <div id="sec-jogos" class="card section">
    <h2 style="margin-top:0">Jogos</h2>

    <div id="countdownCard">
      <div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap;justify-content:center">
        <strong>Contagem regressiva:</strong>
        <span id="countdownLabel">00:00:00</span>
      </div>
      <div style="font-size:12px;color:#666">
        <button type="button" id="editCountdownBtn" class="small-ghost" title="Editar (senha A8)">⚙️ Editar contador</button>
      </div>
      <div class="smallNote">Tempo visível em tempo real para todos os utilizadores.</div>
    </div>

    <div class="jogosGrid">
      <div class="card" style="margin-top:10px">
        <h3 style="margin-top:0">IBAN da Playmates</h3>
        <div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap">
          <div id="ibanText" style="font-weight:800;word-break:break-all">AO06005500007150984310146</div>
          <button type="button" id="copyIban" class="ghost">Copiar</button>
        </div>
        <div style="font-size:12px;color:#666;margin-top:6px">IBAN fixo para apoio à plataforma.</div>
      </div>

      <div class="card" style="margin-top:10px">
        <h3 style="margin-top:0">Concorrentes</h3>
        <div id="competitorsList" class="competitorsGrid"></div>
      </div>
    </div>

    <div class="card" style="margin-top:12px">
      <h3 style="margin-top:0">Painel de pedidos — Packs de votos</h3>
      <div style="display:grid;gap:8px">
        <input id="req_name" placeholder="Nome"/>
        <input id="req_school" placeholder="Escola"/>
        <input id="req_whatsapp" placeholder="Número WhatsApp (ex: 2449...)"/>
        <select id="req_pack">
          <option value="30 votos">Grátis — ganha 400 Kz</option>
          <option value="50 Votos">Pack ouro — ganha 1000 Kz</option>
        </select>
        <div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
          <button id="reqSubmit">Enviar pedido</button>
          <button id="simPlus" class="ghost">+1 (Simulador)</button>
          <div class="pill">Pedidos: <span id="reqCount">0</span></div>
        </div>
        <div id="requestsList" class="requestsList"></div>
      </div>
    </div>

    <!-- hidden file inputs: 6 concorrentes -->
    <input type="file" id="compPhoto_1" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_2" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_3" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_4" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_5" accept="image/*" style="display:none"/>
    <input type="file" id="compPhoto_6" accept="image/*" style="display:none"/>
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

<!-- FINAL: REELS / DEBATE / RANKING -->
<div id="sec-reels" class="card section">
  <div style="display:flex;align-items:center;justify-content:space-between;gap:8px">
    <div><h2 style="margin:0">🎬 Reels</h2><p class="smallNote">Vídeos curtos, até 5 minutos.</p></div>
    <button id="v5ReelsAdmin" type="button" title="Publicar Reel">＋</button>
  </div>
  <div id="v5ReelsCount" class="smallNote" style="margin:10px 0">A carregar...</div>
  <div id="v5ReelsList"></div>
</div>

<div id="sec-chat" class="card section">
  <h2 style="margin-top:0">💬 Sala de Debate</h2>
  <div class="smallNote"><b id="v5Online">0</b> pessoas online</div>
  <div id="v5ChatList" class="v5Chat"></div>
  <div style="display:flex;gap:7px">
    <input id="v5ChatInput" maxlength="500" placeholder="Escreva uma mensagem..." style="margin:0">
    <button id="v5ChatSend" type="button">ENVIAR</button>
  </div>
  <button id="v5ChatHistory" type="button" class="ghost" style="margin-top:8px">VER HISTÓRICO</button>
</div>

<div id="sec-ranking" class="card section">
  <div style="display:flex;align-items:center;justify-content:space-between;gap:8px">
    <div><h2 style="margin:0">🏆 Ranking</h2><p class="smallNote">Top 10 estudantes por pontuação.</p></div>
    <button id="v5RankAdmin" type="button" title="Administrar ranking">⚙</button>
  </div>
  <div id="v5RankList" style="margin-top:10px"></div>
</div>
</main>
</div> 

<nav>
  <button type="button" data-tab="sms" class="active"><small class="navIcon">⌂</small><span>Início</span></button>
  <button type="button" data-tab="eventos"><small class="navIcon">🛍️</small><span>Marketplace</span></button>
  <button type="button" data-tab="jogos"><small class="navIcon">🏆</small><span>Jogos</span></button>
  <button type="button" data-tab="historia"><small class="navIcon">ⓘ</small><span>Sobre</span></button>
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
document.querySelectorAll('nav button[data-tab]').forEach(btn=>{
  btn.addEventListener('click', ev=>{
    const tab = ev.currentTarget.dataset.tab;
    const sec = document.getElementById('sec-'+tab);
    if(!sec) return;

    // Troca visual da aba primeiro.
    document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
    document.querySelectorAll('nav button[data-tab]').forEach(b=>b.classList.remove('active'));
    sec.classList.add('active');
    ev.currentTarget.classList.add('active');
    window.scrollTo({top:0, behavior:'smooth'});

    // Marketplace e Jogos exigem login, mas a navegação continua funcional.
    if((tab==='eventos' || tab==='jogos') && !currentUser){
      alert('Faça login para acessar esta aba.');
      document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
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
const fileInputs = {1: $('compPhoto_1'), 2: $('compPhoto_2'), 3: $('compPhoto_3'), 4: $('compPhoto_4'), 5: $('compPhoto_5'), 6: $('compPhoto_6')};

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
/* ================= CARTEIRA ================= */

const walletRef = ref(db,'wallet/');
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
    await set(uRef,{
    name,
    pass,
    phone,
    school,
    foto:photoURL,
    points:0,
    votes:0
});

/* cria carteira */

await set(ref(db,"wallet/"+phone),{

    saldo:0,

    historico:{}

});
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
  /* carteira */

const myWalletRef=ref(db,"wallet/"+phone);

onValue(myWalletRef,(snap)=>{

if(!snap.exists()) return;

const dados=snap.val();

document.getElementById("walletSaldo").innerHTML=

Number(dados.saldo||0).toLocaleString(

"pt-PT",

{

minimumFractionDigits:2

}

)+" Kz";

});
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
const numeroEvento="244941530467";

document.getElementById("btnComprarEvento").onclick=function(){

const mensagem=

"Olá! Tenho interesse em comprar este E-Book de Importaçaõ no Playmates..";

window.open(

"https://wa.me/"+numeroEvento+

"?text="+encodeURIComponent(mensagem),

"_blank"

);

};

/* Fallback visual para a imagem do Marketplace */
const marketplaceImg = $('eventoBannerImg');
if(marketplaceImg){
  marketplaceImg.addEventListener('error', ()=>{
    marketplaceImg.removeAttribute('src');
    marketplaceImg.alt = 'E-book de Importação Playmates';
    marketplaceImg.style.objectFit = 'contain';
    marketplaceImg.style.padding = '30px';
    marketplaceImg.style.background = 'linear-gradient(145deg,#eef2ff,#e2e8f0)';
  }, {once:true});
}

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
  const current = snap.exists() ? (snap.val() || {}) : {};
  const defaults = {
    1:{ name:'Concorrente 1', school:'Escola A', votes:0, photo:'' },
    2:{ name:'Concorrente 2', school:'Escola B', votes:0, photo:'' },
    3:{ name:'Concorrente 3', school:'Escola C', votes:0, photo:'' },
    4:{ name:'Concorrente 4', school:'Escola D', votes:0, photo:'' },
    5:{ name:'Concorrente 5', school:'Escola E', votes:0, photo:'' },
    6:{ name:'Concorrente 6', school:'Escola F', votes:0, photo:'' }
  };
  const updates = {};
  for(let id=1; id<=6; id++){
    if(!current[id]) updates[id] = defaults[id];
  }
  if(Object.keys(updates).length){
    await update(competitorsRef, updates);
  }
}
ensureDefaultCompetitors();

function renderCompetitors(listObj){
  competitorsList.innerHTML = '';
  [1,2,3,4,5,6].forEach(id=>{
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
  [1,2,3,4,5,6].forEach(id=>{
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
  reqName.value=''; reqSchool.value=''; reqWhats.value=''; reqPack.value='30 votos';
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
document.getElementById("walletCard").onclick=function(){

abrirCarteira();

};
function abrirCarteira(){

const saldo=document.getElementById("walletSaldo").innerHTML;

openModal(`

<h3>

💰 Carteira Digital

</h3>

<div style="

font-size:34px;

font-weight:bold;

color:#16a34a;

text-align:center;

margin:20px;

">

${saldo}

</div>

<button id="btnDeposito">

Depositar

</button>

<br><br>

<button id="btnSaque">

Sacar

</button>

<br><br>

<div id="historicoCarteira">

Nenhuma movimentação.

</div>

`,()=>{});

setTimeout(()=>{

document.getElementById("btnDeposito").onclick=()=>{

alert("Bloco 2");

};

document.getElementById("btnSaque").onclick=()=>{

alert("Bloco 3");

};

},100);

}



/* ================= PLAYMATES FINAL FEATURES ================= */
const productsV2 = [
  {id:'1',name:'E-book de Importação',price:'3.000 Kz',image:'https://diogopaixao-67.github.io/importação.png',desc:'Guia digital para aprender fundamentos de importação, fornecedores e organização de compras.'},
  {id:'2',name:'Curso de Vendas Online',price:'5.000 Kz',image:'https://via.placeholder.com/800x800/f97316/ffffff?text=VENDAS+ONLINE',desc:'Material prático para divulgação, atendimento e melhoria de conversões.'},
  {id:'3',name:'Curso de Marketing Digital',price:'7.500 Kz',image:'https://via.placeholder.com/800x800/f97316/ffffff?text=MARKETING+DIGITAL',desc:'Conteúdo introdutório para campanhas, conteúdo e presença digital.'},
  {id:'4',name:'E-book Finanças Pessoais',price:'2.500 Kz',image:'https://via.placeholder.com/800x800/f97316/ffffff?text=FINANCAS',desc:'Guia para organizar receitas, despesas, metas e hábitos financeiros.'},
  {id:'5',name:'Pack de Templates',price:'4.000 Kz',image:'https://via.placeholder.com/800x800/f97316/ffffff?text=TEMPLATES',desc:'Modelos digitais para publicações e divulgação de produtos e serviços.'},
  {id:'6',name:'Curso de Atendimento',price:'4.500 Kz',image:'https://via.placeholder.com/800x800/f97316/ffffff?text=ATENDIMENTO',desc:'Estratégias para atendimento profissional, relacionamento e retenção.'},
  {id:'7',name:'Pack Empreendedor',price:'10.000 Kz',image:'https://via.placeholder.com/800x800/f97316/ffffff?text=EMPREENDEDOR',desc:'Conjunto de materiais digitais para quem está a iniciar um pequeno negócio.'}
];
const affiliatesRefV2 = ref(db,'marketplace/affiliates');
const escV2 = v => escapeHtml(String(v ?? ''));

function closeModalV2(){
  const r=$('modalRoot');
  if(r){r.style.display='none';r.innerHTML='';}
}
function openModalV2(html){
  const r=$('modalRoot');
  r.innerHTML=`<div class="modal-back"><div class="modal v2Modal">${html}</div></div>`;
  r.style.display='block';
  return r;
}
function renderMarketplaceV2(){
  const box=$('marketplaceProductsV2');
  if(!box)return;
  box.innerHTML=productsV2.map(p=>`
    <article class="marketCard">
      <img src="${p.image}" alt="${escV2(p.name)}" onerror="this.src='https://via.placeholder.com/800x800/f97316/ffffff?text=PLAYMATES'">
      <div class="marketBody">
        <span class="marketBadge">PRODUTO DIGITAL</span>
        <h3>${escV2(p.name)}</h3>
        <div class="marketPrice">${escV2(p.price)}</div>
        <button type="button" class="marketBtn affiliateProductV2" data-id="${p.id}">AFILIAR-SE</button>
      </div>
    </article>`).join('');
  box.querySelectorAll('.affiliateProductV2').forEach(b=>b.onclick=()=>openAffiliateV2(b.dataset.id));
}
function openAffiliateV2(id){
  const p=productsV2.find(x=>x.id===id);
  if(!p)return;
  const link=location.href.split('#')[0]+'?produto='+encodeURIComponent(p.id);
  const r=openModalV2(`
    <button type="button" class="modalCloseV2" id="affiliateCloseV2">×</button>
    <img src="${p.image}" alt="${escV2(p.name)}" style="width:100%;max-height:260px;object-fit:contain;background:#fff7ed;border-radius:14px">
    <span class="marketBadge">PROGRAMA DE AFILIADOS</span>
    <h2>${escV2(p.name)}</h2>
    <p style="color:#667085;line-height:1.55">${escV2(p.desc)}</p>
    <div class="commissionV2"><strong>COMISSÃO: 50%</strong><span>Por cada venda elegível.</span></div>
    <label>Link do produto</label>
    <div class="copyRowV2"><input id="affiliateLinkV2" value="${escV2(link)}" readonly><button type="button" id="copyAffiliateV2">COPIAR</button></div>
    <p id="copyStatusV2" class="smallNote">O link será copiado para a área de transferência.</p>
    <button type="button" id="beAffiliateV2" style="width:100%;margin-top:10px">SER AFILIADO</button>
  `);
  $('affiliateCloseV2').onclick=closeModalV2;
  $('copyAffiliateV2').onclick=async()=>{
    try{await navigator.clipboard.writeText(link);$('copyStatusV2').textContent='✓ Link copiado automaticamente.'}
    catch(e){$('affiliateLinkV2').select();document.execCommand('copy');$('copyStatusV2').textContent='✓ Link copiado.'}
  };
  $('beAffiliateV2').onclick=async()=>{
    if(!currentUser)return alert('Faça login antes de se tornar afiliado.');
    await push(affiliatesRefV2,{
      productId:p.id,productName:p.name,productLink:link,
      name:currentUserObj?.name||currentUser,phone:currentUser,
      commission:'50%',ts:Date.now()
    });
    alert('Afiliado registrado com sucesso.');
    closeModalV2();
  };
}
function openAffiliateAdminV2(){
  if(prompt('Senha do administrador:')!=='2')return alert('Senha incorreta.');
  const r=openModalV2(`
    <button type="button" class="modalCloseV2" id="adminCloseV2">×</button>
    <h2>Admin — Afiliados</h2>
    <p style="color:#667085">Registos em tempo real.</p>
    <div id="affiliateListV2">A carregar...</div>
  `);
  $('adminCloseV2').onclick=closeModalV2;
  onValue(affiliatesRefV2,snap=>{
    if(!$('affiliateListV2'))return;
    const arr=[];
    if(snap.exists())snap.forEach(x=>arr.push({id:x.key,...x.val()}));
    arr.sort((a,b)=>(b.ts||0)-(a.ts||0));
    $('affiliateListV2').innerHTML=arr.length?arr.map(a=>`
      <div class="affRowV2">
        <b>${escV2(a.name||'Sem nome')}</b>
        <div>📱 ${escV2(a.phone||'')}</div>
        <div>Produto: ${escV2(a.productName||'')}</div>
        <div>Comissão: <b>50%</b></div>
        <small>${a.ts?new Date(a.ts).toLocaleString('pt-AO'):''}</small>
      </div>`).join(''):'<div style="padding:18px;text-align:center;color:#667085">Nenhum afiliado registrado.</div>';
  });
}

/* PLATFORM menu */
function openPlatformMenuV2(){
  const r=openModalV2(`
    <button type="button" class="modalCloseV2 v5Close" id="v5PlatformClose">×</button>
    <h2>Menu PLAYMATES</h2>
    <div class="v5PlatformModal">
      <button type="button" id="v5GoReels">🎬 <span><b>Reels</b><small>Vídeos curtos até 5 minutos</small></span>›</button>
      <button type="button" id="v5GoChat">💬 <span><b>Debate</b><small>Sala de bate-papo em tempo real</small></span>›</button>
      <button type="button" id="v5GoRank">🏆 <span><b>Ranking</b><small>Top 10 estudantes</small></span>›</button>
      <button type="button" id="contestMenuV2">🌍 <span><b>Concurso Internacional PLAYMATES</b><small>Inscrição no concurso</small></span>›</button>
      <button type="button" id="dollarMenuV2">💵 <span><b>Ganhar em dólar</b><small>Informações sobre oportunidades</small></span>›</button>
      <button type="button" id="mariaMenuV2">🤖 <span><b>IA Maria</b><small>FAQ da Playmates</small></span>›</button>
      <button type="button" id="sponsorMenuV2">📣 <span><b>Patrocinar</b><small>Planos de divulgação</small></span>›</button>
    </div>`);
  $('v5PlatformClose').onclick=closeModalV2;
  $('v5GoReels').onclick=()=>{closeModalV2();v5Show('reels')};
  $('v5GoChat').onclick=()=>{closeModalV2();v5Show('chat')};
  $('v5GoRank').onclick=()=>{closeModalV2();v5Show('ranking')};
  $('contestMenuV2').onclick=()=>{closeModalV2();openContestV2()};
  $('dollarMenuV2').onclick=()=>{closeModalV2();openDollarV2()};
  $('mariaMenuV2').onclick=()=>{closeModalV2();openMariaV2()};
  $('sponsorMenuV2').onclick=()=>{closeModalV2();openSponsorV2()};
}
function openContestV2(){
  const r=openModalV2(`
    <button type="button" class="modalCloseV2" id="contestCloseV2">×</button>
    <h2>Concurso Internacional PLAYMATES</h2>
    <p style="color:#667085">Preencha os dados e encaminhe a inscrição para o WhatsApp.</p>
    <input id="contestNameV2" placeholder="Nome completo">
    <input id="contestPhoneV2" type="tel" placeholder="Número de telefone">
    <input id="contestAgeV2" type="number" min="1" placeholder="Idade">
    <label>Fazer upload de BI</label>
    <input id="contestBIV2" type="file" accept="image/*,.pdf">
    <button type="button" id="contestSendV2" style="width:100%">ENVIAR</button>`);
  $('contestCloseV2').onclick=closeModalV2;
  $('contestSendV2').onclick=()=>{
    if(!$('contestNameV2').value.trim()||!$('contestPhoneV2').value.trim()||!$('contestAgeV2').value||!$('contestBIV2').files[0])
      return alert('Preencha todos os campos e faça upload do BI.');
    const msg=`Olá! Quero inscrever-me no Concurso Internacional PLAYMATES.%0ANome: ${encodeURIComponent($('contestNameV2').value)}%0ATelefone: ${encodeURIComponent($('contestPhoneV2').value)}%0AIdade: ${encodeURIComponent($('contestAgeV2').value)}%0AO BI foi selecionado no formulário.`;
    window.open('https://wa.me/244941530467?text='+msg,'_blank');
    alert('No WhatsApp, anexe o BI selecionado antes de enviar.');
  };
}
function openDollarV2(){
  const r=openModalV2(`<button type="button" class="modalCloseV2" id="dollarCloseV2">×</button><h2>Ganhar em dólar</h2><div class="commissionV2">Explore oportunidades de divulgação, afiliação e campanhas disponibilizadas pela Playmates.<br><br>Crie a sua conta, escolha uma oportunidade elegível, divulgue e acompanhe os resultados.</div><p class="smallNote">Os resultados dependem das vendas e das condições de cada campanha. Não há garantia de rendimento.</p>`);
  $('dollarCloseV2').onclick=closeModalV2;
}
function openMariaV2(){
  const r=openModalV2(`
    <button type="button" class="modalCloseV2" id="mariaCloseV2">×</button>
    <h2>IA Maria</h2><p style="color:#667085">Assistente de FAQ da PLAYMATES.</p>
    <div id="mariaChatV2" class="mariaChatV2"><div class="mariaMsgV2 bot">Olá! Pergunte sobre cadastro, Marketplace, afiliados, concurso ou patrocínio.</div></div>
    <div class="copyRowV2"><input id="mariaQuestionV2" placeholder="Escreva a sua pergunta"><button type="button" id="mariaAskV2">ENVIAR</button></div>`);
  $('mariaCloseV2').onclick=closeModalV2;
  const faq=[[/cadastro|conta/i,'Para criar conta, vá à aba Início e selecione Criar conta.'],[/marketplace|produto/i,'No Marketplace escolha um produto e clique em AFILIAR-SE.'],[/afiliad/i,'O programa de afiliados apresentado oferece comissão de 50%.'],[/concurso/i,'A inscrição do Concurso Internacional PLAYMATES é feita pelo formulário e encaminhada para o WhatsApp indicado.'],[/patrocin/i,'Patrocinar permite solicitar divulgação de um produto pelos planos apresentados.'],[/d[oó]lar|ganhar/i,'Consulte Ganhar em dólar no menu para ver as informações sobre oportunidades.']];
  const answer=q=>{for(const [rx,a] of faq)if(rx.test(q))return a;return'Posso responder apenas perguntas frequentes da Playmates sobre cadastro, Marketplace, afiliados, concurso, patrocínio ou ganhar em dólar.'};
  $('mariaAskV2').onclick=()=>{const q=$('mariaQuestionV2').value.trim();if(!q)return;$('mariaChatV2').insertAdjacentHTML('beforeend',`<div class="mariaMsgV2 user">${escV2(q)}</div><div class="mariaMsgV2 bot">${answer(q)}</div>`);$('mariaQuestionV2').value='';};
  $('mariaQuestionV2').onkeydown=e=>{if(e.key==='Enter')$('mariaAskV2').click()};
}
function openSponsorV2(){
  const r=openModalV2(`
    <button type="button" class="modalCloseV2" id="sponsorCloseV2">×</button>
    <h2>Patrocinar</h2>
    <div class="sponsorPlansV2"><div><b>1 DIA</b><strong>5.000 Kz</strong></div><div><b>1 SEMANA</b><strong>15.000 Kz</strong></div><div><b>1 MÊS</b><strong>26.000 Kz</strong></div></div>
    <input id="sponsorNameV2" placeholder="Nome / empresa">
    <input id="sponsorPhoneV2" placeholder="Número de telefone">
    <input id="sponsorProductV2" placeholder="Nome do produto">
    <select id="sponsorPlanV2"><option>1 DIA — 5.000 Kz</option><option>1 SEMANA — 15.000 Kz</option><option>1 MÊS — 26.000 Kz</option></select>
    <textarea id="sponsorInfoV2" placeholder="Informação do produto"></textarea>
    <button type="button" id="sponsorSendV2" style="width:100%">ENVIAR PEDIDO</button>`);
  $('sponsorCloseV2').onclick=closeModalV2;
  $('sponsorSendV2').onclick=()=>{
    if(!$('sponsorNameV2').value.trim()||!$('sponsorPhoneV2').value.trim()||!$('sponsorProductV2').value.trim())return alert('Preencha nome, telefone e produto.');
    const msg=`Olá! Quero patrocinar um produto na PLAYMATES.%0ANome: ${encodeURIComponent($('sponsorNameV2').value)}%0ATelefone: ${encodeURIComponent($('sponsorPhoneV2').value)}%0AProduto: ${encodeURIComponent($('sponsorProductV2').value)}%0APlano: ${encodeURIComponent($('sponsorPlanV2').value)}%0ADescrição: ${encodeURIComponent($('sponsorInfoV2').value)}`;
    window.open('https://wa.me/244941530467?text='+msg,'_blank');
  };
}

renderMarketplaceV2();
$('affiliateAdminBtnV2').onclick=openAffiliateAdminV2;
$('platformMenuBtn').onclick=openPlatformMenuV2;


/* ================= FINAL V5: REELS + DEBATE + RANKING ================= */
const v5Show = tab => {
  document.querySelectorAll('.section').forEach(x=>x.classList.remove('active'));
  document.querySelectorAll('nav button[data-tab]').forEach(x=>x.classList.remove('active'));
  const sec=document.getElementById('sec-'+tab);
  if(sec)sec.classList.add('active');
  window.scrollTo({top:0,behavior:'smooth'});
  if(tab==='reels')v5LoadReels();
  if(tab==='chat')v5LoadChat();
  if(tab==='ranking')v5LoadRanking();
};

const v5Esc = x => String(x??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));

const v5ReelsRef=ref(db,'reels/');
const v5CommentsRef=id=>ref(db,'reelsComments/'+id);
let v5ReelsStarted=false;

function v5LoadReels(){
  if(v5ReelsStarted)return;
  v5ReelsStarted=true;
  onValue(v5ReelsRef,snap=>{
    const a=[]; if(snap.exists())snap.forEach(x=>a.push({id:x.key,...x.val()}));
    a.sort((x,y)=>(y.ts||0)-(x.ts||0));
    $('v5ReelsCount').textContent=a.length+' vídeos disponíveis';
    $('v5ReelsList').innerHTML=a.map(x=>`
      <article class="v5Reel" data-reel="${v5Esc(x.id)}">
        <video class="v5ReelVideo" src="${v5Esc(x.url||'')}" controls playsinline preload="metadata"></video>
        <div class="v5ReelMeta">
          <h3 style="margin:0 0 5px">${v5Esc(x.title||'Reel PLAYMATES')}</h3>
          <p style="margin:0 0 8px">${v5Esc(x.description||'')}</p>
          <button type="button" class="v5Share" data-url="${v5Esc(x.url||'')}">↗ REENCAMINHAR</button>
        </div>
        <div class="v5CommentBox">
          <div id="v5Comments-${v5Esc(x.id)}">A carregar comentários...</div>
          <div style="display:flex;gap:6px;margin-top:8px">
            <input id="v5CommentInput-${v5Esc(x.id)}" placeholder="Escreva um comentário..." maxlength="300" style="margin:0">
            <button type="button" class="v5CommentSend" data-id="${v5Esc(x.id)}">ENVIAR</button>
          </div>
        </div>
      </article>`).join('') || '<div class="smallNote">Nenhum Reel publicado ainda.</div>';

    a.forEach(x=>v5WatchComments(x.id));
    document.querySelectorAll('.v5Share').forEach(b=>b.onclick=async()=>{
      try{
        if(navigator.share) await navigator.share({title:'PLAYMATES Reel',url:b.dataset.url});
        else {await navigator.clipboard.writeText(b.dataset.url);alert('Link copiado.')}
      }catch(e){}
    });
    document.querySelectorAll('.v5CommentSend').forEach(b=>b.onclick=()=>v5SendComment(b.dataset.id));
  });
}
function v5WatchComments(id){
  onValue(v5CommentsRef(id),snap=>{
    const box=$('v5Comments-'+id);if(!box)return;
    const a=[];if(snap.exists())snap.forEach(x=>a.push(x.val()));
    a.sort((x,y)=>(x.ts||0)-(y.ts||0));
    box.innerHTML=a.map(c=>`<div class="v5Comment"><b>${v5Esc(c.name||c.user||'Utilizador')}</b>: ${v5Esc(c.text||'')}<div class="smallNote">${c.ts?new Date(c.ts).toLocaleString('pt-AO'):''}</div></div>`).join('')||'<div class="smallNote">Ainda sem comentários.</div>';
  });
}
async function v5SendComment(id){
  if(!currentUser)return alert('Faça login para comentar.');
  const i=$('v5CommentInput-'+id),text=i?.value.trim();if(!text)return;
  await push(v5CommentsRef(id),{user:currentUser,name:currentUserObj?.name||currentUser,text,ts:Date.now()});
  i.value='';
}

$('v5ReelsAdmin').onclick=async()=>{
  if(prompt('Senha para publicar Reels:')!=='4')return alert('Senha incorreta.');
  const r=openModalV2(`
    <button type="button" class="modalCloseV2 v5Close" id="v5ReelClose">×</button>
    <h2>Publicar Reel</h2>
    <input id="v5ReelTitle" placeholder="Título">
    <textarea id="v5ReelDesc" placeholder="Descrição"></textarea>
    <input id="v5ReelUrl" type="url" placeholder="URL direta do vídeo (MP4)">
    <p class="smallNote">O vídeo deve ter no máximo 5 minutos e ser acessível pelo navegador.</p>
    <button id="v5ReelPublish" type="button" style="width:100%">PUBLICAR</button>`);
  $('v5ReelClose').onclick=closeModalV2;
  $('v5ReelPublish').onclick=async()=>{
    const title=$('v5ReelTitle').value.trim(),desc=$('v5ReelDesc').value.trim(),url=$('v5ReelUrl').value.trim();
    if(!title||!url)return alert('Preencha título e URL.');
    const v=document.createElement('video');v.preload='metadata';v.src=url;
    v.onloadedmetadata=async()=>{
      if(v.duration>300)return alert('O vídeo ultrapassa 5 minutos.');
      await push(v5ReelsRef,{title,description:desc,url,ts:Date.now(),author:currentUser||'admin'});
      alert('Reel publicado com sucesso.');closeModalV2();
    };
    v.onerror=()=>alert('Não foi possível validar a URL do vídeo.');
  };
};

/* DEBATE */
const v5ChatRef=ref(db,'debate/messages');
const v5PresenceRef=ref(db,'debate/presence');
let v5ChatStarted=false;
function v5LoadChat(){
  if(v5ChatStarted)return;v5ChatStarted=true;
  onValue(v5ChatRef,snap=>{
    const a=[];if(snap.exists())snap.forEach(x=>a.push({id:x.key,...x.val()}));
    a.sort((x,y)=>(x.ts||0)-(y.ts||0));
    $('v5ChatList').innerHTML=a.map(m=>`
      <div class="v5Bubble ${m.user===currentUser?'me':''}">
        <div><b>${v5Esc(m.name||m.user||'Utilizador')}</b></div>
        <div>${v5Esc(m.text||'')}</div>
        <div class="smallNote">${m.ts?new Date(m.ts).toLocaleString('pt-AO'):''}</div>
        <button type="button" class="ghost v5Reply" data-name="${v5Esc(m.name||m.user||'Utilizador')}">RESPONDER</button>
      </div>`).join('')||'<div class="smallNote">Ainda não há mensagens.</div>';
    $('v5ChatList').scrollTop=$('v5ChatList').scrollHeight;
    document.querySelectorAll('.v5Reply').forEach(b=>b.onclick=()=>{$('v5ChatInput').value='@'+b.dataset.name+' ';$('v5ChatInput').focus()});
  });
  onValue(v5PresenceRef,snap=>{
    let n=0,now=Date.now();if(snap.exists())snap.forEach(x=>{if(now-(x.val()?.ts||0)<90000)n++});
    $('v5Online').textContent=n;
  });
}
$('v5ChatSend').onclick=async()=>{
  if(!currentUser)return alert('Faça login para participar no Debate.');
  const i=$('v5ChatInput'),text=i.value.trim();if(!text)return;
  await push(v5ChatRef,{user:currentUser,name:currentUserObj?.name||currentUser,text,ts:Date.now()});i.value='';
};
$('v5ChatInput').onkeydown=e=>{if(e.key==='Enter')$('v5ChatSend').click()};
$('v5ChatHistory').onclick=()=>{v5LoadChat();alert('O histórico disponível está apresentado na sala de Debate.')};

/* RANKING */
const v5RankRef=ref(db,'ranking/students');
const v5RankDefaults=[
 ['Estudante 1','Escola PLAYMATES 1',9850,'100.000 Kz',12],
 ['Estudante 2','Escola PLAYMATES 2',9200,'75.000 Kz',32],
 ['Estudante 3','Escola PLAYMATES 3',8750,'50.000 Kz',5],
 ['Estudante 4','Escola PLAYMATES 4',8300,'40.000 Kz',44],
 ['Estudante 5','Escola PLAYMATES 5',7900,'30.000 Kz',47],
 ['Estudante 6','Escola PLAYMATES 6',7400,'25.000 Kz',13],
 ['Estudante 7','Escola PLAYMATES 7',6900,'20.000 Kz',49],
 ['Estudante 8','Escola PLAYMATES 8',6400,'15.000 Kz',56],
 ['Estudante 9','Escola PLAYMATES 9',5900,'10.000 Kz',8],
 ['Estudante 10','Escola PLAYMATES 10',5400,'5.000 Kz',15]
];
async function v5SeedRank(){
  const snap=await get(v5RankRef);if(snap.exists())return;
  for(const x of v5RankDefaults)await push(v5RankRef,{name:x[0],school:x[1],score:x[2],prize:x[3],photo:'https://i.pravatar.cc/150?img='+x[4]});
}
let v5RankStarted=false;
function v5LoadRanking(){
  if(v5RankStarted)return;v5RankStarted=true;
  onValue(v5RankRef,snap=>{
    const a=[];if(snap.exists())snap.forEach(x=>a.push({id:x.key,...x.val()}));
    a.sort((x,y)=>Number(y.score||0)-Number(x.score||0));
    $('v5RankList').innerHTML=a.slice(0,10).map((x,i)=>`
      <div class="v5Rank">
        <img src="${v5Esc(x.photo||'https://via.placeholder.com/100/f97316/ffffff?text=PLAY')}" onerror="this.src='https://via.placeholder.com/100/f97316/ffffff?text=PLAY'">
        <div><span class="v5RankNo">#${i+1}</span> <span class="v5RankName">${v5Esc(x.name||'')}</span>
          <div class="v5RankSchool">${v5Esc(x.school||'')}</div>
          <div class="v5RankScore">${Number(x.score||0).toLocaleString('pt-AO')} pontos</div>
        </div>
        <div class="v5RankPrize">${v5Esc(x.prize||'')}</div>
      </div>`).join('')||'<div class="smallNote">Nenhum estudante.</div>';
  });
}
$('v5RankAdmin').onclick=async()=>{
  if(prompt('Senha do administrador do Ranking:')!=='7')return alert('Senha incorreta.');
  const snap=await get(v5RankRef),a=[];if(snap.exists())snap.forEach(x=>a.push({id:x.key,...x.val()}));
  const r=openModalV2(`
    <button type="button" class="modalCloseV2 v5Close" id="v5RankClose">×</button>
    <h2>Administração do Ranking</h2>
    <p class="smallNote">Edite nome, escola, pontuação, prémio e URL da fotografia.</p>
    <div id="v5RankAdminList"></div>
    <button id="v5RankAdd" type="button" style="width:100%">+ ADICIONAR ESTUDANTE</button>`);
  $('v5RankClose').onclick=closeModalV2;
  $('v5RankAdminList').innerHTML=a.map(x=>`
    <div class="v5AdminRow">
      <input data-k="name" data-id="${x.id}" value="${v5Esc(x.name||'')}" placeholder="Nome">
      <input data-k="school" data-id="${x.id}" value="${v5Esc(x.school||'')}" placeholder="Escola">
      <input data-k="score" data-id="${x.id}" type="number" value="${Number(x.score||0)}" placeholder="Pontuação">
      <input data-k="prize" data-id="${x.id}" value="${v5Esc(x.prize||'')}" placeholder="Prémio">
      <input data-k="photo" data-id="${x.id}" value="${v5Esc(x.photo||'')}" placeholder="URL da foto">
      <button type="button" class="v5RankSave" data-id="${x.id}" style="width:100%">GUARDAR</button>
    </div>`).join('');
  document.querySelectorAll('.v5RankSave').forEach(b=>b.onclick=async()=>{
    const d={};document.querySelectorAll(`[data-id="${b.dataset.id}"][data-k]`).forEach(i=>d[i.dataset.k]=i.value);
    d.score=Number(d.score||0);
    await update(ref(db,'ranking/students/'+b.dataset.id),d);alert('Estudante atualizado.');
  });
  $('v5RankAdd').onclick=async()=>{
    await push(v5RankRef,{name:'Novo estudante',school:'Nova escola',score:0,prize:'0 Kz',photo:'https://via.placeholder.com/100/f97316/ffffff?text=PLAY'});
    alert('Estudante adicionado.');
  };
};

/* Inicialização segura. Não altera o menu original nem o Marketplace existente. */
v5SeedRank().catch(console.error);

</script>

<footer style="width:100%; text-align:center; padding:15px; margin-top:20px;
background:#f2f2f2; color:#333; font-size:14px; border-top:1px solid #ddd;">
  © 2023–2026 Playmates • Todos os direitos reservados  
  | <a href="#" style="color:#333; text-decoration:underline;">Termos de Uso</a>
</footer>

</body>
</html>
