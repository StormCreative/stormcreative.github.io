---
layout: post
title: "Pallaxing vertical slider with CSS3"
description: ""
category: 
tags: []
author: rich
---
{% include JB/setup %}

After seeing lots of cool parallax websites, I noticed all of them use javascript so I decided to try and create one with just CSS3.

<!--break-->

A parallax website involves the background moving at a slower rate to the foreground, creating a 3D effect as you scroll down the page.

Here are some of the best examples I have seen: [Peugeot](http://graphicnovel-hybrid4.peugeot.com/start.html), [Oakley](http://moto.oakley.com/), [Nike](http://www.nike.com/jumpman23/aj2012/).

Basically decided to create a vertical slider with css3 animations moving the layers at different rates.

So what are css3 animations?

CSS3 animations make it possible to animate transitions from one CSS style to another. Animations consist of two components, a style describing the CSS animation and a set of keyframes that indicate the start and end states of the animation's style, as well as possible intermediate waypoints along the way.

The key advantages using CSS3 over Javascript:

They're easy to use for simple animations, you can create them without even having to know JavaScript.

The animations run well, even under moderate system load. Simple animations can often perform poorly in JavaScript. The rendering engine can use frame-skipping and other techniques to keep the performance as smooth as possible.

Letting the browser control the animation sequence lets the browser optimise performance and efficiency by, for example, reducing the update frequency of animations running in tabs that aren't currently visible.


Let take a look at the markup:

{% highlight html %}

<!-- slider container -->
<div class="rich_main">

  <!-- hidden radio buttons -->
  <input id="r_1" type="radio" class="select_page_1" checked="checked" /> 
  <input id="r_2" type="radio" class="select_page_2" />
  <input id="r_3" type="radio" class="select_page_3" />
  <input id="r_4" type="radio" class="select_page_4" />

  <!-- up and down controls -->
  <label for="r_1" class="rich_control c1"></label> 
  <label for="r_2" class="rich_control c2"></label>
  <label for="r_3" class="rich_control c3"></label>
  <label for="r_4" class="rich_control c4"></label>

  <div class="rich_slides">
    <!-- the slides -->
    <ul> 
      <li>
      <!-- can put anything you like in these LIs -->
        <img src="assets/images/image1.png"/>
      </li>
      <li>
        <img src="assets/images/image2.png"/>
      </li>
        <li>
        <img src="assets/images/image3.png"/>
      </li>
      <li>
        <img src="assets/images/image4.png"/>
      </li>
    </ul>
  </div>
</div>

{% endhighlight %}

As you can see there's a container div holding every element inside. We use the radio buttons and labels to select which slide the reason for this is unfortunately when this post was written you can't detect mouse scrolling in CSS3, so to get round this we have made radio buttons that once clicked will change which slide is displayed. All sides are contained in an unordered list called rich_slides.

The CSS:

Quick setup to make body and html 100% high and wide so that the slider will fill the screen width.

{% highlight css %}

/*** body ***/

html, body {
	background: black;
	margin: 0;
	padding: 0;
	width: 100%;
	height: 100%;
}

{% endhighlight %}

These are the general styles for our main slider element, inputs and their labels (used for the up and down buttons)

{% highlight css %}

/*** scrolling slider ***/

.rich_main {
    height:100%;
    position:relative;
    width:100%;
}
.rich_main input {
    display:none;
}
.rich_control {
    background: orange url(../images/up.png) no-repeat center center;
    cursor:pointer;
    display:none;
    height:70px;
    left:50%;
    opacity:0.7;
    position:absolute;
    top:15%;
    width:70px;
    z-index:2;

    /* css3 transition */
    -webkit-transition:opacity linear 0.3s;
    -moz-transition:opacity linear 0.3s;
    -o-transition:opacity linear 0.3s;
    -ms-transition:opacity linear 0.3s;
    transition:opacity linear 0.3s;
    border-radius:50% 50% 50% 50%;
    box-shadow:0 1px 2px #AF2C19 inset;
}
.rich_control:hover {
    opacity:1;
}
.select_page_1:checked ~ .rich_control.c2, .select_page_2:checked ~ .rich_control.c3,
.select_page_3:checked ~ .rich_control.c4 {
    background-image:url(../images/down.png);
    display:block;
    top:85%;
}
.select_page_2:checked ~ .rich_control.c1, .select_page_3:checked ~ .rich_control.c2,
.select_page_4:checked ~ .rich_control.c3  {
    background-image:url(../images/up.png);
    display:block;
}
{% endhighlight %}

All the radio buttons are hidden, as we have no reason to show them, as instead we are going to use labels. Each label is an orange circle just so it’s noticeable. We have set transition for the opacity when hovered over. By default all the labels are hidden, we will display necessary up and down buttons only on the relative slide.

This next section of css is about setting  up the transition between slides and elements.

{% highlight css %}
.rich_slides {
    height:100%;
    overflow:hidden;
    position:relative;
}
.rich_main input:checked ~ .rich_slides {
    /* css3 transition */
    -webkit-transition:all linear 1.0s;
    -moz-transition:all linear 1.0s;
    -o-transition:all linear 1.0s;
    -ms-transition:all linear 1.0s;
    transition:all linear 1.0s;
}
.select_page_1:checked ~ .rich_slides {
    background-image: url(../images/1.png);
}
.select_page_2:checked ~ .rich_slides {
    background-image: url(../images/2.png);
}
.select_page_3:checked ~ .rich_slides {
    background-image: url(../images/3.png);
}
.select_page_4:checked ~ .rich_slides {
    background-image: url(../images/4.png);
}
.rich_main input:checked ~ .rich_slides .rich_background {
    /* css3 transition */
    -webkit-transition:all linear 1.5s;
    -moz-transition:all linear 1.5s;
    -o-transition:all linear 1.5s;
    -ms-transition:all linear 1.5s;
    transition:all linear 1.5s;
}
.select_page_1:checked ~ .rich_slides .rich_background {
    background-position:0 0;
}
.select_page_2:checked ~ .rich_slides .rich_background {
    background-position:0 -500px;
}
.select_page_3:checked ~ .rich_slides .rich_background {
    background-position:0 -1000px;
}
.select_page_4:checked ~ .rich_slides .rich_background {
    background-position:0 -1500px;
}
.rich_slides ul {
    height:100%;
    list-style:none;
    position:relative;
    top:0;

    /* css3 transition */
    -webkit-transition:ease-in 1.0s;
    -moz-transition:ease-in 1.0s;
    -o-transition:ease-in 1.0s;
    -ms-transition:ease-in 1.0s;
    transition:ease-in 1.0s; 
}
.rich_slides ul  li {
    height:100%;
    opacity:0.1;
    position:relative;
    text-align:center;

    /* css3 transition */
    -webkit-transition:opacity ease-in 1.0s;
    -moz-transition:opacity ease-in 1.0s;
    -o-transition:opacity ease-in 1.0s;
    -ms-transition:opacity ease-in 1.0s;
    transition:opacity ease-in 1.0s; 
}
.select_page_1:checked ~ .rich_slides ul li:first-child,
.select_page_2:checked ~ .rich_slides ul li:nth-child(2),
.select_page_3:checked ~ .rich_slides ul li:nth-child(3),
.select_page_4:checked ~ .rich_slides ul li:nth-child(4) {
    opacity:1;
}
.rich_slides ul li img{
    display:block;
    margin:0 auto;
    padding:100px;
}

.select_page_1:checked ~ .rich_slides ul {
    top:0;
}
.select_page_2:checked ~ .rich_slides ul {
    top:-100%;
}
.select_page_3:checked ~ .rich_slides ul {
    top:-200%;
}
.select_page_4:checked ~ .rich_slides ul {
    top:-300%;
}
{% endhighlight %}

Once a up/down button is click, this triggers the animations to run which you should notice four changing per click. First that will change is the slide itself which will in turn change the background image/colour and bring that slides content on to the slide been display moving faster than the background to give a parallax effect.

The slide transition changes the opacity and slides it up are down depending on which button is pressed up/down by 100% -/+ which is set to move at the rate of one second.

The background transition is set to ease in (fade in) from the current slide via opacity which gives a fading up effect over the top at the rate of 0ne and half seconds.

The content transition is set to move the element from the bottom to the top of the slide with an opacity easing up to full at the rate of one second.

There you have it we have created a CSS3 parallaxing slider without the use of any javascript.

