---
layout: post
title: My First Post
date: 2021-02-16 14:31:00 +0100
---

Well, this is my first post on GitHub Pages. These are the steps
that I used to get this running. I followed a bit the steps created
by [Anindya Chatterjee](http://www.abstractclass.org/tutorial/blog/2015/05/19/tutorial-personal-blog-with-github.html).

For the examples below I am using my OSX laptop.

First install Ruby using home brew:

```(bash)
$ brew install ruby
```

And add the Ruby PATH to your `.zshrc` file:

```
export PATH="/usr/local/opt/ruby/bin:$PATH"
```

Reload the `.zshrc` to add the settings:

```
$ source ~/.zshrc
```

Next we need to install Jekyll and some dependencies:

```
$ gem install --user-install bundler jekyll
$ gem install --user-install bundler jekyll-seo-tag
$ gem install --user-install bundler jekyll-paginate
$ gem install --user-install bundler webrick
```

And add the PATH to the binaries to the `.zshrc` file. In my case this
was:

```
export PATH="${HOME}/.gem/ruby/3.0.0/bin:$PATH"
```

Reload the `.zshrc` to add the settings:

```
$ source ~/.zshrc
```

Now we can start creating my own site. I went over to
[http://jekyllthemes.org/](http://jekyllthemes.org/)
and choose a nice layout. In my case this was the layout of
[https://jekyll-themes.com/hagura/](https://jekyll-themes.com/hagura/).

To install a new blog on my computer I went to my `~/Dev` directory
and followed the steps Anindya Chatterjee provided me:

```
$ cd Dev
$ git clone git@github.com:sharu725/hagura.git phochste.github.io
$ cd phochste.github.io
$ git remote set-url origin https://github.com/phochste/phochste.github.io.git
```

I modified the `_config.yml` a bit to suit my needs:

* Changed `title` to my name
* Set `baseurl` to an empty string
* Commented out the `analytics` and `disqus-shortname`

Next I started a local version of my blog on `http://localhost:4000/` with the command:

```
$ jekyll serve
```

Next I needed to get this all up to GitHub. First I created over there
a new repository named `phochste.github.io`. And on my laptop I run
these commands:

```
$ git add --all .
$ git commit -m "Config changes"
$ git branch -M main
$ git push -u origin main
```

And there it is: my blog at [https://phochste.github.io](https://phochste.github.io).
