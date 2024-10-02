# Indexes
Indexes (sic.) in Ent are used to increase the speed of queries and can define uniqueness over multiple fields. Both of these options will be explored here.

## Defining Indexes
Similarly to fields and edges, indexes are defined unsing an `Indexes` method on the schema:
```go
func (Foo) Indexes() []ent.Index {
    return []ent.Index{
        index.Fields("baz"),
    }
}
```

### Indexing Edges
Edges can be indexed the same way using the `index.Edges` method:
```go
func (Foo) Indexes() []ent.Index {
    return []ent.Index{
        index.Edges("bars"),
    }
}
```

### Indexing multiple fields
A single index can be created over multiple fields or edges:
```go
func (Foo) Indexes() []ent.Index {
    return []ent.Index{
        index.Fields("baz", "qux"),
    }
}
```
This helps with performance when querying both fields as the *combinations* of values are indexed.

In the above example the combination of the `baz` and `qux` field is indexed, but query speed for either of them on their own is unaffected.

## Defining Uniqueness
Also like fields and edges, indexes can take a `Unique` modifier as well. When applying it to an index over a single field or edge it as the same effect as if it were applied to the field/edge itself. 

Using indexes to define uniqueness becomes useful when an index includes multiple fields or edges:
```go
func (Foo) Indexes() []ent.Index {
    return []ent.Index{
        index.Fields("baz", "qux").Unique(),
    }
}
```
That way either of them can take duplicate values, as long as the combination of them is unique.

As Ent does not support composite primary keys, using unique indexes is a useful way to emulate that feature.

## Querying Indexes
Indexes are not queried directly. Instead the index is used automatically if it applies to the query:
```go
// foo.go
func (Foo) Indexes() []ent.Index {
    return []ent.Index{
        index.Fields("baz", "qux"),
    }
}

// service.go
func (s *Service) GetFoo(baz string, qux string, ctx context.Context) (foo *ent.Foo, err error) {
    foo, err = s.DBClient.Foos.
        Query().
        Where(foo.
            And(foo.Baz(baz), foo.Qux(qux))).
        Only(ctx)

    return
}
```
Since an index is defined over `baz` and `qux`, the above example automatically uses the index when querying both of them in an `And` clause.

The following code would not utilize the index since only the `baz` field is queried:
```go
func (s *Service) GetFoo(baz string, ctx context.Context) (foo *ent.Foo, err error) {
    foo, err = s.DBClient.Foos.
        Query().
        Where(foo.Baz(baz)).
        Only(ctx)

    return
}
```