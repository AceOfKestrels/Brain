# Getter and Setter Methods
Getter and Setter Methods (also called Accessors and Mutators) are special methods used to access properties of an object while giving more control about the values being passed. It also allows for the creation of "fake" properties that simply calculate values when accessed.

Many programming languages provide ways to automatically generate getters and setters and make using them seamless by using the same syntax as accessing a property diretcly.

In most languages it is possible to declare getters and setters separately, only allowing one or the other. This is often used to create properties that are read-only to the outside.

## Getters
Getters are used to retrieve the value of a property. 

If the property is a [mutable object](./mutability.md) its values may be changed after retrieving it. To prevent this, Getters often simply return a copy of the actual property value.

## Setters
Setters are used to change the value of a property.

## Examples
C#
```cs
public DateTime BirthDate { get; private set; }

public TimeSpan Age { get { return DateTime.Now - BirthDate; } }
```

## Read on
- [Mutator method (Wikipedia)](https://en.wikipedia.org/wiki/Mutator_method)