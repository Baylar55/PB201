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