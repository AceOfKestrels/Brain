# Pipes

Pipes are a feature of Angular that allows you to transform output in your template without changing the original value.

## Usage
Pipes are used with the `|` symbol:
```html
<h1>HELLO {{ username | uppercase }}!!</h1>
<p>Hello {{ username }}.</p>
```
The `uppercase` pipe transforms a string to all uppercase.

Multiple pipes can be chained by using another `|`:
```html
<h1>HELLO {{ username | uppercase }}!!</h1>
<p>Hello {{ username }}.</p>
```

## Pipe Parameters
If a pipe accepts parameters, they can be added after it using a `:`:
```html
<p>Your birthdate is {{ birthDate | date:"fullDate" }}.</p>
```
Additional parameters can be added separated by `:`

## Custom Pipes
To make custom pipes, create a new class (file naming convention is `[name].pipe.ts`) with the  that implements `PipeTransform`. Implement the `transform` method, which takes the value to transform and any arguments as parameters:
```js
export class ShortenPipe {
    transform(value: any, length: number = 10) {
        return value.substr(0, length);
    }
}
```
Add the `Pipe` decorator to the class and define the pipe's name:
```js
@Pipe({
    name: "shorten"
})
export class ShortenPipe {
    transform(value: any, length: number = 10) {
        return value.substr(0, length);
    }
}
```
Then you can use the pipe as normal:
```html
<h1>HELLO {{ username | uppercase | shorten:5 }}!!</h1>
```

## Pure vs Impure Pipes
By default, pipes only update when their input changes. This behaviro is called "pure". We can set the `pure` property to false on our pipe to make it update whenever anything on the page changes:
```js
@Pipe({
    name: "shorten",
    pure: false
})
export class FilterPile {
    transform(value: any, filterStr: string, propName: string) {
        if(value.lenth === 0) return [];

        result = [];
        for(let item of value) {
            if(item[propName].startsWith(filterStr)) {
                result.push(item);
            }
        }
        return result;
    }
}

export class ShortenPipe {
    transform(value: any, length: number = 10) {
        return value.substr(0, length);
    }
}
```