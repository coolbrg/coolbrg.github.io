---
layout: post
title: Use of bootbox.js for creating multiple prompts sequentially
date: 2014-06-16 17:45:18.000000000 +05:30
technical: true
categories:
- bootboxjs
- javascript
- twitter-bootstrap
- underscorejs
tags:
- js-plugins
- partial
- twitter-bootstrap
type: post
author: BRG
thumbnail_path: blog/bootbox.png
---

[bootbox.js](http://bootboxjs.com) is a tiny and handy JavaScript library which allows one to create dialog boxes using [Twitter’s Bootstrap modals](http://getbootstrap.com/2.3.2/javascript.html#modals) programmaticallly.

##### Basic Example:
{% highlight text %}
  bootbox.prompt("What is your name?", function(result) {
    if (result === null) {
      // notify some error message
    } else {
      // do some stuff with result
    }
  });
{% endhighlight %}

Recently there is a requirement in my project which requires prompting user with few questions and based on user's response process further stuff.

My first choice was to use **bootbox.js's prompt** function and I went with it.

But then after some time I started to feel like **"Oops! I selected wrong library here."**. I don't know how to call another `bootbox.js` prompt in the callback of first one without any redundancy.

Moreover, I need to store the result of each prompt's response for later processing purpose.

Then, my team lead suggested me to use [Underscore's partial function](http://underscorejs.org/#partial) and gave me pseudo code.

Underscore's partial function basically defines a function named `_.partial` which accepts as a parameter a function and arbitrarily no. of arguments. The return value of  `_.partial` is a new function that when called will pass both its own parameters and the initially provided arguments to the original function.

##### Basic Example of `_.partial`:
{% highlight text %}
  var add = function(a, b) {
    return a + b;
  };

  var increment = _.partial(add, 1);
{% endhighlight %}

We can then call the *increment* function like this:

{% highlight text %}
  var six = increment(5);       // 6
{% endhighlight %}

For the `add` function in the above example, the parameter `a` is fixed to
the value `1`. The first argument that's passed to `increment` will be passed to `add`
as the `b` parameter.

The same technique I used in the implementation of invoking multiple bootbox.js prompts on
successive prompt's callback.

##### Implementation:

{% highlight text %}
  var survey_pair, survey_pairs, survey_answers = {};
  // Survey type : Question
  survey_pairs = {
      name: 'What is your name?',
      age: 'How old are you (in number only)?',
      color: 'What is your favourite color?',
      movie: 'What is your favourite movie?'
  };
  function displaySurveyResults(responses) {
      var survey_results_div = $('#survey_results');
      survey_results_div.html('');
      _.each(responses, function(value, key) {
          survey_results_div.append('<li><strong>' + key + '</strong>: ' + value + '</li>');
      });
  }
  function getFirstSurvey() {
      var first_key, first_prompt = {};
      first_key = _.first(_.keys(survey_pairs));
      if (first_key) {
          first_prompt[first_key] = survey_pairs[first_key];
          delete survey_pairs[first_key];
      }
      return first_prompt;
  }

  // main functions
  function showSurvey(survey, callback) {
      // Making survey's type as fixed argument to partial
      var new_callback = _.partial(callback, _.keys(survey)[0]);

      // Calling bootbox.prompt with survey's question and
      // partial callback which has survey's type already
      // and user response will be given through the new_callback
      bootbox.prompt(_.values(survey)[0], new_callback);
  }
  function surveyCallback(survey_type, survey_response) {
      survey_answers[survey_type] = survey_response;
      survey_pair = getFirstSurvey();
      if (survey_pair && !_.isEmpty(survey_pair)) {
          showSurvey(survey_pair, surveyCallback);
      } else {
          displaySurveyResults(survey_answers);
      }
  }
  // Get first question and initiate survey
  survey_pair = getFirstSurvey();
  if (survey_pair && !_.isEmpty(survey_pair)) {
      showSurvey(survey_pair, surveyCallback);
  } else {
      displaySurveyResults(survey_answers);
  }
{% endhighlight %}

Here in `showSurvey`, I am fixing first argument of `callback` which refer to `surveyCallback` passed from `showSurvey` (line no. 53).
The second argument i.e. `survey_response` of `surveyCallback`(or `callback` in `showSurvey`)
will refer to **bootbox.js prompt's result(or user's response)**.

Rest logic is simple. In `surveyCallback` I am setting survey's response in some variable
`survey_answers` here and then fetching next first survey and repeating same iteration.

### [Demo Link](http://jsbin.com/xayup/6)
