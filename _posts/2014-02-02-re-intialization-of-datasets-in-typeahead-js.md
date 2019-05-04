---
layout: post
title: Re-initialization of datasets in typeahead.js
date: 2014-02-02 16:38:52.000000000 +05:30
categories:
- javascript
tags:
- autocomplete
- js-gotchas
- js-plugins
- twitter-bootstrap
- typeaheadjs
type: post
author: Budh Ram Gurung
thumbnail_path: blog/typeaheadjs.png
---

[typeahead.js](https://github.com/twitter/typeahead.js) is one of autocomplete search plugin inspired by [twitter.com](http://twitter.com).

I have been using it quite from a while in my projects and had wonderful experience so far until I encountered with one requirement.
*Requirement:* Remove the selected data from dataset and don't show it in next autosearch suggestions.
I used [`$('.typeahead').typeahead('destroy');`](https://github.com/twitter/typeahead.js/blob/master/doc/jquery_typeahead.md#jquerytypeaheaddestroy) as suggested but it didn't work as expected.
It reverts the **input** element back to its original state but again adding typeahead feature to this same input with new **dataSets** doesn't remove the auto-complete suggestions with new dataSets.
While googling for few hours for the ideal solution I found one good solution out of the discussion made in [this github issue thread.](https://github.com/twitter/typeahead.js/issues/41?source=cc#issuecomment-16120411)

So following is solution I used and it works like charm!!! :smile:

### Steps I followed for the solution:

- Define a function to enable typeahead functionality as:
{% highlight text %}
  function enable_auto_complete(name, dataSets){
    $('#typeahead_input_element').typeahead({
      name: name,
      local: dataSets
    });
  }
{% endhighlight %}

- Initialize the input with **typeahead** for first time with some name.

- Remove the selected name from global 'dataSets'.

- Destroy typeahead feature as:

{% highlight text %}
  $('#typeahead_input_element').typeahead('destroy')
{% endhighlight %}

Initialize the input again with typeahead but with different name and updated dataSets.
Working [**JSFiddle Link**](http://jsfiddle.net/budhrg/42ESM/).
