# Selectors
CSS Selectors control which elements a style in a stylesheet is applied to. What style takes precendence depends on the selector's [specificity](./specificity.md).

## Universal Selector
The most basic selecttor is the universal selector `*`, which matches any element. Styles defined here are applied to any element. This is useful to remove the default margin and padding many elements have for instance:
```css
* {
    margin: 0;
    padding: 0;
}
```

## Type Selectors
Any elements of a specific type can be selected using the element's name. This example will make any paragraph (`<p>`) element green:
```css
p {
    color: green;
}
```

## ID Selectors
Selection by ID works by suffixing it with a `#`. The following example will apply the style only to the element with the `main-heading` ID:
```css
#main-heading {
    font-size: 1.5em;
    font-family: Helvetica;
}
```
```html
<h1 id="main-heading">Kestrel's Wiki</h1>
```

## Class Selectors
CSS classes can be applied to elements using the `class` property, sepearated by spaces. Prefixed with a `.`, any elements with a certain class can be selected:
```html
<div class="content main-content">foo</div>
<div class="spacer"></div>
<div class="content">bar</div>
```
```css
.content {
    background-color: lightgray;
}

.spacer {
    padding: 2em;
}

.main-content {
    color: red;
}
```

## Pseudo Classes
Pseudo classes indicate the state of an element, such as whether or not the user is currently hovering over it. They can be selected by prefixing them with a colon `:
```css
button:hover {
    background-color: gray;
}
```
Pseudo class selectors are often applied more specifically, using [selector combinations](#combination-selectors).

## Pseudo Elements
Pseudo Elements are special elements not present in HTML that can be styled separately. See [pseudo elements](./pseudo-elements.md) for more.

## Combination Selectors
Selectors can be combined using operators to establish a relationship between selectors:

### Grouping
Using a comma `,` sign, this combination applies a style to elements that match either A or B:
```css
.A, .B {
    color: red;
}
```
```html
<div>
    <p class="A">Red text</p>
    <p class="B">Red text</p>
</div>
```

### Next-Sibling
Using a `+` sign, this combination applies if the elements selected by A and B have the same parent and B immediately follows A:
```css
.A + .B {
    color: red;
}
```
```html
<div>
    <p class="A">Default text</p>
    <p class="B">Red text</p>
</div>
```

### Subsequent Sibling
Using a `~` sign, this combination applies if the elements selected by A and B have the same parent and B comes after (but not necessarily immediately follows) A:
```css
.A ~ .B {
    color: red;
}
```
```html
<div>
    <p class="A">Default text</p>
    <p>Some text in between</p>
    <p class="B">Red text</p>
</div>
```

### Child
Using a `>` sign, this combination applies if B is a direct child of A:
```css
.A > .B {
    color: red;
}
```
```html
<div class="A">
    <p class="B">Red text</p>
</div>
```

### Descendent
Using no sign between the two, this combination applies if B is a descendent (but not necessarily a direct child) of A:
```css
.A .B {
    color: red;
}
```
```html
<div class="A">
    <div>
        <p class="B">Red text</p>
    </div>
</div>
```

## Inheritence
Some properties, such as `font` will be inherited by the children of whatever element they are applied to. In this example, any text inside a `div` with the `quote` class will be cursive:
```css
.quote {
    font-style: italic;
}
```
```html
<div class="quote">
    <p>A very important quote!</p>
</div>
```