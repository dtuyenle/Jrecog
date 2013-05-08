<h1 align="center">mslider.js</h1>
<h2 align="center">javascript plugin - simple - easy to use</h2>
This plugin is JQuery independence. This plugin was developed for touch devices. You will not see how it works without trying it on touch devices.

With the future trend of using touch screen devices, more and more people using mobile and tablet devices. Unlike computer screen, touch devices have very limited space. The more space saved for main content usage, the better it is. This is the reason i came up with mslider. Using your finger, draw something on the screen to make either the menu or popup shows up or any creative ways you come up with by executing a callback function. 

You can simply init mslider with one line: mslider(callback)

// mslider is JQuery independence which means you don't need JQuery to run it.
// In this example i am using JQuery just to illustrate.
 
<pre>$(function(){

	// mslider is tied to the DOM, pass callback function as parameter
	mslider(callback);
	
	// This is the function you call to show up the menu
	function callback() {   
		$('.menu').show();
	}		
});</pre>

Supported Browser

It should work for all of the versions of the browsers below:
Android Browser   
Chrome Browser for IOS and Android 	
Safari IOS Browser 	
Firefox Browser for IOS and Android (Firefox is buggy and it only works when you are at the very top of the page)
