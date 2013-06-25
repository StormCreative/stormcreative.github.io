---
layout: post
title: "Using HTML5 to cache your site so it works offline"
description: ""
category: 
tags: []
author: dave
---
{% include JB/setup %}

Giving your users enough of your site so they can use it offline is a great feature to offer. How do we make something that requires internet access to work, function without it?

<!--break-->

HTML5 offers some great features. The one I am going to be talking about is application caching. Offline accessibility is only one of the advantages of application caching. Application caching also speeds up load times and reduces the load on the server. The one major downside is that it is not that compatible with dynamic sites. We use PHP as our server side language so we will only be able to cache sites that are mostly static or the bulk of the functionality is JavaScript. This being said the user will not be able to make database calls while offline so realistically only static sites can utilise application caching.

If you were wondering implementing application caching to a existing site will not be too much hassel. You will not have to rewrite the current code base, a basic implementation will only require the following simple steps.

The first step is to add the "manifest" attribute to the html tags in your site. Worth pointing out that your browser will not cache a page that does not have the manifest attribute in the html tag, unless it is set in the manifest cache file.

{% highlight html %}
	<html manifest="test.appcache">
	</html>
{% endhighlight html %}

Next we need to create the test.appcache file. So create it in the root of your project and add the following to it.

{% highlight html %}
	CACHE MANIFEST
{% endhighlight html %}

It is required that you have this as the first line in all of your cache manifest files.

Your appcache file needs to have a mime-type of "text/cache-manifest" so you may need to set the type in your .htaccess file. To make sure our appcache file is always has the mime-type "text/cache-manifest" make sure to have this line in your .htaccess file.

{% highlight html %}
	AddType text/cache-manifest .appcache
{% endhighlight html %}

Now you have set up everything you need to you can go ahead and start adding files you want to cache. Here is a example of how your cache manifest could look like.

{% highlight html %}
	CACHE MANIFEST

	index.html
	assets/images/generic.jpg
{% endhighlight html %}

That is all we need to get our site working offline. To make sure application caching is working you can open the browsers console and you should see a log similar to "Document was loaded from Application Cache with manifest http://yoursite.co.uk/test.appcache".

To extend this little project and experiment with how to handle database communications I built a VERY basic blog. The way it works is when the user is connected to the internet it will get the blog posts from the database as you would expect it to. Instead of displaying them directly on the page they are put into individual files and saved onto the server in JSON format. The location of the file is also added to the test.appcache file that is re-generated on each page load so it always represents exactly what needs to be cached. Next the JavaScript loops through each blog file and displays it onto the page.

If the user is offline then the JavaScript will just loop through the cached blog files and display their content.

We now have a blog that can be viewed both online and offline.

You can view the source code of this little project [here](https://github.com/iamjones/blogoff)