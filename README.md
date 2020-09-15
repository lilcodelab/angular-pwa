# Angular Progressive Web Application (PWA)
DESCRIPTION

## Requirements
Angular requires a current, active LTS, or maintenance LTS version of Node.js.

## Setup Angular PWA application
### Install and setup Angular
1. Install Angular CLI globally ```npm install -g @angular/cli```
2. Install Angular using Angular CLI, called AngularPWA ```ng new AngularPwa```
    - For this demo purposes :
         - Would you like to add Angular routing? (y/n) **n**
         - Which stylesheet format would you like to use? (Use arrow keys): **SCSS**

### Start Angular application
In period of development you can run application with command ```ng serve```.   
By default your application will be hosted on http://localhost:4200 .  

If you want to build your Angular application, run command ```ng build```. For production build add production parameter ```ng build --prod```. 
Inside your Angular application folder, ``dist`` folder will be created with compiled files.

___
## Make Angular application as Progressive web application
First you need to add Angular PWA package through Angular CLI ```ng add @angular/pwa```. This command made some changes inside a project: 

- Files ```ngsw-config.json``` and ```src/manifest.webmanifest``` are created.
- Also some files are updated, such as ```angular.json```, ```package.json```, ```src/app.module.ts```, ```src/index.html ```.

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

 ##### Cache

 ##### Offline mode

 ##### Notifications

### Manifest 

