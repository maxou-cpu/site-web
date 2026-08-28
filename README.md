<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Missive</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,500;0,700;1,600&family=EB+Garamond:ital,wght@0,400;0,500;1,400&family=Special+Elite&family=Caveat:wght@600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --paper: #F3EEE1;
    --paper-dark: #E7DFC9;
    --ink: #2B2A26;
    --ink-soft: #55503F;
    --seal: #8B2E2E;
    --seal-dark: #6E2020;
    --postal: #3D4C5E;
    --line: #C9BFA3;
    --shadow: rgba(43,42,38,0.18);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;height:100%;}
  body{
    background:
      radial-gradient(ellipse at top left, rgba(255,255,255,0.35), transparent 60%),
      var(--paper);
    background-attachment:fixed;
    font-family:'EB Garamond', serif;
    color:var(--ink);
    min-height:100vh;
    position:relative;
  }
  body::before{
    content:"";
    position:fixed; inset:0;
    background-image:
      repeating-linear-gradient(0deg, rgba(0,0,0,0.015) 0px, rgba(0,0,0,0.015) 1px, transparent 1px, transparent 3px);
    pointer-events:none;
    opacity:0.5;
    z-index:0;
  }
  #app{position:relative; z-index:1; min-height:100vh; display:flex; flex-direction:column;}

  h1,h2,h3{font-family:'Playfair Display', serif; margin:0; letter-spacing:0.01em;}

  /* ---------- SCREEN: ENTRY ---------- */
  #entry-screen{
    flex:1; display:flex; align-items:center; justify-content:center;
    padding:32px 18px;
  }
  .envelope{
    width:100%; max-width:440px;
    background:
      linear-gradient(180deg, rgba(255,255,255,0.25), transparent 30%),
      var(--paper-dark);
    border:1px solid var(--line);
    box-shadow: 0 18px 40px var(--shadow), inset 0 0 0 1px rgba(255,255,255,0.4);
    padding:38px 34px 30px;
    position:relative;
  }
  .envelope::before, .envelope::after{
    content:"";
    position:absolute; top:0; left:0; right:0; height:0;
    border-left:220px solid transparent;
    border-right:220px solid transparent;
    border-top:110px solid rgba(0,0,0,0.045);
  }
  .envelope::after{ display:none; }
  .brand{
    text-align:center; font-family:'Playfair Display', serif; font-style:italic;
    font-weight:600; font-size:15px; letter-spacing:0.14em; text-transform:uppercase;
    color:var(--seal); margin-bottom:14px;
  }
  .eyebrow{
    font-family:'Special Elite', monospace;
    font-size:11.5px; letter-spacing:0.18em; text-transform:uppercase;
    color:var(--postal); text-align:center; margin-bottom:6px;
  }
  .envelope h1{
    text-align:center; font-size:30px; margin-bottom:6px; color:var(--ink);
  }
  .envelope .sub{
    text-align:center; color:var(--ink-soft); font-size:16.5px; margin-bottom:30px; font-style:italic;
  }
  .field{ margin-bottom:20px; }
  .field label{
    display:block; font-family:'Special Elite', monospace; font-size:11px;
    letter-spacing:0.12em; text-transform:uppercase; color:var(--ink-soft); margin-bottom:7px;
  }
  .field input{
    width:100%; padding:11px 4px; font-family:'EB Garamond', serif; font-size:19px;
    background:transparent; border:none; border-bottom:1.5px solid var(--line);
    color:var(--ink); outline:none; transition:border-color .2s;
  }
  .field input:focus{ border-color:var(--seal); }
  .hint{ font-size:13.5px; color:var(--ink-soft); margin-top:-10px; margin-bottom:24px; line-height:1.5; }

  .seal-btn{
    display:block; margin:26px auto 4px; width:88px; height:88px; border-radius:50%;
    border:none; cursor:pointer; position:relative;
    background: radial-gradient(circle at 32% 30%, #b64444, var(--seal) 55%, var(--seal-dark) 100%);
    box-shadow: 0 8px 16px rgba(139,46,46,0.4), inset 0 -4px 8px rgba(0,0,0,0.25), inset 0 3px 6px rgba(255,255,255,0.25);
    font-family:'Playfair Display', serif; color:#f3e9dc; font-size:13px; letter-spacing:0.06em;
    transition: transform .12s ease;
  }
  .seal-btn:active{ transform:scale(0.92); }
  .seal-btn span{ position:relative; z-index:1; }
  .seal-btn::after{
    content:""; position:absolute; inset:6px; border-radius:50%;
    border:1px dashed rgba(243,233,220,0.5);
  }
  .error-msg{ text-align:center; color:var(--seal); font-size:14px; min-height:18px; margin-top:14px; font-style:italic; }

  /* ---------- SCREEN: CORRESPONDENCE ---------- */
  #corr-screen{ display:none; flex:1; flex-direction:column; }
  .topbar{
    display:flex; align-items:center; justify-content:space-between;
    padding:16px 24px; border-bottom:1px solid var(--line);
    font-family:'Special Elite', monospace; font-size:12px; color:var(--postal);
    letter-spacing:0.05em; flex-wrap:wrap; gap:8px;
  }
  .topbar .who{ color:var(--ink); }
  .topbar button{
    background:none; border:1px solid var(--line); color:var(--ink-soft);
    font-family:'Special Elite', monospace; font-size:11px; padding:6px 12px;
    cursor:pointer; letter-spacing:0.08em; text-transform:uppercase;
  }
  .topbar button:hover{ border-color:var(--seal); color:var(--seal); }

  .tabs{ display:none; border-bottom:1px solid var(--line); }
  .tabs button{
    flex:1; padding:12px; background:none; border:none; cursor:pointer;
    font-family:'Playfair Display', serif; font-size:15px; color:var(--ink-soft);
    border-bottom:2px solid transparent;
  }
  .tabs button.active{ color:var(--ink); border-bottom-color:var(--seal); }

  .layout{ flex:1; display:flex; overflow:hidden; }
  .pane{ overflow-y:auto; }
  #write-pane{
    width:38%; min-width:320px; border-right:1px solid var(--line);
    padding:28px 30px; display:flex; flex-direction:column;
  }
  #library-pane{ flex:1; padding:28px 30px; }

  #write-pane h2{ font-size:22px; margin-bottom:4px; }
  #write-pane .sub{ color:var(--ink-soft); font-style:italic; font-size:14.5px; margin-bottom:20px; }
  #letter-text{
    flex:1; min-height:280px; width:100%; resize:none;
    font-family:'EB Garamond', serif; font-size:18px; line-height:1.9;
    background:
      repeating-linear-gradient(180deg, transparent 0, transparent 37px, var(--line) 38px);
    background-color: rgba(255,255,255,0.3);
    border:1px solid var(--line); padding:14px 16px 0; outline:none; color:var(--ink);
  }
  .write-footer{
    display:flex; align-items:center; justify-content:space-between; margin-top:18px;
  }
  .char-count{ font-family:'Special Elite', monospace; font-size:11px; color:var(--ink-soft); }
  .send-seal{
    width:66px; height:66px; border-radius:50%; border:none; cursor:pointer;
    background: radial-gradient(circle at 32% 30%, #b64444, var(--seal) 55%, var(--seal-dark) 100%);
    box-shadow: 0 6px 14px rgba(139,46,46,0.4), inset 0 -3px 6px rgba(0,0,0,0.25), inset 0 2px 5px rgba(255,255,255,0.25);
    color:#f3e9dc; font-family:'Playfair Display', serif; font-size:12px; letter-spacing:0.04em;
    transition: transform .12s ease;
  }
  .send-seal:active{ transform:scale(0.9) rotate(-4deg); }
  .send-seal:disabled{ opacity:0.4; cursor:default; }

  #library-pane h2{ font-size:22px; margin-bottom:22px; }
  .empty-shelf{ color:var(--ink-soft); font-style:italic; padding-top:20px; }

  .thread{ position:relative; padding-left:2px; }
  .thread::before{
    content:""; position:absolute; left:50%; top:0; bottom:0; width:1px;
    background:var(--line); transform:translateX(-0.5px);
  }
  .letter-row{ display:flex; margin-bottom:26px; position:relative; }
  .letter-row.mine{ justify-content:flex-end; }
  .letter-row.theirs{ justify-content:flex-start; }

  .letter-card{
    width:min(78%, 480px);
    background: linear-gradient(180deg, rgba(255,255,255,0.5), transparent 20%), var(--paper);
    border:1px solid var(--line);
    box-shadow: 0 6px 16px var(--shadow);
    padding:18px 20px 16px;
    position:relative;
  }
  .letter-row.mine .letter-card{ margin-right:0; }
  .letter-row.theirs .letter-card{ margin-left:0; }

  .postmark{
    position:absolute; top:-12px; font-family:'Special Elite', monospace; font-size:10.5px;
    color:var(--postal); background:var(--paper); padding:2px 9px;
    border:1px solid var(--postal); border-radius:14px; transform:rotate(-3deg);
    letter-spacing:0.03em;
  }
  .letter-row.mine .postmark{ right:14px; }
  .letter-row.theirs .postmark{ left:14px; }

  .letter-body{
    font-size:16.5px; line-height:1.75; white-space:pre-wrap; margin-top:14px; color:var(--ink);
  }
  .letter-sign{
    font-family:'Caveat', cursive; font-size:24px; color:var(--seal);
    text-align:right; margin-top:10px;
  }
  .new-badge{
    display:inline-block; font-family:'Special Elite', monospace; font-size:9.5px;
    background:var(--seal); color:#f3e9dc; padding:2px 7px; margin-left:8px; letter-spacing:0.05em;
  }

  @media (max-width: 760px){
    .layout{ flex-direction:column; }
    .tabs{ display:flex; }
    #write-pane{ width:100%; min-width:0; border-right:none; border-bottom:1px solid var(--line); display:none; }
    #library-pane{ display:none; }
    .layout.show-write #write-pane{ display:flex; }
    .layout.show-library #library-pane{ display:block; }
    .letter-card{ width:92%; }
  }
</style>
</head>
<body>
<div id="app">

  <!-- ENTRY -->
  <div id="entry-screen">
    <div class="envelope">
      <div class="brand">Missive</div>
      <div class="eyebrow">Correspondance privée</div>
      <h1>Ouvrir le courrier</h1>
      <div class="sub">Deux personnes, un fil de lettres</div>

      <div class="field">
        <label for="room-code">Code du duo</label>
        <input id="room-code" type="text" placeholder="ex. lucie-et-marc" autocomplete="off">
      </div>
      <div class="hint">Choisissez un code connu de vous deux seulement — il ouvre la même correspondance des deux côtés.</div>

      <div class="field">
        <label for="user-name">Votre prénom</label>
        <input id="user-name" type="text" placeholder="ex. Lucie" autocomplete="off">
      </div>

      <button type="button" class="seal-btn" id="enter-btn"><span>Entrer</span></button>
      <div class="error-msg" id="entry-error"></div>
    </div>
  </div>

  <!-- CORRESPONDENCE -->
  <div id="corr-screen">
    <div class="topbar">
      <div>Missive — <span class="who" id="tb-code"></span></div>
      <div>Connecté·e en tant que <span class="who" id="tb-name"></span> &nbsp;
        <button type="button" id="leave-btn">Changer</button>
      </div>
    </div>

    <div class="tabs">
      <button type="button" id="tab-write" class="active">Écrire</button>
      <button type="button" id="tab-library">Bibliothèque</button>
    </div>

    <div class="layout show-write" id="layout">
      <div id="write-pane" class="pane">
        <h2>Nouvelle lettre</h2>
        <div class="sub">Elle sera datée et rangée dans la bibliothèque commune.</div>
        <textarea id="letter-text" placeholder="Chère..."></textarea>
        <div class="write-footer">
          <span class="char-count" id="char-count">0 caractère</span>
          <button type="button" class="send-seal" id="send-btn" disabled><span>Sceller<br>&amp; envoyer</span></button>
        </div>
      </div>
      <div id="library-pane" class="pane">
        <h2>La bibliothèque</h2>
        <div id="thread-container"><div class="empty-shelf">Aucune lettre pour l'instant. La première ouvrira le fil.</div></div>
      </div>
    </div>
  </div>

</div>

<script>
(function(){
  if(!window.storage){
    console.warn('window.storage indisponible dans cet environnement — utilisation d\'un stockage temporaire en mémoire (non partagé, non persistant).');
    const mem = {};
    window.storage = {
      async get(key, shared){ const k=(shared?'s:':'p:')+key; return (k in mem) ? {key, value:mem[k], shared} : null; },
      async set(key, value, shared){ const k=(shared?'s:':'p:')+key; mem[k]=value; return {key, value, shared}; },
      async delete(key, shared){ const k=(shared?'s:':'p:')+key; delete mem[k]; return {key, deleted:true, shared}; },
      async list(prefix, shared){ return {keys:[], prefix, shared}; }
    };
  }
  const entryScreen = document.getElementById('entry-screen');
  const corrScreen = document.getElementById('corr-screen');
  const roomInput = document.getElementById('room-code');
  const nameInput = document.getElementById('user-name');
  const enterBtn = document.getElementById('enter-btn');
  const entryError = document.getElementById('entry-error');
  const tbCode = document.getElementById('tb-code');
  const tbName = document.getElementById('tb-name');
  const leaveBtn = document.getElementById('leave-btn');
  const letterText = document.getElementById('letter-text');
  const charCount = document.getElementById('char-count');
  const sendBtn = document.getElementById('send-btn');
  const threadContainer = document.getElementById('thread-container');
  const tabWrite = document.getElementById('tab-write');
  const tabLibrary = document.getElementById('tab-library');
  const layout = document.getElementById('layout');

  let ROOM = null, ME = null, lastCount = 0, pollTimer = null;

  function slug(s){ return s.trim().toLowerCase().replace(/\s+/g,'-'); }
  function letterKey(room){ return 'letters:' + slug(room); }

  function formatDate(iso){
    const d = new Date(iso);
    const jours = ['dim','lun','mar','mer','jeu','ven','sam'];
    const mois = ['janv','févr','mars','avr','mai','juin','juil','août','sept','oct','nov','déc'];
    const j = jours[d.getDay()], da = d.getDate(), mo = mois[d.getMonth()], y = d.getFullYear();
    const h = String(d.getHours()).padStart(2,'0'), mi = String(d.getMinutes()).padStart(2,'0');
    return `${j} ${da} ${mo} ${y} · ${h}h${mi}`;
  }

  async function loadRemembered(){
    try{
      const r = await window.storage.get('last-session', false);
      if(r && r.value){
        const v = JSON.parse(r.value);
        if(v.room) roomInput.value = v.room;
        if(v.name) nameInput.value = v.name;
      }
    }catch(e){ /* no previous session, ignore */ }
  }

  async function rememberSession(room, name){
    try{ await window.storage.set('last-session', JSON.stringify({room, name}), false); }
    catch(e){ console.error('storage error', e); }
  }

  function participantsKey(room){ return 'participants:' + slug(room); }

  async function getParticipants(room){
    try{
      const r = await window.storage.get(participantsKey(room), true);
      if(!r || !r.value) return [];
      const arr = JSON.parse(r.value);
      return Array.isArray(arr) ? arr : [];
    }catch(e){ return []; }
  }

  async function saveParticipants(room, arr){
    try{
      const res = await window.storage.set(participantsKey(room), JSON.stringify(arr), true);
      return !!res;
    }catch(e){ console.error('storage error', e); return false; }
  }

  async function getLetters(){
    try{
      const r = await window.storage.get(letterKey(ROOM), true);
      if(!r || !r.value) return [];
      const arr = JSON.parse(r.value);
      return Array.isArray(arr) ? arr : [];
    }catch(e){ return []; }
  }

  async function saveLetters(arr){
    try{
      const res = await window.storage.set(letterKey(ROOM), JSON.stringify(arr), true);
      return !!res;
    }catch(e){ console.error('storage error', e); return false; }
  }

  function renderThread(letters, opts){
    opts = opts || {};
    if(letters.length === 0){
      threadContainer.innerHTML = "<div class=\"empty-shelf\">Aucune lettre pour l'instant. La première ouvrira le fil.</div>";
      return;
    }
    const sorted = letters.slice().sort((a,b)=> new Date(a.date) - new Date(b.date));
    threadContainer.innerHTML = '<div class="thread"></div>';
    const thread = threadContainer.querySelector('.thread');
    sorted.forEach((l, idx)=>{
      const mine = l.author === ME;
      const row = document.createElement('div');
      row.className = 'letter-row ' + (mine ? 'mine' : 'theirs');
      const isNew = opts.newIds && opts.newIds.has(l.id) && !mine;
      row.innerHTML = `
        <div class="letter-card">
          <div class="postmark">${formatDate(l.date)}</div>
          <div class="letter-body">${escapeHtml(l.content)}</div>
          <div class="letter-sign">${escapeHtml(l.author)}${isNew ? '<span class=\"new-badge\">nouveau</span>' : ''}</div>
        </div>`;
      thread.appendChild(row);
    });
    threadContainer.scrollTop = threadContainer.scrollHeight;
  }

  function escapeHtml(s){
    return s.replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
  }

  async function refreshLibrary(silent){
    const letters = await getLetters();
    if(letters.length !== lastCount){
      const known = new Set();
      renderThread(letters, { newIds: new Set() });
      lastCount = letters.length;
    } else if(!silent){
      renderThread(letters);
    }
  }

  async function pollLoop(){
    pollTimer = setInterval(async ()=>{
      const letters = await getLetters();
      if(letters.length !== lastCount){
        renderThread(letters);
        lastCount = letters.length;
      }
    }, 4000);
  }

  async function enterCorrespondence(room, name){
    ROOM = room; ME = name;
    tbCode.textContent = slug(room);
    tbName.textContent = name;
    entryScreen.style.display = 'none';
    corrScreen.style.display = 'flex';
    await rememberSession(room, name);
    const letters = await getLetters();
    lastCount = letters.length;
    renderThread(letters);
    pollLoop();
  }

  enterBtn.addEventListener('click', async (e)=>{
    e.preventDefault();
    const room = roomInput.value.trim();
    const name = nameInput.value.trim();
    if(!room || !name){
      entryError.textContent = 'Merci de renseigner le code et votre prénom.';
      return;
    }
    entryError.textContent = '';
    enterBtn.disabled = true;
    const participants = await getParticipants(room);
    const nameLower = name.toLowerCase();
    const alreadyIn = participants.some(p => p.toLowerCase() === nameLower);
    if(!alreadyIn){
      if(participants.length >= 2){
        entryError.textContent = 'Ce code est déjà utilisé par un duo complet. Choisissez un autre code, ou vérifiez l\'orthographe de votre prénom.';
        enterBtn.disabled = false;
        return;
      }
      const updated = participants.concat([name]);
      const ok = await saveParticipants(room, updated);
      if(!ok){
        entryError.textContent = "Une erreur est survenue, réessayez.";
        enterBtn.disabled = false;
        return;
      }
    }
    enterBtn.disabled = false;
    enterCorrespondence(room, name);
  });

  [roomInput, nameInput].forEach(el=>{
    el.addEventListener('keydown', e=>{
      if(e.key === 'Enter'){ e.preventDefault(); enterBtn.click(); }
    });
  });

  leaveBtn.addEventListener('click', (e)=>{
    e.preventDefault();
    if(pollTimer) clearInterval(pollTimer);
    corrScreen.style.display = 'none';
    entryScreen.style.display = 'flex';
    entryError.textContent = '';
  });

  letterText.addEventListener('input', ()=>{
    const n = letterText.value.length;
    charCount.textContent = n + (n === 1 ? ' caractère' : ' caractères');
    sendBtn.disabled = letterText.value.trim().length === 0;
  });

  sendBtn.addEventListener('click', async (e)=>{
    e.preventDefault();
    const content = letterText.value.trim();
    if(!content) return;
    sendBtn.disabled = true;
    const letters = await getLetters();
    const newLetter = {
      id: Date.now() + '-' + Math.random().toString(36).slice(2,8),
      author: ME,
      content: content,
      date: new Date().toISOString()
    };
    letters.push(newLetter);
    const ok = await saveLetters(letters);
    if(ok){
      letterText.value = '';
      charCount.textContent = '0 caractère';
      lastCount = letters.length;
      renderThread(letters);
      if(window.innerWidth <= 760){ tabLibrary.click(); }
    } else {
      sendBtn.disabled = false;
      alert("La lettre n'a pas pu être envoyée. Réessayez.");
    }
  });

  tabWrite.addEventListener('click', (e)=>{
    e.preventDefault();
    tabWrite.classList.add('active'); tabLibrary.classList.remove('active');
    layout.classList.add('show-write'); layout.classList.remove('show-library');
  });
  tabLibrary.addEventListener('click', (e)=>{
    e.preventDefault();
    tabLibrary.classList.add('active'); tabWrite.classList.remove('active');
    layout.classList.add('show-library'); layout.classList.remove('show-write');
  });

  loadRemembered();
})();
</script>
</body>
</html>
