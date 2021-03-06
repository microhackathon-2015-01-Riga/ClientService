boot-microservice-gui
=======================

This is an extension of the non-GUI boot-microservice. To read about the backend side, please refer to 
[boot-microservice README](https://github.com/4finance/boot-microservice/blob/master/README.md)

GUI
---

The webapp is generated by [yeoman](http://yeoman.io/) using the [angular generator](https://github.com/yeoman/generator-angular).

It uses grunt that will automatically 'compile' your whole application, allowing cool dev mode with live reloads.

Inside you will find [bower](http://bower.io/) for javascript dependency management and [node](http://nodejs.org/) with 
[npm](https://www.npmjs.org/) that grunt uses.

### Development mode

Before first use, build your whole application with `gradle build`. It will download auto-magically all npms and bower dependencies.

Then run you application (for example from Idea, just run main in `com.ofg.twitter.Application` specifying
the correct -Dspring.profiles.active).

Now your application (backend) works. But you still need js+html. And since this is 2014, you don't just write html anymore, you have to use a shitload of libs :)

Install npm if you don't have it already. For example on Debian-based linux run:
```
sudo apt-get install npm
```

Then install grunt.
```
sudo npm install -g grunt-cli
```

And now make a symbolic link, because nodejs from Debian repos has a wrong name
```
ln -s /usr/bin/nodejs /usr/bin/node
```

Finally go to `src/main/web` and type `grunt serve`. This will run a local webserver on port 9000, your application will 
automatically open in the browser and from now on on every change in you webapp the browser will automatically refresh
(no need to hit cmd-R all the time!).

Easy, right? Writing HTML in 2014 is simple... nooooot! :D 

### Production mode

When you build your application there is a special directory `src/main/web/dist` created, where grunt puts your minified, compacted
and production-ready application. Then the whole folder will be bundled in your jar's `static` folder which will make
it available when you run the jar.

### Calling your microservice REST services

Once you expose some REST services on your backend, you will probably want to call them from Angular.

To make it possible in the Development mode, you will have to expose them via proxy (your dev page is available
on port 9000 while the app is on 8080, remember?). Look into `src/main/web/Gruntfile.js` and inside `connect` you will have
`proxies` section like this

```js
connect: {
            proxies: [
                {context: '/info', host: 'localhost', port: 8080},
                {context: '/city', host: 'localhost', port: 8080},
                {context: '/api', host: 'localhost', port: 8080}
            ],
```

Just add whatever you wish. If you don't like exposing every service explicitly, you can expose them all in spring under some
 common path like /rest, and then you have to specify only the /rest in the proxy.
 
### Cleaning npm and bower deps
 
 Type `gradle cleanGUIDeps`
 
## Build status
[![Build Status](https://travis-ci.org/4finance/boot-microservice-gui.svg?branch=master)](https://travis-ci.org/4finance/boot-microservice-gui) [![Coverage Status](http://img.shields.io/coveralls/4finance/boot-microservice-gui/master.svg)](https://coveralls.io/r/4finance/boot-microservice-gui)
