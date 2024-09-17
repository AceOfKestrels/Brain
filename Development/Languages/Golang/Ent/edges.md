# Edges
Edges in Ent represent relations between schemas. In the database itself they are generally a separate table, but in our Go code they can be access similarly to Fields of the schema itself.

## Defining an Edge
Defining edges works similarly to fields of a schema. Use the `edge.To` to define an edge with a name and another schema:
```go
func (Foo) Edges() []ent.Edge {
    return []ent.Edge{
        edge.To("bars", Bar.Type),
    }
}
```

### Reverse Edges
By default, edges can only be queried from the schema they are defined on. To allow queries in the other direction, define a reverse edge using the `edge.From` and `Ref` methods:
```go
func (Bar) Edges() []ent.Edge {
    return []ent.Edge{
        edge.From("foos", Foo.Type).Ref("bars"),
    }
}
```
The name given in the `Ref` must match the name of the `To` edge.

## Cardinalities
By default, edges are M2M (many to many; n to m). This can be changed with the `Unique` modifier method:
```go
func (Foo) Edges() []ent.Edge {
    return []ent.Edge{
        edge.To("bars", Bar.Type).Unique(),
    }
}
```

Refer to the [official documentation](https://entgo.io/docs/schema-edges/) for now.