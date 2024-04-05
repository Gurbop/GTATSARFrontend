---
layout: default
title: Admission Predictor
permalink: /prediction
---

<html>
<head>
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #f4f4f4;
            color: #333;
            line-height: 1.6;
        }
        h1 {
            text-align: center;
            font-size: 48px;
            margin-bottom: 30px;
            color: #2c3e50;
            animation: bounce 1s infinite alternate;
        }
        @keyframes bounce {
            0% {
                transform: translateY(0);
            }
            100% {
                transform: translateY(-30px);
            }
        }
        p#predictionResult {
            text-align: center;
            font-size: 50px;
            margin-top: 20px;
        }
        .container {
            width: 50%;
            margin: 0 auto;
        }
        .input-box {
            display: inline-block;
            margin-bottom: 20px;
            text-align: center;
        }
        .input-box label {
            display: block;
            margin-bottom: 5px;
            font-size: 18px;
            color: #2c3e50;
        }
        .input-box input {
            width: 250px;
            padding: 10px;
            border: 2px solid #3498db;
            border-radius: 10px;
            font-size: 16px;
            transition: border-color 0.3s;
        }
        .input-box input:focus {
            outline: none;
            border-color: #2980b9;
        }
        .btn {
            display: inline-block;
            background-color: #27ae60;
            color: #fff;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        .btn:hover {
            background-color: #219652;
        }
        .confetti {
            width: 10px;
            height: 10px;
            position: absolute;
            top: 0;
            animation: confetti-fall linear 4s infinite;
        }
        @keyframes confetti-fall {
            0% {
                transform: translateY(-100vh) rotateZ(0deg);
            }
            100% {
                transform: translateY(100vh) rotateZ(360deg);
            }
        }
    </style>
</head>
<body>
<center>
<h1>Admission Predictor</h1>
<p id="predictionResult"></p>
<div class="container">
<div class="input-box">
<label for="sat">SAT Score:</label><br>
<input type="number" id="sat" name="sat" placeholder="SAT score">
</div>
<div class="input-box">
<label for="gpa">GPA:</label><br>
<input type="number" step="0.01" id="gpa" name="gpa" placeholder="GPA">
</div>
<div class="input-box">
<label for="extracurriculars">Extracurriculars:</label><br>
<input type="number" id="Extracurricular_Activities" name="Extracurricular_Activities" placeholder="Extracurriculars">
</div>
<br>
<button class="btn" id="checkCompatibility">Check</button>
</div>
</center>
<script>
function makePrediction() {
    var gpa = document.getElementById("gpa").value;
    var sat = document.getElementById("sat").value;
    var extracurriculars = document.getElementById("Extracurricular_Activities").value;
    if (sat > 1600) {
        alert("SAT score cannot exceed 1600");
        return;
    }
    if (gpa > 5) {
        alert("GPA cannot exceed 5");
        return;
    }
    var data = {
        gpa: gpa,
        SAT: sat,
        Extracurricular_Activities: extracurriculars
    };
    fetch('http://127.0.0.1:8085/api/users/Prediction', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    })
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(result => {
        var predictionResultElement = document.getElementById("predictionResult");
        if (result === "Rejected") {
            predictionResultElement.style.color = "red";
        } else if (result === "Waitlisted") {
            predictionResultElement.style.color = "yellow";
        } else if (result === "Accepted") {
            predictionResultElement.style.color = "green";
            createConfetti();
        }
        predictionResultElement.textContent = result;
        alert("Prediction successful: " + result); 
    })
    .catch(error => {
        console.error('Error:', error);
        alert("Prediction request failed: " + error.message); 
    });
}
function createConfetti() {
    for (var i = 0; i < 100; i++) {
        var confetti = document.createElement('div');
        confetti.className = 'confetti';
        confetti.style.left = Math.random() * window.innerWidth + 'px';
        confetti.style.animationDelay = Math.random() * 4 + 's';
        confetti.style.backgroundColor = getRandomColor();
        document.body.appendChild(confetti);
    }
}
function getRandomColor() {
    var letters = '0123456789ABCDEF';
    var color = '#';
    for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
}
document.getElementById("checkCompatibility").addEventListener("click", makePrediction);
</script>
</body>
</html>