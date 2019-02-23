Software Change Log
==============================

Contact us or post to CD to suggest upgrades for Limelight!

2019.5 (2/9/19)
~~~~~~~~~~~~~~~~~~~~~

With 2019.5 we are introducing the brand new compute3D camera localization feature. Only a handful of teams have ever attempted to add this feature to their vision systems, and now it is available to all Limelight 1 and Limelight 2 users. 

This is not a silver bullet for this year's game. We highly recommend thinking of creative ways to use the standard high-speed 90 fps tracking unless this feature is absolutely necessary.

.. image:: https://giant.gfycat.com/LeftHalfBluewhale.gif

All example gifs were created with an LL2 mounted on the side of a kitbot. This is why you will see slight changes in translation during turns.

Features
----------------
* High-Precision Mode and PnP
	
	* In the following gif, a Limelight 2 was placed 37 inches behind and 14.5 inches to the right of the target.
		* .. image:: https://giant.gfycat.com/ThirstyFailingGreatdane.gif
	* The Limelight was later turned by hand. Notice how the distances remain mostly unchanged:
		* .. image:: https://giant.gfycat.com/DisloyalUnfinishedAntipodesgreenparakeet.gif
	* With 2019.4, we introduced corner sending. This allowed advanced teams to write their own algorithms using OpenCV's solvePNP(). With 2019.5, this is all done on-board.
	* Upload a plain-text csv file with a model of your target. We have pre-built models of 2019 targets hosted on our website. All models must have a centered origin, and use counter-clockwise point ordering with inch scaling.
	* Enable the new high-res 960x720 mode, and then enable "Solve 3D" to aquire the position and rotation of your Limelight relative to your target.
	* Corner numbers are now displayed on the image for easier model creation and threshold tuning.
	* Read all 6 dimensions of your camera's transform (x,y,z,pitch,yaw,roll) by reading the "camtran" networktable number array.

* Black Level

	* With the new black level slider, thresholding is even easier. Increase the black level offset to further darken your images.
	* .. image:: https://giant.gfycat.com/FoolishUnimportantLacewing.gif

Breaking Changes
----------------

* The reported vertical FOV for LL2 has been fixed to match the listed value of 49.7 degrees. This will change your "ty" values

Bug Fixes
----------------

* Fix stream-only crash that could occur when fisheye USB cameras were attached.
* Fix rare hang caused by networking-related driver.
* Corner approximation is now always active.


2019.4 (1/26/19)
~~~~~~~~~~~~~~~~~~~~~

We believe all critical bug reports are covered with this release.

Features
----------------
* Corners
	
	* Send the corners of your target as two arrays (tcornx, tcorny) by enabling "send corners" in the "Output" tab. This will further enable teams that are interested in advanced pipelines with methods like solvePNP().
	* Adjust corner approximation with the "corner approximation" slider in the "Output" tab.

Bug Fixes
----------------

* Fix hang + rare crash that would occur when two targets had exactly the same area, x coordinate, or y coordinate.
* Fix area calculation in dual- and tri-target modes.
* Optimize contour sorting for better performance.

2019.3 (1/19/19)
~~~~~~~~~~~~~~~~~~~~~
2019.3 addresses a number of bugs and feature requests.

Features
----------------
* Stream Rate (bandwidth reduction)
	
	* Set the stream rate to "low" in the settings page for a lower-bandwidth 15fps video stream.

* Raw Contours disabled in NetworkTables (bandwidth reduction)
	
	* Raw contours are now disabled by default. This will further reduce Limelight's overall bandwidth.
	* There are now ~180 fewer networktables entries submitted per second by default.
	* See the new "Raw Contours" pipeline option in the "Output" tab to re-enable raw contours. 

* Active Pipeline Index
	
	* Read the networktable key "getpipe" to get the true active pipeline index of the camera. This is updated at 90hz.
	* The active pipeline index is now written to the videostream underneath the FPS display.

