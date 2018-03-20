Software Change Log
==============================
2018.4 (3/19/18)
~~~~~~~~~~~~~~~~~~~~~
2018.4 adds new contour sorting options. This turns out to be quite important for cube tracking this year, as teams don't necessarily want to track the largest cube in view. In many cases, teams want to track the cube that is closest to their intakes. Many users have had to use the raw contours feature to implement their own sorting, so we want to make this as easy as possible.

* Contour Sort Mode

	* Select between "largest", "smallest", "highest", "lowest", "leftmost", "rightmost", and "closest" sort options.
	* We feel that many teams will make use of the "closest" option for cube tracking.
	* .. image:: https://thumbs.gfycat.com/PlaintiveSizzlingEskimodog-max-14mb.gif
	
2018.3 (2/28/18)
~~~~~~~~~~~~~~~~~~~~~
2018.3 fixes a major networktables reconnection bug which would cause NetworkTables settings changes to not propagate to Limelight. Thanks to Peter Johnson and the WPILib team for pinpointing and fixing the underlying NT bug. This was (as far as we know) the last high-priority bug facing Limelight.

Settings changes such as ledMode, pipeline, and camMode should always apply to Limelight. You should no longer need workarounds to change Limelight settings while debugging, after restarting robot code, and after rebooting the roborio.

Changes
----------------
* Fix major NT syncing issue which broke settings changes (ledMode, pipeline, and camMode) during LabView debugging, and after a reset/reboot of the roborio.
* Eye-dropper wand:
	
	* The eye dropper wand uses the same 10 unit window for Hue, but now uses a 30 unit window for saturation and value. This means that thresholding is more often a one-click operation, rather than a multi-step process.
* Snapshots

	* Setting the snapshot value to "1" will only take a single snapshot and reset the value to 0. Snapshotting is throttled to 2 snapshots per second.
	* Snapshot limit increased to 100 images.
	* Snapshot selector area is now scrollable to support 100 images.
	* .. image:: https://thumbs.gfycat.com/ComplexConstantGalapagosalbatross-max-14mb.gif

2018.2 (2/10/18)
~~~~~~~~~~~~~~~~~~~~~
2018.2 fixes all known streaming bugs with various FRC dashboards. It also makes Limelight easier to tune and more versatile during events.

Features
----------------
* Thresholding wands
	
	* Setup HSV threshold parameters in a matter of clicks
	* The "Set" wand centers HSV parameters around the selected pixel
	* The "Add" wand adjusts HSV parameters to include the selected pixel
	* .. image:: https://thumbs.gfycat.com/FarHandyCanvasback-max-14mb.gif
	* The "Subtract" wand adjusts HSV paramters to ignore the selected pixel
	* .. image:: https://thumbs.gfycat.com/HoarseEnragedIslandwhistler-max-14mb.gif

* Snapshots
	
	* .. image:: https://thumbs.gfycat.com/WindyDefiantCrayfish-max-14mb.gif
	* Snapshots allow users to save what Limelight is seeing during matches or event calibration, and tune pipelines while away from the field.
	* Save a snapshot with the web interface, or by posting a "1" to the "snapshot" NetworkTables key
	* To view snapshots, change the "Image Source" combo box on the input tab. This will allow you to test your pipelines on snapshots rather than Limelight's camera feed
	* Limelight will store up to 32 snapshots. It will automatically delete old snapshots if you exceed this limit.

* New Streaming options
	
	* We've introduced the "stream" NetworkTables key to control Limelight's streaming mode. We've received requests for PiP (Picture-in-Picture) modes to better accomodate certain dashboards.
	* 0 - Standard - Side-by-side streams if a webcam is attached to Limelight
	* 1 - PiP Main - The secondary camera stream is placed in the lower-right corner of the primary camera stream.
	* 2 - PiP Secondary - The primary camera stream is placed in the lower-right corner of the secondary camera stream.

* Increase streaming framerate to 22fps

	* Look out for faster streams in an upcoming update

* Erosion and Dilation

	* Enable up to one iteration of both erosion and dilation. 
	* Erosion will slightly erode the result of an HSV threshold. This is useful if many objects are passing through a tuned HSV threshold.
	* Dilation will slightly inflate the result of an HSV threshold. Use this to patch holes in thresholding results.

* Restart Button
	
	* Restart Limelight's vision tracking from the web interface. This is only useful for teams that experience intermittent issues while debugging LabView code.


Optimizations
----------------

* Drop steady-state pipeline execution time to 3.5-4ms.

Bug Fixes
----------------

* Fix Shuffleboard streaming issues
* Fix LabView dashboard streaming issues

2018.1 (1/8/18)
~~~~~~~~~~~~~~~~~~~~~
* Red-Balance slider
* Blue-Balance slider
* Better default color balance settings
* Increased max exposure setting

2018.0 (1/3/18)
~~~~~~~~~~~~~~~~~~~~~
On top of a ton of new case studies, more detailed documentation, and a full example program for an autonomous STEAMWORKS shooter, the software has received a major upgrade.

Features
----------------
* New Vision Pipeline interface:

	* .. image:: https://thumbs.gfycat.com/UnfitLankyHadrosaurus-max-14mb.gif

	* Add up to 10 unique vision pipelines, each with custom crosshairs, thresholding options, exposure, filtering options, etc.
	* Name each vision pipeline.
	* Mark any pipeline as the "default" pipeline.
	* Instantly switch between pipelines during a match with the new "pipeline" NetworkTables value. This is useful for games that have multiple vision targets (eg. the gear peg and boiler from 2017). This is also useful for teams that need to use slightly different crosshair options per robot, field, alliance, etc.
	* Download vision pipelines from Limelight to backup or share with other teams.
	* Upload vision pipelines to any "slot" to use downloaded pipelines.
* Target "Grouping" option:
	* Instantly prefer targets that consist of two shapes with the "dual" grouping mode". "Single" and "Tri" options are also available
	* .. image:: https://thumbs.gfycat.com/ScalyDeficientBrahmanbull-size_restricted.gif
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
* Drop steady-state pipeline execution time to 5ms with SIMD optimizations.

.. image:: img/20180_latency.png	

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

