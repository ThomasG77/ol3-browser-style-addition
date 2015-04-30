Response illustrated for https://github.com/openlayers/ol3/pull/3627

First, install with NPM doing :

    npm install

Then, edit the file node_modules/openlayers/package.json and to simulate if PR was accepted, add after section

    "bugs": {
      "url": "https://github.com/openlayers/ol3/issues"
    },

the following (PS: I encounter an issue with the path in PR and I've updated my PR accordindly)

    "browser": "dist/ol.js",
    "style": [
      "css/ol.css"
    ],

Do the following

    ./node_modules/.bin/browserify js/app.js -o js/bundle.js -p [parcelify -o css/bundle.css]

You will generate a file in js/bundle.js and css/bundle.css and will now be able to browse index.html without missing files.

What you have to see is the fact that package.json at the project your are using look this way

    {
      "name": "test-ol3-new-parameters",
      "version": "1.0.0",
      "description": "",
      "main": "",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "author": "Thomas Gratier",
      "license": "MIT",
      "dependencies": {
        "openlayers": "^3.4.0"
      },
      "style": "main.css",
      "devDependencies": {
        "browserify": "^9.0.8",
        "parcelify": "^1.1.0",
        "watchify": "^3.1.2"
      }
    }

I volontary use `style` and you will see that `parcelify` already guessed well that ol.css should be retrieve from "css/ol.css" relatively to node_modules/openlayers/ directory and then add content from main.css.

You have a css dependency tool for free depending on js `require` calls.

It's also possible to use it with `watchify`

    ./node_modules/.bin/watchify js/app.js -o js/bundle.js -p [ parcelify -wo css/bundle.css ]

You can use it as a standalone tool too with

    ./node_modules/.bin/parcelify js/app.js -j js/bundle.js -c css/bundle.css

You can also compile saas or less files with **transforms** in Browserify.

In a case where you will need some OpenLayers 3 plugins, you will be able to solve css dependencies with js `require` calls.
