# How to develop a Angular Progressive Web Application
Progressive web applications are web applications served through the web browser, built using common web technologies such as HTML, CSS and JavaScript, with ability of offline work, push notifications and accesibility to hardware of device. They are often designed with resposinve principle, which means the design is adjusted for the screen size and make them looks like a native application.

On of the main benefit of developing PWA application is less built time and smaller costs, if you are comparing them with developing of native applications. Furthermore, you dont need developers for each platform (Android, iOS), because they are build using common technologies.

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
Service worker is a script that runs in background of your browser, separeted from web application. The advantage of service worker is ability to handle a request, kind of interceptor. Because it can handle every request of application, offten is used as cache manager. Furthermore, service worker implements ```onfetch``` and ```onmessage``` handlers, that means push notifications are suporrted (depends on browser support).

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

 - **performance** optimizes for responses that are as fast as possible. If a resource exists in the cache, the cached version is used, and no network request is made. This is suitable for resources that don't change often; for example, user avatar image.

 - **freshness** optimizes for currency of data. Only if the network times out, according to timeout, does the request fall back to the cache. This is useful for resources that change frequently; for example, account balances.

** Reference from [Angular documentation](https://angular.io/guide/service-worker-config)

### Manifest 
Manifest file describes the application, necessery for web application to be installed and presented to user as "native" application. It provides information such as name, author, icon(s), version, display type, theme colors, etc.

### Responsive web design
Responsive web design (RWD) is an approach of designing web application to adjust for every screen size. Web application designed with RWD adapts the layout by using ```fluid and proportion-based grid```, ```flexible-images``` and ```CSS3 media queries```:

- The fluid grid concept calls for page element sizing to be in relative units like percentages, rather than absolute units like pixels or points.
- Flexible images are also sized in relative units, so as to prevent them from displaying outside their containing element.
- Media queries allow the page to use different CSS style rules based on characteristics of the device the site is being displayed on, e.g. width of the rendering surface (browser window width or a physical display size).
- Responsive layouts automatically adjust and adapt to any device screen size, whether it is a desktop, a laptop, a tablet, or a mobile phone.
***

*** RWD reference [Responsive design](https://en.wikipedia.org/wiki/Responsive_web_design)


### Update packages
Before you update all packages, frist update Angular version using ```ng update @angular/cli @angular/core```
If you want to update packages to latest I recommend to install [npm-check-updates](npm i -g npm-check-updates). After that run command ```ncu -u``` which will update package.json to latest version of packages.

Maybe some packages will need to be downgrades, becauses they are dependency package of some other package. Be aware of it!
## References
- https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps
- https://developers.google.com/web/fundamentals/primers/service-workers
- https://angular.io/guide/service-worker-intro
- https://angular.io/guide/service-worker-config
- https://caniuse.com/serviceworkers
- https://www.smashingmagazine.com/2011/01/guidelines-for-responsive-web-design/
- https://en.wikipedia.org/wiki/Responsive_web_design
- https://en.wikipedia.org/wiki/Man-in-the-middle_attack

## Github
https://github.com/bpenovic/angular-pwa
