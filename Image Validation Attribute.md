# Asp.NET MVC

## Custom Validation Attribute 

#### Business layer-də Attributes adlı folder yaradın. Həmin folder içərisinə aşağıdakı classı əlavə edin. 
#### Hətta konstruktorda string array əvəzinə FileExtension adında bir enum qəbul edib array kimi ötürsəz və onun üzərindən işləsəz daha yaxşı olar. 

```cs
public class AllowedExtensionsAttribute : ValidationAttribute
{
    private readonly string[] _extensions;

    public AllowedExtensionsAttribute(params string[] extensions)
    {
        _extensions = extensions;
    }

    protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)
    {
        if (value != null)
        {
            if (value is IFormFile file)
            {
                if (!_extensions.Contains(file.ContentType))
                {
                    return new ValidationResult($"Content type is not valid. Only {string.Join(',',_extensions)} is allowed");
                }
            }
            else if (value is List<IFormFile> files)
            {
                if (files.Any(file => !_extensions.Contains(file.ContentType)))
                {
                    return new ValidationResult($"Content type is not valid. Only {string.Join(',',_extensions)} are allowed");
                }
            }
        }
        return ValidationResult.Success;
    }
}
```

```cs
public class BookCreateVM
{
    [AllowedExtensions("image/png","image/jpeg")]
    public IFormFile PosterImage { get; set; }
    
    [AllowedExtensions("image/png", "image/jpeg")]
    public IFormFile HoverImage { get; set; }
    
    [AllowedExtensions("image/png", "image/jpeg")]
    public List<IFormFile>? Images { get; set; }
}
```


## Registering DI Using Scrutor

#### Assembly yerinə uyğun olanları dəyişdirib istifadə edərsiniz.
```cs
builder.Services.Scan(selector => selector
    .FromAssemblies(
        typeof(PersistenceAssembly).Assembly,
        typeof(InfrastructureAssembly).Assembly)
    .AddClasses(publicOnly: false)
    .UsingRegistrationStrategy(RegistrationStrategy.Skip)
    .AsMatchingInterface()
    .WithScopedLifetime());
```

## Unique Property Value Vallidation Attribute


#### Normalda entity-lərin CRUD-nı yazarkən əsas diqqət eləməli olduğumuz hissələr entity-lərdə Title, Name və s. kimi property-lərin unique olmağıdır. Bunu service içində entity-ni yaradarkən və ya update edərkən yoxlamaq olar. Ancaq hər dəfə bunu yazıb yoxlamaq yorucudur. Bunun üçün custom attribute yazmaq olar. Ama elə bir attribut yazmaq lazımdır ki entity və property adlarını dinamik şəkildə ala bilək. Çünki fərqli entity-lərdə fərqli property-lər unique ola bilər. Bunun üçün reflection və expression tree istifadə etmək olar.   
```cs
 public class UniqueAcrossEntitiesAttribute : ValidationAttribute
 {
     private readonly string _entityTypeName;
     private readonly string _propertyName;

     public UniqueAcrossEntitiesAttribute(string entityTypeName, string propertyName)
     {
         _entityTypeName = entityTypeName;
         _propertyName = propertyName;
     }

     protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)
     {
         AppDbContext? dbContext = validationContext.GetService(typeof(AppDbContext)) as AppDbContext;                                       //ValidationContextdən istifadə edərək AppDbContext-i alırıq. 
         
         if (dbContext == null) throw new InvalidOperationException("AppDbContext not found in validation context.");                        //DbContext null olarsa exception atırıq.

         var entityType = dbContext.Model.GetEntityTypes().FirstOrDefault(t => t.ClrType.Name == _entityTypeName)?.ClrType;                  //DbContextdəki bütün entityləri alırıq və bizim təyin etdiyimiz entity adına uyğun olanı seçirik.

         if (entityType == null) throw new ArgumentException($" '{_entityTypeName}' entity not found in AppDbContext.");                     //Entity tapılmazsa exception atırıq.

         var property = entityType.GetProperty(_propertyName, BindingFlags.IgnoreCase | BindingFlags.Public | BindingFlags.Instance);        //Entitydə bizim təyin etdiyimiz property adına uyğun olan propertyni seçirik.

         if (property == null) throw new ArgumentException($" '{_propertyName}' property not found in '{_entityTypeName}'.");                //Property tapılmazsa exception atırıq.

                                                                                                                                             //Aşağıdakı hissədə expression tree ilə query yaradırıq.

         var parameter = Expression.Parameter(entityType, "e");                                                                              //Bir expression yaradırıq. Bu expressionun tipi entityType olacaq və adı "e" olacaq.
         var propertyExpression = Expression.Property(parameter, property);                                                                  //Yuxarıda yaradılan expressiondan istifadə edərək yeni bir expression yaradırıq. Bu expression entityType-in property-si olacaq. Görünüşü belə olacaq: e => e.Property
         var valueExpression = Expression.Constant(value);                                                                                   //Value-nu expressiona çeviririk. Görünüşü belə olacaq: "value"
         var equalityExpression = Expression.Equal(propertyExpression, valueExpression);                                                     //Yuxarıda yaradılan iki expressionu bir-birinə bərabər edirik. Görünüşü belə olacaq: e.Property == "value"

         var lambda = Expression.Lambda(equalityExpression, parameter);                                                                      //Yuxarıda yaradılan expressionları bir lambda expressiona çeviririk. Görünüşü belə olacaq: e => e.Property == "value"

         var dbSet = dbContext.GetType()?
                              .GetMethod("Set", Type.EmptyTypes)?
                              .MakeGenericMethod(entityType)
                              .Invoke(dbContext, null);                                                                                      //DbContextdəki Set methodunu çağırırıq və entityType-in tipində olan DbSet-i alırıq.

         IQueryable? queryable = dbSet as IQueryable;                                                                                        //DbSet-i IQueryable tipinə çeviririk.

         if (queryable == null) throw new InvalidOperationException("Can't convert DbSet to IQueryable.");                                   //DbSet IQueryable tipinə çevrilə bilməzsə exception atırıq.

         MethodInfo? anyMethod = typeof(Queryable).GetMethods()
                                                  .First(m => m.Name == "Any" && m.GetParameters().Length == 2)
                                                  .MakeGenericMethod(entityType);                                                            //Queryable-in Any methodunu çağırırıq və entityType-in tipində olan Any methodunu alırıq.

         bool exists = Convert.ToBoolean(anyMethod.Invoke(null, [queryable, lambda]));                                                       //Any methodunu çağırırıq və queryable və lambda parametrlərini veririk. Görünüşü belə olacaq: queryable.Any(e => e.Property == "value")

         if (exists) return new ValidationResult($"The '{value}' value is already in use in {_entityTypeName}.");                            //Əgər value mövcuddursa error qaytarırıq.

         return ValidationResult.Success;                                                                                                    //Əgər value mövcud deyilsə ValidationResult.Success qaytarırıq.
     }
 }
```



```cs
 public class GenreCreateViewModel
 {
     [UniqueAcrossEntities("Genre", nameof(Name))]
     public string Name { get; set; }
 }
```
