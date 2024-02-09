# Server Side Rendering

## Why use server side rendering?
Angular applications only being rendered on the client has some disadvantages.
For one, web crawlers cannot access the content of your page, which hinders search engine optimization.
Regular Angular apps will also take longer to load than other pages, since a lot more content needs to be downloaded first (even when using [lazy loading](./modules.md#lazy-loading)), and scripts need to be executed first for content to be visible.

Angular SSR (formerly Angular Universal) attempts to fix these issues by rendering the first view of the application server side.

## How to enable server side rendering?
When creating a new Anuglar project, simply add the `--ssr` flag to the `ng new` command:
```
ng new [project name] --ssr
```

&nbsp;
If you have already created a project without SSR, you can add it via the `ng add` command:
```
ng add @angular/ssr
```
