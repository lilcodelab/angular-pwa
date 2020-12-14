# How to develop a Angular Progressive Web Application
Progressive web applications are web applications served through the web browser, built using common web technologies such as HTML, CSS, and JavaScript, with the ability to work offline, sending push notifications and accessibility to device hardware components. They are often designed with responsive principles, which means the design is adjusted for the screen size and makes it look like a native application.

In comparison to developing native applications, one of the main benefits of developing PWA applications is less built time and reduced costs. Furthermore, you do not need developers for each platform (Android, iOS) because they are build using common technologies. These applications are a good use-case for internally used applications that are needed quickly or for a specific niche. 

## Requirements
Angular requires a current, active LTS, or maintenance LTS version of Node.js.

## Setup Angular application
### Install and setup Angular
1. Install Angular CLI globally ```npm install -g @angular/cli```
2. Generate new Angular app using Angular CLI, called AngularPWA ```ng new AngularPwa```

### Start Angular application
While developing you can run application with command ```ng serve```.   
By default, your application will be hosted on http://localhost:4200 .  

If you want to build your Angular application, run command ```ng build```. For production build add production parameter ```ng build --prod```. 
Find your ``dist`` folder with compiled files within the Angular application folder.
___
## Make Angular application as Progressive web application
Add an Angular PWA package through Angular CLI ```ng add @angular/pwa```. This command makes changes within the project: 

- files ```ngsw-config.json``` and ```src/manifest.webmanifest``` are created.
- also some files are updated, such as ```angular.json```, ```package.json```, ```src/app.module.ts```, ```src/index.html ```.

### Service worker
The Service worker is a script that runs in background of your browser separeted from web application. The advantage of service worker is ability to handle a request, kind of interceptor.It can handle every application request and it is offten used as cache manager. Additionally, service worker implements ```onfetch``` and ```onmessage``` handlers, so push notifications are suporrted (depending on the browser support).

### Start service worker (NGSW)
If you start your Angular application using ```ng serve```, the service worker should not be started.
You can check that inside ***DevTools --> Applications --> Service Workers***.

Here's why:

You are using development environment, inside ```src/app.module.ts``` there is rule ```ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production })```, which is false. During development we can use service worker on ```localhost```, but in the production site we will need HTTPS setup on hosting server.

**Requirements**
1. Your browser must support service workers, you can check it on this [link](https://caniuse.com/serviceworkers).
2. In production environment use service workers with HTTPS protocol on your site.

For development purposes, you can run service worker on the ```localhost``` but in the production environment we want to use **HTTPS**.
With the service worker you can filter responses and hijack connections. We want to avoid [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attack using HTTPS protocol.

#### Configure service worker
Inside the ```ngsw-config.json``` file is a configuration for service worker. The generated file includes configuration for statis files, such as fonts, images, script and style files. They are separated in two objects depending on the install or update mode:

***installMode***
The installMode determines how these resources are initially cached. The installMode can be either of two values:

- **prefetch** - service worker fetches every single resource from list, while the new version application is caching. Even though this can be intensive for network bandwith, it ensures that resources are available when they are requested, even in application offline work.  

- **lazy** - service worker caches a resource from list, only if that resource is requested. The resources that are not requested will not be cached.  

***updateMode***
For resources already in the cache, the updateMode determines the caching behavior when a new version of the app is discovered:

- **prefetch** - service worker fetchs and makes a cache of resources from list immediately.  

- **lazy** - service worker will update a cached resources when they are requested again. Lazy mode is only working if a installMode is also lazy one.  

You can configure service worker to cache responses from APIs. After the ```"assetGroups":[]```, you need to add ```"dataGroups":[]```. For example:

```json
"dataGroups": [
    {
      "name": "api-performance",
      "urls": ["apiUrl/subpath1/**"],
      "cacheConfig": {
        "strategy": "performance",
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

 - **performance** - used for resources that are not changed often. Service worker returns resource from cache, if a resource exist for current version of application in cache memory and network will not be send trought a network.  
 
 - **freshness** - used for resources that are changed often. Service worker returns data from cache, only if network times out.  

=> Reference from [Angular documentation](https://angular.io/guide/service-worker-config)

### Manifest 
The manifest file describes the application, necessary for a web application to be installed and presented to the user as a "native" application. It provides information such as name, author, icon(s), version, display type, theme colors, etc.

### Responsive web design
Responsive web design (RWD) is an approach of designing web application to adjust for every screen size. Web application designed with RWD adapts the layout by using ```fluid and proportion-based grid```, ```flexible-images``` and ```CSS3 media queries```:

- **The fluid grid** concept requests a developer to use a relative units for sizing of elements, for example percentages, view height, view width, and not absolute units such as pixels or points.
- **Flexible images** are styled with using a relative units to adjust their size on container element resizing.
- **Media queries** allow developer to apply a different CSS style rules based device or rendering surfrace (browser) characteristics such as height and width.
- **Responsive layouts** adjust and adapt application layout to any device screen size.

=> RWD reference [Responsive design](https://en.wikipedia.org/wiki/Responsive_web_design)

### Update packages
Before you update all packages, frist update Angular version using ```ng update @angular/cli @angular/core```
If you want to update packages to latest, I recommend to install [npm-check-updates](https://www.npmjs.com/package/npm-check-updates). After that run command ```ncu -u``` which will update package.json to the latest version of packages.

Maybe some packages will need to be downgrades becauses they are dependency package of some other package. Be aware of it!
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
