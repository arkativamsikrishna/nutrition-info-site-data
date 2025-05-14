# nutrition-info-site-data
<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Nutrition Info Finder</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f3f3f3;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem;
    }
    h1 {
      color: #2c3e50;
    }
    #app {
      background: white;
      padding: 2rem;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      width: 100%;
      max-width: 500px;
    }
    input {
      width: 100%;
      padding: 0.5rem;
      margin-bottom: 1rem;
      font-size: 1rem;
    }
    button {
      padding: 0.5rem 1rem;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1rem;
    }
    button:hover {
      background-color: #2980b9;
    }
    #result {
      margin-top: 1.5rem;
      font-size: 1.1rem;
      color: #333;
    }
  </style>
</head>
<body>
  <div id="app">
    <h1>Nutrition Info Finder</h1>
    <input type="text" id="foodInput" placeholder="Enter food name (e.g. banana)" />
    <button onclick="getNutrition()">Get Info</button>
    <div id="result"></div>
  </div>  <script>
    async function getNutrition() {
      const food = document.getElementById('foodInput').value.trim();
      const resultDiv = document.getElementById('result');

      if (!food) {
        resultDiv.innerHTML = "Please enter a food name.";
        return;
      }

      const url = `https://world.openfoodfacts.org/cgi/search.pl?search_terms=${encodeURIComponent(food)}&search_simple=1&action=process&json=1`;

      try {
        const response = await fetch(url);
        const data = await response.json();

        if (data.products && data.products.length > 0) {
          const item = data.products[0];
          const nutrients = item.nutriments || {};
          resultDiv.innerHTML = `
            <strong>${item.product_name || 'Unnamed Product'}</strong><br>
            Calories: ${nutrients['energy-kcal'] || 'N/A'} kcal<br>
            Protein: ${nutrients.proteins || 'N/A'} g<br>
            Fat: ${nutrients.fat || 'N/A'} g<br>
            Calcium: ${nutrients.calcium || nutrients.calcium_100g || 'N/A'} mg<br>
            Vitamin A: ${nutrients['vitamin-a'] || 'N/A'} µg<br>
            Vitamin B12: ${nutrients['vitamin-b12'] || 'N/A'} µg<br>
            Vitamin C: ${nutrients['vitamin-c'] || 'N/A'} mg<br>
            Vitamin D: ${nutrients['vitamin-d'] || 'N/A'} µg
          `;
        } else {
          resultDiv.innerHTML = "No data found.";
        }
      } catch (error) {
        resultDiv.innerHTML = "Error fetching data.";
      }
    }
  </script></body>
</html>
