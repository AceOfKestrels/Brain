# Service Workers
Service Workers can be used to add offline capabilities to an Angular application. They run on a separate thread that is decoupled from the rest of the page and listen to outgoing network requests to cache responses and return those cached resources again later.

## Using Service Workers
First we have to install the `@angular/pwa` package:
```
ng add @angular/pwa
```

&nbsp;
Service Workers are only enabled in production mode my default. Use `ng build --prod` to build the app. Caching for any hard coded content should work immediately.

Additional assets to cache can be defined in the `ngsw-config.json` file. They are listed in `assetGroups`, that control how the assets are loaded.

The `installMode` entry governs how assets are fetched. `prefetch` caches all assets even if they were not used yet, while `lazy` only caches assets that have already been used.

The `files` array contains a list of all files that should get cached.
Additionally, an `urls` array can be used to list all resources from other servers that should get cached.

### Caching Dynamic Content
Dynamic data to cache are defined in `dataGroups` instead, which follow a similar structure as `assetGroups`.