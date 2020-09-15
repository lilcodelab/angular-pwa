# Angular Progressive Web Application (PWA)
DESCRIPTION

## Requirements
Angular requires a current, active LTS, or maintenance LTS version of Node.js.

## Setup Angular PWA application
### Install and setup Angular
1. Install Angular CLI globally ```npm install -g @angular/cli```
2. Generate new Angular app using Angular CLI, called AngularPWA ```ng new AngularPwa```

### Start Angular application
In period of development you can run application with command ```ng serve```.   
By default your application will be hosted on http://localhost:4200 .  

If you want to build your Angular application, run command ```ng build```. For production build add production parameter ```ng build --prod```. 
Inside your Angular application folder, ``dist`` folder will be created with compiled files.

___
## Make Angular application as Progressive web application
First you need to add Angular PWA package through Angular CLI ```ng add @angular/pwa```. This command made some changes inside a project: 

- files ```ngsw-config.json``` and ```src/manifest.webmanifest``` are created.
- also some files are updated, such as ```angular.json```, ```package.json```, ```src/app.module.ts```, ```src/index.html ```.

### Service worker
Service worker is a script that runs in background of your browser, separeted from web application. The advantage of service worker is ability to handle a request, kind of interceptor. Because it can handle every request of application, offten is used as cache manager. Further more, service worker implements ```onfetch``` and ```onmessage``` handlers, that means push notifications are suporrted (depends on browser support).

### Start service worker (NGSW)
If you start your Angular application using ```ng serve```, the service worker should not be started.
You can check that inside ***DevTools --> Applications --> Service Workers***.

First reason is because we are using development environment, inside ```src/app.module.ts``` there is rule ```ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production })```, which is false. During development we can use service worker on ```localhost```, but in the production site we will need HTTPS setup on hosting server.

**Requirements**
1. Your browser must support service workers, you can check it on this [link]('https://caniuse.com/serviceworkers').
2. In production environment use service workers with HTTPS protocol on your site.

For development purposes, you can run service worker on the ```localhost``` but in the production environment we want to use **HTTPS**. Through service worker you have a ability to filter responses and hijack connections, we want to avoid [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attack, using HTTPS protocol.

#### Configure service worker **
Inside file ```ngsw-config.json``` is configuration for service worker. Generated file includes configuration for statis files, such as fonts, images, script and style files. They are separated in two objects, depends on install or update mode:

***installMode***
The installMode determines how these resources are initially cached. The installMode can be either of two values:

- **prefetch** tells the Angular service worker to fetch every single listed resource while it's caching the current version of the app. This is bandwidth-intensive but ensures resources are available whenever they're requested, even if the browser is currently offline. 

- **lazy** does not cache any of the resources up front. Instead, the Angular service worker only caches resources for which it receives requests. This is an on-demand caching mode. Resources that are never requested will not be cached.

***updateMode***
For resources already in the cache, the updateMode determines the caching behavior when a new version of the app is discovered:

- **prefetch**- tells the service worker to download and cache the changed resources immediately.

- **lazy** tells the service worker to not cache those resources. Instead, it treats them as unrequested and waits until they're requested again before updating them. An updateMode of lazy is only valid if the installMode is also lazy.

Furthermore, you can configure service worker to cache responses from APIs. After the ```"assetGroups":[]```, you need to add ```"dataGroups":[]```. For example:

```
"dataGroups": [
    {
      "name": "api-performance",
      "urls": ["apiUrl/subpath1/**"],
      "cacheConfig": {
        "strategy": "performance"
        "maxAge": "7d",
        "maxSize": 100,
      }
    },
    {
      "name": "api-freshness",
      "urls": ["apiUrl/subpath2/**"],
      "cacheConfig": {
        "strategy": "freshness",
        "maxSize": 100,
        "maxAge": "3d",
        "timeout": "3s"
      }
    }
];
```
The Angular service worker can use either of two caching strategies for data resources.

 - **performance**  optimizes for responses that are as fast as possible. If a resource exists in the cache, the cached version is used, and no network request is made. This is suitable for resources that don't change often; for example, user avatar image.

 - **freshness** optimizes for currency of data. Only if the network times out, according to timeout, does the request fall back to the cache. This is useful for resources that change frequently; for example, account balances.

** Reference from [Angular documentation](https://angular.io/guide/service-worker-config)

### Manifest 
Manifest file describes the application, necessery for web application to be installed and presented to user as "native" application. It provides information such as name, author, icon(s), version, display type, theme colors, etc.
