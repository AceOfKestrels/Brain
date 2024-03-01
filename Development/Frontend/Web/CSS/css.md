# CSS - Cascading Style Sheets
From [mdn web docs: CSS](https://developer.mozilla.org/en-US/docs/Web/CSS):
CSS is a stylesheet language used to describe the presentation of documents written is HTML or XML. 

CSS is part of the core specifications of the open web and is standardized across web browsers.

## Styles
CSS styles can be applied as inline styles using the `style` property on any HTML element:
```html
<div style="width: 100px; height: 100px; background-color: red;"></div>
```
While inline styles have the highest [Specificity](./specificity.md) and thus always take precendence over other styles, using inline styles is generally not recommended to keep all styles separated from the content in a central place.

Instead it makes sense to use an external stylesheet. This is a separate file with the `.css` extension containing all the styles:
```css
div {
    width: 100px; 
    height: 100px; 
    background-color: red;
}
```
Which elements this style is then applied to depends on the [selector](./selectors.md) used to define them.

