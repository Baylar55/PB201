## JS - API

### Task 1:
#### Aşağıdakı api-lərdən istifadə edərək şəhərə görə havanın temperaturunu göstərən mini web app yazın.

```cs
https://geocoding-api.open-meteo.com/v1/search?name=${city}
```

```cs
https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current_weather=true&timezone=Europe/London
```

#### Index.html faylı aşağıdakı kimi olacaq. Sadəcə js folderinin src-sini düzgün şəkildə verin.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather Application</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: #f0f0f0;
        }
        .weather-container {
            border: 1px solid #ccc;
            padding: 20px;
            background-color: #fff;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <div class="weather-container">
        <h1>Weather Application</h1>
        <input type="text" id="cityInput" placeholder="Enter city name">
        <button id="getWeatherBtn">Get Weather</button>
        <div id="weatherInfo"></div>
    </div>
    <script src="./assets/js/app.js"></script>
</body>
</html>
```

#### app.js faylı aşağıdakı kimi olacaq :
```js
let weatherInfo = document.getElementById('weatherInfo');
document.getElementById('getWeatherBtn').addEventListener('click', function() {
    const city = document.getElementById('cityInput').value;
    if (city === "") {
        document.getElementById('weatherInfo').innerHTML = "<p>Please enter a city name.</p>";
        return;
    }
    fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${city}`)
        .then(response => response.json())  
        .then(data => {
            if (data.results && data.results.length > 0) {

                const latitude = data.results[0].latitude;
                const longitude = data.results[0].longitude;
                
                fetch(`https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current_weather=true&timezone=Europe/London`)
                    .then(response =>  response.json())
                    .then(weatherData => {
                        weatherInfo.innerHTML = `
                            <h2>${city}</h2>
                            <p>Temperature: ${weatherData.current_weather.temperature}°C</p>
                            <p>Weather: ${weatherData.current_weather.weathercode}</p>
                            <p>Wind Speed: ${weatherData.current_weather.windspeed} m/s</p>
                        `;
                    })
            } else {
                weatherInfo.innerHTML = "<p>City not found. Please enter a valid city name.</p>";
            }
        })
});
```
