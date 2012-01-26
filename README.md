Debounced Resize Event for jQuery
=================================

It has always been a pain to deal with cross browser issues of the `window`'s resize event.
According to [PPK](http://www.quirksmode.org/dom/events/resize.html#link1):

*	In IE, Safari, and Chrome many resize events fire as long as the user continues resizing the window.
*	Opera uses as many resize events, but fires them all at the end of the resizing.
*	Firefox fires one resize event at the end of the resizing.

Following Paul Irish's [smartresize plugin](http://paulirish.com/2009/throttled-smartresize-jquery-event-handler/),
here is the smartresize [special event](http://brandonaaron.net/blog/2009/06/4/jquery-edge-new-special-event-hooks)

**NEW**: This project also offers a minimalist, standalone (no jQuery dependency), micro function (91 bytes) for your basic resize debulking needs; see below.

Binding
-------

Simply bind your smartresize event just like a normal resize event. The handler will be executed only once at the end of the resize event:

	$(window).bind("smartresize", function( event ) {
		// Your event handler code goes here.
	});

	// equivalent to...
	$(window).smartresize(function( event ) {
		// Your event handler code goes here.
	
	});

Trigger
-------

To trigger the smartresize event and cancel the debulk timeout (100ms), use the following snippet:

	$(window).smartresize();
	// or...
	$(window).trigger("smartresize", ["execAsap"]);

Event API
---------

Since smartresize is implemented as a special event, it is actually possible to use all usual methods of [jQuery's event API](http://docs.jquery.com/Events).

Minimalist Standalone Version
=============================

Most of the time, I find myself using this plugin just to register a single debulked resize listener on `window`.
As it turns out, all the features I need actually fit in 91 bytes:

  // debulked onresize handler
  function on_resize(c,t){onresize=function(){clearTimeout(t);t=setTimeout(c,100)};return c};

Using it is pretty simple:

  on_resize(function() {
    // handle the resize event here
    ...
  });

Initializing a page (by executing the resize handler when the page loads) couldn't be easier:

  on_resize(function() {
    ...
  })(); // these parenthesis does the trick

No files are provided for this function, simply copy/paste it from this README.

License
=======

MIT licensed http://louisremi.mit-license.org/

Copyright (c) 2012 [Louis-Rémi Babé](http://twitter.com/louis_remi).