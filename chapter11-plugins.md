# Plugins for all your build needs

This is part of [The Brunch.io Guide](README.md).

With Brunch, features don't get provided through the same architectural split as you’d fine in Grunt, Gulp, etc.  A ton of features and behaviors are **built-in** (build pipeline, incremental watcher, sourcemaps, etc.) but everything else remains in **plugins**, including the handling of every **source language**.

You will generally use at least one plugin for scripts, one for styles, and a minifier for every category.

The official website [has a decent list](http://brunch.io/plugins.html), based on authors' pull requests, but there are actually [a lot more](https://www.npmjs.com/search?q=brunch); we'll try and browse through the main ones below.

**Note:** in this chapter’s text, plugin names are always **links** to their npm homepage (featuring their description, lins, download counts, etc.).

## Enabling a plugin

For a plugin to be enabled and used, **you just need to install it**, which means it is both in `package.json` and `node_modules`.  The easiest way to do that for the first time is with `npm install --save-dev`, and the easiest way from an existing `package.json` is through a simple `npm install`.

Brunch will then inspect all modules that satisfy both these requirements, looking for any module whose name contains “brunch” (usually at the end, after a hyphen, but it can actually be anywhere), and verifies that the module exports a constructor featuring a `brunchPlugin` property set to `true` on its `prototype` property.  Otherwise, the module is ignored.

If it conforms to this, it gets automatically instantiated, with the global configuration passed as argument, and gets registered based on its scope declaration (file type, extensions, pattern…  we’ll dive into this later).

In short, forget about crazy splatters of redundant `loadNpmTasks` here.  Brunch keeps it short and sweet.

## Fine-tuning through optional configuration-configuration)

Every plugin is usually designed to be **operational and useful without any configuration**; that being said, it's often possible to tweak their behavior through specific configuration.  These settings are defined inside `brunch-config.coffee`, under the `plugins` key and a subkey named after the plugin.

For instance, the [`appcache-brunch`](https://www.npmjs.com/package/appcache-brunch) plugin looks for `plugins.appcache`.  Most often, key names are trivial to infer, but they can stray from an exact match, or opt for camel case…  Just like [`browser-sync-brunch`](https://www.npmjs.com/package/browser-sync-brunch) that looks for `plugins.browserSync`.  Just check out the plugin’s documentation to be sure!

## Brunch and CSS

CSS-related plugins feature a `type` of `"stylesheet"` on their prototype, and usually provide a specific value for their `extension` property.  These are mostly *transpilers*, what Brunch generically refers to as *compilers*.  At the time of this writing, the main ones are:

  * [`css-brunch`](https://www.npmjs.com/package/css-brunch), for vanilla (W3C) CSS files;
  * [`cssnext-brunch`](https://www.npmjs.com/package/cssnext-brunch), targeting upcoming evolutions (CSS4+, etc.) thanks to [cssnext](https://cssnext.github.io/);
  * [`less-brunch`](https://www.npmjs.com/package/less-brunch) and [`sass-brunch`](https://www.npmjs.com/package/sass-brunch), obviously, along with [`compass-brunch`](https://www.npmjs.com/package/compass-brunch) that delegates to SASS for aficionados of stuff that is… er… heavyweight?!
  * [`stylus-brunch`](https://www.npmjs.com/package/stylus-brunch) for [Stylus](http://learnboost.github.com/stylus/), my personal sweetheart;
  * There are various Swiss-army knives for CSS; they get plugins, such as [`rework-brunch`](https://github.com/bolasblack/rework-brunch) for [rework](https://github.com/reworkcss/rework), [`pleeease-brunch`](https://www.npmjs.com/package/pleeease-brunch) for [pleeease](http://pleeease.io/) or [`postcss-brunch`](https://www.npmjs.com/package/postcss-brunch) for [PostCSS](https://github.com/postcss/postcss).
  * [`autoprefixer-brunch`](https://www.npmjs.com/package/autoprefixer-brunch) focuses on auto-prefixing rules using the aptly-named [autoprefixer](https://github.com/postcss/autoprefixer), which is part of PostCSS, by the way.
  * When it comes to *coding style*, [CSSComb](http://csscomb.com/) is nice and offers plugins for builders, including [`csscomb-brunch`](https://www.npmjs.com/package/csscomb-brunch).

## Brunch and JavaScript

This is a similar landscape to CSS, except `type` is now `"javascript"`.  I’ll talk about linters later, but sticking with *transpilers* we’re pretty well stocked already:

  * [`javascript-brunch`](https://www.npmjs.com/package/javascript-brunch) for vanilla (ES3/5, ECMA-262 standards) JS;
  * [`coffee-script-brunch`](https://www.npmjs.com/package/coffee-script-brunch), of course, and even [`iced-coffee-script-brunch`](https://www.npmjs.com/package/iced-coffee-script-brunch) for hotheads;
  * [`json-brunch`](https://www.npmjs.com/package/json-brunch), so you can use JSON files directly as modules;
  * in the same corner, but way less popular, you can get [`LiveScript-brunch`](https://www.npmjs.com/package/LiveScript-brunch) for [LiveScript](http://gkz.github.io/LiveScript/), [`ember-script-brunch`](https://www.npmjs.com/package/ember-script-brunch) for the rather niche [EmberScript](https://github.com/ghempton/ember-script), [`roy-brunch`](https://www.npmjs.com/package/roy-brunch) for the *even more niche* [Roy](http://roy.brianmckenna.org/) and [`typescript-brunch`](https://www.npmjs.com/package/typescript-brunch) for the vastly more popular [TypeScript](http://www.typescriptlang.org/).
  * Subtler stuff: [`wisp-brunch`](https://www.npmjs.com/package/wisp-brunch) handles [Wisp](https://github.com/Gozala/wisp), a kind of ClojureScript, and [`sweet-js-brunch`](https://www.npmjs.com/package/sweet-js-brunch) opens the heavenly doors of “hygienic macros” thanks to [sweet.js](http://sweetjs.org/).

And just because this is 2015 after all, you’ll find a bunch of options for automatic JSX (React) processing and ES6 goodness:

  * [`react-brunch`](https://www.npmjs.com/package/react-brunch) auto-compiles `.jsx` *via* React;
  * [`es6-module-transpiler-brunch`](https://www.npmjs.com/package/es6-module-transpiler-brunch) focuses on the native ES6 module syntax (the future!);
  * [`traceur-brunch`](https://www.npmjs.com/package/traceur-brunch) and [`babel-brunch`](https://www.npmjs.com/package/babel-brunch) let you use a ton of ES6 features.  Currently, [Babel](https://babeljs.io/) (anciennement 6to5 + CoreJS) is [solidly ahead](http://kangax.github.io/compat-table/es6/#babel) when it comes to feature coverage, and what's more, it can handle JSX too!

## Brunch and templates

After scripts and styles, the third category of files that Brunch has special processing for is templates.

Let me reiterate: a template plugin for Brunch is a compiler that turns a template into a **module whose default export is a pre-compiled function**, hence you do not incur any run-time penalty.  That function takes as unique argument an objet whose properties are usable directly by your template, just like local variables: what is commonly referred to as a *presenter* or *view model*.  The function synchronously returns HTML.

When it comes to template languages, we have **a world of choices**:

  * **All-time classics**:
    * [`handlebars-brunch`](https://www.npmjs.com/package/handlebars-brunch) and an [`ember-handlebars-brunch`](https://www.npmjs.com/package/ember-handlebars-brunch) specialization, obviously;
    * [`hoganjs-brunch`](https://www.npmjs.com/package/hoganjs-brunch) if you favor Hogan, a common alternative;
    * [`jade-brunch`](https://www.npmjs.com/package/jade-brunch) for my beloved [Jade](http://jade-lang.com/), and even a [`jade-angularjs-brunch`](https://www.npmjs.com/package/jade-angularjs-brunch) specialization, that produces an Angular module.
  * **The almighty** [Dust](http://akdubya.github.io/dustjs/) is very well represented as well *via* [`dustjs-linkedin-brunch`](https://www.npmjs.com/package/dustjs-linkedin-brunch) for LinkedIn’s [Dust extension](http://linkedin.github.io/dustjs/), also used by PayPal, among other high-profile users…
  * Someone came up with [`jade-react-brunch`](https://www.npmjs.com/package/jade-react-brunch), that **avoids having to use kludgy JSX by letting you use a separate Jade file**, but outputs code using the `React.DOM` builder, just like JSX literals…  This makes me drool, I must say!
  * Then you'll get a lot of more niche syntaxes:
    * [`eco-brunch`](https://www.npmjs.com/package/eco-brunch) for [Eco](https://github.com/sstephenson/eco), an ERB variant that used CoffeeScript;
    * [`emblem-brunch`](https://www.npmjs.com/package/emblem-brunch) for [Emblem](http://emblemjs.com/), which is much like Jade but has a lot of syntactic sugar for Ember and Handlebars regulars;
    * [`markdown-brunch`](https://www.npmjs.com/package/markdown-brunch) and [`yaml-front-matter-brunch`](https://www.npmjs.com/package/yaml-front-matter-brunch), which end up looking kinda like Jekyll;
    * [`swig-brunch`](https://www.npmjs.com/package/swig-brunch) for [Swig](http://paularmstrong.github.io/swig/), for the Django fans out there;
    * [`ractive-brunch`](https://www.npmjs.com/package/ractive-brunch) for [RactiveJS](http://www.ractivejs.org/);
    * [`nunjucks-brunch`](https://www.npmjs.com/package/nunjucks-brunch) for [Nunjucks](http://mozilla.github.io/nunjucks/), and finally
      * [`html2js-brunch`](https://www.npmjs.com/package/html2js-brunch) for [HTML2JS](https://github.com/aberman/html2js-brunch).
  * You'll also find plugins that “statically” compile templates: this is for people who don't want to have a vanilla HTML static asset, and would rather use an alternative syntax, possibly injecting a *presenter* in there from, say, a JSON file:
      * [`static-jade-brunch`](https://www.npmjs.com/package/static-jade-brunch);
      * [`static-underscore-brunch`](https://www.npmjs.com/package/static-underscore-brunch) (based on Underscore.js' micro-templating).

## Brunch and development workflows

These days, **web front dev is hard**.  We use a metric ton of different techs, want to debug in a snap, get a super-fast in-browser feedback loop, pay attention to performance, and so on and so forth.

There are many tools to help us get there, but having to manually install, setup and run these individually is a major PITA.  **Brunch can help**, thanks to integration plugins.

**Linters** first:

  * [`jshint-brunch`](https://www.npmjs.com/package/jshint-brunch) of course, that will run [JSHint](http://jshint.com/) with current settings (e.g. coming from `.jshintrc`) on all our applicative codebase (by default, `app`).  This can operate either in warning mode (log but don't break the build) or error (stop the build).  Runs during incremental watching as well.
  * [`coffeelint-brunch`](https://www.npmjs.com/package/coffeelint-brunch) for [CoffeeLint](http://www.coffeelint.org/), if you’re going with CoffeeScript.
  * [`jsxhint-brunch`](https://www.npmjs.com/package/jsxhint-brunch) for [JSXHint](https://github.com/STRML/JSXHint/), which can run JSHint over JSX without tripping over markup literals.
  * Unfortunately no integration (yet!) with [ESLint](http://eslint.org/docs/integrations/), but **why not contribute it** yourself?
  * No integration either for JSLint, but I sure won't whine about *that*…

A **fast feedback loop** is a must-have when doing web front dev, that lets us see the result of our CSS or JS tweaks nearly instantly in our open browser(s).  There are a few plugins for this, all designed to run in watcher mode:

  * [`auto-reload-brunch`](https://www.npmjs.com/package/auto-reload-brunch) uses a fairly heavy-handed approach: it auto-reloads the whole page as soon as something changes.  This can be a tad overkill;  plus this relies on native Web Sockets, so IE10+.
  * [`browser-sync-brunch`](https://www.npmjs.com/package/browser-sync-brunch) embeds the excellent [BrowserSync](http://www.browsersync.io/), that lets you live inject CSS (no page reload), remote debug pages (embeds Weinre), sync a lot of interactions across open browsers (form filling, scrolling, clicking, etc.).  Super handy to test responsive stuff  *(full disclaimer: I’m one of the maintainers of the plugin).*
  * [`fb-flo-brunch`](https://github.com/deliciousinsights/fb-flo-brunch), by yours truly, transparently embeds the awesome [fb-flo](https://facebook.github.io/fb-flo/), check it out *now*!

**Code documentation** isn't forgotten either: several integrations let you regenerate docs at build time, to spare you and extra command line.

  * [`jsdoc-brunch`](https://www.npmjs.com/package/jsdoc-brunch) naturally, but also…
  * [`docco-brunch`](https://www.npmjs.com/package/docco-brunch), for [Docco](http://jashkenas.github.io/docco/), the tool that popularized annotated sources.
  * I’d love to see someone contribute `groc-brunch`, because [Groc](http://nevir.github.io/groc/) goes way beyond Docco!

There are also a number of plugins designed to **replace** keywords, markers or translation keys during the build:

  * [`process-env-brunch`](https://www.npmjs.com/package/process-env-brunch) uses environment variables;
  * [`keyword-brunch`](https://www.npmjs.com/package/keyword-brunch) (two variants) uses the global configuration to map keys and switch between its replacement behaviors;
  * [`jspreprocess-brunch`](https://www.npmjs.com/package/jspreprocess-brunch) adds a “C-style” preprocessor (with `#BRUNCH_IF` directives inside comments) that lets you change the resulting code depending on the build target;
  * [`constangular-brunch`](https://www.npmjs.com/package/constangular-brunch), along the same lines, injects YAML-based configurations insde your AngularJS app as a specific module, in an environment-sensitive way (development, production);
  * [`yaml-i18n-brunch`](https://www.npmjs.com/package/yaml-i18n-brunch) is a bit more specialized, and convertsYAML files into JSON, taking care to fill in the blanks in your *locales* from the default *locale* (assumed to be complete).

A few more plugins are worth mentioning:

  * [`dependency-brunch`](https://www.npmjs.com/package/dependency-brunch) lets you tell Brunch about specific dependencies you have between source files, when it doesn't auto-detect these, so that it triggers proper rebuilds.  For instance, when Jade views extend a layout or include mixins, such dependencies can ensure you only need to change the layout/mixins for views that use them to get rebuilt.
  * [`groundskeeper-brunch`](https://www.npmjs.com/package/groundskeeper-brunch) strips from your JS files anything that could hinder production: `console` calls, `debugger` statements, specific blocks… (if minification is used, it must happen *after* this).
  * [`after-brunch`](https://www.npmjs.com/package/after-brunch) provides a simple way to register command lines for execution after a build, which lets you add custom tasks in a generic way!

## Brunch and web performance

Brunch naturally cares about your performance, so it attempts to produce **assets that are as optimized as possible**, through third-party technologies.  Most of these plugins are irrelevant in watch mode, but are more targeted at one-shot production builds.

Let's start with **images**:

  * [`retina-brunch`](https://www.npmjs.com/package/retina-brunch) takes a high-res “Retina” image (one with `@2x` in its name) and creates a lower-res variant for lower-DPI screens;
  * [`sprite-brunch`](https://www.npmjs.com/package/sprite-brunch) relies on [Spritesmith](https://github.com/Ensighten/spritesmith) to produce an *image sprite* and the matching CSS (using SASS, LESS or Stylus) from your source images.  Not as versatile and powerful as Glue, but pretty good still.
  * [`imageoptmizer-brunch`](https://www.npmjs.com/package/imageoptmizer-brunch) (note the missing central `i`…) runs in production/optimized mode to automatically run your target folder’s images through whatever relevant tools you have installed: [JPEGTran](http://desgeeksetdeslettres.com/programmation-java/jpegtran-un-outil-permettant-doptimiser-les-images-jpeg), [OptiPNG](http://optipng.sourceforge.net/) and [SmushIt](http://imgopt.com/).  For a systematic, express weight reduction.

We certainly have top-notch JS/CSS minifiers, too:

  * [`uglify-js-brunch`](https://www.npmjs.com/package/uglify-js-brunch) uses [UglifyJS 2](https://github.com/mishoo/UglifyJS2) for badass reduction of target JS files;
  * [`clean-css-brunch`](https://www.npmjs.com/package/clean-css-brunch) relies on [CleanCSS](https://github.com/jakubpawlowicz/clean-css), one of the best CSS minifiers out there (and if you want to play with the all-new more-css, feel free to contribute a plugin!).
  * Let's not forget [`csso-brunch`](https://www.npmjs.com/package/csso-brunch).
  * [`uncss-brunch`](https://www.npmjs.com/package/uncss-brunch) uses the awesome [UnCSS](https://github.com/giakki/uncss) to detect **unused code in our stylesheets**; if you want to combine that with clean-css, you can either use both separately, or go with the [`clean-css-uncss-brunch`](https://www.npmjs.com/package/clean-css-uncss-brunch) combo.

There are also a number of plugins designed to maintain a “fingerprint” on filenames, allowing for **far-expiry caching**, and to GZip your files for static gzipping (e.g. on [nginx](http://nginx.org/en/docs/http/ngx_http_gzip_static_module.html)):

  * [`digest-brunch`](https://www.npmjs.com/package/digest-brunch) computes the fingerprint based on the file's contents;
  * [`git-digest-brunch`](https://www.npmjs.com/package/git-digest-brunch) and [`hg-digest-brunch`](https://www.npmjs.com/package/hg-digest-brunch) use the current commit's SHA instead (which assumes you're committing in a manner consistent with that).
  * [`gzip-brunch`](https://www.npmjs.com/package/gzip-brunch) compresses your finalized CSS/JS assets, either as copies (preferred) or replacements of your original files.

If you’re using AppCache (and until we can all get our hands on `ServiceWorker`, you should!), there are a few useful plugins too:

  * [`appcache-brunch`](https://www.npmjs.com/package/appcache-brunch) maintains an **up-to-date manifest**, complete with the names of all files in the target folder, but also with a unique *digest*, so that **if a source file changes, so does the manifest!** Without that, invalidating the AppCache in dev quickly grows super tedious…
  * In the same spirit, [`brunch-signature`](https://www.npmjs.com/package/brunch-signature) computes *digest* from produced files and puts it in a static file of your choosing;  you could then, for instance, do a **low-frequency Ajax polling** from your app to detect it's changed and suggest a refresh.
  * Still along these lines, [`version-brunch`](https://www.npmjs.com/package/version-brunch) auto-maintains a version number for your app, based on the one in your `package.json` but with an extra build number tacked onto it.  It also puts it in a static file (that you can therefore poll), and auto-replaces specific version markers in all your produced files.

Finally, [`cloudfront-brunch`](https://www.npmjs.com/package/cloudfront-brunch) is one of the plugins capable of auto-uploading your **assets to an S3 bucket**, and send the proper invalidation request to CloudFront, to boot.  Sweet.

## Writing a Brunch plugin

So that's quite a few cool plugins, but I'm sure you're already thinking about the shiny new one you'd like to contribute, right?  Fear not, as I'm going to show you how to **write your own Brunch plugin**.

Brunch recognizes several plugin categories: compilers, linters, optimizers…  It detects that category based on which predefined methods you implement.  Depending on that category, you'll get called at various moments of the build cycle, and in specific environments, too.

The [online API docs](https://github.com/brunch/brunch/blob/stable/docs/plugins.md) is not too bad, and then of course you can browse the source code for existing plugins to see how *they* pull it off.  In order to get your feet wet, we'll make a compiler-type plugin—that'll apply regardless of file extensions, though.

Earlier in this chapter I mentioned the `git-digest-brunch` plugin, that scans the resulting files for `?DIGEST` markers and replaces them with your Git HEAD's SHA1: it's trying to invalidate asset URLs for cache-busting purposes.  This plugin is confined to production mode, too (more specifically, it requires the `optimize` setting to be enabled).

We'll write a variation on this: a plugin that replaces a free-form marker on the fly, both in one-shot build and watcher modes, regardless of the environment.  Our **functional spec** would read like this:

  * The **marker** defaults to `!GIT-SHA!`, but the part between exclamation marks can be **configured** through `plugins.gitSHA.marker`.
  * The transformation happens **any time, on the fly** (one-shot builds or watcher, production or not).
  * All watched files, regardless of their extension, are processed; the only exception is “pure static” files (those under an `assets` directory).
  * The marker, just like watched file paths, **can contain regex-special characters** without breaking anything.

Now that we have this nailed, where should we start?  A Brunch plugin is first and foremost **a Node module**, so let's begin by creating a `git-sha-plugin` folder and creating the following `package.json` in it:

```json
{
  "name": "git-sha-brunch",
  "version": "1.7.0",
  "private": true,
  "peerDependencies": {
    "brunch": "~1.7"
  }
}
```

The `peerDependencies` part is optional (it's even on its way to deprecation), but I like it…  On the other hand, it's an informal convention that Brunch plugins should track the major and minor version numbers of the Brunch they're compatible with.  So if you don't try for compatibility below Brunch 1.7, your plugin version should start at 1.7, for instance.

Because we didn't specify a `main` property in our package file, Node will assume that the module's entry point file is `index.js`.  We also know that a Brunch plugin is a constructor with a `prototype` equipped with several specific properties, that we mentioned earlier in this chapter:

  * `brunchPlugin` must be `true`;
  * `type`, `extension` or `pattern` can be used to filter down the files that should trigger processing;
  * `compile(…)`, `lint(…)` or `optimize(…)`, depending on what role your plugin has;
  * `onCompile(…)` if you want to be notified when the build is complete (even in watcher mode);
  * `teardown(…)` if you need to clean up when Brunch shuts down (e.g. stop an embedded server started by your constructor).

(Actually, `brunchPlugin` is the only property that **has to be on `prototype`**: all the other ones are used on the instance, so they could be defined dynamically by the constructor if need be, which in practice mostly happens for `pattern`).

So here's our skeleton `index.js` file:

```javascript
"use strict";

// Default marker.  Can be configured via `plugins.gitSHA.marker`.
var DEFAULT_MARKER = 'GIT-SHA';

function GitShaPlugin(config) {
  // 1. Build `pattern` from config

  // 2. Precompile the marker regexp from config
}

// Tell Brunch we are indeed a plugin for it
GitShaPlugin.prototype.brunchPlugin = true;

// On-the-fly compilation callback (file by file); assumes Brunch already
// cleared that file for our plugin by checking `type`, `extension` and
// `pattern`.
GitShaPlugin.prototype.compile = function processMarkers(params, callback) {
  // No transformation for now
  callback(null, params);
};

// Helper: escapes any regex-special character
function escapeRegex(str) {
  return String(str).replace(/[-[\]{}()*+?.,\\^$|#\s]/g, '\\$&');
}

// The plugin has to be the module's default export
module.exports = GitShaPlugin;
```

Alright!  Let's start with the constructor.  We don't mandate a specific file type (scripts, styles or templates), so we don't define a `type` property on our prototype, not a specific `extension` value.  That leaves us with `pattern`, which is a [regular expression](http://regexone.com/).

Because we are dependent on paths, not extensions, we need access to the configuration, so we can dynamically build our filters from it.  That makes for the following code at the beginning of the constructor:

```javascript
var pattern = config.paths.watched.map(escapeRegex).join('|');
pattern = '^(?:' + pattern + ')/(?!assets/).+';
this.pattern = new RegExp(pattern, 'i');
```

This way, the default watched paths (`['app', 'vendor', 'test']`) yield the following pattern: `/^(?:app|vendor|test)\/(?!assets\/).+/i`.

Now on to the marker.  The code to get it is a bit simpler:

```javascript
var marker = (config.plugins.gitSHA || {}).marker || DEFAULT_MARKER;
this.marker = new RegExp('!' + escapeRegex(marker) + '!', 'g');
```

We're certain that `config.plugins` exist, even if it's an empty object.  So its `gitSHA` property might be `undefined`, hence the `|| {}` to guarantee an object, even if empty.  We grab `marker` from it, again possibly `undefined`, which would result in `DEFAULT_MARKER`.  But if the settings's defined, we get that.

Then we compile the regex once and for all.

Now, eery time `compile(…)` is called (which means the file we get matched our `pattern`), we'll need to get the current Git HEAD's SHA1, and proceed to replacing it through the file's in-memory contents.

We don't get this SHA just once at construction time, because committing along througout the dev phase is a common scenario (without stopping Brunch's watcher, that is), so our value would quickly become obsolete.

We get this information by running a `git rev-parse --short HEAD` as a command line, which we'll do the Node way: asynchronously.  Therefore, we need the caller to supply a callback we'll call in due time, possibly with an error (like, you're not even in a Git repo, pal!).

Here's our small helper function:

```javascript
function getSHA(callback) {
  exec('git rev-parse --short HEAD', function(err, stdout) {
    callback(err, err ? null : stdout.trim());
  });
}
```

Finally, we write the processing proper:

```javascript
GitShaPlugin.prototype.compile = function processMarkers(params, callback) {
  var self = this;
  getSHA(function(err, sha) {
    if (!err) {
      params.data = params.data.replace(self.marker, sha);
    }
    callback(err, params);
  });
};
```

And *voilà!*

To **test our plugin without littering the npm registry**, we'll do what's called an `npm link`: the local installation of a module that is still under development.

If you grabbed the [sample repo](https://github.com/deliciousinsights/brunch-article-demos), we'll use two of its directories:

  * `6-templates`, the last phase where we didn't have a custom server, and
  * `8-git-sha-plugin`, that contains this demo plugin's code, all nicely commented.

Here's how to perform the link:

  1. Get inside `8-git-sha-plugin` from the command line;
  2. Run an `npm link`:  this will register the current folder as source for future `npm link git-sha-plugin` commands;
  3. Get inside `6-templates` from the command line;
  4. Run an `npm link git-sha-plugin`: this will sort of install it locally, linking to your source folder;
  5. Do add your new local module to `package.json` (`npm link` won't), and make sure you don't forget the extra comma this will require on the previous line, so you don't break the JSON:

```json
{
  "name": "simple-brunch",
  "version": "0.1.0",
  "private": true,
  "devDependencies": {
    "brunch": "^1.7.20",
    "jade-brunch": "^1.8.1",
    "javascript-brunch": "^1.7.1",
    "sass-brunch": "^1.8.9",
    "git-sha-brunch": "^1.7.0"
  }
}
```

If you don't list it in `package.json`, Brunch won't see it (it loops through `package.json`, not just the contents of `node_modules`).

If you hadn't grabbed this repo through a `git clone`, you're probably not in a Git repo just now.  If you do have Git available in your command line, here's how to get a HEAD quickly:

```sh
$ git init
Initialized empty Git repository in …/6-templates/.git/
$ git commit --allow-empty -m "Initial commit"
[master (root-commit) 8dfa8d9] Initial commit
```

(You'll get a different SHA, of course.)

OK, we're all set to try this out.  Here's a simple test scenario: open `app/application.js` (from this same tree) in your editor, and add a comment alone the lines of `// Version: !GIT-SHA!` in a couple spots.  Save.  Run the build.  Then check the contents of `public/app.js`: the SHA replaced the marker.  You can also try it as the watcher is running: this works too! ᕙ(⇀‸↼‶)ᕗ

----

« Previous: [Web server: built-in or custom](chapter10-web-server.md) • Next: [Conslusion](chapter12-conclusion.md) »
