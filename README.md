<!doctype html>
<html lang="pt">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Playmates — Feed (Realtime)</title>
<style>
  /* (mantive o CSS praticamente igual ao teu original para não mudar a interface) */
  body { background: #f0f2f5; font-family: "Segoe UI", sans-serif; margin:0; padding:0; }
  header { background:#ff7e15;color:#fff;text-align:center;padding:12px;font-size:1.5em;font-weight:bold; }
  #stats { background:#fff;text-align:center;padding:10px 5px;font-size:15px;color:#333;box-shadow:0 2px 4px rgba(0,0,0,0.1);display:flex;justify-content:space-around;}
  #filters { display:flex; justify-content:center; gap:10px; padding:10px; }
  #filters button { background:#fff;border:1px solid #ccc;border-radius:8px;padding:6px 10px;cursor:pointer;}
  #filters button.active { background:#ff7e15;color:#fff;border-color:#ff7e15; }
  main { max-width:600px;margin:0 auto;padding:10px; }
  .post { background:#fff;border-radius:12px; box-shadow:0 1px 3px rgba(0,0,0,0.1); margin-bottom:12px; padding:10px; }
  .author { font-weight:bold;color:#1877f2;margin-bottom:5px; }
  .text { margin:6px 0; white-space:pre-wrap; }
  .media { width:100%; border-radius:10px; margin-top:8px; cursor:pointer; }
  .reactions { display:flex; gap:6px; margin-top:8px; }
  .reaction-btn { background:transparent;border:none;cursor:pointer;font-size:20px;}
  .views { font-size:13px;color:#555;margin-top:5px; }
  .comments { border-top:1px solid #ddd;margin-top:8px;padding-top:6px; }
  .comment { background:#f5f6f7;border-radius:8px;padding:6px 8px;margin-top:5px; }
  .comment strong { color:#1877f2; }
  .reply { margin-left:20px;background:#e9ebee; }
  .comment-input { display:flex; gap:6px; margin-top:6px; }
  .comment-input input { flex:1; padding:6px; border-radius:8px; border:1px solid #ccc; }
  .comment-input button { background:#1877f2;color:#fff;border:none;border-radius:8px;padding:6px 10px;cursor:pointer;}
  .reply-btn { background:none;border:none;color:#1877f2;cursor:pointer;font-size:13px;padding:0; }
  #postBtn { position:fixed; bottom:15px; right:15px; background:#1877f2;color:white;border:none;border-radius:50%; width:55px;height:55px;font-size:28px;cursor:pointer; box-shadow:0 4px 10px rgba(0,0,0,0.3); z-index:99; }
  #resetBtn { background:transparent;color:#666;border:none;font-size:13px;opacity:0.5;cursor:pointer;position:absolute;top:15px;right:15px; }
  #resetBtn:hover { color:red; opacity:1; }
  #overlay { position:fixed; inset:0; background:rgba(0,0,0,0.85); display:none; align-items:center; justify-content:center; z-index:999; }
  #overlay img,#overlay video { max-width:95%; max-height:90%; border-radius:10px; }
  #closeOverlay { position:absolute; top:15px; right:15px; font-size:28px; color:white; cursor:pointer; background:rgba(0,0,0,0.4); border-radius:50%; width:40px; height:40px; text-align:center; line-height:38px; }
  #postArea { background:white;border-radius:12px;padding:10px;box-shadow:0 2px 8px rgba(0,0,0,0.1); position:fixed; bottom:70px; left:50%; transform:translateX(-50%); width:90%; max-width:500px; display:none; z-index:10; }
  textarea { width:100%; border:1px solid #ccc; border-radius:8px; resize:none; padding:8px; font-size:15px; }
  input[type=file] { margin-top:6px; }
  #submitPost { margin-top:6px; background:#1877f2;color:white;border:none;padding:8px 16px;border-radius:8px;cursor:pointer; }
</style>
</head>
<body>

<header>Feed — Playmates</header>
<button id="resetBtn">⟳</button>

<div id="stats">
  <div>👁️ Visualizações: <span id="totalViews">0</span></div>
  <div>📝 Postagens: <span id="totalPosts">0</span></div>
</div>

<div id="filters">
  <button class="active" data-filter="all">📰 Todos</button>
  <button data-filter="image">🖼️ Imagens</button>
  <button data-filter="video">📹 Vídeos</button>
</div>

<main id="feed"></main>

<div id="overlay"><span id="closeOverlay">❌</span></div>

<div id="postArea">
  <textarea id="postText" placeholder="O que está pensando, Playmate?" rows="3"></textarea>
  <input type="file" id="mediaInput" accept="image/*,video/*">
  <button id="submitPost">Postar</button>
</div>

<button id="postBtn">＋</button>

<!-- Firebase (compat) -->
<script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-database-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-storage-compat.js"></script>

<script>
/* ---------- CONFIG (tu forneceste) ---------- */
const firebaseConfig = {
  apiKey: "AIzaSyClzY30up3gZTsgIqT1b_nYW7EHpKpwcaI",
  authDomain: "playmates-cc4f7.firebaseapp.com",
  databaseURL: "https://playmates-cc4f7-default-rtdb.firebaseio.com",
  projectId: "playmates-cc4f7",
  storageBucket: "playmates-cc4f7.firebasestorage.app",
  messagingSenderId: "104004735810",
  appId: "1:104004735810:web:d3ee9a75399d6f0f222edb"
};
/* ---------- INIT FIREBASE ---------- */
firebase.initializeApp(firebaseConfig);
const db = firebase.database();
const storage = firebase.storage();

/* ---------- UI refs ---------- */
const feed = document.getElementById('feed');
const postBtn = document.getElementById('postBtn');
const resetBtn = document.getElementById('resetBtn');
const overlay = document.getElementById('overlay');
const postArea = document.getElementById('postArea');
const submitPost = document.getElementById('submitPost');
const postText = document.getElementById('postText');
const mediaInput = document.getElementById('mediaInput');
const totalViews = document.getElementById('totalViews');
const totalPosts = document.getElementById('totalPosts');
const filters = document.querySelectorAll('#filters button');

const user = "Playmate"; // sem autenticação - podes ligar Firebase Auth depois
let selectedMedia = null;
let currentFilter = "all";

/* ---------- ESCUTA POSTS EM TEMPO REAL ---------- */
const postsRef = db.ref('posts');

postsRef.on('value', snapshot => {
  const data = snapshot.val() || {};
  // transforma em array ordenada por timestamp (id é a chave gerada pelo push)
  const arr = Object.keys(data).map(k => ({ id: k, ...data[k] }));
  // adiciona ordenação descendente: posts mais recentes primeiro (assumindo timestamp)
  arr.sort((a,b)=>{
    const ta = a.createdAt || 0;
    const tb = b.createdAt || 0;
    return tb - ta;
  });
  renderFeed(arr);
});

/* ---------- FUNÇÕES DE RENDER ---------- */
function renderFeed(posts) {
  feed.innerHTML = "";
  let totalV = 0;
  posts.forEach(post => {
    if(currentFilter !== 'all' && post.type !== currentFilter) return;
    addPostToDOM(post);
    totalV += (post.views||0);
  });
  totalViews.textContent = Math.min(totalV,1000000).toLocaleString();
  totalPosts.textContent = posts.length;
}

function addPostToDOM(post) {
  const div = document.createElement('div');
  div.className = "post";
  div.innerHTML = `
    <div class="author">${escapeHtml(post.author||'Playmate')}</div>
    <div class="text">${escapeHtml(post.text||'')}</div>
    ${post.media ? 
      (post.type === 'image'
        ? `<img src="${post.media}" class="media" alt="imagem">`
        : `<video src="${post.media}" class="media" controls></video>`) 
      : ""}
    <div class="views">👁️ ${Math.min(post.views||0,1000000)} visualizações</div>
    <div class="reactions">
      ${['😍','😂','👍','😡','❤️'].map(e=>`<button class="reaction-btn" data-emoji="${e}">${e} <span>${(post.reactions && post.reactions[e])||0}</span></button>`).join('')}
    </div>
    <div class="comments">
      <div class="comment-list"></div>
      <div class="comment-input">
        <input type="text" placeholder="Escreva um comentário...">
        <button>Comentar</button>
      </div>
    </div>
  `;
  feed.appendChild(div);

  const listEl = div.querySelector('.comment-list');
  renderComments(post.comments || {}, listEl, post);

  const mediaEl = div.querySelector('.media');
  if(mediaEl){
    if(post.type === 'video'){
      mediaEl.addEventListener('play', () => increaseViews(post.id));
    } else {
      mediaEl.onclick = () => { showMedia(post); increaseViews(post.id); };
    }
  }

  div.querySelectorAll('.reaction-btn').forEach(btn=>{
    btn.onclick = async () => {
      const emoji = btn.dataset.emoji;
      // rate-limit por browser (30s) para evitar spam do mesmo navegador
      const key = `react_${post.id}`;
      const last = parseInt(localStorage.getItem(key) || "0",10);
      if(Date.now() - last < 30000){ alert("Espere 30s para reagir novamente."); return; }
      localStorage.setItem(key, Date.now());
      // usa transaction para evitar problemas com vários utilizadores
      const path = `posts/${post.id}/reactions/${escapePath(emoji)}`;
      const rRef = db.ref(path);
      rRef.transaction(curr => (curr||0)+1);
    };
  });

  const commentBtn = div.querySelector('.comment-input button');
  const commentInput = div.querySelector('.comment-input input');
  commentBtn.onclick = () => {
    const text = commentInput.value.trim();
    if(!text) return;
    const comment = { author: user, text, createdAt: Date.now() };
    // push comment object under posts/{postId}/comments/
    db.ref(`posts/${post.id}/comments`).push(comment);
    commentInput.value = "";
  };
}

/* ---------- RENDER COMMENTS (recursivo para replies) ---------- */
function renderComments(commentsObj, parentEl, post) {
  parentEl.innerHTML = "";
  // commentsObj é um object: { commentId: {author,text,createdAt, replies: { ... } } }
  const keys = Object.keys(commentsObj || {});
  keys.forEach(key => {
    const c = commentsObj[key];
    const commentEl = document.createElement('div');
    commentEl.className = "comment " + (c.parentId ? 'reply' : '');
    commentEl.innerHTML = `<strong>${escapeHtml(c.author||'')}</strong>: ${escapeHtml(c.text||'')}
      <div><button class="reply-btn">Responder</button></div>
      <div class="replies"></div>
    `;
    parentEl.appendChild(commentEl);

    const replyBtn = commentEl.querySelector('.reply-btn');
    const repliesEl = commentEl.querySelector('.replies');

    // se houver replies, renderiza recursivamente
    if(c.replies) renderComments(c.replies, repliesEl, post);

    replyBtn.onclick = () => {
      if(commentEl.querySelector('.comment-input')) return;
      const replyBox = document.createElement('div');
      replyBox.className = "comment-input";
      replyBox.innerHTML = `<input type="text" placeholder="Responda a ${escapeHtml(c.author||'...')}..."><button>Enviar</button>`;
      commentEl.appendChild(replyBox);
      const input = replyBox.querySelector('input');
      replyBox.querySelector('button').onclick = () => {
        const text = input.value.trim();
        if(!text) return;
        // push reply under posts/{postId}/comments/{commentId}/replies
        db.ref(`posts/${post.id}/comments/${key}/replies`).push({
          author: user,
          text,
          createdAt: Date.now(),
          parentId: key
        });
        // remove caixa
        replyBox.remove();
      };
    };
  });
}

/* ---------- MEDIA OVERLAY ---------- */
function showMedia(post){
  overlay.style.display = "flex";
  overlay.innerHTML = `<span id="closeOverlay">❌</span>` + 
    (post.type === 'image' ? `<img src="${post.media}">` : `<video src="${post.media}" controls autoplay></video>`);
  document.getElementById('closeOverlay').onclick = ()=> overlay.style.display="none";
}

/* ---------- INCREMENT VIEWS (transaction para segurança) ---------- */
function increaseViews(postId){
  if(!postId) return;
  const viewsRef = db.ref(`posts/${postId}/views`);
  viewsRef.transaction(v => (v||0) + 1);
}

/* ---------- SUBMIT POST (com upload opcional de media) ---------- */
mediaInput.onchange = e => selectedMedia = e.target.files ? e.target.files[0] : null;

submitPost.onclick = async () => {
  const senha = localStorage.getItem('postedPw') || null;
  // o postArea só aparece após prompt de password no postBtn, mas deixo check igual
  const text = postText.value.trim();
  if(!text && !selectedMedia){ alert("Escreva algo ou envie mídia!"); return; }
  // se houver media -> upload para Storage primeiro
  if(selectedMedia){
    // cria caminho único: media/{timestamp}_{filename}
    const fname = `${Date.now()}_${selectedMedia.name.replace(/\s+/g,'_')}`;
    const storageRef = storage.ref(`media/${fname}`);
    try {
      const snap = await storageRef.put(selectedMedia);
      const url = await snap.ref.getDownloadURL();
      // cria o post com media
      createPost(text, url, selectedMedia.type.startsWith('video') ? 'video' : 'image');
    } catch(err){
      alert("Erro ao enviar mídia: " + err.message);
    }
  } else {
    createPost(text, null, null);
  }
};

function createPost(text, mediaUrl, type){
  const post = {
    author: user,
    text: text,
    media: mediaUrl || null,
    type: type || null,
    reactions: {}, // inicialmente vazio
    views: 0,
    createdAt: Date.now()
  };
  // push para /posts
  postsRef.push(post).then(()=>{
    // limpar UI
    postText.value = "";
    mediaInput.value = "";
    selectedMedia = null;
    postArea.style.display = "none";
  }).catch(err => alert("Erro ao criar post: " + err.message));
}

/* ---------- FILTROS ---------- */
filters.forEach(btn=>{
  btn.onclick = () => {
    filters.forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    currentFilter = btn.dataset.filter;
    // força re-render pedindo snapshot atual
    postsRef.once('value').then(snapshot=>{
      const data = snapshot.val() || {};
      const arr = Object.keys(data).map(k => ({ id:k, ...data[k] }));
      arr.sort((a,b)=> (b.createdAt||0) - (a.createdAt||0));
      renderFeed(arr);
    });
  };
});

/* ---------- POST BUTTON / PASSWORD PROMPT ---------- */
postBtn.onclick = ()=>{
  const senha = prompt("Senha para postar:");
  if(senha !== "LEX"){ alert("Senha incorreta!"); return; }
  // grava local para evitar pedir outra vez durante a sessão
  localStorage.setItem('postedPw','LEX');
  postArea.style.display = "block";
};

/* ---------- RESET (apaga tudo no DB) ---------- */
resetBtn.onclick = ()=>{
  const senha = prompt("Senha para resetar:");
  if(senha !== "LEX"){ alert("Senha incorreta!"); return; }
  if(confirm("Apagar tudo (posts) para todos os utilizadores?")){
    postsRef.remove().then(()=> {
      alert("Posts apagados.");
    }).catch(err => alert("Erro ao apagar: " + err.message));
  }
};

/* ---------- UTILS ---------- */
function escapeHtml(str){
  if(!str) return "";
  return String(str)
    .replace(/&/g,'&amp;')
    .replace(/</g,'&lt;')
    .replace(/>/g,'&gt;')
    .replace(/"/g,'&quot;')
    .replace(/'/g,'&#039;');
}
function escapePath(s){ return encodeURIComponent(String(s)); }
</script>
</body>
</html>
