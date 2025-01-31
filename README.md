<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neon Tic Tac Toe</title>
  <style>
    /* General Styling */
    body {
      font-family: 'Arial', sans-serif;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: #000;
      overflow: hidden;
      color: #fff;
    }

    .container {
      text-align: center;
      width: 400px;
      position: relative;
    }

    /* Player Info Styling */
    .player-info {
      position: absolute;
      width: 100%;
      display: flex;
      justify-content: space-between;
      color: #0ff;
    }

    .player {
      font-size: 1.2rem;
      text-shadow: 0 0 5px #0ff, 0 0 10px #0ff, 0 0 20px #0ff;
    }

    #player1 {
      position: absolute;
      top: 9px;
      left: 10px;
    }

    #player2 {
      position: absolute;
      bottom: 280px;
      left: 10px;
    }

    .current-player {
      color: #ff0;
    }

    /* Board Styling */
    .board {
      display: grid;
      grid-template-columns: repeat(3, 120px);
      gap: 10px;
      justify-content: center;
      margin: 50px 0;
    }

    .cell {
      width: 120px;
      height: 120px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 3rem;
      font-weight: bold;
      border: 2px solid #0ff;
      color: #fff;
      cursor: pointer;
      text-shadow: 0 0 10px #0ff, 0 0 20px #0ff, 0 0 40px #0ff;
      box-shadow: 0 0 15px #0ff, 0 0 25px #0ff inset;
      transition: transform 0.3s;
    }

    .cell:hover {
      transform: scale(1.1);
    }

    /* Neon Button */
    .neon-button {
      padding: 10px 20px;
      font-size: 1.2rem;
      font-weight: bold;
      color: #0ff;
      border: 2px solid #0ff;
      background: none;
      cursor: pointer;
      margin-top: 20px;
      border-radius: 5px;
      text-shadow: 0 0 10px #0ff, 0 0 20px #0ff, 0 0 40px #0ff;
      box-shadow: 0 0 15px #0ff, 0 0 25px #0ff inset;
      transition: transform 0.3s;
    }

    .neon-button:hover {
      transform: scale(1.1);
    }

    /* Winning Effect */
    .cell.winner {
      animation: pulse 1s infinite alternate;
      color: #fff;
      text-shadow: 0 0 10px #ff0, 0 0 20px #ff0, 0 0 40px #ff0;
    }

    @keyframes pulse {
      from {
        transform: scale(1);
      }
      to {
        transform: scale(1.2);
      }
    }

    .dimmed {
      opacity: 0.3;
    }

    /* Message */
    #message {
      margin: 20px 0;
      font-size: 1.5rem;
      color: #0ff;
      text-shadow: 0 0 10px #0ff, 0 0 20px #0ff, 0 0 40px #0ff;
    }

     /* Scoreboard */
    #scoreboard {
      margin: 20px auto;
      padding: 10px 20px;
      font-size: 1.5rem;
      font-weight: bold;
      color: white;
      background-color: #ff4d4d; /* Vibrant red */
      border: 2px solid #cc0000; /* Darker red for contrast */
      border-radius: 10px; /* Rounded corners */
      text-align: center; /* Center the text */
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Subtle shadow */
      width: fit-content; /* Adjust width to fit content */
      transition: transform 0.2s ease, box-shadow 0.2s ease; /* Smooth hover effect */
    }

    #scoreboard:hover {
      transform: scale(1.05); /* Slight zoom on hover */
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3); /* More prominent shadow */
    }
    

    /* Modal for Skin Selection */
    #skinModal {
      display: none; 
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.8);
      display: none;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      padding: 20px;
    }

    .skin-option {
      font-size: 2rem;
      cursor: pointer;
      margin: 10px;
    }

    .skin-option:hover {
      transform: scale(1.1);
    }

    #colorChangeButton {
      position: absolute;
      bottom: 50px; /* Adjust distance from the bottom */
      left: 50%; /* Center horizontally */
      transform: translateX(-50%);
      background-color: #007BFF;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    #colorChangeButton:hover {
      background-color: #0056b3; /* Changes color on hover */
    }

    .skin-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr); /* 3 items per row */
      gap: 15px;
      width: 90%;
      max-width: 600px;
      margin: auto;
      height: 100%; /* Maintain the grid's full size */
      overflow-y: auto; /* Enable vertical scrolling */
      padding: 10px;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid #0ff;
      border-radius: 10px;
    }

    #sound-toggle {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 28px;
      cursor: pointer;
      color: #0ff;
      background: none;
      border: 2px solid #0ff;
      border-radius: 50%;
      width: 50px;
      height: 50px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: transform 0.3s, box-shadow 0.3s;
      text-shadow: 0 0 5px #0ff, 0 0 10px #0ff;
    }

    #sound-toggle:hover {
      transform: scale(1.2);
      box-shadow: 0 0 15px #0ff, 0 0 25px #0ff;
    }
    
    
  </style>
