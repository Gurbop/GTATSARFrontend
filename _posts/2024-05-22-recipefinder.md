---
layout: post
title: Gourmet Guide
toc: false
permalink: recipe.html
---


<html>
<head>
    <title>Recipe Finder</title>
    <style>
        body {
            background-color: #ffffff; /* White background */
            color: #000000; /* Black text */
            font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
        }
        h1 {
            color: #0074D9; /* Blue heading */
        }
        #recipeForm {
            background-color: #f0f0f0; /* Light gray background for the form */
            padding: 20px;
            border-radius: 8px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #0074D9; /* Blue label text */
        }
        input[type="text"] {
            width: 100%;
            padding: 5px;
            border: 1px solid #0074D9; /* Blue border */
            border-radius: 4px;
            margin-bottom: 10px;
        }
        button {
            background-color: #0074D9; /* Blue button background */
            color: #ffffff; /* White button text */
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3; /* Darker blue on hover */
        }
        table {
            border-collapse: collapse;
            width: 100%;
        }
        table, th, td {
            border: 1px solid #0074D9; /* Blue table borders */
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
    </style>
</head>
<script>
    async function getData() {
        ingredients = document.getElementById('ingredients').value;
        const apiUrl = "http://127.0.0.1:8085/api/recipes/getrecipes/" + ingredients;
        const response = await fetch(apiUrl);
        var data = await response.json();
        console.log(data);
        let text = "<table>";
        for (let x in data) {
            text += "<tr><td>Recipe: " + data[x].title + "</td></tr>";
            if (data[x].image) {
                text += "<tr><td><img src='" + data[x].image + "' alt='Recipe Image'></td></tr>";
            } else {
                text += "<tr><td>Image not available</td></tr>";
            }
            text += "<tr><td>Likes: " + data[x].likes + "</td></tr>";
            text += "<tr><td>Spoonacular ID: " + data[x].id + "</td></tr>";
        }
        text += "</table>";
        document.getElementById("recipeForm").innerHTML = text;
    };
</script>
<body>
    <h1>Recipe Finder</h1>
    <img src="/student/images/Gourmet Guide.png" alt="Gourmet Guide Logo" width="100">
    <form id="recipeForm">
        <label for="ingredients">Enter ingredients (comma-separated):</label>
        <input type="text" name="ingredients" id="ingredients" required>
        <button type="button" onClick='getData()'>Find Recipe</button>
    </form>

</body>
</html>
