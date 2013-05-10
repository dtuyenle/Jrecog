<h1 align="center">jrecog.js</h1>
<h2 align="center">javascript plugin - simple - easy to use</h2>
This plugin is JQuery independence. This plugin was developed for touch devices. You will not see how it works without trying it on touch devices.

With the future trend of using touch screen devices, more and more people using mobile and tablet devices. Unlike computer screen, touch devices have very limited space. The more space saved for main content usage, the better it is. This is the reason i came up with jrecog. Using your finger, draw something on the screen to make either the menu or popup shows up or any creative ways you come up with by executing a callback function. 
<h3>You can simply init jrecog with one line, the syntax is quite the same as JQuery:</h3>
<pre><i>jrecog({
	k:callbackk,
	a:callbacka,
	u:callbacku
})</i></pre>
<h3 style="font-size:14px;color:#ED5E2F">
Each of variable: k,a,u represents a gesture, you can choose to use all of 3 gestures
or just one or just two of them. Each gesture is tied to one function. Currently Jrecog only supports 3 gestures.
Read section "How it works" to know how to draw each of the gesture on the screen.
</h3>
<pre>
// jrecog is JQuery independence which means you don't need JQuery to run it.
// You can still use JQuery or any javascript library for other usuage.
// In this example i only use 1 gesture: k

$(function(){

	// jrecog is tied to the DOM, pass a function to gesture variable k
	jrecog({
		k:showMenu,
	});
	
	// This is the function you call to show up the menu
	function showMenu() {   
		$('.menu').show();
	}
});</pre>

<h3>Supported Browser</h3>
It should work for all of the versions of the browsers below:

Android Browser   
Chrome Browser for IOS and Android 	
Safari IOS Browser 	
Firefox Browser for IOS and Android (Firefox is buggy and it only works when you are at the very top of the page)

<h3>How it works</h3>

Please go to <a href="http://jrecog.co.nf/">jrecog</a> to know how this plugin works.

<h3>Discussion</h3>

I will not discuss about the algorithm or method to achieve pattern recognition here, refer to Coursera machine learning classes to know how. This discussion is purely about touch event.

Although i tried my best as i could, but you might eperience some buggy behaviors.Generally, in those cases just re-draw the gesture. This issue is due to a bug in android browser, or webkit engine. Since you have to preventDefault for either 'touchstart' or 'touchmove' event so that 'touchend' event can fire, preventDefault will prevent you from scrolling so what i did was i removeEventListeaner for 'touchmove' event.

Another issue i encountered is when you scroll, only 'touchstart' event is fired, this might due to the fact that i addEventListener for 'touchmove' event only if 'touchstart' is fired. At this point 'touchmove' already bind to the DOM, which means it would prevent you from scrolling. So what i did was when the 'touchstart' is fired i check to see if this is a scrolling or not; if not i removeEventListener for 'touchmove' event.

The final issue, also the one that i beat myself the most. Since i only fire the function bind to 'touchstart' after a specific time say 1s, i used setTimeout to achieve this. When 'touchstart' is fired and if 'touchend' is not fired, doesn't matter how long 'touchstart' is held (how long you keeo your finger on the screen at one place), the function inside setTimeout will fire anyway. So what i did was instead of addEventListener for 'touchend' event when 'touchstart' is fired, i addEventListener for 'touchend' right after the DOM starts.

Below is the code i described:

Init variables

<pre>// timer for setTimeout

var timer = null;	

// Get position of the scrollbar. We need 2 variables 
//because one is the first before touchstart is fired, one is after.

var vscroll = (document.all ? document.scrollTop : window.pageYOffset);
var vscroll_after = (document.all ? document.scrollTop : window.pageYOffset);</pre>

touchmove function

		
<pre>//touchmove function
var touchmovefunction = function(e){
	// this is needed for touchend to fire 
	//but it will disable scrolling	
	e.preventDefault();	
	
	// execute wanted function for touchmove event	
	execute_move();
	return false;
}</pre>

touchend function

<pre>//touchend function
var touchendfunction = function(e){
	// unbind touchmove so that users can scroll again	
	document.removeEventListener('touchmove',touchmovefunction,false);
	
	//clearTimeout	
	clearTimeout(timer);
	
	// execute wanted function for touchend event	
	execute_end();
	return false;
}</pre>

touchstart function

		
<pre>//touchstart function
var touchstartfunction = function(e){

//setTimeout, only execute after 1000 ms
timer = setTimeout(function(){

	//bind touchmove event	
	document.addEventListener("touchmove",touchmovefunction, false);
	
	//get scrollbar position after touchstart is fired
	vscroll_after = (document.all ? document.scrollTop : window.pageYOffset);
	
	//check if scrollbar position is changed compare to the first time
	if(vscroll !== vscroll_after) {
		
		//if it is different it means users scrolling unbind touchmove event
		document.removeEventListener('touchmove',touchmovefunction,false);
		
		//assign scrollbar position to vscroll
		vscroll = vscroll_after;
		return;
	}
	
	//execute wanted function for touchstart event
	execute_start();
	return false;
},1000);
return false;		
}</pre>

bind touchstart and touchend to DOM

	
<pre>//bind event to DOM
document.addEventListener("touchend",touchendfunction, false);	
document.addEventListener("touchstart",touchstartfunction,false);</pre>

<h3>Future Improvement</h3>

More browsers will be supported in the future. Currently although Jrecog does detect if the browser is supported or not if not it would not init. But i would recommend to use another third party library such as modernizr. Feel free to use any images or usage description from this page for your products.

Go to the <a href="http://jrecog.co.nf/">main page</a> to see how it works and more information, specially check out the interactive example on your touch devices.
