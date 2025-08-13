<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Building Scoreboard</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        :root {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --text-color: #e0e0e0;
            --highlight-color: #4a90e2;
            --primary-btn-color: #50e3c2;
            --secondary-btn-color: #383838;
            --border-color: #333;
            --shadow-color: rgba(0, 0, 0, 0.5);
        }
        body {
            font-family: 'Montserrat', sans-serif;
            margin: 0;
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            min-height: 100vh;
        }
        #dashboard-container {
            background-color: var(--card-bg);
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 10px 30px var(--shadow-color);
            width: 100%;
            max-width: 950px;
            display: grid;
            grid-template-columns: 1fr 1fr;
            grid-template-rows: auto 1fr auto;
            gap: 25px;
            grid-template-areas:
                "header header"
                "users ranking"
                "points points"
                "actions actions";
        }
        h1 {
            grid-area: header;
            text-align: center;
            font-size: 2.8rem;
            color: var(--highlight-color);
            margin-bottom: 20px;
        }
        .card {
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        h2 {
            font-size: 1.6rem;
            margin-top: 0;
            margin-bottom: 20px;
            border-bottom: 2px solid var(--border-color);
            padding-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        #users-list-container {
            grid-area: users;
        }
        #ranking-container {
            grid-area: ranking;
        }
        #points-container {
            grid-area: points;
        }
        #actions-container {
            grid-area: actions;
            text-align: center;
        }
        button {
            border: none;
            padding: 12px 24px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
        }
        #clear-data-btn {
            background-color: #d9534f;
            color: white;
            width: fit-content;
            margin: 0 auto;
        }
        #clear-data-btn:hover {
            background-color: #c9302c;
            transform: translateY(-2px);
        }
        .clear-btn-wrapper {
            margin-top: 20px;
            text-align: center;
        }
        ul, ol {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .options-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 15px;
        }
        .option-btn {
            background-color: var(--secondary-btn-color);
            border: 1px solid var(--border-color);
            color: var(--text-color);
            text-align: left;
            padding: 15px;
            border-radius: 8px;
        }
        .option-btn:hover {
            background-color: #555;
            transform: translateY(-2px);
        }
        li {
            background-color: #2b2b2b;
            padding: 12px 15px;
            border-radius: 8px;
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border: 1px solid var(--border-color);
        }
        li span {
            font-weight: 600;
        }
        /* Classes específicas para o ranking */
        .ranking-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .ranking-item .position {
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--highlight-color);
            margin-right: 15px;
        } 
        @media (max-width: 768px) {
            #dashboard-container {
                grid-template-columns: 1fr;
                grid-template-areas:
                    "header"
                    "users"
                    "ranking"
                    "points"
                    "actions";
                padding: 20px;
            }     
            h1 {
                font-size: 2.2rem;
            }
            .options-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div id="dashboard-container">
        <h1>Team Building Scoreboard</h1>
        <div id="users-list-container" class="card">
            <h2><i class="fas fa-users"></i> Membros do Time</h2>
            <ul id="users-list"></ul>
        </div>
        <div id="ranking-container" class="card">
            <h2><i class="fas fa-trophy"></i> Ranking</h2>
            <ol id="ranking-list"></ol>
        </div>
        <div id="points-container" class="card">
            <h2><i class="fas fa-star"></i> Pontuações</h2>
            <div id="points-options" class="options-grid"></div>
        </div>
        <div id="actions-container">
            <button id="clear-data-btn">Limpar Dados</button>
        </div>
    </div>
    <script>
        const users = ["Cristian", "Eduardo", "Emanuela", "Kauane", "Lipe", "Pamela", "Patrick", "Pedro Henrique", "Pedro Leite", "Richard", "Thais", "Vitória"];
        const pointsOptions = [
            { label: "Bom dia ao chegar (Em cada salão)!", value: 1 },
            { label: "Boa tarde ao retornar/chegar (Em cada salão)!", value: 1 },
            { label: "Tchau ao ir embora (em cada salão)!", value: 1 },
            { label: "Bom dia/Boa tarde na cozinha sempre que entrar e tiver outra(s) pessoa(s)!", value: 3 },
            { label: "Ao descer, passar no Comercial e dar Bom dia/Boa tarde!", value: 5 }
        ];
        const usersList = document.getElementById("users-list");
        const pointsContainer = document.getElementById("points-options");
        const rankingList = document.getElementById("ranking-list");
        const clearBtn = document.getElementById("clear-data-btn");
        let scores = JSON.parse(localStorage.getItem("scores"));
        if (!scores) {
            scores = {};
            users.forEach(user => scores[user] = 0);
            localStorage.setItem("scores", JSON.stringify(scores));
        }
        function renderUsers() {
            usersList.innerHTML = "";
            users.forEach(user => {
                const li = document.createElement("li");
                li.innerHTML = `<span><i class="fas fa-user-circle"></i> ${user}</span><span>${scores[user] || 0} pts</span>`;
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
            if (!user) {
                return;
            }
            const foundUser = users.find(u => u.toLowerCase() === user.toLowerCase());
            if (!foundUser) {
                alert("Usuário não encontrado!");
                return;
            }
            scores[foundUser] = (scores[foundUser] || 0) + value;
            localStorage.setItem("scores", JSON.stringify(scores));
            renderUsers();
            renderRanking();
        }
        function renderRanking() {
            const sorted = Object.entries(scores).sort((a, b) => b[1] - a[1]);
            rankingList.innerHTML = "";
            sorted.forEach(([user, points], index) => {
                const li = document.createElement("li");
                let iconClass = "fas fa-trophy";
                if (index === 0) iconClass += " first-place"; 
                li.innerHTML = `<div class="ranking-item"><span><i class="${iconClass}"></i> ${user}</span><span>${points} pts</span></div>`;
                rankingList.appendChild(li);
            });
        }
        clearBtn.onclick = () => {
            if (confirm("Tem certeza que deseja limpar os dados?")) {
                scores = {};
                users.forEach(user => scores[user] = 0);
                localStorage.setItem("scores", JSON.stringify(scores));
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
