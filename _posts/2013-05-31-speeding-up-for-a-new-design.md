---
layout: post
title: "Speeding up for a new design"
description: ""
author: ash
category: 
tags: []
---
{% include JB/setup %}
We're currently working on an entirely new site for the Digital Department - with the main aim to improve, and hopefully abolish bounce rates. The key to this is having an extremely fast, streamlined site so there's no wait for the user to load the website. 
<!--break-->

The new site is about 85% static, however the parts that are dynamic have still rely on a number of queries to display data. Although they probably don't look too cost effective, our framework and database architecture do rely on a fair bit of relational queries; for example we store our images in their own database table - because of this, having multiple calls to different data sets could have a performance impact.

Content within the dynamic sections of the site isn't always going to be fresh - probably an update to the blog every couple of weeks. Therefore it wouldn't be neccessary to continually query the database for each visitor, as the content is always going to be the same. Caching the query results would be the best option, so they are always locally accessible without having to do any additonal queries. 

The caching route I decided to take was to just saving the serialized query results into a text file, which would be re-cached after a week. However if we made an update to the site that was not within the week we could just manually remove the cache file.

As our caching requirements and solution was straight-forward, I opted to write our own [Caching Library](https://github.com/banksy89/cacher/blob/master/cacher.php) for this specific site and to be included into our internal Framework.

The library simply creates a new cache file, puts data into the cache file if it's within the required cache time and returns the data set.

Now there are a few other measures we're planning to put in place - but I just wanted to discuss how we will be caching our dynamic data.