* Left and Right Intersection Filters
	
	* Teams may now choose between "left" and "right" dual-contour intersection filters. The "above" and "below" intersection filters were added in 2019.2.

Bug Fixes
----------------

* Fix LabView Dashboard streaming bug introduced in 2019.2
* The webpage no longer requests icon fonts from a the internet. All fonts are stored locally. This should speed up interface loading.
* Reduce "driver mode" exposure.
* Fix "Distance Transform" GRIP implementation
* Fix 20-second communication delays caused by changing the hostname.

2019.2 (1/7/19)
~~~~~~~~~~~~~~~~~~~~~
2019.2 adds new features to better equip teams for the 2019 season.

* Intersection Filter

	* .. image:: https://thumbs.gfycat.com/ThunderousWholeDinosaur-max-14mb.gif
		:align: center

	* The all-new intersection filter will allow teams to better distinguish between different groups of contours. The filter extends contours to infinity, and checks where they would intersect.

* Direction Filter

	* .. image:: https://thumbs.gfycat.com/HalfUnselfishHarvestmen-max-14mb.gif
		:align: center

	* The new direction filter will allow teams to better distinguish between contours of different orientations.

* Additional Changes and Fixes
	
	* IP Address is auto-posted to SmartDashboard/Shuffleboard for easier event troubleshooting. We have had almost no reports of this being an issue, but this serves as another contingency feature.
	* Pipeline name is auto-posted to SmartDashboard/Shuffleboard
	* Access the width and height of the rough bounding box in pixels with new networktables keys.
	* Access the longest side and shortest side of the perfect bounding box in pixels with new networktables keys.
	* "Sort Mode" now applies to dual-contours
	* "Sort Mode" is now fixed
	* 5802 stream is less taxing on the camera

2019.1 (12/18/18)
~~~~~~~~~~~~~~~~~~~~~
2019.1 fixes all major bugs reported during the beta

* Performance is back to a steady 90fps. A driver issue was the root cause of our performance problems during beta
* IP and hostname settings actually apply and "stick"
* Magic Wands have been re-ordered to match those of Limelight 2018 software
* We now support Grip inputs like masks through the web interface
* NetworkTables freeze fixed

2019.0 (12/10/18)
~~~~~~~~~~~~~~~~~~~~~
With 2019, we are introducing GRIP support, a brand-new interface, and a cleaner flashing procedure.

* Grip Support
	
	* Build pipelines with GRIP For Limelight, and export "LL Script" files to upload to your camera.
	* Masks and NetworkTables support will be added in a future update
	* Expose a bare video stream at http://<limelighturl>:5802 for testing in grip
	
* All-new web interface
	
	* Smaller controls
	* More tooltips
	* Add the ability to turn off the LEDs from the interface
	* Move display combobox underneath stream for easier display switching
	* Faster communication to Limelight. New web tech allowed us to simplify other parts of our code.

* Flashing
	
	* We have migrated to "Balena Etcher"
	* Etcher is twice as fast and works on all platforms
	* Flash popups are fixed with the migration.
	
* Other
	
	* The LED Mode has been modified to allow for pipeline-specific LED Modes. LED MODE 0 is now "pipeline mode", while mode 1 is "force off"
	* Faster boot times in automatic IP assignment mode.
	* Optimizations and minor bug fixes

2018.5 (3/28/18)
~~~~~~~~~~~~~~~~~~~~~
2018.5 fixes a critical issue that would prevent users from tuning pipelines on snapshots.

2018.4 (3/19/18)
~~~~~~~~~~~~~~~~~~~~~
2018.4 adds new contour sorting options. These are fairly important for cube tracking this year, as teams don't necessarily want to track the largest cube in view. In many cases, teams want to track the cube that is closest to their intakes. Many users have had to use the raw contours feature to implement their own sorting, so we want to make this as easy as possible.

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