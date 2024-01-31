# Directives
Directives are instructions in our DOM. They are similar to components (and components *are* directives), but don't have a template attached to them. Because of this the process of [making a custom directive](#custom-directives) is similar to that of components.

Directives generally have an "attribute-style" selector, attaching them to an element. However some might have an element selector, as we can see with [`ng-content`](./databinding.md#content-projection).

We generally differentiate between *[structural](./structural-directives.md)* and *[attribute](./attribute-directives.md)* directives.

## Read on
- [Attribute Directives](./attribute-directives.md)
- [Structural Directives](./structural-directives.md)
- [Creating Custom Directives](./custom-directives.md)