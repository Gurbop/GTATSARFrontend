---
layout: base
---

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tic Tac Toe</title>
<style>
    body, input, button {
    font-family: 'Roboto', sans-serif;
    }
    body {
        margin: 0;
        padding: 0;
        height: 100vh;
        background-image: url('https://wallpapers.com/images/hd/plain-black-background-02fh7564l8qq4m6d.jpg');
        background-size: cover;
        background-position: center;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .container {
        display: grid;
        grid-template-columns: repeat(3, 150px);
        grid-template-rows: repeat(3, 150px);
        gap: 15px;
    }
    .box {
        background-color: white;
        width: 150px;
        height: 150px;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 48px;
        color: black;
        cursor: pointer;
    }
    .emptybox {
        background-color: white;
        width: 150px;
        height: 150px;
    }
    .cross::after {
        content: 'X';
    }
    .circle::after {
        content: 'O';
    }
    .money-container {
        display: flex;
        justify-content: space-between;
        width: 482px; /* Adjust as needed */
        margin-top: 20px;
    }
    .money-box {
        padding: 10px;
        border-radius: 5px;
        color: white;
        font-size: 20px;
    }
    .player-money {
        background-color: red;
    }
    .ai-money {
        background-color: blue;
    }
        /* Styling for the input field */
    input[type="text"] {
    padding: 10px;
    border: 2px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    outline: none;
    color: black
    }
    /* Styling for the submit button */
    button {
    padding: 10px 20px;
    background-color: blue;
    color: #fff;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    }
    button:hover {
    background-color: #0056b3;
    }
</style>
</head>
<body>

<div id="game-container">
    <div class="container">
        <!-- Boxes for the tic-tac-toe game -->
        <div class="box emptybox" id="box0"></div>
        <div class="box emptybox" id="box1"></div>
        <div class="box emptybox" id="box2"></div>
        <div class="box emptybox" id="box3"></div>
        <div class="box emptybox" id="box4"></div>
        <div class="box emptybox" id="box5"></div>
        <div class="box emptybox" id="box6"></div>
        <div class="box emptybox" id="box7"></div>
        <div class="box emptybox" id="box8"></div>
    </div>
</div>

<div class="money-container">
    <div id="player-money" class="money-box player-money">Player Score: 0</div>
    <div id="ai-money" class="money-box ai-money">AI Score: 0</div>
</div>

<!-- New input box for entering player's name -->
<br><br>
<div>
    <label for="player-name" color="white">Input Name:</label>
    <input type="text" id="player-name" name="player-name">
    <button onclick="submitToLeaderboard()">Submit to Leaderboard</button>
</div>

<script>
// JavaScript to handle the tic-tac-toe game logic
const boxes = document.querySelectorAll('.box');
let currentPlayer = ''; // Player's symbol (X or O)
let aiSymbol = ''; // AI's symbol (opposite of player)
let playerMoney = 0;
let aiMoney = 0;
let gameBoard = ['', '', '', '', '', '', '', '', ''];

// Define winning combinations
const winningCombinations = [
    [0, 1, 2], // Top row
    [3, 4, 5], // Middle row
    [6, 7, 8], // Bottom row
    [0, 3, 6], // Left column
    [1, 4, 7], // Middle column
    [2, 5, 8], // Right column
    [0, 4, 8], // Diagonal from top-left
    [2, 4, 6]  // Diagonal from top-right
];

// Function to display the winning combination
function displayWinningCombination(winningCombination, player) {
    winningCombination.forEach(index => {
        boxes[index].textContent = player;
        boxes[index].style.fontSize = '100px'; // Increase font size
        boxes[index].classList.add('winning-box'); // Add a class to identify winning boxes
    });
}

function checkWin(player) {
    let winningCombination = winningCombinations.find(combination => {
        return combination.every(index => gameBoard[index] === player);
    });

    if (winningCombination) {
        // Display the winning symbols
        displayWinningCombination(winningCombination, player);

        // Check if AI wins after its move
        if (player === aiSymbol) {
            setTimeout(() => {
                alert(`AI wins!`);
                // Update AI's score
                aiMoney++;
                document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
                resetGame();
            }, 500); // Adjust delay as needed
        } else {
            // Check if player wins after its move
            setTimeout(() => {
                alert(`Player wins!`);
                // Update player's score
                playerMoney++;
                document.getElementById('player-money').textContent = `Player Score: ${playerMoney}`;
                resetGame();
            }, 500); // Adjust delay as needed
        }
        return true;
    }

    // Check for a tie (if the board is full)
    if (gameBoard.every(cell => cell !== '')) {
        // Delay before showing the alert
        setTimeout(() => {
            // Alert tie message
            alert('It\'s a tie!');
            // Reset the game to start a new one
            resetGame();
        }, 500); // Adjust delay as needed
        return true;
    }

    return false;
}

// Event handler for box clicks
function boxClickHandler(event) {
    const box = event.target;
    const index = parseInt(box.id.replace('box', ''));
    if (box.textContent === '' && gameBoard[index] === '') {
        box.textContent = currentPlayer;
        box.style.fontSize = '100px'; // Increase font size
        gameBoard[index] = currentPlayer;

        // Check if player wins after their move
        if (checkWin(currentPlayer)) {
            return;
        }

        // Check for a tie (if the board is full)
        if (gameBoard.every(cell => cell !== '')) {
            alert('It\'s a tie!');
            return;
        }

        makeAIMoveWithDelay(); // Make AI move after a delay
    }
}

// Inside the click event listener for player's move
// After the player's move, call makeAIMove with a delay
boxes.forEach(box => {
    box.addEventListener('click', boxClickHandler);
});

// Function to randomly assign X or O to player and determine AI's symbol
function assignRoles() {
    currentPlayer = Math.random() < 0.5 ? 'X' : 'O'; // Randomly assign X or O to player
    aiSymbol = currentPlayer === 'X' ? 'O' : 'X'; // Determine AI's symbol based on player's
}

// Function to let the AI make a move
function makeAIMove() {
    let emptyIndexes = [];
    gameBoard.forEach((cell, index) => {
        if (cell === '') {
            emptyIndexes.push(index);
        }
    });

    // Check if AI can win in the next move
    for (let i = 0; i < emptyIndexes.length; i++) {
        let tempBoard = [...gameBoard];
        tempBoard[emptyIndexes[i]] = aiSymbol;
        for (let j = 0; j < winningCombinations.length; j++) {
            const [a, b, c] = winningCombinations[j];
            if (tempBoard[a] === aiSymbol && tempBoard[b] === aiSymbol && tempBoard[c] === aiSymbol) {
                // AI can win, so block the winning move
                gameBoard[emptyIndexes[i]] = aiSymbol;
                boxes[emptyIndexes[i]].textContent = aiSymbol;
                boxes[emptyIndexes[i]].style.fontSize = '100px'; // Increase font size

                // Check if AI wins after its move
                if (checkWin(aiSymbol)) {
                    document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
                }
                return;
            }
        }
    }

    // Check if player can win in the next move and block it
    for (let i = 0; i < emptyIndexes.length; i++) {
        let tempBoard = [...gameBoard];
        tempBoard[emptyIndexes[i]] = currentPlayer;
        for (let j = 0; j < winningCombinations.length; j++) {
            const [a, b, c] = winningCombinations[j];
            if (tempBoard[a] === currentPlayer && tempBoard[b] === currentPlayer && tempBoard[c] === currentPlayer) {
                // Player can win, so block the winning move
                gameBoard[emptyIndexes[i]] = aiSymbol;
                boxes[emptyIndexes[i]].textContent = aiSymbol;
                boxes[emptyIndexes[i]].style.fontSize = '100px'; // Increase font size

                // Check if AI wins after its move
                if (checkWin(aiSymbol)) {
                    alert(`AI wins!`);
                    // Update AI's score
                    aiMoney++;
                    document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
                }
                return;
            }
        }
    }

    // If no winning moves to block, choose a random empty cell
    let randomIndex = Math.floor(Math.random() * emptyIndexes.length);
    let aiIndex = emptyIndexes[randomIndex];
    // Place AI's symbol at the chosen index
    gameBoard[aiIndex] = aiSymbol;
    boxes[aiIndex].textContent = aiSymbol;
    boxes[aiIndex].style.fontSize = '100px'; // Increase font size

    // Check if AI wins after its move
    if (checkWin(aiSymbol)) {
        alert(`AI wins!`);
        // Update AI's score
        aiMoney++;
        document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
    }
}


// Function to reset the game
function resetGame() {
    gameBoard = ['', '', '', '', '', '', '', '', ''];
    boxes.forEach(box => {
        box.textContent = '';
        box.style.fontSize = '48px'; // Reset font size
        box.classList.remove('winning-box'); // Remove the winning-box class
    });
    assignRoles();
    // Add event listeners for box clicks
    boxes.forEach(box => {
        box.addEventListener('click', boxClickHandler);
    });
}

// Inside the click event listener for player's move
// After the player's move, call makeAIMove with a delay
boxes.forEach((box, index) => {
    box.addEventListener('click', () => {
        if (box.textContent === '' && gameBoard[index] === '') {
            box.textContent = currentPlayer;
            box.style.fontSize = '100px'; // Increase font size
            gameBoard[index] = currentPlayer;

            // Check if player wins after their move
            if (checkWin(currentPlayer)) {
                return;
            }

            // Check for a tie (if the board is full)
            if (gameBoard.every(cell => cell !== '')) {
                alert('It\'s a tie!');
                return;
            }

            makeAIMoveWithDelay(); // Make AI move after a delay
        }
    });
});

// Function to let the AI make a move after a delay
function makeAIMoveWithDelay() {
    setTimeout(makeAIMove, 500); // 0.5-second delay
}

// Function to initialize the game
function initGame() {
    assignRoles();
}

initGame(); // Call initGame() to start the game

// Function to submit player's score to the leaderboard
function submitToLeaderboard() {
    const playerName = document.getElementById('player-name').value;
    const playerScore = playerMoney; // Assuming playerMoney contains the player's score

    // Data to be sent to the API
    const data = {
        name: playerName,
        game_points: playerScore
    };

    // Configuring the fetch request
    const options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    };

    // Sending the POST request to the leaderboard API
    fetch('http://127.0.0.1:5000/players', options)
        .then(response => {
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
            return response.json();
        })
        .then(data => {
            console.log('Data sent to leaderboard:', data);
            alert('Score submitted to leaderboard successfully!');
        })
        .catch(error => {
            console.error('Error submitting score to leaderboard:', error);
            alert('Failed to submit score to leaderboard!');
        });
}
</script>

</body>
</html>