</head>
<body>
  <audio id="bubblepop" src="bubblepop.mp3"></audio>
  <audio id="winSound" src="crowd-cheer-for game.mp3"></audio>
    
  <div class="container">
    <div id="player1" class="player">Player 1: 🦁</div>
    <div class="board">
      <div class="cell" data-index="0"></div>
      <div class="cell" data-index="1"></div>
      <div class="cell" data-index="2"></div>
      <div class="cell" data-index="3"></div>
      <div class="cell" data-index="4"></div>
      <div class="cell" data-index="5"></div>
      <div class="cell" data-index="6"></div>
      <div class="cell" data-index="7"></div>
      <div class="cell" data-index="8"></div>
    </div>
    <div id="player2" class="player">Player 2: 🦅</div>
    <button id="reset" class="neon-button">Restart Game</button>
    <p id="message"></p>
    <div id="scoreboard">Score: Player 1 - 0 | Player 2 - 0</div>
    <button id="skinButton" class="neon-button"> ✨ Choose Skins</button>
  </div>
  <button id="colorChangeButton" class="neon-button"> 🎨  Change Colors</button>
  <div id="skinModal" class="modal">
    <div class="skin-grid">
      <div class="skin-option" onclick="selectSkin('🦁', '🦅')">🦁 vs 🦅</div>
      <div class="skin-option" onclick="selectSkin('🐶', '🐱')">🐶 vs 🐱</div>
      <div class="skin-option" onclick="selectSkin('🐻', '🐼')">🐻 vs 🐼</div>
      <div class="skin-option" onclick="selectSkin('🚀', '🛸')">🚀 vs 🛸</div>
      <div class="skin-option" onclick="selectSkin('🇵🇰', '🇳🇪')">🇵🇰 vs 🇳🇪</div>
      <div class="skin-option" onclick="selectSkin('🐰', '🐢')">🐰 vs 🐢</div>
      <div class="skin-option" onclick="selectSkin('🫅', '🐯')">🫅 vs 🐯</div>
      <div class="skin-option" onclick="selectSkin('🤡', '😎')">🤡 vs 😎</div>
      <div class="skin-option" onclick="selectSkin('🦸', '🧌')">🦸 vs 🧌</div>
      <div class="skin-option" onclick="selectSkin('👳', '🧕')">👳 vs 🧕</div>
      <div class="skin-option" onclick="selectSkin('🧑‍💻', '🧑‍⚕️')">🧑‍💻 vs 🧑‍⚕️</div>
      <div class="skin-option" onclick="selectSkin('🐈', '🐒')">🐈 vs 🐒</div>
      <div class="skin-option" onclick="selectSkin('🥷', '🧛')">🥷 vs 🧛</div>
      <div class="skin-option" onclick="selectSkin('🐂', '🐘')">🐂 vs 🐘</div>
      <div class="skin-option" onclick="selectSkin('🐺', '🦊')">🐺 vs 🦊</div>
      <div class="skin-option" onclick="selectSkin('🦌', '🐇')">🦌 vs 🐇</div>
      <div class="skin-option" onclick="selectSkin('🐢', '🐸')">🐢 vs 🐸</div>
      <div class="skin-option" onclick="selectSkin('🦍', '🐒')">🦍 vs 🐒</div>
      <div class="skin-option" onclick="selectSkin('🦔', '🦥')">🦔 vs 🦥</div>
      <div class="skin-option" onclick="selectSkin('🐓', '🦃')">🐓 vs 🦃</div>
      <div class="skin-option" onclick="selectSkin('🦜', '🦢')">🦜 vs 🦢</div>
      <button onclick="closeSkinModal()" class="neon-button">Close</button>
    </div>
  </div>
    
  <script>
    const cells = document.querySelectorAll('.cell');
    const resetButton = document.getElementById('reset');
    const message = document.getElementById('message');
    const player1Display = document.getElementById('player1');
    const player2Display = document.getElementById('player2');
    const scoreboard = document.getElementById('scoreboard');
    const skinModal = document.getElementById('skinModal');
    const skinButton = document.getElementById('skinButton');
    const colorChangeButton = document.getElementById('colorChangeButton');
    const winSound = document.getElementById('winSound');
    const clickSound = new Audio('bubblepop.mp3'); // Ensure this path is correct

    let player1Emoji = '🦁';
    let player2Emoji = '🦅';
    let currentPlayer = player1Emoji;
    let board = ['', '', '', '', '', '', '', '', ''];
    let player1Score = 0;
    let player2Score = 0;

    const winningCombinations = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];

    function updatePlayerDisplay() {
      player1Display.classList.toggle('current-player', currentPlayer === player1Emoji);
      player2Display.classList.toggle('current-player', currentPlayer === player2Emoji);
    }

    function highlightWinner(combination) {
      combination.forEach(index => {
        cells[index].classList.add('winner');
      });
      cells.forEach((cell, index) => {
        if (!combination.includes(index)) {
          cell.classList.add('dimmed');
        }
      });
    }

    function checkWinner() {
      for (let combination of winningCombinations) {
        const [a, b, c] = combination;
        if (board[a] && board[a] === board[b] && board[a] === board[c]) {
          highlightWinner(combination);
          return board[a];
        }
      }
      return board.includes('') ? null : 'Draw';
    }

    function handleCellClick(event) {
      const index = event.target.dataset.index;
      if (board[index] || event.target.classList.contains('dimmed')) return;

      board[index] = currentPlayer;
      event.target.textContent = currentPlayer; // Set the emoji in the clicked cell

      // Play sound on move
      clickSound.currentTime = 0; // Reset sound to start
      clickSound.play(); // Play sound

      const winner = checkWinner();
      if (winner) {
        message.textContent = winner === '🎭' ? "It's a 🎭 !" : `${winner} wins!`;
        if (winner !== '🎭 ') {
          if (winner === player1Emoji) {
            player1Score++;
          } else {
            player2Score++;
          }
          updateScoreboard();
        }
        cells.forEach(cell => cell.removeEventListener('click', handleCellClick));
      } else {
        currentPlayer = currentPlayer === player1Emoji ? player2Emoji : player1Emoji;
        updatePlayerDisplay();
        message.textContent = `${currentPlayer}'s turn`;
      }
    }

    function updateScoreboard() {
      scoreboard.textContent = `Score: Player 1 - ${player1Score} | Player 2 - ${player2Score}`;
    }

    function resetGame() {
      board = ['', '', '', '', '', '', '', '', ''];
      currentPlayer = player1Emoji; // Reset to player 1
      message.textContent = `${currentPlayer}'s turn`;
      updatePlayerDisplay();
      cells.forEach(cell => {
        cell.textContent = ''; // Clear the cell content
        cell.className = 'cell'; // Reset the cell class
        cell.addEventListener('click', handleCellClick); // Re-add the click event
      });
    }

    function selectSkin(emoji1, emoji2) {
      player1Emoji = emoji1;
      player2Emoji = emoji2;
      player1Display.textContent = `Player 1: ${player1Emoji}`;
      player2Display.textContent = `Player 2: ${player2Emoji}`;
      closeSkinModal();
      resetGame();
    }

    function openSkinModal() {
      skinModal.style.display = 'flex';
    }

    function closeSkinModal() {
      skinModal.style.display = 'none';
    }

    // Add event listeners to cells
    cells.forEach(cell => cell.addEventListener('click', handleCellClick));
    resetButton.addEventListener('click', resetGame);
    skinButton.addEventListener('click', openSkinModal);
    resetGame();

    // Predefined colors
    const colors = [
      '#0ff', // Cyan
      '#ff0', // Yellow
      '#f0f', // Magenta
      '#f00', // Red
      '#0f0', // Green
      '#00f', // Blue
      '#fff', // White
    ];
    let currentColorIndex = 0;

    function changeColors() {
      currentColorIndex = (currentColorIndex + 1) % colors.length;
      const newColor = colors[currentColorIndex];
      document.documentElement.style.setProperty('--color', newColor);
      cells.forEach(cell => {
        cell.style.borderColor = newColor;
        cell.style.textShadow = `0 0 10px ${newColor}, 0 0 20px ${newColor}, 0 0 40px ${newColor}`;
        cell.style.boxShadow = `0 0 15px ${newColor}, 0 0 25px ${newColor} inset`;
      });
      colorChangeButton.style.color = newColor;
      colorChangeButton.style.borderColor = newColor;
      colorChangeButton.style.textShadow = `0 0 10px ${newColor}, 0 0 20px ${newColor}, 0 0 40px ${newColor}`;
    }

    colorChangeButton.addEventListener('click', changeColors);

    const audioToggleButton = document.getElementById('audioToggleButton');
    let isAudioEnabled = true; // Audio is enabled by default

    function toggleAudio() {
      isAudioEnabled = !isAudioEnabled;
      audioToggleButton.textContent = isAudioEnabled ? ' 🔇 ' : '🔊';
    }

    // Modify audio playback logic
    function playSound(audioElement) {
      if (isAudioEnabled) {
        audioElement.currentTime = 0; // Reset sound to the beginning
        audioElement.play(); // Play the sound
      }
    }

    // Update existing sound logic
    function handleCellClick(event) {
      const index = event.target.dataset.index;
      if (board[index] || event.target.classList.contains('dimmed')) return;

      board[index] = currentPlayer;
      event.target.textContent = currentPlayer;

      // Play click sound
      playSound(clickSound);

      const winner = checkWinner();
      if (winner) {
        message.textContent = winner === 'Draw' ? "It's a draw 🎭 !" : `${winner} wins!`;
        if (winner !== 'Draw') {
          playSound(winSound); // Play winning sound
          if (winner === player1Emoji) {
            player1Score++;
          } else {
            player2Score++;
          }
          updateScoreboard();
        }
        cells.forEach(cell => cell.removeEventListener('click', handleCellClick));
      } else {
        currentPlayer = currentPlayer === player1Emoji ? player2Emoji : player1Emoji;
        updatePlayerDisplay();
        message.textContent = `${currentPlayer}'s turn`;
      }
    }

    // Add event listener for the toggle button
    audioToggleButton.addEventListener('click', toggleAudio);
    
    // Function to open the skin modal
    function openSkinModal() {
      skinModal.style.display = 'flex'; // Show the modal
    }

    // Function to close the skin modal
    function closeSkinModal() {
      skinModal.style.display = 'none'; // Hide the modal
    }

    // Function to select a skin
    function selectSkin(emoji1, emoji2) {
      player1Emoji = emoji1;
      player2Emoji = emoji2;
      player1Display.textContent = `Player 1: ${player1Emoji}`;
      player2Display.textContent = `Player 2: ${player2Emoji}`;
      closeSkinModal(); // Close the modal after choosing skins
      resetGame(); // Reset the game to start with the new skins
    }

    // Add event listeners for buttons
    skinButton.addEventListener('click', openSkinModal); // Opens the modal on button click
    function toggleAudio() {
      isAudioEnabled = !isAudioEnabled; // Toggle the audio state
      const soundToggleButton = document.getElementById('sound-toggle');
      soundToggleButton.textContent = isAudioEnabled ? '🔊' : '🔇'; // Update button text
    }
    
    // Function to open the skin modal
function openSkinModal() {
  skinModal.style.display = 'flex'; // Show the modal when the button is clicked
}

// Function to close the skin modal
function closeSkinModal() {
  skinModal.style.display = 'none'; // Hide the modal
}

// Add event listener to the "Choose Skins" button to open the modal
skinButton.addEventListener('click', openSkinModal);
    
    
   


  </script>
  <div id="sound-toggle" onclick="toggleAudio()">🔊</div>
</body>
</html>
