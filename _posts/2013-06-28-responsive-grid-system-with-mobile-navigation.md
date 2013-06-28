---
layout: post
title: "Responsive grid system with mobile navigation"
description: ""
category: 
author: alex
tags: []
---
{% include JB/setup %}

Using current practices of grid systems and incorporating them into a mobile first build approach.

<!--break-->

Mobile websites are becoming increasingly popular and more essential for businesses. Currently our builds are not mobile first and end up backward developing the project from desktop down meaning code is duplicated and does not create a flow within the design, this is a solution I have been looking into to find out how we can use current practices but use for a mobile first approach.

Currently we use grid layouts for our responsive builds so that every section (holder) has a set width (in percentage) and scales down depending on which device its being viewed on. Problems that occur are that smaller widths mean squashed content = bad user experience. So to combat this media queries are set for mobile sized devices that put all the holders to 100% width of the device. This is ok for setting up a layout but a mobile first approach to building a site will lead to duplication of code.

##Solution

A solution that was found is to build the stylesheet aimed first at mobile devices, and then add changes to styles later on in the build for tablet and desktop should they require it. This means that styles wont be duplicated again later on in the build. This is a quick tutorial to show how the setup would look.

I am going to be using grid layout classes aimed at desktop to show how they can be used in relation to the desktop build. Firstly, we need to create the html layout.

{% highlight html %}

<section class="grid">
    <div class="grid__layout"><p>Grid</p></div>
    <div class="grid__layout grid__layout--one-half"><p>one-half</p></div>
    <div class="grid__layout grid__layout--one-half"><p>one-half</p></div>
    <div class="grid__layout grid__layout--one-third"><p>one-third</p></div>
    <div class="grid__layout grid__layout--one-third"><p>one-third</p></div>
    <div class="grid__layout grid__layout--one-third"><p>one-third</p></div>
    <div class="grid__layout grid__layout--one-quarter"><p>one-quarter</p></div>
    <div class="grid__layout grid__layout--one-quarter"><p>one-quarter</p></div>
    <div class="grid__layout grid__layout--one-quarter"><p>one-quarter</p></div>
    <div class="grid__layout grid__layout--one-quarter"><p>one-quarter</p></div>
    <div class="grid__layout grid__layout--one-third"><p>one-third</p></div>
    <div class="grid__layout grid__layout--two-thirds"><p>two-thirds</p></div>
    <div class="grid__layout grid__layout--one-half"><p>one-half</p></div>
    <div class="grid__layout grid__layout--one-quarter"><p>one-quarter</p></div>
    <div class="grid__layout grid__layout--one-quarter"><p>one-quarter</p></div>
    <div class="grid__layout grid__layout--one-quarter"><p>one-quarter</p></div>
    <div class="grid__layout grid__layout--three-quarters"><p>three-quarters</p></div>
    <div class="grid__layout grid__layout--one-sixth"><p>one-sixth</p></div>
    <div class="grid__layout grid__layout--one-sixth"><p>one-sixth</p></div>
    <div class="grid__layout grid__layout--one-sixth"><p>one-sixth</p></div>
    <div class="grid__layout grid__layout--one-sixth"><p>one-sixth</p></div>
    <div class="grid__layout grid__layout--one-sixth"><p>one-sixth</p></div>
    <div class="grid__layout grid__layout--one-sixth"><p>one-sixth</p></div>
</section>

{% endhighlight html %}

