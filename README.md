<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Team Building Scoreboard</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #0d0d0d;
      --card: #1a1a1a;
      --text: #f0f0f0;
      --highlight: #4a90e2;
      --danger: #d9534f;
      --gold: #FFD700;
      --silver: #C0C0C0;
      --bronze: #CD7F32;
    }
    body {
      font-family: 'Montserrat', sans-serif;
      background: var(--bg);
      color: var(--text);
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
    }
    #dashboard {
      background: var(--card);
      padding: 25px;
      border-radius: 15px;
      width: 100%;
      max-width: 1000px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.6);
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-gap: 20px;
      grid-template-areas:
        "header header"
        "rules rules"
        "users ranking"
        "actions actions"
        "countdown countdown";
    }
    h1 {
      grid-area: header;
      text-align: center;
      font-size: 2.5rem;
      color: var(--highlight);
    }
    .card {
      background: #222;
      padding: 20px;
      border-radius: 12px;
    }
    h2 {
      margin-top: 0;
      font-size: 1.4rem;
      border-bottom: 2px solid #333;
      padding-bottom: 8px;
    }
    ul,ol { list-style: none; margin: 0; padding: 0; }
    li {
      background: #2c2c2c;
      margin: 6px 0;
      padding: 10px 14px;
      border-radius: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .rules { grid-area: rules; }
    .rules li { background: #333; font-size: 0.95rem; }
    #users { grid-area: users; }
    #ranking { grid-area: ranking; }
    #actions { grid-area: actions; text-align: center; }
    #countdown { grid-area: countdown; text-align: center; margin-top: 10px; }
    button {
      border: none;
      padding: 10px 18px;
      border-radius: 8px;
      background: var(--highlight);
      color: white;
      font-weight: bold;
      margin: 4px;
      cursor: pointer;
      transition: 0.3s;
    }
    button:hover { transform: translateY(-2px); }
    .danger { background: var(--danger); }
    /* Ranking cores */
    .first { color: var(--gold); }
    .second { color: var(--silver); }
    .third { color: var(--bronze); }
    /* Modal */
    .modal {
      display: none;
      position: fixed; inset: 0;
      background: rgba(0,0,0,0.7);
      justify-content: center; align-items: center;
      z-index: 10;
    }
    .modal-content {
      background: var(--card);
      padding: 25px;
      border-radius: 10px;
      width: 90%;
      max-width: 400px;
    }
    .modal-content h3 { margin-top: 0; text-align: center; color: var(--highlight); }
    .close { float: right; cursor: pointer; font-size: 22px; }
    .user-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit,minmax(130px,1fr));
      gap: 10px;
      margin-top: 15px;
    }
    .user-grid button { background: #333; }
    .user-grid button:hover { background: var(--highlight); }
    .input-points {
      margin-top: 15px;
      display: flex;
      justify-content: center;
      gap: 10px;
    }
    .input-points input {
      width: 80px;
      padding: 6px;
      border-radius: 6px;
      border: none;
      text-align: center;
    }
  </style>
</head>
<body>
  <div id="dashboard">
    <h1>Team Building Scoreboard</h1>
    <!-- Regras -->
    <div class="card rules">
      <h2>Regras de Pontuação</h2>
      <ul>
        <li>Bom dia ao chegar (salão) → +1</li>
        <li>Boa tarde ao retornar (salão) → +1</li>
        <li>Tchau ao ir embora (salão) → +1</li>
        <li>Cozinha com colegas → +3</li>
        <li>Passar no Comercial e cumprimentar → +5</li>
      </ul>
    </div>
    <!-- Membros -->
    <div class="card" id="users">
      <h2>Membros do Time</h2>
      <ul id="users-list"></ul>
      <button onclick="showAddUser()">Adicionar Membro</button>
    </div>
    <!-- Ranking -->
    <div class="card" id="ranking">
      <h2>Ranking</h2>
      <ol id="ranking-list"></ol>
      <div id="last-champion"></div>
    </div>
    <!-- Ações -->
    <div id="actions">
      <button class="danger" onclick="resetData()">Limpar Dados</button>
    </div>
    <!-- Contagem regressiva -->
    <div id="countdown">
      <h2>Fechamento em: <span id="timer"></span></h2>
    </div>
  </div>
  <!-- Modal escolha usuário -->
  <div id="user-modal" class="modal">
    <div class="modal-content">
      <span class="close" onclick="closeModal()">&times;</span>
      <h3>Selecione um Usuário</h3>
      <div id="modal-user-list" class="user-grid"></div>
      <div class="input-points">
        <input type="number" id="manual-points" placeholder="Pts">
        <button onclick="applyManualPoints()">Aplicar</button>
      </div>
    </div>
  </div>
<script>
const users = JSON.parse(localStorage.getItem("users")) || 
 ["Cristian","Eduardo","Emanuela","Kauane","Lipe","Pamela","Patrick","Pedro Henrique","Pedro Leite","Richard","Thais","Vitória"];
let scores = JSON.parse(localStorage.getItem("scores")) || Object.fromEntries(users.map(u=>[u,0]));
let lastChampion = localStorage.getItem("lastChampion") || "";
let currentUser = null;
function saveData(){
  localStorage.setItem("users", JSON.stringify(users));
  localStorage.setItem("scores", JSON.stringify(scores));
  if(lastChampion) localStorage.setItem("lastChampion", lastChampion);
}
function renderUsers(){
  const ul=document.getElementById("users-list");
  ul.innerHTML="";
  users.forEach(u=>{
    const li=document.createElement("li");
    li.innerHTML=`<span>${u}</span><span>${scores[u]} pts</span>
      <button onclick="removeUser('${u}')">x</button>`;
    ul.appendChild(li);
  });
}
function renderRanking(){
  const ol=document.getElementById("ranking-list");
  ol.innerHTML="";
  const sorted=Object.entries(scores).sort((a,b)=>b[1]-a[1]);
  sorted.forEach(([u,p],i)=>{
    const li=document.createElement("li");
    let cls=i==0?"first":i==1?"second":i==2?"third":"";
    li.className=cls;
    li.innerHTML=`${i+1}º ${u} - ${p} pts`;
    ol.appendChild(li);
  });
  document.getElementById("last-champion").innerHTML=lastChampion?`🏆 Último campeão: <b>${lastChampion}</b>`:"";
}
function showUserModal(){
  const modal=document.getElementById("user-modal");
  const list=document.getElementById("modal-user-list");
  list.innerHTML="";
  users.forEach(u=>{
    const btn=document.createElement("button");
    btn.textContent=u;
    btn.onclick=()=>{currentUser=u;};
    list.appendChild(btn);
  });
  modal.style.display="flex";
}
function closeModal(){ document.getElementById("user-modal").style.display="none"; }
function applyManualPoints(){
  const val=parseInt(document.getElementById("manual-points").value);
  if(currentUser && !isNaN(val)){
    scores[currentUs]()
