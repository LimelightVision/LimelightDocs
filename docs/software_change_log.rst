Software Change Log
==============================
2017.8 (12/31/17)
~~~~~~~~~~~~~~~~~~~~~
Features
----------------
* New Vision Pipeline interface:
	* Add up to 10 unique vision pipelines, each with custom crosshairs, thresholding options, exposure, filtering options, etc.
	* Name each vision pipeline.
	* Mark any pipeline as the "default" pipeline.
	* Instantly switch between pipelines during a match with the new "pipeline" NetworkTables value. This is useful for games that have multiple vision targets (eg. the gear peg and boiler from 2017). This is also useful for teams that need to use slightly different crosshair options per robot, field, alliance, etc.
	* Download vision pipelines from Limelight to backup or share with other teams.
	* Upload vision pipelines to any "slot" to use downloaded pipelines.
* New Crosshair Calibration interface:
	* "Single" and "Dual" crosshair modes.
	* "Single" mode is what Limelight utilized prior to this update. Teams align their robots manually, and "calibrate" to re-zero targeting values about the crosshair.
	* "Dual" mode is an advanced feature for robots that need a dynamic crosshair that automatically adjusts as a target's area / distance to target changes. We've used this feature on some of our shooting robots, as some of them shot with a slight curve. This feature will also be useful for robots with uncentered andor misaligned Limelight mounts.
 	* Separate X and Y calibration.
* Add Valid Target "tv" key to Network Tables.
* Add Targeting Latency "tl" key to Network Tables. "tl" measures the vision pipeline execution time. Add at least 11 ms for capture time.
* Draw additional rectangle to help explain aspect ratio calculation.
* Remove throttling feature, and lock Limelight to 90fps.
* Disable focusing on most web interface buttons. Fixes workflow problem reported by teams who would calibrate their crosshairs, then press "enter" to enable their robots.
* Post three "raw" contours and both crosshairs to Network Tables.
	* Access a raw contour with tx0, tx1, ta0, ta1, etc.
	* Access both raw crosshairs with cx0, cy0, cx1, cy1.
	* All x/y values are in normalized screen space (-1.0 to 1.0)
* Add "suffix" option to web interface. Allows users to add a suffix to their Limelights' hostnames and NetworkTables (e.g. limelight-boiler). This feature should only be utilized if teams intend to use multiple Limelights on a single robot.
* Display image version on web interface
Optimizations
----------------
* Decrease networking-related latency to ~0.2 ms from ~10ms (Thanks Thad House)
* Move stream encoding and jpg compression to third core, eliminating 10ms hitch (25 - 30ms hitch with two cameras) seen every six frames.
* New Latency testing shows 22 ms total latency from photons to targeting information.
* Upgrade Network Tables to v4 (Thanks Thad House)
* Optimize contour filtering step. Latency no longer spikes when many contours exist.
* Much improved hysterisis tuning.
* Significantly improve responsiveness of webinterface<->limelight actions. 
Bugfixes
------------------
* Fix minor area value inaccuracy which prevented value from reaching 100% (maxed ~99%).
* Fix half-pixel offset in all targeting calculations
* Fix camera stream info not populating for NT servers started after Limelight's boot sequence. Regularly refresh camera stream info.
* Fix bug which caused aspect ratio to "flip" occasionally.
* Force standard stream output (rather than thresholded output) in driver mode.
* Fix bug which prevented LEDs from blinking after resetting Networking information


2017.7 (11/21/17)
~~~~~~~~~~~~~~~~~~~~~
* Improved contour sorting. Was favoring small contours over larger contours. 
* New Coordinate system: Center is (0,0). ty increases as the target moves "up" the y-axis, and tx increases as the target moves "right" along the x-axis.
* More accurate angle calculations (Pinhole camera model).
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

