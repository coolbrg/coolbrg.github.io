---
layout: post
title: Shrinking issue in jQuery Splitter on screen resize
date: 2014-04-28 05:04:09.000000000 +05:30
categories:
- javascript
- jQuery
tags:
- issue
- js-plugins
- splitter
- workaround
type: post
author: BRG
thumbnail_path: blog/splitter/minimized.png
---

After using jQuery Splitter, everything was working as according to my requirement.

But as *you never get your destination smoothly* I also got stuck by one issue.

The issue was when I **minimize** the screen the three vertical panes doesn't resize as proportion to their width ratio as shown below:

{% include image.html
           img="/blog/splitter/minimized.png"
           title="Right Pane shrinked"
           class="centered"
%}

It looks like limitation of the [jQuery Splitter](https://github.com/jcubic/jquery.splitter) plugin. Same issue found in [plugin demo url](http://jquery.jcubic.pl/splitter.php).

To solve this issue, I need to do following workaround.

> I had to handled **window resize** event, calculate and apply each **div's width** and slider **left** attribute.


This really helped me to meet my requirement properly.

{% highlight text %}
  function resizeClassView() {
    var main_div_width, left_pane_width, right_pane_width,
        first_left_pane_width, second_left_pane_width, left_splitter, right_splitter;

    main_div_width = $(window).width() - 72;
    left_pane_width = (0.67 * main_div_width);
    right_pane_width = main_div_width - left_pane_width;
    first_left_pane_width = (0.5 * left_pane_width);
    second_left_pane_width = (left_pane_width - first_left_pane_width);
    left_splitter = $($('.vsplitter')[0]);
    right_splitter = $($('.vsplitter')[1]);

    $('#main_div').width(main_div_width + 'px');
    $('#left_pane').width(left_pane_width - 2 + 'px');
    $('#first_left_pane').width(first_left_pane_width - 5 + 'px');
    $('#second_left_pane').width(second_left_pane_width + 'px');
    $('#right_pane').width(right_pane_width + 'px');

    left_splitter.css('left', first_left_pane_width - 7 + 'px');
    right_splitter.css('left', left_pane_width - 5 + 'px');
  }

  $(document).ready(function() {
    var main_div_width = '100%';
    $('#main_div').width(main_div_width).height(500).split({
      orientation: 'vertical',
      limit: 3,
      position: '67%'
    });
    $('#left_pane').width($('#left_pane').width()).height(500).split({
      orientation: 'vertical',
      limit: 3,
      position: '50%'
    });
    // handling widow resize event
    $(window).resize(function() {
      resizeClassView();
    });
  });
{% endhighlight %}

