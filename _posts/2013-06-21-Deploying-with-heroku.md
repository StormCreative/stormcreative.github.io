---
layout: post
title: "Deploying with heroku"
description: ""
category: 
tags: []
author: dave
---
{% include JB/setup %}

Learn how to deploy a web application with heroku and github.

<!--break-->

Recently I built a real time chat application that the team and I use to communicate. The issue with this was it was stored on my local machine so it was not as flexible to use as the project was intended to be. I could have just stuck it on our server to solve this short fall but I decided to rewrite it in Ruby and deploy it using Heroku.

Heroku is a cloud based application platform used to deploy fast and powerful apps. You can use git to deploy your apps to heroku the same way as pushing it up to your repository.

First things first we need to create our Heroku account and download the tool belt so we can login. So go [here](https://id.heroku.com/signup/devcenter) and sign up then go [here](https://toolbelt.heroku.com/) and install the Heroku toolbelt for your machine.

Lets look at how we can create our application and use git to manage deployment.

First lets login into Heroku by using this command.

{% highlight php %}
	heroku login
{% endhighlight php %}

Next we need to create the local directory, initialise it as a local git repo and push our current files to it.

{% highlight php %}
	cd king-cobras
	git init
	git add .
	git commit -m 'First commit'
{% endhighlight php %}

Now we need to create the actual Heroku remote. This command will create a new application on Heroku and a git remote so we can push to Heroku.

{% highlight php %}
	heroku create
{% endhighlight php %}

As I am working with Ruby I need to make sure I have two files in my application to make sure it gets deployed successfully. These are a Gemfile and a Procfile. The Gemfile is a easy way to manage the Gems that the application uses. The Procfile tells Heroku what command to execute to start a web dyno. Now we have everything we need in place to push and deploy our application to Heroku.

{% highlight php %}
	git push heroku master
{% endhighlight php %}

Thats all we need to do to get set up with Heroku. There are a few other useful tools that I have discovered that really helped me develop.

The first of these is using the Postgres CLI to start a session with our remote database. I mainly use this as a debugging tool to check if tables have been made and if data has been inserted correctly and in the correct format. This comes in handy whenever I need to run a CRUD query.

To start a session with your remote database use this command.

{% highlight php %}
	heroku pg:psql
{% endhighlight php %}

The second is running a console with the Heroku toolbelt. I use this to check if all my files have been pushed up correctly when I experience 404 errors. A Heroku console can be started with this command.

{% highlight php %}
	heroku heroku run bash --app "APP NAME"
{% endhighlight php %}