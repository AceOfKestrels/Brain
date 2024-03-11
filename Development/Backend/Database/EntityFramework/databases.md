# Entity Framework Core
> Entity Framework (EF) Core is a lightweight, extensible, open source and cross-platform version of the popular Entity Framework data access technology.
> 
> EF Core can serve as an object-relational mapper (O/RM), which:
> - Enables .NET developers to work with a database using .NET objects.
> - Eliminates the need for most of the data-access code that typically needs to be written.
> 
> *([Microsoft Learn](https://learn.microsoft.com/en-us/ef/core/))*

## Setup
Run `dotnet tool install --global dotnet-ef` to install Entity Framework.

To use EF Core, the `Microsoft.EntityFrameworkCore` package must be added to the project, as well as another package depending on the database provider.

In addition, `Microsoft.EntityFrameworkCore.Tools` and `Microsoft.EntityFrameworkCore.Design` should be added for managing the database.

The following examples will use [SQLExpress](https://www.microsoft.com/de-de/sql-server/sql-server-downloads), which needs to be installed separately. 
The NuGet package we need for this provider is `Microsoft.EntityFrameworkCore.SqlServer`. This needs to be added to the project.

### DbContext
Now, the `DbContext` class can be inherited to create an instance representing a session with a database:
```cs
public class UserContext: DbContext
{
    public UserContext(DbContextOptions<UserContext> options): base(options) { }

    public DbSet<User> Users { get; set; }
}
```
The `DbSet` represents the databse table.

### Connection String
In the `appsettings.json` set up the connection string for the database. This will depend on the databse provider used.
```json
{
    "ConnectionStrings": {
        "Default": "server=localhost\\sqlexpress;database=usersdb;trusted_connection=true;TrustServerCertificate=true"
    },
    // ...
}
```

### Add the Service
To inject the `DbContext` into other classes it has to be added as a service in the `Program.cs`. Again, the method to call will depend on the database provider.
```cs
builder.Services.AddDbContext<UserContext>( options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("Default"));
});
```

### Code-First-Migration
To create the database and add representations of our models, a new migration must first be created. To do this run `dotnet ef migrations add` in the project directory. The name of the migration should be unique:
```
dotnet ef migrations add CreateInitial
```

<br>

Then run `dotnet ef database update` to create the database:
```
dotnet ef database update
```

## Accessing the Database
Now the `DbContext` can be injected into any class to access the database:
```cs
private UserContext _dbContext;

public UserService(UserContext dbContext)
{
    _dbContext = dbContext;
}
```

<br>

Using that the database table can be accessed like any list. Make sure to use the async versions of the respective methods:
```cs
public async Task<IEnumerable<User>> GetUsers()
{
    return await _dbContext.Users.ToListAsync();
}
```