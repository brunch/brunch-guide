# Using Brunch on a legacy codebase

This is part of [The Brunch.io Guide](../../README.md).

So far we've started from scratch, letting us lay out our codebase in a manner that stuck to Brunch's conventions.  What if we already have a codebase, and wish to entrust our front assets building to Brunch?  Perhaps you come from Grunt, Gulp whatever…  There are a number of questions you need to ask yourself:

  1. **Where are the source files** for the build?
  2. What **languages** do they use?
  3. In what **target folder** does the build go?
  4. What are the source &#2192; target **mappings** for the build?
  5. Do I want to wrap my application JS in **modules**?

Item 1 determines how you'll set `paths.watched`, to describe the base path(s) you'll be building from / watching.  The default value is `['app', 'test', 'vendor']`, but you'll likely want to change that.  Another setting is `conventions.assets`, that'll define what folders get copy-pasted as-is, and `conventions.vendor`, that specifies what directories contain JS not to be wrapped in modules, if any (be careful if you go with Bower, these components are never wrapped).

Item 2 states with **Brunch plugins** you'll need to use; it's absolutely possible to mix-and-match multiple options for a given type of source: for instance, when I use Bootstrap, I like to go with its SASS source, to make it easier for me to tweak and theme it, usually through `_variables.scss`.  But for my own styling, I favor Stylus, so I often have both `sass-brunch` and `stylus-brunch` installed.

Item 3 impacts the `paths.public` setting, that defaults to `'public'`.  This path doesn't need to exist prior to your build.  Target file paths are relative to this path.

Item 4 drives the structure of your `files` setting, that has up to three sub-sections:

  * `javascripts`: everything that ends up being JS, except pre-compiled templates;
  * `stylesheets`: everything that ends up being CSS;
  * `templates`: every pre-compiled template (again, templates pre-compile to single functions that take a *presenter*—or *view model*—object as argument and return HTML).  Most often, the target is the same as the core JS target.

Each of these keys can range from super-simple to quite advanced.  If the value is a single `String`, it's a unique file all concatenations for that scope merge into.  If it's an object, keys are the target files, and values define the sources for a specifi target.  These values are *[anymatch sets](https://github.com/es128/anymatch#anymatch-)*, which means they can be:

  * A simple **`String`**, which will have to match the exact file path that Brunch sees (more on that in a bit);
  * A **regular expression** that will have to match the file path; very useful for path prefixes, such as `/^app\//` or `/^vendor\//`;
  * A **predicate function** that takes the exact file path as argument and synchronously returns a boolean stating whether to include it or not in the concatenation;
  * An **array** mixing any of the previous items.

This is an **amazingly versatile** way to define your split targets (or a unique target that still filters incoming sources).

Item 5 shouldn't be an actual question: **of course you want to use modules**.  This is 2015, for crying out loud, get out of the Cretacean already, people!  And you should pick CommonJS for now, too (this makes both isomorphic JS and later migration to ES6B modules easier).

Still, you tweak this through the `modules.wrapper` and `modules.definition` settings.  Should you decide to remain in the swamp, you can set both to `false`.  If you still believe in AMD (or that the Earth is flat), you can even set them to `"amd"`.  Or you can go all-exotic and provide custom functions for both.  By default, they are set to `"commonjs"`.

## What’s in a (module) name?

One last important topic.  If you elect to go with modules (well done you!), you should consider the **module names**.  By default, what Brunch does is:

  1. Take your file's exact path, starting from the watched directory (e.g. `"app/application.js"`);
  2. Strip the extension (e.g. `"app/application"`);
  3. If you only have one watched path that is subject to module wrapping (by default there's only `"app"`), it strips that prefix (e.g. `"application"`).  But if you have multiple watched paths that use wrapping, the prefix stays.

If that doesn't cut it for you, you can provide a custom name-computing function, through the `modules.nameCleaner` setting.

For instance, in one of my JS training classes, I have this situation where I wrap third-party libraries in modules but want to group them inside my `app/externals` directory, and also retain version and language info in their filenames (so I have, say, `jquery-1.11.2-min.js` and `moment-2.2.1-fr.js`), but **I still want generic module names** (e.g. `"jquery"` and `"moment"`).  So I put in the following code in `brunch-config.coffee`:

```coffeescript
modules:
  nameCleaner: (path) ->
    path
      # Strip app/ and app/externals/ prefixes
      .replace /^app\/(?:externals\/)?/, ''
      # Allow -x.y[.z…] version suffixes in mantisses
      .replace /-\d+(?:\.\d+)+/, ''
      # Allow -fr lang suffixes in mantisses
      .replace '-fr.', '.'
```

No biggie.

----

« Previous: [A shot at templating](chapter06-a-shot-at-templating.md) • Next: [Production builds](chapter08-production-builds.md) »
