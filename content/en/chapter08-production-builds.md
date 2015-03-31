# Production builds

This is part of [The Brunch.io Guide](../../README.md).

By default, Brunch runs in a **development** environment, or mode.  This mostly means minification plugins (be it JS or CSS) don’t run (because the `optimize` setting defaults to `false` in development mode; which you can change, if you really want to).

You specify what environment to run in through the `-e` or `--env` CLI option, followed by the environment’s name.  You can have any environments you want, with a special case for `production`, that is predefined with `optimize` set to `true`, and has its own CLI shortcut `-P` (or `--production`).

More importantly, you can **alter settings based on the environment**, through the root settings key `overrides`.  In it, you can have one key per environment, each with replacements (overrides…) for any general settings you may have.  The official docs use an example that is not too shabby:

```coffeescript
overrides:
  production:
    optimize: true
    sourceMaps: false
    plugins: autoReload: enabled: false
```

In this situation, production enables minifiers, skips source maps (I would keep them, but hey), and disables the `auto-reload` (LiveReload-style) plugin when in watcher mode.

----

« Previous: [Using Brunch on a legacy codebase](chapter07-using-brunch-on-legacy-code.md) • Next: [Watcher](chapter09-watcher.md) »
