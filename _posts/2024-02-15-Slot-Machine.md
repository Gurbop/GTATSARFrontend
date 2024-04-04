---
toc: false
comments: false
layout: default
title: Slot Machine Game
description: 
type: hacks
courses: { compsci: {week: 2} }
---

<html>
<head>
    <title>Slot Machine Game</title>
    <style>
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
        /* Animation for winning lights */
        @keyframes winLight {
            0% {
                background-color: #ffec40;
            }
            50% {
                background-color: #ffcc00;
            }
            100% {
                background-color: #ffec40;
            }
        }
        /* Add a class for winning animation */
        .win-animation {
            animation: winLight 2s ease-in-out infinite;
        }
        /* Casino-style background */
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            background-image: url('https://your-casino-background-image-url.jpg');
            background-size: cover;
            opacity: 0.5;
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
                        // Add the win animation class to the slots
                        document.getElementById('slot1').classList.add('win-animation');
                        document.getElementById('slot2').classList.add('win-animation');
                        document.getElementById('slot3').classList.add('win-animation');
                        // Re-enable the spin button after the animation duration
                        setTimeout(() => {
                            document.getElementById('slot1').classList.remove('win-animation');
                            document.getElementById('slot2').classList.remove('win-animation');
                            document.getElementById('slot3').classList.remove('win-animation');
                            // Re-enable the spin button
                            spinButton.disabled = false;
                        }, 2000);
                    } else if (result[0] === result[1] || result[1] === result[2] || result[0] === result[2]) {
                        credits += 3;
                        updateCredits();
                        resultMessage.textContent = 'You won 3 credits!';
                        // Add the win animation class to the slots
                        document.getElementById('slot1').classList.add('win-animation');
                        document.getElementById('slot2').classList.add('win-animation');
                        document.getElementById('slot3').classList.add('win-animation');
                        // Re-enable the spin button after the animation duration
                        setTimeout(() => {
                            document.getElementById('slot1').classList.remove('win-animation');
                            document.getElementById('slot2').classList.remove('win-animation');
                            document.getElementById('slot3').classList.remove('win-animation');
                            // Re-enable the spin button
                            spinButton.disabled = false;
                        }, 2000);
                    } else {
                        resultMessage.textContent = 'You lost. Try again!';
                        // Re-enable the spin button if not a win
                        spinButton.disabled = false;
                    }
                }
            }
            // Start the slot animation
            animateSlots();
        }
        function updateCredits() {
            document.getElementById('credits').textContent = `Credits: ${credits}`;
        }
    </script>
</body>
</html>