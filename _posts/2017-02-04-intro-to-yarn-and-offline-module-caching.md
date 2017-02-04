---
layout: post
title: Introduction to Yarn & Offline Node Module Caching
date: 2017-02-04
tags: javascript, yarn, packages, node, node modules, modules, yarn init, yarn install, yarn add, yarn offline, offline modules, what is yarn, yarn tutorial, yarn intro, yarn add
---

You've probably heard about the new _hawtness_ that is Yarn. These are both pretty cool tools that allow Javascript developers to get their work done correctly. In this tutorial, you'll be introduced to the Yarn package manager and how to offline cache yuor commonly/favourite node libraries. This tutorial expects that you have experience using the **bash/\*nix** command line and have a basic familiarity with *git*.

#### Goals

* Learn what Yarn is and why it is useful
* Understand what a lockfile is
* Create a node project using Yarn
* Store your commonly used Javascript libraries for offline use
* Test offline caching

## What is Yarn?

Yarn is a Javascript **package manager** similar to _npm_. It allows you to create a new project, add dependencies, update your dependencies, and more. If you've used npm you may wonder why this is a big deal. Yarn can store common node modules offline so you do not need to re-download them over and over again. These can then be centralized in a single repository. Yarn uses _lockfiles_ to save **specific** versions of packages, similar to Ruby's `Gemfile.lock`. A lockfile _locks in_ a specific version of a node module to be used in a project. This means that if I share a project with you and you install packages, you'll get the _exact same_ versions that I have (which ultimately leads to no headaches due to version differences). The lockfiles are

> _Fun Fact_: Yarn's [website](https://yarnpkg.com/) art was designed by [Shitty Watercolour](http://shittywatercolour.com/).

In the first part of this tutorial, you are going to create a project that stores your favourite/commonly used node modules. You'll then upload this project to a Git repository that you can use locally and share with teammates online. Finally, you'll learn Yarn as you follow along!

#### Creating an Offline Cache for Node Modules

Let's get started by installing Yarn on your system. Mac users: `brew install yarn`; Linux users: `apt install yarn` or `yum install yarn`.

Now, change to your home directory in your terminal: `cd` or `cd ~`. Inside, `mkdir yarn-offline-cache`. This is where Yarn will save node modules for future use. Then, run `yarn config set yarn-offline-mirror ./yarn-offline-cache`. This is where the `*.tar.gz` files containing your core Javascript libraries will be saved.

![Yarn Offline Cache in Terminal](img-yarn-offline.png)

Inside of your home (`~`) directory, a `.yarnrc` will be created. Inside, it looks like the following:

![.yarnrc](img-yarn-yarnrc.png)

For **every project that you want to use the offline cache in**, you _must_ have a copy of this `.yarnrc` in your directory. I personally add it to boilerplate projects.

#### Your First Yarn Project

Now, create a directory called `yarn-for-cats` or `yarn-boilerplate`. Change into it and we'll get started! First, run `yarn init`. This will create a new Javascript project using Yarn. You'll be asked a few questions to initialize your project.

![Yarn Init](img-yarn-init.png)

> _Note_: You can name your project anything you want; these options are up to you!

Once this is complete, a `package.json` will be created (similar to the `npm init` process). It will look like this:

![Yarn Init](img-yarn-packagejson.png)

Now, add your favourite dependencies using yarn. To do so, I'll demonstrate how I add a few things I depend on in my projects. You should add these and/or add your own favourites.

`yarn add webpack`

The syntax basically starts 'Hey yarn, add webpack as a dependency of my project'. You can also add multiple projects at once.

`yarn add webpack-dev-server babel-core babel-loader babel-preset-es2015`

This will install Webpack, the Webpack development server, Babel, the Webpack loader for Babel, and ES2015 support for Babel. You could also add React, Angular, Ember, Aurelia, Backbone, jQuery, ... One of the cool things about yarn is that it will install dependencies asynchronously, unlike npm.

Here are a few commonly used Yarn commands:

* Starting a new project: `yarn init`
* Adding a dependency: `yarn add [package]`, `yarn add [package]@[version]`, or `yarn add [package]@[tag]`
* Upgrading a dependency: `yarn upgrade [package]`, `yarn upgrade [package]@[version]`, or `yarn upgrade [package]@[tag]`
* Removing a dependency: `yarn remove [package]`
* Installing all the dependencies of project: `yarn` or `yarn install`

> Yarn has a [usage guide for commands](https://yarnpkg.com/docs/usage). Check it out and bookmark it for future help!

Now, remember the `.yarnrc` in our home folder from earlier? Let's add a copy of it to our home directory. Change back into your home `~` directory and `cp` it to your project directory.

Next, you should initialize a Git project in this repository. You can do so by running `git init` in your boilerplate's directory. Then, run `git status` to see what is in your project.

![Git Status](img-yarn-git-init.png)

> You can view what this [project looks like](https://github.com/code-for-coffee/yarn-boilerplate) on Github.

Now, let's install our packages in our project. This may seem redundant but by doing so, we'll install our packages into our offline cache folder, too! Let's do that now.

`yarn install`

This command will install all of your packages. If you clone down a javascript project with a `package.json`, Yarn will take care of installing all of the dependencies for you. 

Navigate back to your `~/yarn-offline-cache` directory. Run `ls` or `tree` to check out what is in your directory! You'll notice that every dependency that your project relies on is stored in your offline cache project. 

![Offline Cache](img-yarn-offline-cache.png)

> You can view my personal [Yarn offline cache's repository](https://github.com/code-for-coffee/yarn-offline-cache) on Github.

#### Testing Offline Caching

Now, if your internet ever goes down you'll still be able to install packages in other projects so long as the `.yarnrc` is in your project's folder! Isn't that awesome?! You can also share your `.yarnrc` and offline repository with teammates so that everyone is guaranteed to be on the same versions of each library (which helps resolve versioning conflicts). Awesome, eh? 

If you'd like to test this, disconnect your internet connection and create a new project. Add your `.yarnrc` over to the project.

![Yarn Offline](img-yarn-offline-init.png)

Next, `yarn add` a library in your cache. What are the results? I'll `yarn add react --offline` because I added it earlier to my offline cache. It works! Huzzah!

![Yarn Offline Test](img-yarn-offline-test.png)

#### Conclusion

In conclusion, you've learned what Yarn is - a Javascript package manager. You created a node project using Yarn, stored your favourite libraries for offline use, and verified that this actually works when offline. The next time you find yourself without internet in an airport, waiting on a train, or if your ISP goes down, you can still get work done. Even better - you can share your `.yarnrc` and offline repositories with co-workers and teammates for other projects to keep your library versions under control. Be sure to check out the [Yarn Getting Started Guide](https://yarnpkg.com/docs/getting-started) if you haven't already. 

