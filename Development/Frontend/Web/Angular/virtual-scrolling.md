# Virtual Scrolling

Sometimes many items need to be displayed in a list. To prevent having to load all of them at once we can use the Angular CDK library, which provides a Virtual Scrolling feature.

When using Virtual Scrolling, only the items that fit in the viewport are actively loaded. Scrolling further loads more items and displays them.

## Setup
To use Virutal Scrolling, first install Angular CDK:
```
ng add @angular/cdk
```

Then add the `ScrollingModule` to your application's module:
```js
imports: [
    // ...
    ScrollingModule
]
```

## Usage
After adding the package and importing the module, you can use the `cdk-virtual-scroll-viewport` component to specify the viewport, and the `cdkVirtualFor` directive to specify the items:
```html
<cdk-virtual-scroll-viewport itemSize="50" class="box">
  <div class="row" *cdkVirtualFor="let row of rows">
    {{ row }}
  </div>
</cdk-virtual-scroll-viewport>
```
As seen in the code, use `cdkVirtualFor` similarly to `ngFor` to specify the items that should be displayed.

### Context Variables
`cdkVirtualFor` makes a handful of context variables available:
```html
<cdk-virtual-scroll-viewport [itemSize]="18 * 7" class="example-viewport">
    <div *cdkVirtualFor="let item of items;
                        let index = index;
                        let count = count;
                        let first = first;
                        let last = last;
                        let even = even;
                        let odd = odd;" [class.example-alternate]="odd">
        <div class="example-item-detail">
            {{ item }}: {{ index+1 }}/{{ count }} {{ first ? "First" }} {{ last ? "Last" }} ({{ even ? "Even" : "Odd" }})
        </div>
    </div>
</cdk-virtual-scroll-viewport>

```

### Separate Viewport and Scrolling Element
Normally, the `cdk-virtual-scrolling-viewport` acts as the scrolling element (scrollbars will be displayed inside this). To change this behavior to use a different parent element, use the `cdkVirtualScrollingElement` directive:
```html
<div class="example-viewport" cdkVirtualScrollingElement>
    <div class="example-header">Content before</div>
    <cdk-virtual-scroll-viewport itemSize="50">
        <div *cdkVirtualFor="let item of items" class="example-item">{{item}}</div>
    </cdk-virtual-scroll-viewport>
    <div class="example-footer">Content after</div>
</div>
```

