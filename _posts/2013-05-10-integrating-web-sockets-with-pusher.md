---
layout: post
title: "Integrating Web Sockets with Pusher"
description: ""
category: 
tags: []
author: dave
---

{% include JB/setup %}

Learn how to integrate web sockets into your next project by following this example on how to make a real time chat application.

<!--break-->

Real time chat using the HTML5 websockets and Pusher? Don't mind if I do!

What is a websocket and why should I use one?

A websocket is a persistent connection between the client and the server. This persistent connection means that both the client and the server can send data at any time. So why is this useful?

This can best be explained by taking a step back in time. Before HTML5 came along but after AJAX become popular we had a little thing called "long polling". Long polling used the client to send a request to the server at regular intervals. As you can imagine this is extremely wasteful because we have no idea if the server has any new information to send back. Get 50 users in your chat room and a lot of server resources are needed to process all these requests.

Now come back to the present.

Although websockets are making their mark on the web development world they still only compatible with newer browsers. A full list can be found [here](http://caniuse.com/#search=websocket). It is also worth mentioning that there are legacy alternatives to HTML5 websockets such as Comet but I didn't use it so I am not going to talk about it.

Why Pusher?

Nothing in particular drove me to use Pusher over alternatives such as socket.io, Ratchet and Cowboy. I came about Pusher by doing a little research and I was surprised about how easy it was to set up.

Pusher also has a fallback to Flash. This makes me happy!

How I developed my real time chat application

First things first you need a Pusher account. Don't worry they have a free package that offer enough connections to make a small application. So visit [Pusher](http://pusher.com/) and sign up!

Next things you will need is the Pusher PHP library that can be found [here](https://github.com/pusher/pusher-php-server) and the Pusher JavaScript library that can be found [here](https://github.com/pusher/pusher-js).

The set up I am using is as follows. I submit the message with AJAX to a PHP script that triggers the Pusher event and sends the message through the specified channel. Mean while a client side script is listening to the channel and picks up any communication. Depending on the callback I specify when the event is triggered, the reaction from the client side script is altered. Don't worry this will all be explained in detail as we progress.

Lets look at the server side code

Authenticating the Pusher object

In order to connect to Pusher you will need your API key, API secret and API ID that are given you when you register your application. 

If we put this into code it looks like this:

{% highlight php %}
class AJAX_save
{
	private $_pusher;

	private $_api_key = 'api_key';
	private $_api_secret = 'api_secret';
	private $_api_id = 'api_id';

	public function __Construct ()
	{
		//Instantiate the Pusher class and pass in the api credentials
		$this->_pusher = new Pusher ( $this->_api_key, $this->_api_secret, $this->_api_id );
	}
}
{% endhighlight %}

At the moment all this class will do is instantiate and authenticate the Pusher object and save it as a property. This is all we need to do on the server side code at this time.

Next thing we need to do is set up our JavaScript so we subscribe and listen to our channel. Your will only need your API key with this stage.

{% highlight javascript %}
Pusher.log = function(message) {
	if (window.console && window.console.log)
                    window.console.log(message);
};

// Flash fallback logging - don't include this in production
WEB_SOCKET_DEBUG = true;

var pusher = new Pusher ( 'app_id' );
var channel = pusher.subscribe ( 'test_channel' );
{% endhighlight %}

This script gives us the ability to listen to our channel for specific events and then take action depending on what event was triggered by our server side script.

Lets look at setting up an event called "message_event". 

In our PHP file we need to add a method to deal with the message that is sent through the AJAX.

{% highlight php %}
public function send ()
{
	try
	{
		$this->_pusher->trigger ( 'test_channel', 'message_event', array ( 'msg' => $_POST[ 'message' ]  ) );
	}
	catch ( Exception $e )
	{
		die ( $e->getMessage () );
	}
}
{% endhighlight %}

The main thing to take note of here is the "trigger" method. We need to feed in the channel we want to use, the event name and a array of any data we want to send, at the moment we are only going to send the message received from AJAX. 

We now need to set up a listener in our JavaScript for the "message_event" event. 

{% highlight javascript %}
channel.bind('message_event', function ( data ) {
         //Pass it to a function to handle the prepending
        display_message ( data );
});
{% endhighlight %}

This sets an anonymous function to be called when ever a message with the "message_event" event is picked up. This is then passed to the "display_message" function to handle actually organising the data retrieved from the channel and displaying it to the user.

Now the Pusher interaction is sorted we need to be able to send a message. To do this we need to set up a simple form in the view with a input field and add this to our JavaScript file. This is a simple AJAX call to the "send" method we created earlier in our main controller. This will trigger the "message_event" and send the message through our channel.

{% highlight javascript %}
$.ajax({ url: window.site_path + '/AJAX_save/send',
         data: { message: $( '.js-message' ).val(), },
         type: 'POST',
         dataType: 'JSON'
});
{% endhighlight %}

The final thing to do is create the "display_message" function so we can print new messages to the view. This can be as simple as appending the message to a div. For example:

{% highlight javascript %}
display_message: function ( data ) {

    $( '.js-chat-display' ).append( '<p>' + data.msg + '</p>' );
}
{% endhighlight %}

This is all we need to get a very simple real time chat application up and running. There are several improvements to this that can be seen by viewing the source code on [GitHub](https://github.com/StormCreative/chat). This version takes things a little further implementing broswer notifications for new messages, gravatars to make the chat a little more personalised and a log in system to make it secure.
