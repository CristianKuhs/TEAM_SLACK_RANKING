<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard de Pontuação</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <div id="dashboard-container">
        <header>
            <h1>Dashboard de Pontuação</h1>
            <button id="clear-data-btn">Limpar Dados</button>
        </header>
        <section id="users-list-container">
            <h2>Usuários</h2>
            <ul id="users-list"></ul>
        </section>
        <section id="points-container">
            <h2>Pontuação</h2>
            <div id="points-options" class="options-grid"></div>
        </section>
        <section id="ranking-container">
            <h2>Ranking</h2>
            <ol id="ranking-list"></ol>
        </section>
    </div>
    <script src="js/script.js"></script>
</body>
</html>
