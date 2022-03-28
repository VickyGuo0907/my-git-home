# My GitHub Home Page

This is My GitHub Home page.

## Local tests/development

First, install Jekyll on terminal:

```shell
bundle install
```

Then, execute:

```shell
bundle exec jekyll serve
```

### Put in a Jalpc Plug

If you want to give credit to the Jalpc theme with a link to my personal website <http://www.jarrekk.com>, that'd be awesome. No worries if you don't.

### Upgrading Jalpc

Jalpc is always being improved by its users, so sometimes one may need to upgrade.

### Ensure there's an upstream remote

If `git remote -v` doesn't have an upstream listed, you can do the following to add it:

```
git remote add upstream https://github.com/jarrekk/Jalpc.git
```

### Pull in the latest changes

```
git pull upstream gh-pages
```

There may be merge conflicts, so be sure to fix the files that git lists if they occur. That's it!

## Testing Locally
To test your site locally, you’ll need

- [ruby](https://www.ruby-lang.org/en/)
- the [github-pages](https://github.com/github/pages-gem) gem

### Installing ruby
There are [lots of different ways to install ruby](https://www.ruby-lang.org/en/documentation/installation/).

In Mac OS X, older versions of ruby will already be installed. But I use the [Ruby Version Manager (RVM)](https://rvm.io/) to have a more recent version. You could also use [Homebrew](https://brew.sh/).

In Windows, use [RubyInstaller](https://rubyinstaller.org/). (In most of this tutorial, I’ve assumed you’re using a Mac or some flavor of Unix. It’s possible that none of this was usable for Windows folks. Sorry!)

### Installing the github-pages gem
Run the following command:

```
gem install github-pages
```

This will install the github-pages gem and all dependencies (including [jekyll](https://jekyllrb.com/)).

### Later, to update the gem, type:

```
gem update github-pages
```

Testing your site locally
To construct and test your site locally, go into the directory and type

```
jekyll build
```

This will create (or modify) a `_site/ directory`, containing everything from `assets/`, and then the `index.md` and all `pages/*.md` files, converted to html. (So there’ll be `_site/index.html` and the various `_site/pages/*.html.`)

Type the following in order to “serve” the site. This will first run build, and so it does not need to be preceded by `jekyll build`.

```
jekyll serve
```

Now open your browser and go to `http://localhost:4000/site-name/`


