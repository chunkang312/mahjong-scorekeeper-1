<!DOCTYPE html>
<html lang="zh-Hans">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>麻将记分器</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f7f7f7;
      padding: 20px;
      max-width: 600px;
      margin: auto;
    }

    h1 {
      text-align: center;
      color: #333;
    }

    .player {
      background: #fff;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      margin: 15px 0;
      padding: 15px;
    }

    .player-name {
      font-size: 1.2em;
      margin-bottom: 10px;
    }

    .score {
      font-size: 2em;
      color: #007bff;
      margin-bottom: 10px;
    }

    .buttons {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    button {
      flex: 1;
      padding: 10px;
      font-size: 1em;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: all 0.2s ease-in-out;
    }

    button:hover {
      opacity: 0.85;
    }

    .add {
      background-color: #28a745;
      color: white;
    }

    .subtract {
      background-color: #dc3545;
      color: white;
    }

    .reset-btn {
      display: block;
      margin: 30px auto;
      padding: 10px 20px;
      font-size: 1em;
      background: #ffc107;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .delta {
      font-size: 1.2em;
      color: #888;
      margin-top: 5px;
      height: 20px;
      transition: 0.3s ease-in-out;
    }
  </style>
</head>
<body>
  <h1>麻将记分器</h1>
  <div id="players"></div>

  <button class="reset-btn" onclick="resetScores()">重置所有分数</button>

  <script>
    const playerNames = ['东风', '南风', '西风', '北风']; // 你可以改成你们的名字
    const playerData = [];

    const container = document.getElementById('players');

    playerNames.forEach((name, index) => {
      const player = {
        name,
        score: 0,
        element: document.createElement('div'),
        deltaTimeout: null,
      };

      const el = player.element;
      el.className = 'player';
      el.innerHTML = `
        <div class="player-name">${name}</div>
        <div class="score" id="score-${index}">0</div>
        <div class="delta" id="delta-${index}"></div>
        <div class="buttons">
          <button class="add" onclick="changeScore(${index}, 10)">+10</button>
          <button class="subtract" onclick="changeScore(${index}, -10)">-10</button>
          <button class="add" onclick="changeScore(${index}, 20)">+20</button>
          <button class="subtract" onclick="changeScore(${index}, -20)">-20</button>
        </div>
      `;

      container.appendChild(el);
      playerData.push(player);
    });

    function changeScore(index, delta) {
      const player = playerData[index];
      player.score += delta;
      document.getElementById(`score-${index}`).textContent = player.score;

      const deltaEl = document.getElementById(`delta-${index}`);
      deltaEl.textContent = delta > 0 ? `+${delta}` : delta;

      clearTimeout(player.deltaTimeout);
      player.deltaTimeout = setTimeout(() => {
        deltaEl.textContent = '';
      }, 1000);
    }

    function resetScores() {
      playerData.forEach((player, index) => {
        player.score = 0;
        document.getElementById(`score-${index}`).textContent = 0;
        document.getElementById(`delta-${index}`).textContent = '';
      });
    }
  </script>
</body>
</html>