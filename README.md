# grunt-bower-task [![Build Status](https://travis-ci.org/yatskevich/grunt-bower-task.png)](https://travis-ci.org/yatskevich/grunt-bower-task)

> Install Bower packages. Smartly.

## Getting Started
_If you haven't used [grunt][] before, be sure to check out the [Getting Started][] guide._

Please note, this plugin works **only with grunt 0.4+**. If you are using grunt 0.3.x then consider an [upgrade to 0.4][].

From the same directory as your project's [Gruntfile][Getting Started] and [package.json][], install this plugin with the following command:

```bash
npm install grunt-bower-task --save-dev
```

Once that's done, add this line to your project's Gruntfile:

```js
grunt.loadNpmTasks('grunt-bower-task');
```

If the plugin has been installed correctly, running `grunt --help` at the command line should list the newly-installed plugin's task or tasks. In addition, the plugin should be listed in package.json as a `devDependency`, which ensures that it will be installed whenever the `npm install` command is run.

[grunt]: http://gruntjs.com/
[Getting Started]: https://github.com/gruntjs/grunt/wiki/Getting-started
[package.json]: https://npmjs.org/doc/json.html
[upgrade to 0.4]: https://github.com/gruntjs/grunt/wiki/Upgrading-from-0.3-to-0.4

## Grunt task for Bower

### Overview
In your project's Gruntfile, add a section named `bower` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  bower: {
    install: {
       //just run 'grunt bower:install' and you'll see files from your Bower packages in lib directory
    }
  },
})
```

### Options

#### options.targetDir
Type: `String`
Default value: `./lib`

A directory where you want to keep your Bower packages.

#### options.cleanup
Type: `boolean`
Default value: `false`

If you set `cleanup: true` then every time you run `grunt bower:install` both `components` and `./lib` (specified by `options.targetDir`) will be deleted.

#### options.install
Type: `boolean`
Default value: `true`

You can disable installation of Bower packages and just use this task to perform `cleanup`.

### Usage Examples

#### Default Options
Default options are good enough if you want to install Bower packages and keep only `"main"` files (as specified by package's `component.json`) in separate directory.

```js
grunt.initConfig({
  bower: {
    install: {
      // options: { 
      //   targetDir: './lib',
      //   cleanup: false,
      //   install: true
      // }
    }
  },
})
```

#### Custom Options
In this initial version there are no more options in plugin itself. **BUT!**

### Advanced usage
At this point of time "Bower package" = "its git repository". It means that package includes tests, licenses, etc.
Bower's community actively discusses this issue (GitHub issues [#46][],[#88][], [on Google Groups][GG])
That's why you can find such tools like [blittle/bower-installer][] which inspired this project.

[GG]: https://groups.google.com/forum/?fromgroups=#!topic/twitter-bower/SQEDDA_gmh0
[#88]: https://github.com/twitter/bower/issues/88
[#46]: https://github.com/twitter/bower/issues/46
[blittle/bower-installer]: https://github.com/blittle/bower-installer

Okay, if you want more than `"main"` files in `./lib` directory then put `"exportsOverride"` section into your `component.json`:

```json
{
  "name": "simple-bower",
  "version": "0.0.0",
  "dependencies": {
    "jquery": "~1.8.3",
    "bootstrap-sass": "*",
    "requirejs": "*"
  },
  "exportsOverride": {
    "bootstrap-sass": {
      "js": "js/*.js",
      "scss": "lib/*.scss",
      "img": "img/*.png"
    },
    "requirejs": {
      "js": "require.js"
    }
  }
}
```
`grunt-bower-task` will do the rest:

* If Bower package has defined `"main"` files then they will be copied to `./lib/<package>/`.
* If `"main"` files are empty then the whole package directory will be copied to `./lib`.
* When you define `"exportsOverride"` only asset types and files specified by you will be copied to `./lib`.

For the example above you'll get the following files in `.lib` directory:

```
jquery/jquery.js
js/bootstrap-sass/bootstrap-affix.js
...
js/bootstrap-sass/bootstrap-typeahead.js
js/requirejs/require.js
scss/bootstrap-sass/_accordion.scss
...
scss/bootstrap-sass/_wells.scss
scss/bootstrap-sass/bootstrap.scss
scss/bootstrap-sass/responsive.scss
img/bootstrap-sass/glyphicons-halflings-white.png
img/bootstrap-sass/glyphicons-halflings.png
```

## TODO
I'm going to add options to configure target dirs for asset types, like "css" goes to `public/vendor/css`, "img" goes to `dist/img`.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [grunt][].

## Release History
* 2012/11/25 - v0.1.0 - Initial release.

## License
Copyright (c) 2012 Ivan Yatskevich

Licensed under the MIT license.

<https://github.com/yatskevich/grunt-bower-task/blob/master/LICENSE-MIT>
