# Redis


#### Aşağıdakı command-dan istifadə edərək redis instance-nı başladırsınız.Commandı həm cmd-də həm də powershelldə istifadə edə bilərsiniz. Commandı run edəndə docker açıq olmalıdı yoxsa terminalda errorla qarşılaşacaqsınız.
```cs
docker run --name some-redis -d redis
```

#### Default port 6379 olduğu üçün istəsəniz aşağıdakı command-dan istifadə edərək fərqli port üzərindən də connection qura bilərsiniz. 2222 əvəzinə istədiyiniz portu yaza bilərsiniz.
```cs
docker run --name some-redis -p 2222:6379 -d redis
``` 


# Redisin .Net-də konfiqurasiyası

1. İlk öncə aşağıdakı command-ı Package Manager Console-da yazaraq redis üçün package yükləyirik. Package adını nuget üzərindəndə axtarışa verib yükləyə bilərsiniz.

    ```cs
    Install-Package StackExchange.Redis
    ```
2. appsettings.json faylına Redis connection stringini əlavə edirsiz. 6379 əvəzinə hansı port üzərindən qoşulmusunuzsa onun portunu qeyd edirsiz. Əgər docker üzərindən run edəndə port qeyd eləməmisizsə 6379 olaraq qalsın port.
    ```json
    {
        "ConnectionStrings": {
        "Redis": "localhost:6379" 
        }
    }
    ```
3. Program.cs -də aşağıdakı kimi redisi register etməlisiniz.
    ```cs
    builder.Services.AddSingleton<IConnectionMultiplexer>(sp =>
    {
        var configuration = ConfigurationOptions.Parse(builder.Configuration.GetConnectionString("Redis"), true);            
        return ConnectionMultiplexer.Connect(configuration);
    });
    ```
4. Redisin istifadəsi. Tutaq ki aşağıdakı kimi bir Product class var. Bu obyekti keşləmək üçün serialize etməliyik. 

    ```cs
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
    ```
    ```cs
    [Route("api/[controller]")]
    [ApiController]
    public class ProductController : ControllerBase
    {
        private readonly IDatabase database;
        public ProductController(IConnectionMultiplexer connectionMultiplexer)
        {
            database = connectionMultiplexer.GetDatabase(); // Burada Redis database-ə bağlanırıq
        }

        [HttpGet("{id}")]
        public async Task<IActionResult> Get(int id)
        {
            var cacheKey = $"product:{id}";                             // Burada cache key yaradırıq ki, Redis-də bu key ilə məlumatları saxlayaq. Key belə görünəcək => product:1
            var product = await database.StringGetAsync(cacheKey);      // Burada Redis-dən məlumatı almaq üçün StringGetAsync() metodundan istifadə edirik

            if (product.IsNullOrEmpty) return NotFound();
            var data = JsonSerializer.Deserialize<Product>(product);    // Burada database-dən gələn məlumatı Product obyektinə çeviririk
            return Ok(data);                                            // Burada Product obyektini geri qaytarırıq
        }

        [HttpPost]
        public async Task<IActionResult> Post(Product product)
        {
            var cacheKey = $"product:{product.Id}";
            await database.StringSetAsync(cacheKey, JsonSerializer.Serialize(product)); // Burada Redis-də məlumatı saxlamaq üçün StringSetAsync() metodundan istifadə edirik və məlumatı JSON formatına çevirib göndəririk
            return Ok();
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> Delete(int id)
        {
            var cacheKey = $"product:{id}";
            await database.KeyDeleteAsync(cacheKey); // Burada Redis-dən məlumatı silmək üçün KeyDeleteAsync() metodundan istifadə edirik və cache key göndəririk
            return Ok();
        }
    }
    ```

## Əlavə mənbələr:
[`.NET and Redis`](https://redis.io/learn/develop/dotnet)

[`Redis Data Structures`](https://redis.io/redis-enterprise/data-structures/)

[`C#/.NET Guide`](https://redis.io/docs/latest/develop/connect/clients/dotnet/)
