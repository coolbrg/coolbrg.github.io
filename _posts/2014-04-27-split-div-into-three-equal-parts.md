---
layout: post
title: Split div into three equal parts vertically
date: 2014-04-27 10:58:14.000000000 +05:30
categories:
- javascript
- jQuery
tags:
- js-plugins
- splitter
type: post
author: Budh Ram Gurung
thumbnail_path: blog/splitter/three_pane.jpg
---

There has been a requirement in my project where I have to split a particular `div` into three equal parts vertically. It must possess following properties:

* Each part should have a slider to resize its width
* One should be able to display two parts at a time or complete one part at a time
* The content within resizing part should also resize as according to slider movement

I googled for the possible available plugins.
Out of many I found [jQuery splitter](https://github.com/jcubic/jquery.splitter) plugin interesting and decided to go with it.

The [Plugin Demo](http://jquery.jcubic.pl/splitter.php) demonstrates **partially** what I need but I know I need to do some tweak by myself to make this plugin work exactly what I want.

### Following are steps I followed to achieve my goal:

- Setup plugin as :
{% gist 92151ccd4c0a96ec23b9 %}
**NOTE:** jQuery should be loaded first as jQuery splitter **depends on** jQuery.

You can get **jquery.splitter-0.14.0.js**(or latest version) and **jquery.splitter.css** in its [git repo](https://github.com/jcubic/jquery.splitter) under **js** and **css** folders respectively.

In **html** body, create **div's** structure as bellow:

{% gist 4580c0c10c262a95f958 %}

- Call `splitter` method as:
{% gist d1d8b8a6a6eca3036278 %}
Here, I am calling splitter method `split` 2nd time to split the `left_pane` div into two equal parts.

That's it. Three simple steps and you get the div splitted into approximately three equal parts.

- Optional:
You may want to apply **extra CSS** to slider bar you can do as:
{% gist fc15d00524603b9959f0 %}

{% include image.html
           img="/blog/splitter/three_pane.jpg"
           title="Three Pane"
           class="centered"
%}
