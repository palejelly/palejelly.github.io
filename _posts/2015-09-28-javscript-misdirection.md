---
layout: post
title: Spotting Malicious Code Hidden in Plain Sight
comments: true
image: https://jonathanmann.github.io/public/img/evilpad.png
excerpt: Recently, Peter Jaric hosted a Javascript misdirection contest where contestants attempted to write elegant code to surreptitiously pass a generated key to a remote server. The trick of the contest, however, was to hide the malicious code in plain sight. 
---

Recently, [Peter Jaric](https://twitter.com/peterjaric/) hosted a [Javascript misdirection contest](http://misdirect.ion.land/) where contestants attempted to write elegant code to surreptitiously pass a generated key to a remote server. The trick of the contest, however, was to hide the malicious code in plain sight. 

![evilpad](https://jonathanmann.github.io/public/img/evilpad.png)

I decided that it would be good practice for me to try to get inside the head of a malicious coder, so that I could get better at watching out for it since I frequently experiment with obscure open source projects by authors of unknown repute.

### Contest Entry

For [my entry](http://jsfiddle.net/1b41dehs/0/), I decided to try to disguise assignments within the evaluation portions of if statements as comparisons. 

For example, 

{% highlight js %}
if (isvalid = validate(key,1))
{% endhighlight %}

should actually be

{% highlight js %}
if (isvalid == validate(key,1))
{% endhighlight %}

Simple, but easily overlooked, and a common enough error to pass off as an "honest mistake". 

Because, in Javascript, assignments evaluate as true, in the code below, the index gets set to "2" at the first evaluation, and, because the first if statement is true, the interpreter skips the rest of the conditions, so the function always returns the key regardless of the original index input.

{% highlight js %}
var validate = function(key,index){
    var isvalid,debug_statement;
    if (index = 2){
        debug_statement = "test for key";
    } else if (index = 1){
        debug_statement = "test for validation";
    } else if (index = 0){
        debug_statement = "test for debug_statement";
    } else {
        index=0; debug_statement = "invalid index";
    }
    var validation = new RegExp("(\\d|[a-f]){8}-(\\d|[a-f]){4}-(\\d|[a-f]){4}-(\\d|[a-f]){4}-(\\d|[a-f]){12}");
    if (validation.test(key)){
        isvalid = true;
    } else{
        isvalid = false;
    }
    return [debug_statement,isvalid,key][index];
}
{% endhighlight %}


Although many of the contest entries were far more advanced than my own, I found the competition to be an engaging exercise. Check out the [winning submission](http://jsfiddle.net/c6z0kb4g/0/) and Peter Jaric's [analysis of the competition](http://javahacker.com/the-first-javascript-misdirection-contest/). 

