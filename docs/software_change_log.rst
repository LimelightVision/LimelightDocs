Image Change Log
==============================

2017.7 (11/21/17)
~~~~~~~~~~~~~~~~~~~~~
* Fix severe bug that broke contour sorting. This caused the favoring of very small contours over larger contours. 
* New Coordinate system: Center is (0,0). ty increases as the target moves "up" the y-axis, and tx increases as the target moves "right" along the x-axis.
* More accurate angle calculations.
* Display targeting info (tx, ty, ta, and ts) on webpage
* Default targeting values are zeros. This means zeros are returned if no target is in view.
* New side-by-side webpage layout. Still collapses to single column on small devices.
* Continuous slider updates don't hurt config panel performance.
* Aspect ratio slider scaled such that 1:1 is centered.

2017.6 (11/13/17)
~~~~~~~~~~~~~~~~~~~~~
* New Imaging tool. Tested on Win7, Win8 and Win10.
* Post camera stream to cameraserver streams. Works with smart dashboard camera streams, but shuffleboard has known bugs here
* Quartic scaling on area sliders, quadratic scaling on aspect ratio sliders. This makes tuning much easier
* Organize controls into “input”, “threshold”, “filter”, and “output” tabs
* Continuous updates while dragging sliders
* Area sent to NT as a percentage (0-100)
* Image size down to 700MB from 2.1GB

2017.5 (11/9/17)
~~~~~~~~~~~~~~~~~~~~~
* Image size down to 2.1GB from 3.9GB
* Add driver mode and led mode apis 
* Set ledMode to 0, 1, or 2 in the limelight table.
* Set camMode to 0 or 1 in the limelight table.
* Add ability to toggle between threshold image and raw image via web interface (will clean up in later release)
* Post camera stream to network tables under CameraPublishing/limelight/streams (will need a hotfix)
* Add skew to targeting information (“ts” in limelight table)
* Add base “CommInterface” in anticipation of more protocols

2017.4 (10/30/17)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Lots of boot and shutdown bullet-proofing
.. dhcpcd and var/log/samba every 20 minutes

2017.3 (10/25/17)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Hue range is 0-179 from 0-255
* Decrease max log size, clear logs, clear apt cache

2017.2 (10/23/17)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Manual ISO sensitivity
* Minimum exposure increased to 2

2017.1 (10/21/17)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Read-only boot partition
* Mount all partitions with noatime option
* Disable all swap functionality (100 mb)
* Auto chmod 777 adminserver and visionserver from rc.local
* Smaller image
.. Removed WiringPi zip
* “Convexity” changed to “Fullness”
* Exposure range set to 0-128 ms from 0-255 ms
* Support two cameras
* Fully support single-point calibration

