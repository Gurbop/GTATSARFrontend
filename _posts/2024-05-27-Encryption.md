---
toc: true
comments: false
layout: post
title: Ceaser-Cipher Encrypter
description: 
type: hacks
courses: { compsci: {week: 36} }
image: images/plan-dice-760.jpg
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Encrypter</title>
</head>
<style>
    body {
        font-family: 'Roboto', sans-serif;
        background-color: #000;
        color: #fff;
    }
    input[type="text"] {
        padding: 15px;
        font-size: 18px;
        width: 70%;
        max-width: 500px;
        box-sizing: border-box;
        border: none;
        border-radius: 20px;
        text-align: center;
        box-shadow: 0 0 0 2px #fff; /* 2px white outline */
    }
    #message {
        display: none;
        margin-top: 10px;
        padding: 10px;
        background-color: #4CAF50;
        color: white;
        border-radius: 5px;
        width: 70%;
        max-width: 500px;
        text-align: center;
    }
    button {
        padding: 15px;
        font-size: 18px;
        background-color: #4CAF50;
        color: white;
        border: none;
        border-radius: 20px;
        cursor: pointer;
        margin-top: 20px;
        transition: background-color 0.3s;
                    box-shadow: 0 0 0 2px #fff; /* 2px white outline */
    }
    button:hover {
        background-color: #45a049;
    }
        table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 20px;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
}
th, td {
    padding: 10px;
    text-align: left;
    border-bottom: 1px solid #fff;
}
th {
    background-color: #333;
}
tr:nth-child(even) {
    background-color: #444;
}
.position {
    text-align: center;
}
</style>
<body>


<h3>User Guidlines</h3>
  <p style="color: red;">- Please enter your raw data for encryption first and then click to get!</p>
        <p style="color: red;">- Use Encrypted ID for sharing and decryption, dont share it with anyone you dont want known the encrypted data</p><br><br><center><br><br>
    <input type="text" id="inputText" placeholder="Type & Enter for Encryption" onkeydown="if (event.keyCode === 13) sendData()"><br><br><br><br></center>
<center>
    <button onclick="getEncryptedData()">Click to Get Encrypted Data</button><br><br><br><br>
    </center>


<div id="notification-popup" style="display: none;"></div>

<table id="encryptedTable" border="1">
<thead>
    <tr>
        <th>Encrypted ID</th>
        <th>Raw Data</th>
        <th>Encrypted Data</th>
    </tr>
</thead>
<tbody id="encryptedTableBody"></tbody>
</table>

<script>
function sendData() {
    var inputText = document.getElementById("inputText").value;
    var xhr = new XMLHttpRequest();
    xhr.open("POST", "http://127.0.0.1:8085/encrypted_data", true);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
            if (xhr.status === 201) {
                console.log("Data sent successfully");
                showAlert("Data sent successfully to the backend API.");
            } else {
                console.error("Error sending data:", xhr.statusText);
                showAlert("Data successfully send to the backend! Please now click thte GET button to recieve encrypted data!", true);
            }
        }
    };
    var data = JSON.stringify({ "raw_data": inputText });
    xhr.send(data);
}

function showAlert(message, isError = false) {
    if (isError) {
        alert("Success :) " + message);
    } else {
        alert(message);
    }
}

function displayEncryptedData(encryptedData) {
    var tableBody = document.getElementById("encryptedTableBody");
    tableBody.innerHTML = "";

    encryptedData.forEach(function (item) {
        var row = document.createElement("tr");

        var idCell = document.createElement("td");
        idCell.innerText = item.encrypted_id;
        row.appendChild(idCell);

        var rawDataCell = document.createElement("td");
        rawDataCell.innerText = item.raw_data;
        row.appendChild(rawDataCell);

        var dataCell = document.createElement("td");
        dataCell.innerText = item.encrypted_data;
        row.appendChild(dataCell);

        tableBody.appendChild(row);
    });
}

function getEncryptedData() {
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "http://127.0.0.1:8085/encrypted_data", true);
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
            if (xhr.status === 200) {
                var responseData = JSON.parse(xhr.responseText);
                displayEncryptedData([responseData]);
            } else {
                console.error("Error fetching encrypted data:", xhr.statusText);
                showAlert("Error fetching encrypted data from the backend API. Please try again later.", true);
            }
        }
    };
    xhr.send();
}
</script>

</body>
</html>
