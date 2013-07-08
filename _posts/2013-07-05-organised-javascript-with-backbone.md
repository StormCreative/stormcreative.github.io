---
layout: post
title: "Organised Javascript with Backbone"
description: ""
category: 
tags: []
author: ash
---
{% include JB/setup %}
By breaking your application down into modules is ensure maintainability and an organised code base. We use Backbone  along with RequireJS for structuring our web applications. Backbone is great, however it can become easily messy if your application isn't structured properly.
<!--break-->

A Backbone View handles all events for the front-end application - it is sometimes tempting to put most of the actual application logic within the Backbone View, but this is poor code design and not to mention difficult to individually test.

We take a 'Module' approach, breaking each section of the web application design into it's own module - and loading these modules depending on the event triggered from the Backbone View. 

I like to think of a Module to be different to a Model - our PHP MVC Framework uses the Model to define interaction with a database table. So when using a Model in Backbone it only feels appropriate to use it similarly - so we use a Model for interacting with a database, but the Module to handle the actual functionality of a 'widget' (widget being a bit of functionality within an application).

To break an application down into modules, we would first identify each section of the design that requires interactivity. Take the below design for example, I've circled each section that would be treated as it's own 'module':

<img src="/assets/images/widgets.png" style="width:100%;">

There is a total of seven sections that are going to have some form of interactivity. 

Instead of basing all of their logic into one View, we'll create a seperate file and class for each with it's own internal logic. This way we are removing any logic that does not concern the Backbone View and it can concentrate solely on just handling the events.

Without the modules being coupled with a view, it's easier to write tests for our application and ensure each unit (our modules) of the application is working accordingly on it's own - which of course allows us to reuse that module if need be somewhere else in the application and not confined to that view.

The other benefit to this approach is productivity, each team member can work on a module without interfering or conflicting with somebody elses work; it's the Backbone View that just pieces these other modules together like a jigsaw once they have all been written.

