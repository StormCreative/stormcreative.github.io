---
layout: post
title: "Testing JavaScript with Jasmine"
description: ""
category: 
tags: []
author: dave
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

We also need to make sure we have any dependencies defined in our packages.json file. Here is an example.

{% highlight js %}
	{
	  "name": "StormCreative",
	  "version": "0.1.0",
	  "devDependencies": {
	    "grunt": "~0.4.1",
	    "grunt-contrib-jasmine" : "*",
	    "phantomjs" : "*",
	    "grunt-template-jasmine-requirejs": "~0.1.2"
	  }
	}
{% endhighlight js %}

Remember if you make a change to your packages.json file you need to run the following command to install all the necessary dependencies.

{% highlight php %}
	npm install
{% endhighlight php %}

We are now ready to start writing our tests.

We currently use [RequireJS](http://requirejs.org/) to help us develop our JavaScript so we will also using that in our testing environment. Here is what a basic test spec could look like.

{% highlight js %}
	define( [ '../modules/validation' ], function( Validation ) {

		describe( "A suite", function() {
		    it( "contains spec with an expectation", function() {
		            expect( true ).toBe( false );
		    });
		});
	});
{% endhighlight js %}

Lets take a closer look at what this code is doing.

Firstly we need to require the module we intend to test. In this case we are going to test our validation module. Inside the module definition is where we will write our tests. The example above is simply trying to test that true is going to be false. If we go ahead and run this the test will fail and we will be given the name of the suite, the name of the test and a reason to why the test has failed.

Jasmine has a wide variety of assertions that can be used to test our modules. Here is an explanation of some of the assertions I use a lot.

<u>toBe()</u>
This is the one we used in our example above.










