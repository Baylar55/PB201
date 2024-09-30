# Serilog Configuration
#### İlk öncə package manager istifadə edərək nuget-dən uyğun paketləri yükləməliyik.
- ```NuGet\Install-Package Serilog.Sinks.MSSqlServer -Version 7.0.1 ```  

- ```NuGet\Install-Package Serilog.Sinks.MSSqlServer -Version 7.0.1```

#### Daha sonra Program.cs class-da konfiqurasiyları edirik : 

```cs
var logger = new LoggerConfiguration() 
    .WriteTo.File("logs/log.txt", rollingInterval: RollingInterval.Minute) // Serilog File sinki qururuq. Burada rollinginterval log faylının nə qədər intervalda yenidən yaradılacağını təyin edir.
    .WriteTo.Console() // Serilog Console sinki qururuq.
    .WriteTo.MSSqlServer(builder.Configuration.GetConnectionString("default"), // Serilog MSSqlServer sinki qururuq.
                         sinkOptions: new MSSqlServerSinkOptions // Log table-ı yaradırıq.
                         {
                             AutoCreateSqlTable = true,
                             TableName = "logs",
                         },
                         columnOptions: new ColumnOptions()  // Log table-ına yeni sütun əlavə edirik.
                         {
                             AdditionalColumns = new List<SqlColumn>
                             {
                                 new("UserName", System.Data.SqlDbType.VarChar)
                             }
                         })
    .Enrich.FromLogContext() //LogContextdən istifadə edərək log faylına istifadəçinin adını yazırıq.
    .MinimumLevel.Information() //Minimum log seviyyəsi təyin edilir.
    .CreateLogger(); // Serilog logger yaradılır.

builder.Host.UseSerilog(logger); // Serilog logger hosta əlavə edilir.
```

```cs
app.Use(async (context, next) => 
{
    var username = context.User?.Identity?.IsAuthenticated != null || true ? context.User.Identity.Name : null;
    LogContext.PushProperty("UserName", username);
    await next();
});//Bu middleware istifadəçinin adını alır və log faylına yazır və bunu bütün requestlər üçün edir.
```


## İstifadə edilməsi :

```cs
public class GenreService : IGenreService
{
    private readonly ILogger _logger;

    public GenreService(IGenreRepository genreRepository,IMapper mapper, ILogger<GenreService> logger)
    {
        _genreRepository = genreRepository;
        _mapper = mapper;
        _logger = logger;
    }
    public async Task<ICollection<GenreGetDto>> GetByExpression(bool asNoTracking = false, Expression<Func<Genre, bool>>? expression = null, params string[] includes)
    {
        var datas = await _genreRepository.GetByExpression(asNoTracking, expression, includes).ToListAsync();

        _logger.LogInformation($"All genres queried from database");
        return _mapper.Map<ICollection<GenreGetDto>>(datas);
    }
}
```


## Əlavə mənbələr:
['Serilog'](https://serilog.net)

['Serilog İle Veri Loglama'](https://veyselmutlu.medium.com/asp-net-core-serilog-i̇le-veri-loglama-4f908112e62a)

['Serilog In ASP.NET Core Web API'](https://www.c-sharpcorner.com/article/how-to-implement-serilog-in-asp-net-core-web-api/)