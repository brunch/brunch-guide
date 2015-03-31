# Getting started with Brunch

This is part of [The Brunch.io Guide](README.md).

Let’s start exploring everything you need to know to happily use Brunch in your own projects, be they new ones or legacy codebases.

Just like other tools in this space (and like a truckload of other tools these days), Brunch is **based on Node and gets installed through `npm`**.  You can install it globally, so as to be able to use the `brunch` command from anywhere:

```sh
npm install -g brunch   # You should install it locally instead…
```

…but I would **advise you to install it locally instead**, within your current project, that will need a `package.json` file anyway (just like with other competing tools).  This way, you can use various versions of Brunch from one project to another, **with no conflicts**.  We’ll see that in more detail in later sections of this guide.

## Should I use a skeleton?

Brunch emphasizes the concept of **skeleton**.  Unlike generators like those based on Yeoman, these are just **Git repos suggesting a default infrastructure** for a front-end app using Brunch in development.  There are [a number of existing skeletons](http://brunch.io/skeletons.html), and their main usefuleness lies in providing a **kickstarter** by creating a few folders, pre-requiring dependencies in `package.json` and providing a default Brunch configuration.

Brunch has a `brunch new` command that you pass a GitHub repo to (or any Git repo URL, for that matter), and an optional clone path.  This is really just a `git clone` followed by an `npm install`…

I would advise you to **start from scratch (or from your own codebase)** and then define your own folders and configuration, to get a firm grip on what you’re doing.

And yes, this was a tiny chapter.  Not so with the next ones!

----

« Previous: [Brunch?!  What’s Brunch?](chapter01-whats-brunch.md) • Next: [Conventions and Defaults](chapter03-conventions-and-defaults.md) »
