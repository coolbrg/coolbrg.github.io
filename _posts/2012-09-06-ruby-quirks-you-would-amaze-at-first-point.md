---
layout: post
title: Ruby Quirks (You would amaze at first point)
author: Budh Ram Gurung
date: 2012-09-06 21:52:42.000000000 +05:30
categories:
- ruby
tags:
- ruby
- quirks
thumbnail_path: blog/quirks.png
---

Right now I am at the stage of learning. While studying Ruby I am finding (*still continuing*) that language looks quite simple but has its own complex and hidden secrets.

So, I am writing my experience of unexpected or peculiar behavior (Quirks) I have observed or will be going in future in this post.

1) Why following statement results into 'nil' ?

{% highlight text %}
  expand = defined?( expand ) ? expand : true
{% endhighlight %}

Assume that expand encountered here for the first time.

2) In the following Ruby code, x gets the value nil and pqr remains an undefined local variable. Why?

{% highlight text %}
  if false
    x = pqr
  end

  puts x
  puts pqr
{% endhighlight %}

3) What is happening in following code and why, how?

{% highlight text %}
  x = 5
  y = 10
  x, y = y, x
  puts x
  puts y
{% endhighlight %}

4) Why expression `1 == 1.0` evaluates to true ?
Check following irb session:

{% highlight text %}
  1.class   #output Fixnum
  1.0.class #output Float
  1 == 1.0  #output true
{% endhighlight %}

5) In case of multiple values at right hand side(RHS) of assignment, the rules of parallel assignment come into play.

First, all the RHS values evaluated, left to right, and collected into an array (unless they are already an array). This array will be the eventual value returned by the overall assignment. Next, the left hand side (LHS) is inspected. If it contains a single element, the array is assigned to that element otherwise values get corresponding assignment.

{% highlight text %}
  a = 1, 2, 3, 4             # => a == [1, 2, 3, 4]
  b = [1, 2, 3, 4]           # => b == [1, 2, 3, 4]
  c,d = 1,2,3,4              # => c = 1, d = 2
{% endhighlight %}
