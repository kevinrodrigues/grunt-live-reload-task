# grunt-live-reload-task


Things you need to get started:

* package.json (Sits at the root of your project) - lists the Grunt plugins we'll use:

They are:

* grunt-contrib-connect, to serve the files of our project
* grunt-regarde, to monitor the file changes
* grunt-contrib-livereload, to reload the page in the browser


Which gives us the following package.json file:

```html
{
  "name":"livereload",
  "version":"1.0.0",
  "dependencies":{},
  "devDependencies":{
    "matchdep": "~0.1.1",
    "grunt": "~0.4.0",
    "grunt-regarde": "~0.1.1",
    "grunt-contrib-connect":  "0.1.2",
    "grunt-contrib-livereload": "0.1.1"
  }
}
```

* Grunt is based on Node.js and you'll need to load it's plugins using NPM. See Node.js documentation if you haven't got it installed:
https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager

* You could also spin up a local server or modify it to work with IIS. Example:

```html
// grunt-contrib-connect will serve the files of the project
// on specified port and hostname
connect: {
  all: {
    options:{
      port: 9000,
      hostname: "0.0.0.0",
      // No need for keepalive anymore as watch will keep Grunt running
      //keepalive: true,

      // Livereload needs connect to insert a cJavascript snippet
      // in the pages it serves. This requires using a custom connect middleware
      middleware: function(connect, options) {

        return [

          // Load the middleware provided by the livereload plugin
          // that will take care of inserting the snippet
          require('grunt-contrib-livereload/lib/utils').livereloadSnippet,

          // Serve the project folder
          connect.static(options.base)
        ];
      }
    }
  }
},
```

* Specifying files to watch lives under the following settings:

```html
regarde: {
  all: {
    // This'll just watch the index.html file, you could add **/*.js or **/*.css
    // to watch Javascript and CSS files too.
    files:['index.html', 'css/main.css'],
    // This configures the task that will run when the file change
    tasks: ['livereload']
  }
}
```

* The rest of this project folder should be self explanatory.
