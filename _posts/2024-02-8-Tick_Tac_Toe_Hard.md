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
        width: 482px; 
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
    input[type="text"] {
    padding: 10px;
    border: 2px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    outline: none;
    color: black
    }
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

<br><br>
<div>
    <label for="player-name" color="white">Input Name:</label>
    <input type="text" id="player-name" name="player-name" placeholder="Enter your name">
    <button onclick="submitToLeaderboard()">Submit to Leaderboard</button>
</div>

<script>
const boxes = document.querySelectorAll('.box');
let currentPlayer = ''; 
let aiSymbol = ''; 
let playerMoney = 0;
let aiMoney = 0;
let gameBoard = ['', '', '', '', '', '', '', '', ''];

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

function displayWinningCombination(winningCombination, player) {
    winningCombination.forEach(index => {
        boxes[index].textContent = player;
        boxes[index].style.fontSize = '100px'; 
        boxes[index].classList.add('winning-box'); 
    });
}

function checkWin(player) {
    let winningCombination = winningCombinations.find(combination => {
        return combination.every(index => gameBoard[index] === player);
    });

    if (winningCombination) {
        displayWinningCombination(winningCombination, player);

        if (player === aiSymbol) {
            setTimeout(() => {
                alert(`AI wins!`);
                aiMoney++;
                document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
                resetGame();
            }, 500); 
        } else {
            setTimeout(() => {
                alert(`Player wins!`);
                playerMoney++;
                document.getElementById('player-money').textContent = `Player Score: ${playerMoney}`;
                resetGame();
            }, 500); 
        }
        return true;
    }

    if (gameBoard.every(cell => cell !== '')) {
        setTimeout(() => {
            alert('It\'s a tie!');
            resetGame();
        }, 500); 
        return true;
    }

    return false;
}

function boxClickHandler(event) {
    const box = event.target;
    const index = parseInt(box.id.replace('box', ''));
    if (box.textContent === '' && gameBoard[index] === '') {
        box.textContent = currentPlayer;
        box.style.fontSize = '100px'; 
        gameBoard[index] = currentPlayer;

        if (checkWin(currentPlayer)) {
            return;
        }

        if (gameBoard.every(cell => cell !== '')) {
            alert('It\'s a tie!');
            return;
        }

        makeAIMoveWithDelay(); 
    }
}


boxes.forEach(box => {
    box.addEventListener('click', boxClickHandler);
});

function assignRoles() {
    currentPlayer = Math.random() < 0.5 ? 'X' : 'O'; 
    aiSymbol = currentPlayer === 'X' ? 'O' : 'X'; 
}

function makeAIMove() {
    let emptyIndexes = [];
    gameBoard.forEach((cell, index) => {
        if (cell === '') {
            emptyIndexes.push(index);
        }
    });

    for (let i = 0; i < emptyIndexes.length; i++) {
        let tempBoard = [...gameBoard];
        tempBoard[emptyIndexes[i]] = aiSymbol;
        for (let j = 0; j < winningCombinations.length; j++) {
            const [a, b, c] = winningCombinations[j];
            if (tempBoard[a] === aiSymbol && tempBoard[b] === aiSymbol && tempBoard[c] === aiSymbol) {
                gameBoard[emptyIndexes[i]] = aiSymbol;
                boxes[emptyIndexes[i]].textContent = aiSymbol;
                boxes[emptyIndexes[i]].style.fontSize = '100px'; 

                if (checkWin(aiSymbol)) {
                    document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
                }
                return;
            }
        }
    }

    for (let i = 0; i < emptyIndexes.length; i++) {
        let tempBoard = [...gameBoard];
        tempBoard[emptyIndexes[i]] = currentPlayer;
        for (let j = 0; j < winningCombinations.length; j++) {
            const [a, b, c] = winningCombinations[j];
            if (tempBoard[a] === currentPlayer && tempBoard[b] === currentPlayer && tempBoard[c] === currentPlayer) {
                gameBoard[emptyIndexes[i]] = aiSymbol;
                boxes[emptyIndexes[i]].textContent = aiSymbol;
                boxes[emptyIndexes[i]].style.fontSize = '100px'; 

                if (checkWin(aiSymbol)) {
                    alert(`AI wins!`);
                    aiMoney++;
                    document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
                }
                return;
            }
        }
    }

    let randomIndex = Math.floor(Math.random() * emptyIndexes.length);
    let aiIndex = emptyIndexes[randomIndex];
    gameBoard[aiIndex] = aiSymbol;
    boxes[aiIndex].textContent = aiSymbol;
    boxes[aiIndex].style.fontSize = '100px'; 
    if (checkWin(aiSymbol)) {
        alert(`AI wins!`);
        aiMoney++;
        document.getElementById('ai-money').textContent = `AI Score: ${aiMoney}`;
    }
}


function resetGame() {
    gameBoard = ['', '', '', '', '', '', '', '', ''];
    boxes.forEach(box => {
        box.textContent = '';
        box.style.fontSize = '48px'; 
        box.classList.remove('winning-box'); 
    });
    assignRoles();
    boxes.forEach(box => {
        box.addEventListener('click', boxClickHandler);
    });
}


boxes.forEach((box, index) => {
    box.addEventListener('click', () => {
        if (box.textContent === '' && gameBoard[index] === '') {
            box.textContent = currentPlayer;
            box.style.fontSize = '100px'; // Increase font size
            gameBoard[index] = currentPlayer;

            if (checkWin(currentPlayer)) {
                return;
            }

            if (gameBoard.every(cell => cell !== '')) {
                alert('It\'s a tie!');
                return;
            }

            makeAIMoveWithDelay(); 
        }
    });
});

function makeAIMoveWithDelay() {
    setTimeout(makeAIMove, 500); 
}

function initGame() {
    assignRoles();
}

initGame(); 

function submitToLeaderboard() {
    const playerName = document.getElementById('player-name').value;
    const playerScore = playerMoney; 

    const data = {
        name: playerName,
        game_points: playerScore
    };

    const options = {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    };

    fetch('http://127.0.0.1:8086/players', options)
        .then(response => {
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
            return response.json();
        })
        .then(data => {
            console.log('data sent to leaderboard:', data);
            alert('score submtted to leaderboard successfully!');
        })
        .catch(error => {
            console.error('error submitting score to leaderboard:', error);
            alert('failed to submit score to leaderboard!');
        });
}
</script>

</body>
</html>
