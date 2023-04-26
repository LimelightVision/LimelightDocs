Status Lights and Blink Patterns
===============================

Green Status Light
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The green status light will blink slowly if no targets are detected by the current pipeline. It will blink quickly if any targets are detected by the current pipeline.

Yellow Status Light
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The yellow status light will blink when a static IP address has not been assigned. If a static IP address is assigned, the light will remain either consistently on or off, without any blinking.

Green Illumination LEDs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The green illumination LEDs are controllable via the web interface and various APIs, but there are a few special blink patterns that are designed to help troubleshoot hardware and software issues:

* Left/Right or Top/Bottom alternating blink: The internal camera cable has become unseated, or the image sensor has suffered damage.
* Fast Blink (all leds): The networking reset button has been held for at least 10 seconds.
* Repeated Startup Sequence (three blinks, or multiple fade-in fade-out blinks): The software is crashing, possibly due to hardware damage. 

