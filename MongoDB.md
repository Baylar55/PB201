# Asp.NET + Mongo DB

#### İlk öncə appsettings.json-a connection və databaza adını əlavə edirik.
```json
"MongoDbSettings": {
  "DatabaseName": "PB201",
  "MongoDb": "mongodb://localhost:27017"
}
```

#### Category adında entity yaradaq :
```cs
 public class Category
 {
     [BsonId]
     [BsonRepresentation(BsonType.ObjectId)]
     public string Id { get; set; }
     public string Name { get; set; }
 }
```

#### Hər dəfə kod təkrarı olmasın, collectionları bir yerdən götürək deyə MongoDbContext class-ı yaradaq.

```cs
public class MongoDbContext 
{
    private readonly IMongoDatabase database;
    public MongoDbContext()
    {
        var client = new MongoClient("mongodb://localhost:27017");
        database = client.GetDatabase("PB201");
    }

    public IMongoCollection<Category> Categories => database.GetCollection<Category>("Categories");  
}
```

#### CRUD ücün DTO-ları yaradaq.
```cs
public record CreateCategoryDTO(string Name);
public record UpdateCategoryDTO(string Name);
public record GetCategoryDTO(string Id, string Name);
```

#### AutoMapper-i əlavə edək.
```cs
public class MapProfile : Profile
{
    public MapProfile()
    {
        CreateMap<Category, CreateCategoryDTO>().ReverseMap();
        CreateMap<Category, UpdateCategoryDTO>().ReverseMap();
        CreateMap<Category, GetCategoryDTO>().ReverseMap();
    }
}
```

#### Service üçün interface və class yaradaq.

```cs
public interface ICategoryService
{
    Task<GetCategoryDTO> GetByIdAsync(string id);
    Task<ICollection<GetCategoryDTO>> GetAllAsync();
    Task CreateAsync(CreateCategoryDTO dto);
    Task UpdateAsync(string id, UpdateCategoryDTO dto);
    Task<GetCategoryDTO> DeleteAsync(string id); // Geriyə silinən datanı qaytarmaq üçün GetCategoryDTO qaytarırıq.
}
```

```cs
public class CategoryService(IMapper _mapper, MongoDbContext _context) : ICategoryService   // Primary constructor injection. Burada artıq readonly field-lərə ehtiyac yoxdur.
{
    public async Task CreateAsync(CreateCategoryDTO dto)
    {
        var mappedData = _mapper.Map<Category>(dto);
        await _context.Categories.InsertOneAsync(mappedData);
    }

    public async Task<GetCategoryDTO> DeleteAsync(string id)
    {
        var data = await _context.Categories.FindOneAndDeleteAsync(x => x.Id == id);    // Bu metod geriyə sildiyi datanı qaytarır
        return _mapper.Map<GetCategoryDTO>(data);
    }

    public async Task<ICollection<GetCategoryDTO>> GetAllAsync()
    {
        var data = await _context.Categories.Find(x => true).ToListAsync(); // Find metodunun 1-ci parametri filterdir. Biz burada bütün dataları çəkmək istədiyimiz üçün true yazırıq.
        return _mapper.Map<ICollection<GetCategoryDTO>>(data);
    }

    public async Task<GetCategoryDTO> GetByIdAsync(string id)
    {
        var data = await _context.Categories.Find(x => x.Id == id).FirstOrDefaultAsync();
        return _mapper.Map<GetCategoryDTO>(data);
    }

    public async Task UpdateAsync(string id, UpdateCategoryDTO dto)
    {
        var data = _mapper.Map<Category>(dto);
        data.Id = id;               // Id-ni burada set etmək lazımdır.
                                    // Burda id-ni set etməsək, id null olacaq və
                                    // FindOneAndReplaceAsync metodu id-si null olan datanı set etməyə çalışacaq və error verəcək.

                                    // Əgər id-ni kənardan almasaq, UpdateCategoryDTO içərisində id fieldi yazıb, onu istifadə etsək data içərisində id fieldi avtomatik olaraq olduğu üçün id-ni set etməyə ehtiyac yoxdur.


        await _context.Categories.FindOneAndReplaceAsync(x => x.Id == id, data); // FindOneAndReplaceAsync metodu geriyə əvəz etdiyi datanı qaytarır. Ilk parametr filterdir, ikinci parametr yeni data.

        // Digər bir variant:
        //await _context.Categories.ReplaceOneAsync(x => x.Id == id, data);  // ReplaceOneAsync metodu geriyə əvəz etdiyi datanı qaytarmır.

        // Digər bir variant:
        //await _context.Categories.UpdateOneAsync(x => x.Id == id, Builders<Category>.Update.Set(x => x.Name, dto.Name)); // UpdateOneAsync metodu geriyə əvəz etdiyi datanı qaytarmır. Bu daha çox field-ləri dəyişdirmək üçündür.    
    }
}
```

#### Controlleri yaradaq.
```cs
[Route("api/[controller]")]
[ApiController]
public class CategoriesController(ICategoryService _categoryService) : ControllerBase
{
    [HttpGet]
    public async Task<IActionResult> GetAll()
    {
        return Ok(await _categoryService.GetAllAsync());
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetById(string id)
    {
        return Ok(await _categoryService.GetByIdAsync(id));
    }

    [HttpPost]
    public async Task<IActionResult> Create(CreateCategoryDTO categoryDTO)
    {
        await _categoryService.CreateAsync(categoryDTO);
        return Ok();
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(string id, [FromBody] UpdateCategoryDTO categoryDTO)
    {
        await _categoryService.UpdateAsync(id, categoryDTO);
        return Ok();
    }

    [HttpDelete]
    public async Task<IActionResult> Delete(string id)
    {
        return Ok(await _categoryService.DeleteAsync(id));
    }
}
```

#### Son olaraq servisləri inject edək.
```cs
builder.Services.Configure<MongoDbContext>(builder.Configuration.GetSection("MongoDbSettings"));
builder.Services.AddScoped<MongoDbContext>();
builder.Services.AddScoped<ICategoryService, CategoryService>();
builder.Services.AddAutoMapper(typeof(MapProfile));
```
