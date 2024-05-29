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
<head>
    <script>
        async function getData() {
            // Get ingredients from the input field
            let ingredients = document.getElementById('ingredients').value;  
            // Get selected allergens
            let checkboxes = document.querySelectorAll('input[name="allergens"]:checked');
            let allergens = Array.from(checkboxes).map(checkbox => checkbox.value);
            // Build the API URL
            const apiUrl = "http://127.0.0.1:8092/api/recipes/getrecipes/" + ingredients + "?allergens=" + allergens.join(',');
            try {
                // Fetch data from the API
                const response = await fetch(apiUrl);
                var data = await response.json();
                // Filter out recipes containing any selected allergens in their names
                allergens.forEach(allergen => {
                    data = data.filter(recipe => !recipe.title.toLowerCase().includes(allergen.toLowerCase()));
                });
                // Sort recipes by likes in descending order
                data.sort((a, b) => b.likes - a.likes);
                // Build the HTML table
                let text = "<table border='1'><tr><th>Recipe</th><th>Image</th><th>Likes</th><th>Spoonacular ID</th></tr>";
                for (let x in data) {
                    text += "<tr><td>" + data[x].title + "</td>";
                    if (data[x].image) {
                        text += "<td><img src='" + data[x].image + "' alt='Recipe Image' width='100'></td>";
                    } else {
                        text += "<td>Image not available</td>";
                    }
                    text += "<td>" + data[x].likes + "</td>";
                    text += "<td>" + data[x].id + "</td></tr>";
                }
                text += "</table>";
                // Update the HTML content
                document.getElementById("recipeForm").innerHTML = text;
            } catch (error) {
                console.error("Error fetching data:", error);
                document.getElementById("recipeForm").innerHTML = "Error fetching recipes. Please try again.";
            }
        }
    </script>
</head>
<body>
    <h1>Recipe Finder</h1>
    <img src="/student/images/Gourmet Guide.png" alt="Gourmet Guide Logo" width="100">
    <form id="recipeForm">
        <label for="ingredients">Enter ingredients (comma-separated):</label>
        <input type="text" name="ingredients" id="ingredients" required>
        <br><br>
        <label>Select types of food to exclude:</label><br>
        <input type="checkbox" name="allergens" value="soup"> Soup<br>
        <input type="checkbox" name="allergens" value="juice"> Juice<br>
        <input type="checkbox" name="allergens" value="salad"> Salad<br>
        <input type="checkbox" name="allergens" value="bowl"> Bowl<br>
        <input type="checkbox" name="allergens" value="cake"> Cake<br>
        <!-- Add more allergens as needed -->
        <br>
        <button type="button" onClick='getData()'>Find Recipe</button>
    </form>
</body>
</html>