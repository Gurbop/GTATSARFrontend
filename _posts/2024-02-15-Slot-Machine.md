---
layout: base
---

<html>
<head>
    <title>Slot Machine Game</title>
    <style>
        body {
    font-family: 'Roboto', sans-serif;
    }
        body {
            background-color: #000;
            font-family: Arial, sans-serif;
            color: #fff;
        }
        h1 {
            text-align: center;
            margin-top: 20px;
            font-size: 36px;
            text-transform: uppercase;
            letter-spacing: 4px;
        }
        #slot-machine {
            width: 700px;
            margin: 0 auto;
            padding: 20px;
            background-color: #222;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
            text-align: center;
            margin-top: 50px;
        }
        .slot {
            display: inline-block;
            width: 120px;
            height: 200px;
            border: 2px solid #ccc;
            margin: 20px;
            vertical-align: top;
            font-size: 64px;
            text-align: center;
            line-height: 200px;
            border-radius: 10px;
            background-color: #f9f9f9;
            box-shadow: inset 0px 0px 10px rgba(0, 0, 0, 0.2);
        }
        .spin-button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 24px;
            cursor: pointer;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 5px;
            transition: background-color 0.3s ease-in-out;
        }
        .spin-button:hover {
            background-color: #0056b3;
        }
        .credits {
            text-align: center;
            font-size: 24px;
            margin-top: 20px;
            letter-spacing: 2px;
        }
        .result-message {
            font-size: 24px;
            margin-top: 20px;
        }
        /* Add styles for the input field */
        input[type="text"] {
            padding: 10px;
            border: 2px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
            outline: none;
            color: black;
            margin-top: 20px;
        }
        /* Add styles for the submit button */
        button {
            padding: 10px 20px;
            background-color: blue;
            color: #fff;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            margin-top: 10px;
        }
        button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <h1>Slot Machine Game</h1>
    <div id="slot-machine">
        <div class="slot" id="slot1"></div>
        <div class="slot" id="slot2"></div>
        <div class="slot" id="slot3"></div>
        <div class="credits" id="credits">Credits: 100</div>
        <p id="result-message" class="result-message"></p>
        <button id="spin-button" class="spin-button" onclick="spin()">Spin</button>
        <!-- Name input box and submit button --><br><br>
        <input type="text" id="player-name" placeholder="Enter your name">
        <button onclick="submitToLeaderboard()">Submit Score to Leaderboard</button>
    </div>
    <script>
        const symbols = ['🍒', '🍋', '🍊', '🍇', '🍎', '🍉', '🍓', '🍌'];
        let credits = 100;

        function spin() {
            const resultMessage = document.getElementById('result-message');
            const spinButton = document.getElementById('spin-button');
            if (credits < 1) {
                resultMessage.textContent = 'Out of credits! Game over.';
                return;
            }
            // Disable the spin button during the animation
            spinButton.disabled = true;
            credits--;
            updateCredits();
            // Simulate spinning effect for a short duration
            const spinDuration = 1500; // milliseconds
            const startTime = Date.now();
            function animateSlots() {
                const currentTime = Date.now();
                const elapsedTime = currentTime - startTime;
                if (elapsedTime < spinDuration) {
                    // Update the slots with random symbols
                    document.getElementById('slot1').textContent = symbols[Math.floor(Math.random() * symbols.length)];
                    document.getElementById('slot2').textContent = symbols[Math.floor(Math.random() * symbols.length)];
                    document.getElementById('slot3').textContent = symbols[Math.floor(Math.random() * symbols.length)];
                    // Continue animation
                    requestAnimationFrame(animateSlots);
                } else {
                    // Animation complete, set the final result
                    const result = [];
                    for (let i = 0; i < 3; i++) {
                        const randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                        result.push(randomSymbol);
                    }
                    document.getElementById('slot1').textContent = result[0];
                    document.getElementById('slot2').textContent = result[1];
                    document.getElementById('slot3').textContent = result[2];
                    // Check for a win
                    if (result[0] === result[1] && result[1] === result[2]) {
                        credits += 10;
                        updateCredits();
                        resultMessage.textContent = 'You won 10 credits!';
                    } else if (result[0] === result[1] || result[1] === result[2] || result[0] === result[2]) {
                        credits += 3;
                        updateCredits();
                        resultMessage.textContent = 'You won 3 credits!';
                    } else {
                        resultMessage.textContent = 'You lost. Try again!';
                    }
                    // Re-enable the spin button after the animation
                    spinButton.disabled = false;
                }
            }
            // Start the slot animation
            animateSlots();
        }

        function updateCredits() {
            document.getElementById('credits').textContent = `Credits: ${credits}`;
        }

        // Function to submit player's score to the leaderboard
        function submitToLeaderboard() {
            const playerName = document.getElementById('player-name').value;
            const playerScore = credits; // Assuming credits contains the player's score

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
            fetch('http://127.0.0.1:8085/players', options)
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
