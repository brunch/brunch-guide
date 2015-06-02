# Using third-party module registries

This is part of [The Brunch.io Guide](../../README.md).

In practice, the nicer way to deal with third-party dependencies is through **existing module registries**.  In the JS world, this mostly means **[npm](https://www.npmjs.com/)** (for Node.js and for front-end code; it recently became the official registry for jQuery plugins!) or [Bower](http://bower.io/) (which, IMHO, is on the way out, with npm slowly replacing it).

The big benefit of formal dependency management is that you get to express **flexible version dependencies**, easing the installation and **upgrade** of your dependencies.

At the time of this writing, the Brunch team is hard at work to provide first-class integration with npm, which will greatly ease isomorphic JS and will let us use our `node_modules`-installed code transparently inside our front-end application code, as modules still.  Until then, we’ll need to hack around with the [Browserify plugin for Brunch](https://www.npmjs.com/package/browserify-brunch) plugin.

In the meantime, Bower integration [is already here](https://github.com/brunch/brunch/blob/stable/docs/faq.md#how-to-use-bower).  We could have used that for jQuery, for instance.  If we use the following `bower.json` to describe our project:

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

----

« Previous: [Starting from scratch](chapter04-starting-from-scratch.md) • Next: [A shot at templating](chapter06-a-shot-at-templating.md) »
