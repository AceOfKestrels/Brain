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
By default, edges are M2M (many to many; n to m; *Many* Foos to *many* Bars in the previous example). This can be changed with the `Unique` modifier method:
```go
func (Foo) Edges() []ent.Edge {
    return []ent.Edge{
        edge.To("bars", Bar.Type).Unique(),
    }
}
```
Making an edge `Unique` means that its *target* will have a cardinality of 1. In the above example, if the back edge is not `Unique` as well we have a M2O relationship (*Many* Foos to *one* Bar). 

### O2M and O2O
To define O2M and O2O relations we need to make the reverse edge unique.

This is a O2M relation (*One* Foo to *many* Bars)
```go
// foo.go
func (Foo) Edges() []ent.Edge {
    return []ent.Edge{
        edge.To("bars", Bar.Type),
    }
}

// bar.go
func (Bar) Edges() []ent.Edge {
    return []ent.Edge{
        edge.From("foos", Foo.Type).Ref("bars").Unique(),
    }
}
```
<br>

This is a O2O relation (*One* Foo to *one* Bar):
```go
// foo.go
func (Foo) Edges() []ent.Edge {
    return []ent.Edge{
        edge.To("bars", Bar.Type),
    }
}

// bar.go
func (Bar) Edges() []ent.Edge {
    return []ent.Edge{
        edge.From("foos", Foo.Type).Ref("bars").Unique(),
    }
}
```

## Read on
- [Edges (Ent Documentation)](https://entgo.io/docs/schema-edges/)