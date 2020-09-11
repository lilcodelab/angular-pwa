# Angular Progressive Web Application (PWA)
This project shows how to setup Angular v10 project with PWA package. One of the important step in setup is configuring service worker, called NGSW.

## Progressive Web Application (PWA)
Description

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

### Make Angular application as Progressive web application
First you need to add Angular PWA package through Angular CLI ```ng add @angular/pwa```. This command made some changes inside a project: 

- Files ```ngsw-config.json``` and ```src/manifest.webmanifest``` are created.
- Also some files are updated, such as ```angular.json```, ```package.json```, ```src/app.module.ts```, ```src/index.html ```.

___

### Service worker
If you start your Angular application using ```ng serve```, the service worker should not be started.
You can check that inside ***DevTools --> Applications --> Service Workers***.

First reason is because we are using development environment, inside ```src/app.module.ts``` there is rule ```ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production })```, which is false. During development we can use service worker on ```localhost```, but in the production site we will need HTTPS setup on hosting server.

**Requirements**
1. Your browser must support service workers, you can check it on this [link]('https://caniuse.com/serviceworkers').
2. In production environment use service workers with HTTPS protocol on your site.

 ##### Interceptor

 ##### Cache

 
 ##### Offline mode
 ##### Notifications

### Manifest 

