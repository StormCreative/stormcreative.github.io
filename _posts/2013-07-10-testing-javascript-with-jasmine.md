---
layout: post
title: "new post"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Automated testing has always been something we try and implement with our PHP, so with our increased implementation of JavaScript into all projects we decided it was time to set up a test suit using [Grunt](http://gruntjs.com/) and [Jasmine](http://pivotal.github.io/jasmine/).
<!--break-->

Setting up grunt

First thing you will need to do is install the latest version of grunt. So open up your terminal and enter:

{% highlight php %}
	npm install -g grunt-cli
{% endhighlight php %}

This will install the Grunt CLI globally so you can use Grunt from any directory. This of course relies on you having a Gruntfile in the project you are working on. Lets take a quick look a example Gruntfile with the Jasmine task in it.

{% highlight js %}

	module.exports = function(grunt) {

		grunt.initConfig({
		    pkg: grunt.file.readJSON('package.json'),

		    jasmine : {
		        src : 'assets/scripts/app/*.js',
		        options : {
		            specs : 'assets/scripts/tests/*.js',
		            template: require('grunt-template-jasmine-requirejs'),
		            templateOptions: {
		                requireConfig: {
		                    baseUrl: ''
		                }
		            }
		        }
		    }
		});

		grunt.loadNpmTasks('grunt-contrib-jasmine');
		grunt.registerTask('test', [ 'jasmine' ] );
	});

{% endhighlight js %}

