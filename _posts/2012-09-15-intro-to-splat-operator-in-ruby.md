---
layout: post
author: BRG
title: "Intro to Splat (*) operator in Ruby"
date: 2012-09-15 16:52:33.000000000 +05:30
categories:
- ruby
tags:
- datatype
- coercion
- hash
- array
- operator
- range
- ruby
- splat
thumbnail_path: blog/splat.png
---

I usually use following to create a bunch of array elements in Ruby.

{% highlight text %}
  arr = (10..50).to_a     # converting a range to an array.
{% endhighlight %}

Today I noticed following which performs same operation:

{% highlight text %}
  arr = [*10..50]        # splat operator
{% endhighlight %}

My reaction was "Awesome!!!!" :astonished:.

So, then, one day I took some time to investigate more on this (*) operator.

### Following are my findings.

- Generally used in "Method definition with **variable no. of parameter**".

{% highlight text %}
  def demo(*numbers)
    numbers.each { |num| puts "#{num}" }
  end
  demo(1,2,3,4)

  #output
  1
  2
  3
  4
{% endhighlight %}

- Converting an **array into list of arguments** in method calling.

In this case, the splat converts the array into method arguments.

{% highlight text %}
  def demo(arg1, arg2, arg3)
    puts "arg1 is #{arg1}, arg2 is #{arg2} and arg3 is #{arg3}"
  end
  arr = [10, 20.45, "hello"]
  demo *arr

  #output
  arg1 is 10, arg2 is 20.45 and arg3 is hello
{% endhighlight %}

- Use in `case` statement :

{% highlight text %}
  male = ["ram", "rahul", "karan"]
  female = ["kareena", "aish", "juhi", "katerina"]
  person = "aish"

  case person
  when *male
    puts "Male"
  when *female
    puts "Female"
  end

  #output
  Female
{% endhighlight %}

- Interesting Array data retrieval:

{% highlight text %}
  arr = ["one", "two", "three", "four"]
  first, *others = arr         #first = "one", others = ["two", "three", "four"]
  *others, last = arr          #others = ["one", "two", "three"] , last = "four"
  first, *center, last = arr   #first = "one", center = ["two", "three"], last = "four"
{% endhighlight %}

- **Datatype Coercion** : Splat operator can be used to convert interesting datatype coercion.

a) String into Array : Splat can also be used to coerce string values into array.
{% highlight text %}
  arr = *"Hello"       # ["Hello"]
  "Hello".to_a         # NoMethodError: undefined method 'to_a' for "Hello":String
{% endhighlight %}
**Note** : This will only create array of size 1.

b) Range into Array :
{% highlight text %}
  arr = *(10..20)     # [10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]
{% endhighlight %}
Above is similar behavior as `arr = (10..20).to_a`

c) Hash into an Array:
{% highlight text %}
  arr = *{ :a => "111", :b => "222" }    # [[:a, "111"], [:b, "222"]]
{% endhighlight %}
**Note** : Use `Hash[*arr.flatten]` to reverse it.
