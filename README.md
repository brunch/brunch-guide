# The Brunch.io Guide

This is a attempt at a comprehensive guide to [Brunch](http://brunch.io/), an excellent builder for browser apps that gives Grunt, Gulp, Broccoli et al. a run for their money.  I adapted this from [my (French language) article from early March 2015](http://www.js-attitude.fr/2015/03/04/brunch-mon-builder-prefere/).

## Table of Contents

1. [Brunch?! What’s Brunch?](chapter1-whats-brunch.md)
  * [Brunch vs. others](chapter1-whats-brunch.md#brunch-vs-others)
  * [Task runners vs. builders](chapter1-whats-brunch.md#task-runners-vs-builders)
  * [File-based processing vs. pipelines](chapter1-whats-brunch.md#file-based-processing-vs-pipelines)
  * [Configuration and boilerplate vs. conventions](chapter1-whats-brunch.md#configuration-and-boilerplate-vs-conventions)
  * [Full builds vs. incremental builds](chapter1-whats-brunch.md#full-builds-vs-incremental-builds)
  * [The paramount importance of speed](chapter1-whats-brunch.md#the-paramount-importance-of-speed)
  * [Then why do I only ever hear about the others?](chapter1-whats-brunch.md#then-why-do-i-only-ever-hear-about-the-others)
2. [Getting started with Brunch](chapter2-getting-started.md)
  * [Should I use a skeleton?](chapter2-getting-started.md#should-i-use-a-skeleton)
3. Conventions and defaults
  * Built-in processing
  * Configuration files
  * Folders
  * CommonJS module wrapping
  * Sourcemaps
  * Watcher
  * Built-in web server
  * Plugin loading
4. Starting from scratch
5. Using third-party module registries
6. A shot at templating
7. Using Brunch on a legacy codebase
8. Production builds
9. Watcher
10. Web server: built-in or custom
11. Plugins for all your build needs
  * Enabling a plugin
  * Fine-tuning through optional configuration
  * Brunch and CSS
  * Brunch and JavaScript
  * Brunch and templates
  * Brunch and development workflows
  * Brunch and web performance
  * Writing a Brunch plugin
12. Conclusion

## Brunch version

This guide was written against Brunch 1.7.20.  Most of it likely works in earlier 1.7.x versions, but may need updates when the upcoming 1.8 or 2.0 gets out (especially WRT npm / `node_modules` integration).

## License

This work is © 2015 Christophe Porteneuve, licensed under [the MIT license](LICENSE).

## Contributing

I welcome all useful contributions: typos, bug fixes, rephrasings, better explanations or examples, extra information and demos, etc.

Just fork this repo, push a clean history of your changes to your fork, and submit a well-formed Pull Request!

## Acknowledgments

The [Brunch team](https://github.com/orgs/brunch/people) deserves enormous applause and thanks for their amazing work on this tool.  I write this as Brunch turns 4 already, and it's made my life easier (and that of hundreds of my JS trainees) for all that time!  You guys rule!
