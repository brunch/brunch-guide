# Using third-party module registries

This is part of [The Brunch.io Guide](../../README.md).

In practice, the nicer way to deal with third-party dependencies is through **existing module registries**.  In the JS world, this mostly means **[npm](https://www.npmjs.com/)** (for Node.js and for front-end code; it recently became the official registry for jQuery plugins!) or [Bower](http://bower.io/) (which, IMHO, is on the way out, with npm slowly replacing it).

The big benefit of formal dependency management is that you get to express **flexible version dependencies**, easing the installation and **upgrade** of your dependencies.

At the time of this writing, the Brunch team is hard at work to provide first-class integration with npm, which will greatly ease isomorphic JS and will let us use our `node_modules`-installed code transparently inside our front-end application code, as modules still.  There is experimental npm support in master, more details in the end of this chapter.  Until then, we’ll need to hack around with the [Browserify plugin for Brunch](https://www.npmjs.com/package/browserify-brunch) plugin.

In the meantime, Bower integration [is already here](https://github.com/brunch/brunch/blob/master/docs/faq.md#how-to-use-bower).  We could have used that for jQuery, for instance.  If we use the following `bower.json` to describe our project:

```json
{
  "name": "simple-brunch",
  "version": "0.1.0",
  "private": true,
  "ignore": ["**/.*", "node_modules", "bower_components", "test", "tests"]
}
```

We can then install jQuery like so:

```sh
$ bower install --save jquery#1.*
…

jquery#1.11.3 bower_components/jquery
```

This command looks up the Bower registry for the latest jQuery version in the 1.x range, installs it in the proper local directory, and updates `bower.json` to reflect the newly installed component.

We then can strip `jquery.js` from our `app`.  In order to retain the split targets we had in the previous chapter, we need to adjust our `brunch-config.coffee` so regexes match the new file layout:

```coffeescript
module.exports = config:
  files:
    javascripts: joinTo:
      'libraries.js': /^(?!app\/)/
      'app.js': /^app\//
    stylesheets: joinTo: 'app.css'
```

What’s more (or less, actually) is that Bower **doesn’t expose modules (ಥ﹏ಥ)**.  So we must strip our `require(…)` call from our `app/application.js`, assuming `$` is a global again (bleuargh).

Rebuild, refresh: it works!

## Experimental npm support

The head version of brunch includes an experimental support of npm for front-end packages.  To use it, first add your front-end packages to `packages.json` and `npm install`.

Then, edit `brunch-config.coffee` to enable the integration:

```coffeescript
module.exports = config:
  npm:
    enabled: true
    packages: ['jquery']

  files:
    javascripts: joinTo:
      'libraries.js': /^(?!app\/)/
      'app.js': /^app\//
    stylesheets: joinTo: 'app.css'
```

(Note that at the time being, you have to manually list the packages that should be included into the build.  Other options are being explored, such as automatically figuring this out based on application code.)

This would expose jQuery as a module so the `require` call still can stay there.

However, it can be a case that you absolutely **must** expose a certain package globally.  To do so, you would add a `globals` definition into the config.  For example, if we wanted to expose jQuery globally as `$`, we would modify the config to look like this:

```coffeescript
module.exports = config:
  npm:
    enabled: true
    packages: ['jquery']
    globals:
      $: 'jquery'

  files:
    javascripts: joinTo:
      'libraries.js': /^(?!app\/)/
      'app.js': /^app\//
    stylesheets: joinTo: 'app.css'
```

Additionally, some packages ship with stylesheets.  To instruct Brunch to add these into the build, use the `styles` property in the npm config.  For example, if we installed the Pikaday package and wanted to include its styles, we'd adjust the config like this:

```coffeescript
module.exports = config:
  npm:
    enabled: true
    packages: ['jquery', 'pikaday']
    styles:
      pikaday: ['css/pikaday.css']

  files:
    javascripts: joinTo:
      'libraries.js': /^(?!app\/)/
      'app.js': /^app\//
    stylesheets: joinTo: 'app.css'
```

Unlike with bower, npm packages don't normally point to the css files that should be included so you do have to manually specify these.

----

« Previous: [Starting from scratch](chapter04-starting-from-scratch.md) • Next: [A shot at templating](chapter06-a-shot-at-templating.md) »
