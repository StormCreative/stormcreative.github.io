---
layout: post
author: ash
title: "Using Sinatra with ActiveRecord for Competition Facebook Apps"
description: ""
category: 
tags: []
---
{% include JB/setup %}
I'm about to show you a very easy solution to get a Facebook Competition up and running by using Sinatra. 

<!--break-->

If you didn't already know, Sinatra is a Simple Ruby Web Framework. You may have heard of Ruby on Rails, well Sinatra is a completely stripped
down alternative to Rails - which is exactly why it's a great tool for a Facebook Competition!

I'm going to assume you know how to set-up a Facebook App - if you don't you can follow Facebooks own tutorial [here](https://developers.facebook.com/docs/guides/appcenter/). However
I will say for this purpose we will be taking Facebooks recommendation and hosting with Heroku for free - as Heroku happily supports Ruby.

Sinatra doesn't natively come with a way to interact with a database, so this is why we will be using ActiveRecord - which will also have to be set up, but don't worry I'll be showing you below.

To get started we're going to install Sinatra. Fire up your terminal and type in the below command:

{% highlight ruby %}
sudo gem install sinatra

# Shotgun is a gem that prevents us having to constantly restart the Sinatra server
sudo gem install shotgun
{% endhighlight %}

While we're at installing gem's we may aswell install the ActiveRecord bits:

{% highlight ruby %}
sudo gem install bundle

sudo gem install activerecord

sudo gem install sinatra-activerecord
{% endhighlight %}

Now that is everything we need installed globally onto our machine. Next step is to create a directory for the app, anywhere you would usually
hold your sites that you develop will do. We'll be setting up the directory will the following subdirectories and files:

{% highlight ruby %}
- models
- config
    - environments.rb
- views
app.rb
config.ru
Gemfile
Rakefile
{% endhighlight %}

We'll start backwards from the list and begin with our Rakefile - if you were unaware this is where a projects command line tasks are held. Open 
the Rakefile up and put the below code in, you don't have to worry about what it actually does it just requires in some tasks:

{% highlight ruby %}
require "./app"
require "sinatra/activerecord/rake"
{% endhighlight %}

Within your Gemfile you'll be placing the below code in. Take note at the 'development' and 'production' group; we have this as Heroku only supports
postgres SQL, but for local development we will be using sqlite3. 

{% highlight ruby %}
source 'https://rubygems.org'

gem "sinatra"
gem "activerecord"
gem "sinatra-activerecord"

group :development do
  gem "shotgun"
  gem "tux"
  gem 'sqlite3'
end
 
group :production do
  gem 'pg' # this gem is required to use postgres on Heroku
end
{% endhighlight %}

Your config.ru file should look like below:

{% highlight ruby %}
# config.ru
require "./app"
run Sinatra::Application
{% endhighlight %}

The final step of setting up before we can get to the fun stuff is to setup our environments.rb. The below code sets the database up for both the development
and production environment, so you will need to change the database name to be whatever you want the database to be called.

{% highlight ruby %}

configure :development, :test do
  set :database, 'sqlite3:///competition.db'
end
 
configure :production do
  # Database connection
  db = URI.parse(ENV['DATABASE_URL'] || 'postgres://localhost/mydb')
 
  ActiveRecord::Base.establish_connection(
    :adapter  => db.scheme == 'postgres' ? 'postgresql' : db.scheme,
    :host     => db.host,
    :username => db.user,
    :password => db.password,
    :database => db.path[1..-1],
    :encoding => 'utf8'
  )
end
{% endhighlight %}

That's everything we need to finally get started! Go back into your terminal and you will be running the bundle command; we're doing this to locally
install the Gems that we have put within our Gemfile. 

This may take a while, but once complete you should find a Gemfile.lock within the directory.

{% highlight ruby %}
bundle install
{% endhighlight %}

Once bundle has worked it's magic you will be able to see Rakes avaliable tasks and start setting up some database tables to work with. So go ahead and
type in the following command:

{% highlight ruby %}
rake -T
{% endhighlight %}

The above command will then display three rake tasks, and your terminal window will look like below:

{% highlight ruby %}
$ rake -T
rake db:create_migration  # create an ActiveRecord migration
rake db:migrate           # migrate the database (use version with VERSION=n)
rake db:rollback          # roll back the migration (use steps with STEP=n)
{% endhighlight %}

We will want to setup a table to hold our competition entry data - to do this we will be using the first task from the rake task list 'db:create_migration'. 
If you run the following command, you should see a 'db' directory appear in your site folder and within there a migration with a prepended timestamp. This migration file is where we will put
in the information about our table to create.

{% highlight ruby %}
rake db:create_migration NAME=competition
{% endhighlight %}

Jump to the migration file that was just created and you will want to place something along the lines of the below code within the 'up' method, you will need to alter it
in the future for the table columns your form would be capturing - but here is just an example:

{% highlight ruby %}
# This migration will create two columns title and body and by default a time when the row was created
# just continue to add more columns if needed.
create_table :competitions do |t|
  t.string :title
  t.text :body
  t.timestamps
end
{% endhighlight %}

It's all very well that we have created and altered our migration, but we need to actually process it. This is done through the 'db:migrate' command, just run that within the terminal and 
all the database schema should be created!

Now it's time to create our model, create a new file within the models folder called competition.rb (or whatever you called your table) and place the following code in. This is just
a very simple Ruby class that extends ActiveRecord, we can do more with this model later by adding validation etc. etc.

{% highlight ruby %}
# Notice we don't call me with a plural like the table
class Competition < ActiveRecord::Base
    # and everything to do with the model will go within here
end
{% endhighlight %}

Now we have to tell our app where to go within the browser. This is where Sinatras simplicity comes in, you set all your routes within just one file
and by that given URL you perform an action, see below and read the comments to understand what is happening:

{% highlight ruby %}
# Require in all neccessary files
require "sinatra"
require "sinatra/activerecord"
require "./config/environments"
require "./models/competition"

# the get method tells sinatra to do the following on the default page of /
get "/" do
  # erb tells Sinatra to load the file 'index' within the views folder
  # our index view file will hold a form that posts
  erb :index
end

# as the form method is post we use the post method to capture the form being processed
# from the homepage
post "/" do
  # we simply instantiate the competition model and pass in the post parameters from the form
  comp = Competition.new(params[:competitions])  
  # we check that the save was successful and do something
  if comp.save
    "Item has been saved: #{comp.title}"
  else
    "Item was not saved"
  end
end
{% endhighlight %}

Your view file will look something like below, take note of how the name of the form field looks - this is why we pass in 'params[:competitions]' to the model instantiation.

{% highlight html %}
<h1>Form</h1>
<form method="post" action="/">
    <label for="title">Title: </label><input type="text" name="competitions[title]" id="title">
    <input type="submit" name="submit" value="save">
</form>
{% endhighlight %}

That is everything to get you started with a very simple example - now all you have to do is actually be able to access the app through the browser! Back to terminal and chuck in the command:

{% highlight html %}
shotgun
{% endhighlight %}

This will start the Sinatra server, but allow you to make changes to the site without having the restart the server to register any new changes.
You are then able to access the site through the following URL:

{% highlight ruby %}
localhost:9393
{% endhighlight %}

And that's a rap - it's up to you where you go to from here, but this should get you started.


