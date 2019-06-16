---
layout: post
title: Run AngularJS, Bootstrap and Angular UI bootstrap documentation locally
date: 2015-03-24 14:52:01.000000000 +05:30
categories:
- AngularJS
- javascript
- Twitter Bootstrap
tags:
- angularjs
- docs
- jekyll
- locally
type: post
author: Budh Ram Gurung
thumbnail_path: blog/docs/docs.png
---

It is always better to run the documentation of library locally as you won't be dependent on Internet for any documentation help. Also, navigation is fast due to local hosting.

Recently, I got chance to work with [AngularJS](http://angularjs.org) in one of my project.

I chose my favorite CSS framework [Twitter Bootstrap](http://getbootstrap.com) and also came to know about [Angular UI Boostrap](https://angular-ui.github.io/bootstrap/) which is a set of bootstrap components in AngularJS.

I googled a lot on

> How to run documentation of above libraries locally?

I found few but mostly outdated.

Hence, the inspiration of this blog to **summarize** all in one page.

### Run AngularJS documentation locally
1. Go to [code.angularjs.org](https://code.angularjs.org/) and select your version and download the **zip** version.
**Eg:** For version `1.3.14`, go to `https://code.angularjs.org/1.3.14/` and download `angular-1.3.14.zip`(~9MB)

1. Extract the zip file. Above will be extracted into `angular-1.3.14`.

1. Through terminal, go to above folder `cd angular-1.3.14`
1. Start the server as `python -m SimpleHTTPServer`
1. Hit url [http://localhost:8000/docs](http://localhost:8000/docs)

> For Bootstrap and Angular UI Boostrap, you need [Jekyll](http://jekyllrb.com/docs/installation/).Install Jekyll as `gem install jekyll`(Need Ruby, RubyGems installed)

### Run Bootstrap documentation locally:
1. Clone the *gh-pages* branch repo : `git clone --branch gh-pages git://github.com/twitter/bootstrap.git`
1. Go to *bootstrap* directory : `cd bootstrap`
1. Serve using jekyll: `jekyll serve -P 8001`
1. Hit url [http://localhost:8001/](http://localhost:8001/)

### Run AngularUI Bootstrap documentation locally:
1. Clone the *gh-pages* branch repo: `git clone --branch gh-pages git://github.com/angular-ui/bootstrap.git angular-ui-bootstrap`
1. Go to *angular-ui-bootstrap* directory : `cd angular-ui-bootstrap`
1. Serve using *jekyll* : `jekyll serve -P 8002`
1. Hit url [http://localhost:8002/](http://localhost:8002/)

And Enjoy!!!!

{% include image.html
           img="/blog/docs/docs.png"
           title="Docs"
           class="centered"
%}
