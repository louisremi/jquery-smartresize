Debounced and Throttled Resize Events for jQuery
================================================

It has always been a pain to deal with cross browser issues of the `window`'s resize event:

* in IE, many resize events fire as long as the user continues resizing the window,
* Chrome an Safari behave like IE, but resize events always fire two by two,
* Firefox used to fire one resize event at the end of the resizing, but now behaves like IE,
* Opera behaves like IE, but fires resize events at a reduced rate.

This project offers two scripts, each providing a special jQuery event that make `resize` more manageable:

* **jquery.debouncedresize.js**: adds a special event that fires once after the window has been resized,
* **jquery.throttledresize.js**: adds a special event that fires at a reduced rate (no more double events from Chrome and Safari).

The [Demo](http://louisremi.github.com/jquery-smartresize/demo/index.html) should help you make your choice.

**Note to previous users**: jquery.debouncedresize.js is the equivalent of the old jquery.smartresize.js, only the name of the special event changes. 
Update is not required unless you want to add jquery.throttledresize.js to a page page that already has jquery.smartresize.js.

Binding / Unbinding
-------------------

Simply bind your special event just like a normal resize event.

	$(window).on("debouncedresize", function( event ) {
		// Your event handler code goes here.
	});

	// or...
	$(window).on("throttledresize", function( event ) {
		// Your event handler code goes here.
	});

	// unbind at will
	$(window).off( "debouncedresize" );

Threshold
---------

Both special events have a `.threshold` option:

* in jquery.debouncedresize.js, it defines the interval used to determine if two `resize` events are part of the same `debouncedresize` event. **Defaults to 150 (milliseconds)**
* in jquery.throttledresize.js, it defines the number of animation ticks (or frames) between each `throttledresize` event. **Defaults to 0 (tick)**, which means that it's going to fire at a maximum of 60fps.

They can be modified globally once the script has been loaded:

    // increase the threshold to 250ms
    $.event.special.debouncedresize.threshold = 250;

    // decrease the firing rate to a maximum of 30fps
    $.event.special.throttledresize.threshold = 1;
    // 2 <=> 20fps, 3 <=> 15fps, ...

(Synchronous) Trigger
---------------------

Triggering those events is achieved using jQuery's standard API:

	$(window).trigger( "debouncedresize" );

It's also possible to execute the handler of any listener synchronously (without the delays):

	$(window).trigger( "throttledresize", [true] );

Minimalist Standalone Version
=============================

Most of the time, I find myself using `debouncedresize` just to register a single listener on `window`.
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