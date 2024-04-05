---
layout: default
---

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Leaderboard</title>
<style>
    /* CSS styles for the leaderboard table */
    body {
        font-family: 'Roboto', sans-serif;
        background-color: #000;
        color: #fff;
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

<h1>Leaderboard</h1>
<br><br>
<table>
  <thead>
    <tr>
      <th class="position">Podium</th>
      <th>Username</th>
      <th>Game Points</th>
    </tr>
  </thead>
  <tbody id="result">
    <!-- JavaScript generated data -->
  </tbody>
</table>

<script type="module">
  // Set Users endpoint (list of users)
  const url = 'http://127.0.0.1:5000/players';

  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

  // fetch the API
  fetch(url)
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        console.log(data);

        // Sort data by game points (highest to lowest)
        data.sort((a, b) => b.game_points - a.game_points);

        for (let i = 0; i < data.length; i++) {
            const row = data[i];
            // tr and td build out for each row
            const tr = document.createElement("tr");
            const position = document.createElement("td");
            const username = document.createElement("td");
            const gamePoints = document.createElement("td");
            // data is specific to the API
            position.textContent = i + 1;
            position.classList.add("position");
            username.textContent = row.name;
            gamePoints.textContent = row.game_points;
            // this builds td's into tr
            tr.appendChild(position);
            tr.appendChild(username);
            tr.appendChild(gamePoints);

            // append the row to table
            resultContainer.appendChild(tr);
        }
    })
    // catch fetch errors (ie ACCESS to server blocked)
    .catch(err => {
        console.error(err);
        const tr = document.createElement("tr");
        const td = document.createElement("td");
        td.textContent = err + ": " + url;
        tr.appendChild(td);
        resultContainer.appendChild(tr);
    });
</script>

</body>
</html>
