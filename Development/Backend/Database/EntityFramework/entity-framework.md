# Entity Framework Core
> Entity Framework (EF) Core is a lightweight, extensible, open source and cross-platform version of the popular Entity Framework data access technology.
> 
> EF Core can serve as an object-relational mapper (O/RM), which:
> - Enables .NET developers to work with a database using .NET objects.
> - Eliminates the need for most of the data-access code that typically needs to be written.
> 
> *([Microsoft Learn](https://learn.microsoft.com/en-us/ef/core/))*

Entity Framework supports a variety of database providers. It supplies different accessor methods similar to a list and automatically translates them to access the database it is connected to.

## Setup
Run `dotnet tool install --global dotnet-ef` to install Entity Framework.

To use EF Core, the `Microsoft.EntityFrameworkCore` package must be added to the project, as well as another package depending on the database provider.

In addition, `Microsoft.EntityFrameworkCore.Tools` and `Microsoft.EntityFrameworkCore.Design` should be added for managing the database.

The following examples will use [SQLExpress](https://www.microsoft.com/de-de/sql-server/sql-server-downloads), which needs to be installed separately. 
The NuGet package we need for this provider is `Microsoft.EntityFrameworkCore.SqlServer`. This needs to be added to the project.

### Database Context
The `DbContext` class represents a session with a database. To tell Entity Framework which table to access we can inherit the class.
```cs
public class UserContext: DbContext
{
    public UserContext(DbContextOptions<UserContext> options): base(options) { }

    public DbSet<User> Users { get; set; }
}
```
The `DbSet` represents the databse table and can later be used to access it.

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
To inject the `DbContext` into other classes it has to be added as a service in the `Program.cs`. Again, the method to call to set up the options will depend on the database provider.
```cs
builder.Services.AddDbContext<UserContext>( options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("Default"));
});
```

### Code-First-Migration
We have created the model for our database first in code (as opposed to creating the database first), so now we can use Entity Framework to automatically create the database for us.

To do this, a new migration must first be created. To do this run `dotnet ef migrations add` in the project directory. The name of the migration should be unique:
```
dotnet ef migrations add CreateInitial
```

<br>

Then run `dotnet ef database update` to create the database:
```
dotnet ef database update
```

<br>

These steps are the same in case we want to add another table or change an existing model

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

Using that the database table can be accessed similary to a list:
```cs
public User? GetUser(int id)
{
    return _dbContext.Users.Find(id);
}
```

<br>

When changing entries in the database (including adding, removing or editing entries), make sure to call `DbContext.SaveChanges` to actually save the changes to the database:
```cs
public bool RemoveUser(int id) 
{
    User user = _dbContext.Users.Find(userId);

    if(user is null) {
        return false;
    }

    _dbContext.Users.Remove(user);
    _dbContext.SaveChanges();
}
```

### Adding new entries
Entity Framework automatically generates values for ID properties.

Make sure to unset the ID of entries you add beforehand:
```cs
public void AddUser(User newUser) {
    newUser.Id = null;
    _dbContext.Users.Add(newUser);
    _dbContext.SaveChanges();
}
```

<br>

The original object will be updated with the new ID:
```cs
public User AddUser(User newUser) {
    newUser.Id = null;
    _dbContext.Users.Add(newUser);
    _dbContext.SaveChanges();

    return newUser;
}

public void TestAddUser() {
    User user = AddUser( new() {
        Name: "Kes",
        Email: "kes@test.com"
    });

    Console.WriteLine(user.Id is not null);
}
```

### Updating existing entries
Entity Framework tracks changes to properties of entries, so updating an entry can be as easy as changing its property values:
```cs
public void UpdateUser(User newUser) {
    User oldUser = _dbContext.Users.Find(newUser.Id);
    if(oldUser is null) {
        return;
    }

    oldUser.Name = newUser.Name;
    oldUser.Email = newUser.Email;
    _dbContext.SaveChanges();
}
```

<br>

Alternatively, all properties can be updated automatically, by retrieving a tracked entity:
```cs
public void UpdateUser(User newUser) {
    User oldUser = _dbContext.Users.Find(newUser.Id);
    if(oldUser is null) {
        return;
    }

    _dbContext.Users.Entry(oldUser).CurrentValue.SetValues(newUser);
    _dbContext.SaveChanges();
}
```
When doing this, make sure the ID of the original and modified entry are the same.

## Modifying the Database
When making changes to the model or adding new tables to the database in code, it is necessary to update the database accordingly again before it will work.

To do so, add a new migration and update the database, just like [creating it originally](#code-first-migration):
```
dotnet ef migrations add UpdateUserModel
dotnet ef database update
```