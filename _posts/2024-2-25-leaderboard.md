---
layout: default
---

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Leaderboard</title>
<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    color: white; /* Change text color to white */
    background-color: black; /* Add black background */
}
table {
    width: 50%;
    margin: 20px auto;
    border-collapse: collapse;
    text-align: center; /* Center table content */
}
th, td {
    border: 1px solid #ddd;
    padding: 8px;
}
th {
    background-color: #f2f2f2;
}
</style>
</head>
<body>

<h1>Leaderboard</h1>

<table>
  <tr>
    <th>Username</th>
    <th>Points</th>
  </tr>
  <!-- Leaderboard data will be inserted here -->
</table>

<script>
  fetchLeaderboardData();

  function fetchLeaderboardData() {
    fetch('http://127.0.0.1:5000/leaderboard_data')
      .then(response => response.json())
      .then(data => {
        // Sort data by points
        data.sort((a, b) => b.points - a.points);

        // Clear previous leaderboard data
        const table = document.querySelector('table');
        table.innerHTML = `
          <tr>
            <th>Username</th>
            <th>Points</th>
          </tr>
        `;

        // Populate leaderboard table with sorted data
        data.forEach(entry => {
          const row = document.createElement('tr');
          row.innerHTML = `
            <td>${entry.username}</td>
            <td>${entry.points}</td>
          `;
          table.appendChild(row);
        });
      })
      .catch(error => console.error('Error fetching leaderboard data:', error));
  }
</script>

</body>
</html>