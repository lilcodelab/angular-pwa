# Angular Progressive Web Application (PWA)
This project shows how to setup Angular v10 project with PWA package. One of the important step in setup is configuring service worker, called NGSW.

## Requirements
Angular requires a current, active LTS, or maintenance LTS version of Node.js.

## How to setup Angular with PWA package
1. Install Angular CLI globally ``` npm install -g @angular/cli ```
2. Install Angular using Angular CLI, called AngularPWA ``` ng new AngularPwa ```
    - For current purposes :
         - Would you like to add Angular routing? (y/n) **n**
         - Which stylesheet format would you like to use? (Use arrow keys): **SCSS**

## How to start Angular application
In period of development you can run application with command ```ng serve```.   
By default your application will be hosted on http://localhost:4200 .  

If you want to build your Angular application, run command ```ng build```. For production build add production parameter ```ng build --prod```. 
Inside your Angular application folder, ``dist`` folder will be created with compiled files.

