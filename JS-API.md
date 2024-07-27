# JS - API

## Task 1:
### Aşağıdakı api-lərdən istifadə edərək şəhərə görə havanın temperaturunu göstərən mini web app yazın.

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

## Task 2:
### https://fakestoreapi.com adresindəki api-lərdən istifadə edərək mini web app yazın.Səhifə açıldıqda sol tərəfdə select option hissəsində bütün kateqoriyalar api-dən gəlməlidir. Kateqoriyalardan hansısa biri seçildikdə sadəcə həmin kateqoriyaya uyğun productlar ekranda görünməlidir. Bundan əlavə səhifədə product adına görə search eləmək üçün input olmalıdır. İnput doldurulanda sadəcə həmin value-a və seçilmiş kateqoriyaya görə productlar ekranda görünməlidir.



#### Index.html faylı aşağıdakı kimi olacaq. Sadəcə js folderinin src-sini düzgün şəkildə verin.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FakeStore Product Catalog</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            padding: 20px;
        }
        .container {
            width: 100%;
            max-width: 800px;
            background-color: #fff;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
        }
        .product-list {
            margin-top: 20px;
        }
        .product-item {
            display: flex;
            align-items: center;
            cursor: pointer;
            margin-bottom: 10px;
        }
        .product-item img {
            width: 50px;
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>FakeStore Product Catalog</h1>
        <select id="categorySelect">
            <option value="">Select a category</option>
        </select>
        <input type="text" id="filterInput" placeholder="Filter products by name">
        <div id="productList" class="product-list"></div>
    </div>
    <script src="./assets/js/app.js"></script>
</body>
</html>
```

#### app.js faylı aşağıdakı kimi olacaq :
```js
const productList = document.getElementById("productList");
const categorySelect = document.getElementById("categorySelect");
const filterInput = document.getElementById("filterInput");

//Əvvəlcə bütün kateqoriyaları götürmək üçün fetch istifadə edirik. 
//Sonra hər bir kateqoriyanı option elementi ilə categorySelect elementinə əlavə edirik. 
//Əlavə olunan hər bir option elementinin value atributuna həmin kateqoriyanın adını veririk.
//Value atributuna verdiyimiz adı həmin kateqoriyanın məlumatlarını götürmək üçün istifadə edirik.

fetch("https://fakestoreapi.com/products/categories")
  .then((res) => res.json())
  .then((datas) => {
    datas.forEach((element) => {
      categorySelect.innerHTML += `
                <option value="${element}">${element}</option>
                `;
    });
  });

//Aşağıda categorySelect elementinə change eventi əlavə edirik.
//Bu event işə düşdükdə həmin kateqoriyanın məlumatlarını götürmək üçün fetch istifadə edirik.
//Burada this.value ilə select elementində seçilmiş kateqoriyanın adını götürürük.
//Geriyə qayıdan məlumatları json formatına çeviririk və həmin məlumatları productList elementinə əlavə edirik.
//Daha sonra filterInput elementinə input eventi əlavə edirik. Bu event işə düşdükdə filterInput elementində daxil edilən məlumatı götürürük.
//İlk öncə filterInput elementində daxil edilən məlumatın boş olub olmadığını yoxlayırıq.
//Boş deyilsə productList elementini sıfırlayırıq və json məlumatları filter edirik.
//Filter metodu bir predicate funksiyası qəbul edir və hər bir elementi bu funksiya ilə yoxlayır. Əgər funksiya true qaytardısa həmin elemntlrdən ibarət yeni bir array qaytarır.
//Sonra arraydəki hər bir element üçün productList elementinə yeni bir div elementi əlavə edirik.
//Filter metodu əvəzinə fetch sorğusundan geri qayıdan json-u if şərti ilə yoxlayaraq da həmin işi edə bilərik. 

categorySelect.addEventListener("input", function () {
  fetch(`https://fakestoreapi.com/products/category/${this.value}`)
    .then((res) => res.json())
    .then((json) => {
      productList.innerHTML = "";
      json.forEach((product) => {
        productList.innerHTML += `
                    <div class="product-item">
                        <img src="${product.image}" alt="Product image">
                        <p>${product.title} - $${product.price}</p>
                    </div>
                `;
      });
      filterInput.addEventListener("input", function () {
        if(filterInput.value!=''){
            productList.innerHTML='';
            let products = json.filter((prd) => prd.title.toLowerCase().includes(filterInput.value.trim().toLowerCase()));
            products.forEach(element => {
                productList.innerHTML += `
                <div class="product-item" data-id="${element.id}">
                    <img src="${element.image}" alt="Product image">
                    <p>${element.title} - $${element.price}</p>
                </div>
            `;
            });
            }
        });
    });
});
```