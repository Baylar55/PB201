# SaveChangesAsync() metodu 


#### Entity-lərin nə vaxt yarandığını, nə vaxt update olunduğunu bilmək üçün içərisində CreatedDate, UpdatedDate kimi property-lər saxlayırıq. Bu property-ləri entity-lərin CRUD-nı yazarkən hər dəfə manual olaraq yazmaq bizim üçün əlavə iş yüküdür. Bunun daha rahat bir üsulu ChangeTracker-dən istifadə etməkdir.

 

#### ChangeTracker entity-lər üzərində hər hansı bir dəyişiklik edəndə bu dəyişiklikləri izləyir. Bundan istifadə edib yuxarıda sadaladığım property-lərin tarixini manual yox avtomatik şəkildə vermək mümkündür.

##

#### Context class-ı içərisində SaveChangesAsync() metodunu override edək. Və içərisində ChangeTrackeri istifadə edək.
```cs
public class AppDbContext : IdentityDbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

    public DbSet<Movie> Movies { get; set; }
    public DbSet<Genre> Genres { get; set; }
    public DbSet<AppUser> Users { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfigurationsFromAssembly(typeof(MovieConfiguration).Assembly);
        base.OnModelCreating(modelBuilder);
    }

    public override async Task<int> SaveChangesAsync(CancellationToken cancellationToken = default) 
    {
        var datas = ChangeTracker.Entries<BaseEntity>();   // Burada biz BaseEntity-dən miras alan siniflərin dəyişikliklərini əldə edirik.

        foreach (var data in datas) 
        {
            switch (data.State)
            {
                case EntityState.Added:      // Əgər data əlavə olunubsa CreatedDate-i yeniləyirik.
                    data.Entity.CreatedDate = DateTime.UtcNow;
                    break;
                case EntityState.Modified:   // Əgər data dəyişdirilibsə UpdatedDate-i yeniləyirik.
                    data.Entity.UpdatedDate = DateTime.UtcNow;
                    break;
                default:                     // Əgər başqa bir dəyişiklik olubsa heç bir şey etmirik.
                    break;
            }
        }

        return await base.SaveChangesAsync(cancellationToken); //Daha sonra base-dəki SaveChangesAsync metodunu işə salırıq və dəyişiklikləri save edirik.
    }
}
```