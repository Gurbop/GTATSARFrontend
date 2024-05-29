---
toc: true
comments: false
layout: post
title: Ceaser-Cipher Decrypter
description: 
type: hacks
courses: { compsci: {week: 36} }
image: images/plan-dice-760.jpg
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>API Data Fetcher</title>
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
</head>
<body>
<h3>User Guidlines</h3>
  <p style="color: red;">- Please enter the secret key you recieved when you or someone else encrypted the string of letters</p>
        <p style="color: red;">- Enter said secret key and receive your decrypted data</p><br><br><br><br><center>
    <input type="text" id="encryptedId" placeholder="Enter secret key"><br><br>
    <button onclick="fetchData()">Get Decrypted</button><br><br></center>
    <h2>Decrypted Data</h2><br><br>
    <table id="dataTable">
        <thead>
            <tr>
                <th>Secret Key</th>
                <th>Decrypted Data</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>
    <script>
        async function fetchData() {
            const encryptedId = document.getElementById('encryptedId').value;
            const url = `http://127.0.0.1:8085/encrypted_id/encrypted/${encryptedId}`;
            try {
                const response = await fetch(url);
                if (!response.ok) {
                    throw new Error('Network response was not ok ' + response.statusText);
                }
                const data = await response.json();
                if (data && data.encrypted_id && data.raw_data) {
                    const table = document.getElementById('dataTable').getElementsByTagName('tbody')[0];
                    const newRow = table.insertRow();
                    const cell1 = newRow.insertCell(0);
                    const cell2 = newRow.insertCell(1);
                    cell1.textContent = data.encrypted_id;
                    cell2.textContent = data.raw_data;
                } else {
                    alert('No data found for the given encrypted ID');
                }
            } catch (error) {
                console.error('There has been a problem with your fetch operation:', error);
                alert('Failed to fetch data');
            }
        }
    </script>
</body>
</html>
