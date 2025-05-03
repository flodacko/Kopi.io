<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Mini Game Hub</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
      padding: 20px;
    }

    h1, h2 {
      text-align: center;
    }

    .game {
      background-color: white;
      padding: 15px;
      margin: 20px auto;
      max-width: 500px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    .game button {
      margin-top: 10px;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, 60px);
      gap: 5px;
      justify-content: center;
    }

    .cell {
      width: 60px;
      height: 60px;
      text-align: center;
      font-size: 24px;
      background-color: #eee;
      line-height: 60px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>🎮 Mini Game Hub</h1>

  <!-- Tic Tac Toe -->
  <div class="game" id="tic-tac-toe">
    <h2>Tic-Tac-Toe</h2>
    <div class="grid" id="board"></div>
    <p id="winner"></p>
    <button onclick="resetTicTacToe()">Restart</button>
  </div>

  <!-- Rock Paper Scissors -->
  <div class="game">
    <h2>Rock Paper Scissors</h2>
    <button onclick="playRPS('rock')">Rock</button>
    <button onclick="playRPS('paper')">Paper</button>
    <button onclick="playRPS('scissors')">Scissors</button>
    <p id="rps-result"></p>
  </div>

  <!-- Guess the Number -->
  <div class="game">
    <h2>Guess the Number</h2>
    <p>Pick a number between 1 and 10:</p>
    <input type="number" id="guess" min="1" max="10">
    <button onclick="checkGuess()">Guess</button>
    <p id="guess-result"></p>
  </div>

  <script>
    // ==== Tic Tac Toe ====
    let board = ['', '', '', '', '', '', '', '', ''];
    let currentPlayer = 'X';
    const winPatterns = [
      [0,1,2],[3,4,5],[6,7,8],
      [0,3,6],[1,4,7],[2,5,8],
      [0,4,8],[2,4,6]
    ];
    
    function drawBoard() {
      const boardDiv = document.getElementById('board');
      boardDiv.innerHTML = '';
      board.forEach((cell, i) => {
        const cellDiv = document.createElement('div');
        cellDiv.classList.add('cell');
        cellDiv.textContent = cell;
        cellDiv.onclick = () => makeMove(i);
        boardDiv.appendChild(cellDiv);
      });
    }

    function makeMove(index) {
      if (board[index] || checkWinner()) return;
      board[index] = currentPlayer;
      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      drawBoard();
      const winner = checkWinner();
      if (winner) {
        document.getElementById('winner').textContent = winner + " wins!";
      }
    }

    function checkWinner() {
      for (const pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (board[a] && board[a] === board[b] && board[a] === board[c]) {
          return board[a];
        }
      }
      return board.includes('') ? null : "No one";
    }

    function resetTicTacToe() {
      board = ['', '', '', '', '', '', '', '', ''];
      currentPlayer = 'X';
      document.getElementById('winner').textContent = '';
      drawBoard();
    }

    drawBoard();

    // ==== Rock Paper Scissors ====
    function playRPS(playerChoice) {
      const choices = ['rock', 'paper', 'scissors'];
      const computerChoice = choices[Math.floor(Math.random() * 3)];
      let result = '';

      if (playerChoice === computerChoice) result = "It's a tie!";
      else if (
        (playerChoice === 'rock' && computerChoice === 'scissors') ||
        (playerChoice === 'paper' && computerChoice === 'rock') ||
        (playerChoice === 'scissors' && computerChoice === 'paper')
      ) result = 'You win!';
      else result = 'Computer wins!';

      document.getElementById('rps-result').textContent = `Computer chose ${computerChoice}. ${result}`;
    }

    // ==== Guess the Number ====
    const secretNumber = Math.floor(Math.random() * 10) + 1;

    function checkGuess() {
      const guess = Number(document.getElementById('guess').value);
      let message = '';
      if (guess === secretNumber) message = 'Correct! 🎉';
      else if (guess < secretNumber) message = 'Too low!';
      else message = 'Too high!';
      document.getElementById('guess-result').textContent = message;
    }
  </script>
</body>
</html>