I have used divs here as an example, you can use the classes on any elements. Every div has a class on it called grid__layout and the desired width of the div. I have called them grid__layout—one-half etc, because on the desktop we want these to 50% of our actual container size. Anyways, as we are focused on mobile we want these to all be 100% for best user experience. The first style we need to add box-sizing on everything so that we can add padding and margin without breaking our layout. [Find out more](http://www.paulirish.com/2012/box-sizing-border-box-ftw/), and also add our container class, which is our grid.

{% highlight css %}

* {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
}

.grid {
    display: block;
    letter-spacing: -0.375em;
    margin: 0 auto;
    max-width: 60em;
    overflow: hidden;
    padding: 2px;
}

{% endhighlight css %}

Now we have our holder styled we go about adding the additional classes, which will affect our layout of each div.

{% highlight css %}

.grid__layout {
    display: inline-block;
    letter-spacing: normal;
    line-height: 40px;
    min-height: 40px;
    padding: 2px;
    text-align: left;
    width: 100%;
}

.grid__layout p {
    background: steelblue;
    color: white;
    display: block;
    margin: 0;
    text-align: center;
}

{% endhighlight css %}

This is it, our mobile layout for 320 devices is complete, I then turn my device landscape and thought maybe I should add the grid layout widths in now for certain sections. So I set up a media query at a min-width of 480px, meaning every device that is greater than 480px will now inherit the new styles. All we need to do is place our styles within this media query... @media all and (min-width: 480px) {}

{% highlight css %}

.grid__layout--one-half {
    width: 50%;
}

.grid__layout--one-third {
	width: 33.33333333%;
}

.grid__layout--one-quarter {
	width: 50%;
}

.grid__layout--two-thirds {
	width: 66.66666667%;
}

.grid__layout--one-sixth {
	width: 33.33333333%;
}

{% endhighlight css %}

The next breakpoint is 600 where we use the same media query except this time change it to min-width: 600px.

{% highlight css %}

.grid__layout--one-quarter {
    width: 25%;
}

.grid__layout--three-quarters {
    width: 75%;
}

{% endhighlight css %}

And lastly our ipad sized screen which is 768px, we apply the same as before but include this styling.

{% highlight css %}

.grid__layout--one-sixth {
    width: 16.66666667%;
}

{% endhighlight css %}

And that is it, our responsive grid system has now been set up and is built from mobile up. The only problem is in ie8 and lower, media queries are not supported so a script will need to be added for this to work, [find it here](http://cssmatter.com/blog/ie7-and-ie8-support-for-css3-media-query/).

## What about the responsive navigation?

If you want to add a responsive navigation aswell it can be done using the same html, again not duplicating our code and making it more semantically pleasing. This is done with a little bit of javascript, and jquery. First we need to add our navigation in our html, simply add this code inside the grid section but before all the divs.

{% highlight html %}

<nav class="navigation">
    <h1 class="navigation__title">Responsive Grid Layout</h1>
    <a class="js-nav-dropdown" href="#">Nav dropdown</a>
    <ul class="grid__layout grid__layout--one-half navigation__links">
        <li><a href="#">Link 1</a></li>
        <li><a href="#">Link 2</a></li>
        <li><a href="#">Link 3</a></li>
        <li><a href="#">Link 4</a></li>
    </ul>
</nav>

{% endhighlight html %}

Now we have added our html we need to style how it will look on the mobile. We want to included a dropdown nav, so we create a button that will toggle down our existing menu but on load will not show. For this we use a few lines of javascript...

{% highlight javascript %}

$(document).ready (function(){
    $('.js-nav-dropdown').on('click', function() {

    var dropdown_nav = $('.navigation__links');

    if ($(window).width() < 599) {
    	$(dropdown_nav).slideToggle('normal');
    	}
    });
});

{% endhighlight javascript %}

This will only apply our navigation styles for devices that are below 599px and when on a device thats 600 or more use our original navigation that was built for desktop. The styles for our mobile navigation will be:

{% highlight css %}

.navigation__title {
    display: inline-block;
    font-size: 16px;
    letter-spacing: normal;
    width: 80%;
}

.js-nav-dropdown {
    background: url("../Images/nav-icon.png") 0 0 no-repeat;
    display: inline-block;
    float: right;
    height: 36px;
    letter-spacing: normal;
    margin-right: 2px;
    margin-top: 8px;
    overflow: hidden;
    text-indent: 100%;
    white-space: nowrap;
    width: 45px;
}

.navigation__links {
    display: none;
    list-style: none;
    margin: 0;
    padding: 0 4px;
    text-align: right;
}

.navigation__links li a {
    color: #000;
    text-decoration: none;
}

{% endhighlight css %}

and for our desktop version (using our 600px media query from before)...

{% highlight css %}

.navigation__title {
    width: 50%;
}

.js-nav-dropdown {
    display: none;
}

.navigation__links {
    display: inline-block;
    width: 50%;
}

.navigation__links li {
    display: inline-block;
    margin-left: 16px;
}

.navigation__links li:first-child {
    margin-left: 0;
}

{% endhighlight css %}

And there you are, one, responsive grid layout with responsive navigation built from mobile up. If you want to use this as a template or help improve this, please feel free to [use](https://github.com/AlexShearcroft/responsive-grid).