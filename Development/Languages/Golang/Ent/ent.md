# Ent
Ent is a simple Entity Framwork implementation for Go.

## Creating a Schema
Use the `go run` command to create a new database schema. This first time this is used, Ent will automatically be added to the module:
```
go run -mod=mod entgo.io/ent/cmd/ent new <SchemaName>
```

This will create a new file in the `/ent/schema` directory:
```go
package schema

import "entgo.io/ent"

type Foo struct {
    ent.Schema
}

func (Foo) Fields() []ent.Field {
    return nil
}

func (Foo) Edges() []ent.Edge {
    return nil
}
```

Then you can add the fields to the schema:
```go
func (Foo) Fields() []ent.Field {
    return []ent.Field{
        field.Int("value"),
        field.String("description").Default("unknown"),
    }
}
```

Then run the following command from the root directory to generate the rest of the files according to your schemas:
```
go run -mod=mod entgo.io/ent/cmd/ent generate ./ent/schema/
```
Ent will also create a `generate.go` file with that command.

The generator needs to be run any time a schema is updated.

### Changing the Ent directory
You can use the `--target` parameter to specify a different path when generating your schemas:
```
go run -mod=mod entgo.io/ent/cmd/ent new --target internal/database/ent/schema <SchemaName>
```
The `generate.go` file will not be created when using the `--target` parameter. You must create it manually or run the command yourself. 


## Connecting to the Database
We use a client to connect to the database. How this works exactly will depend on the database.

For SQLite:
```go
import _ "github.com/ncruces/go-sqlite3/driver"
import _ "github.com/ncruces/go-sqlite3/embed"

func main() {
    client, err := ent.Open("sqlite3", "file:./data.db")
    if err != nil {
        // handle error
    }
    defer client.Close()
}
```
The sqlite driver recommended by the Ent developers requires CGo to be enabled, hurting compilation performance and cross-plattform compatibility. The one used in the above example is a CGo-free implementation.

Regardless of database, you can run the auto migration tool to create the database from your schemas:
```go
if err := client.Schema.Create(context.Background()); err != nil {
    log.Fatalf("failed creating schema resources: %v", err)
}
```

## Interacting with the Database
Using the client it is possible to interact with the database. You could inject it into services to allow them to use the database.

In addition to the client, database connections also require a context (`context.Context`), which allows for cancellation of tasks. 

```go
type FooService struct { DBClient *ent.Client }

// Query the database for a Foo by id
func (f *FooService) Query(ctx context.Context, id int) (*ent.Foo, error) {
    foo, error = f.DBClient.Foo
        .Query()
        .Where(foo.Id(id))
        .Only(ctx)

    if error != nil {
        return nil, error
    }

    return foo, nil
}

// Create a new Foo from a DTO and add it to the DB
func (f *FooService) Create(ctx context.Context, dto FooDTO) (*ent.Foo, error) {
    foo, error = f.DBClient.Foo
        .Create()
        .SetValue(dto.Value)
        .SetDescription(dto.Description)
        .Save(ctx)

    if error != nil {
        return nil, error
    }

    return foo, nil
}
```

A new context can be created using the `context` package. It often makes sense to create a context with a timeout to make sure it is cancelled if the operation takes too long:
```go
func CreateFoo(dto FooDTO) {
    background := context.Background()
	ctx, cancel := context.WithTimeout(background, 5 * time.Second)
	defer cancel()

    FooService.Create(ctx, dto)
}
```

### API Request Context
When interacting with the database in response to API calls, you can use the request's context directly for the database calls:
```go
func (ctrl *fooController) GetFoos(c *gin.Context) {
	foos, error := ctrl.FooService.GetAll(c.Request.Context())

	if error != nil {
		c.AbortWithStatus(http.StatusNotFound)
		return
	}
	c.IndentedJSON(http.StatusOK, foos)
}
```

## Read on
- [Ent Documentation](https://entgo.io/docs/getting-started)
- [Edges](./edges.md)