<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard de Pontuação</title>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            margin: 0;
            background: #0f172a;
            color: #f1f5f9;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            padding: 20px;
        }
        #dashboard-container {
            background: rgba(30, 41, 59, 0.8);
            padding: 20px;
            border-radius: 16px;
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            width: 100%;
            max-width: 900px;
        }
        h1, h2 {
            text-align: center;
            margin-bottom: 20px;
        }
        button {
            display: block;
            margin: 0 auto 20px;
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            background: linear-gradient(90deg, #2563eb, #3b82f6);
            color: white;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.3s;
        }
        button:hover {
            transform: scale(1.05);
            background: linear-gradient(90deg, #1d4ed8, #2563eb);
        }
        ul, ol {
            list-style: none;
            padding: 0;
        }
        .options-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 10px;
        }
        .option-btn {
            background: #334155;
            border: none;
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            color: #f1f5f9;
            transition: 0.3s;
        }
        .option-btn:hover {
            background: #475569;
        }
        li {
            background: #1e293b;
            padding: 8px;
            border-radius: 6px;
            margin-bottom: 5px;
        }
    </style>
</head>
<body>
    <div id="dashboard-container">
        <h1>Dashboard de Pontuação</h1>
        <button id="clear-data-btn">Limpar Dados</button>
        <div id="users-list-container">
            <h2>Usuários</h2>
            <ul id="users-list"></ul>
        </div>
        <div id="points-container">
            <h2>Pontuação</h2>
            <div id="points-options" class="options-grid"></div>
        </div>
        <div id="ranking-container">
            <h2>Ranking</h2>
            <ol id="ranking-list"></ol>
        </div>
    </div>
    <script>
        const users = ["Cristian", "Eduardo", "Emanuela", "Kauane", "Lipe", "Pamela", "Patrick", "Pedro Henrique", "Pedro Leite", "Richard", "Thais", "Vitória"];
        const pointsOptions = [
            { label: "Bom dia ao chegar (Em cada salão)!", value: 1 },
            { label: "Boa tarde ao retornar/chegar (Em cada salão)!", value: 1 },
            { label: "Tchau ao ir embora (em cada salão)!", value: 1 }
            { label: "Bom dia/Boa tarde na cozinha sempre que entrar e tiver outra(s) pessoa(s)!", value: 3 }
            { label: "Ao descer, passar no Comercial e dar Bom dia/Boa tarde!", value: 5 }
        ];
        const usersList = document.getElementById("users-list");
        const pointsContainer = document.getElementById("points-options");
        const rankingList = document.getElementById("ranking-list");
        const clearBtn = document.getElementById("clear-data-btn");
        let scores = JSON.parse(localStorage.getItem("scores")) || {};
        function renderUsers() {
            usersList.innerHTML = "";
            users.forEach(user => {
                const li = document.createElement("li");
                li.textContent = `${user} - ${scores[user] || 0} pts`;
                usersList.appendChild(li);
            });
        }
        function renderPointsOptions() {
            pointsContainer.innerHTML = "";
            pointsOptions.forEach(option => {
                const btn = document.createElement("button");
                btn.textContent = `${option.label} (+${option.value})`;
                btn.className = "option-btn";
                btn.onclick = () => addPoints(option.value);
                pointsContainer.appendChild(btn);
            });
        }
        function addPoints(value) {
            const user = prompt("Digite o nome do usuário:");
            if (!users.includes(user)) {
                alert("Usuário não encontrado!");
                return;
            }
            scores[user] = (scores[user] || 0) + value;
            localStorage.setItem("scores", JSON.stringify(scores));
            renderUsers();
            renderRanking();
        }
        function renderRanking() {
            const sorted = Object.entries(scores).sort((a, b) => b[1] - a[1]);
            rankingList.innerHTML = "";
            sorted.forEach(([user, points]) => {
                const li = document.createElement("li");
                li.textContent = `${user} - ${points} pts`;
                rankingList.appendChild(li);
            });
        }
        clearBtn.onclick = () => {
            if (confirm("Tem certeza que deseja limpar os dados?")) {
                scores = {};
                localStorage.removeItem("scores");
                renderUsers();
                renderRanking();
            }
        };
        renderUsers();
        renderPointsOptions();
        renderRanking();
    </script>
</body>
</html>
