---
layout: post
title:  "Creating and sharing library in Angular 6"
date:   2016-03-07 12:00:11 +0000
categories: angular
image:  /preview.jpg
---


{% highlight ruby %}
ng new employee-portal
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-1.png">

<img src="/images/post1-creating-library-angular/screenshot-2.png">

{% highlight ruby %}
ng generate application workitems
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-3.png">

<img src="/images/post1-creating-library-angular/screenshot-4.png">

{% highlight ruby %}
ng generate application timesheet
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-5.png">

<img src="/images/post1-creating-library-angular/screenshot-6.png">

<img src="/images/post1-creating-library-angular/screenshot-7.png">

<img src="/images/post1-creating-library-angular/screenshot-8.png">

{% highlight ruby %}
ng serve timesheet
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-9.png">

<img src="/images/post1-creating-library-angular/screenshot-10.png" class="fit image">

{% highlight ruby %}
ng serve workitems
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-11.png">

{% highlight ruby %}
ng g library emp-shared -p emp
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-12.png">

<img src="/images/post1-creating-library-angular/screenshot-13.png">

<img src="/images/post1-creating-library-angular/screenshot-14.png">

<!-- Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
