<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Campo Minado 💣</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #282c34;
      color: white;
      margin: 0;
      padding: 20px;
    }
    h1 {
      color: #ff6f61;
    }
    #game-container {
      display: grid;
      grid-template-columns: repeat(10, 50px);
      grid-gap: 5px;
      background-color: #444;
      padding: 10px;
      border-radius: 10px;
    }
    .cell {
      width: 50px;
      height: 50px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      background-color: #61dafb;
      border-radius: 5px;
      cursor: pointer;
      transition: transform 0.2s, background-color 0.2s;
      color: transparent;
    }
    .cell:hover {
      transform: scale(1.1);
    }
    .cell.revealed {
      background-color: #b2f7ef;
      color: black;
    }
    .cell.bomb.revealed {
      background-color: red;
      color: white;
    }
    .blue-flag {
      background-color: blue;
    }
    .white-flag {
      background-color: white;
    }
    .hole {
      background-color: black;
    }
    .cobra {
      background-color: green;
    }
    #score {
      margin-top: 10px;
      font-size: 20px;
    }
  </style>
</head>
<body>
  <h1>Campo Minado 💣</h1>
  <div id="score">Pontuação: <span id="points">0</span></div>
  <div id="game-container"></div>
  <button id="restart">Reiniciar</button>
  <script>
    const gameContainer = document.getElementById("game-container");
    const restartButton = document.getElementById("restart");
    const scoreDisplay = document.getElementById("points");
    const rows = 10;
    const cols = 10;
    const bombCount = 15;
    const blueFlagCount = 15;
    const whiteFlagCount = 4;
    const holeCount = 5;
    const cobraCount = 4;
    let score = 0;
    let cells = [];

    function updateScore(value) {
      score += value;
      scoreDisplay.textContent = score;
    }

    function createBoard() {
      gameContainer.innerHTML = "";
      score = 0;
      updateScore(0);
      cells = [];
      let specialCells = new Set();
      
      while (specialCells.size < bombCount + blueFlagCount + whiteFlagCount + holeCount + cobraCount) {
        specialCells.add(Math.floor(Math.random() * (rows * cols)));
      }
      
      let specialArray = Array.from(specialCells);
      for (let i = 0; i < rows * cols; i++) {
        const cell = document.createElement("div");
        cell.classList.add("cell");
        cell.dataset.id = i;
        gameContainer.appendChild(cell);
        cells.push(cell);
        
        if (specialArray.includes(i)) {
          if (bombCount-- > 0) cell.classList.add("bomb");
          else if (blueFlagCount-- > 0) cell.classList.add("blue-flag");
          else if (whiteFlagCount-- > 0) cell.classList.add("white-flag");
          else if (holeCount-- > 0) cell.classList.add("hole");
          else if (cobraCount-- > 0) cell.classList.add("cobra");
        }
        
        cell.addEventListener("click", handleCellClick);
      }
    }

    function handleCellClick(event) {
      const cell = event.target;
      if (cell.classList.contains("revealed")) return;
      cell.classList.add("revealed");

      if (cell.classList.contains("bomb")) {
        cell.innerHTML = "💣";
        alert("Game Over!");
      } else if (cell.classList.contains("blue-flag")) {
        askQuestion(cell);
      } else if (cell.classList.contains("hole")) {
        alert("Você caiu em um buraco! Perde a vez.");
      } else if (cell.classList.contains("cobra")) {
        alert("A Cobra Fumou! Você recebe uma dica na próxima questão.");
      }
    }

    function askQuestion(cell) {
      const question = "Qual é a capital do Brasil?";
      const options = ["A) Rio de Janeiro", "B) São Paulo", "C) Brasília", "D) Salvador"];
      const answer = "C";
      let userAnswer = prompt(question + "\n" + options.join("\n"));
      
      if (userAnswer && userAnswer.toUpperCase() === answer) {
        updateScore(10);
        alert("Correto! Você ganhou 10 pontos.");
      } else {
        updateScore(-5);
        alert("Errado! Você perdeu 5 pontos.");
      }
    }

    restartButton.addEventListener("click", createBoard);
    createBoard();
  </script>
</body>
</html>
