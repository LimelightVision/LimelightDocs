Building A Pipeline
===============================

To configure a limelight vision pipeline, first access its web interface at http://10.te.am.11:5801. If you have opted to use a dynamically assigned ip-address, access the interface at http://limelight.local:5801.

The "Tracking" page is comprised of five tuning tabs: 

* :ref:`Input`
* :ref:`Thresholding`
* :ref:`Contour-Filtering`
* :ref:`Output`
* :ref:`3D`

----------

.. _Input:

Input Tab
~~~~~~~~~~~~~~~~~~~~~~

----------

The Input Tab hosts controls to change the raw camera image before it is passed through the processing pipeline.

Pipeline Type
---------------------
Controls the desired pipeline type. Change this option to utilize GRIP or Python Pipelines.


Source Image
---------------------
Controls the source of the image that is passed through the pipeline. Switch to "Snapshot" to test your vision pipelines on stored Snapshots.

This control auto-resets to "Camera" when the GUI is closed.

.. image:: https://thumbs.gfycat.com/MintyKaleidoscopicHarpseal-size_restricted.gif
	:align: center
	:width: 100%

Resolution + Zoom
---------------------
Controls the resolution of the camera and vision pipeline. We recommend using the 320x240 pipeline unless you are utilizing 3D functionality.

320x240 pipelines execute at 90fps, while 960x720 pipelines execute at 22 fps.
In 2020, 2x and 3x Hardware Zoom options were added to this field. The zoom options are not digital and use 100% real sensor pixels.

.. image:: https://thumbs.gfycat.com/LawfulRapidArchaeocete-size_restricted.gif

LEDs
---------------------
Controls the default LED mode for this pipeline. This may be overidden during a match with the "LED" network table option.

Limelight 2+ users have access to an "LED Brightness" Slider which allows for LED dimming.

Orientation
---------------------
Controls the orientation of incoming frames. Set it to "inverted" if your camera is mounted upside-down.

.. image:: https://thumbs.gfycat.com/ImpeccableWhichCaracal-size_restricted.gif
	:align: center
	:width: 100%

Exposure
---------------------
Controls the camera's exposure setting in .1 millisecond intervals. Think of a camera as a grid of light-collecting buckets - exposure time controls how long your camera's "buckets" are open per frame. Lowering the exposure time will effectively darken your image. Low and fixed exposure times are crucial in FRC, as they black-out the bulk of incoming image data. Well-lit retroreflective tape will stand out in a mostly black image, turning vision processing into a straightforward process.

.. image:: https://thumbs.gfycat.com/IlliterateRemoteIberianmole-size_restricted.gif
	:align: center
	:width: 100%

Black Level Offset
---------------------
Increasing the black level offset can significantly darken your camera stream. This should be increased to further remove arena lights and bright spots from your image. This is a sensor-level setting, and not a fake digital brightness setting.

.. image:: https://thumbs.gfycat.com/SeparateFatHedgehog-size_restricted.gif
	:align: center
	:width: 100%

Red Balance, Blue Balance
---------------------
Controls the intensity of Red and Blue color components in your image. These collecively control your Limelight's white balance. We recommend leaving these at their default values of


----------

.. _Thresholding:

Thresholding Tab
~~~~~~~~~~~~~~~~~~~~~~

----------------------

Thresholding is a critical component of most FRC vision tracking algorithms. It is the act of taking an image, and throwing away any pixels that aren't in a specific color range. The result of thresholding is generally a one-dimensional image in which a pixel is either "on" or "off.

.. image:: https://thumbs.gfycat.com/MisguidedClumsyCusimanse-size_restricted.gif
	:align: center
	:width: 100%
 
Video Feed (Located beneath stream)
--------------------------------------
Controls which image is streamed from the mjpeg server. You should switch to the "threshold" image if you need to tune your HSV thresholding.

Thresholding Wands
--------------------------------

Wands enable users to click on Limelights's video stream to perform automatic HSV thresholding.
	* The "Eyedropper" wand centers HSV parameters around the selected pixel
	* The "Add" wand adjusts HSV parameters to include the selected pixel
	* The "Subtract" wand adjust HSV paramters to ignore the selected pixel

.. image:: https://thumbs.gfycat.com/AdorableScientificLaughingthrush-size_restricted.gif
	:align: center
	:width: 100%

Hue
--------------------------------
Describes a "pure" color. A Hue of "0" describes pure red, and a hue of 1/3 (59 on the slider) describes pure green. Hue is useful because it doesn't change as a pixel "brightens" or "darkens". This is the most important parameter to tune. If you make your hue range as small as possible, you will have little if any trouble transitioning to an actual FRC field.

.. image:: img/huebar.png 
	:align: center

Saturation
--------------------------------
Describes the extent to which a color is "pure". Another way to think of this is how washed-out a color appears, that is, how much "white" is in a color. Low saturation means a color is almost white, and high saturation means a color is almost "pure".

Value
--------------------------------
Describes the darkness of a color, or how much "black" is in a color. A low value corresponds to a near-black color. You should absolutely increase the minimum value from zero, so that black pixels are not passed through the processing pipeline.

Erosion and Dilation
--------------------------------
Erosion slightly erodes the result of an HSV threshold. This is useful if many objects are passing through a tuned HSV threshold.
Dilation slightly inflates the result of an HSV threshold. Use this to patch holes in thresholding results.


.. image:: https://thumbs.gfycat.com/PastBouncyGnat-size_restricted.gif
	:align: center
	:width: 100%


------------------------------

.. _Contour-Filtering:

Contour Filtering
~~~~~~~~~~~~~~~~~~~~~~

------------------------------

After thresholding, Limelight generates a list of contours. After that, each contour is wrapped in a bounding rectangle an unrotated rectangle, and a "convex hull". 
These are passed through a series of filters to determine the "best" contour. If multiple contours pass through all filters, Limelight chooses the best contour using the "Sort Mode" Control.

Sort Mode
------------------
Controls how contours are sorted after they are passed through all other filters. 

In 2019, the "closest" sort mode was added. This mode will select the target that is closest to the configurable crosshair.

.. image:: https://thumbs.gfycat.com/EnormousImpishDeer-size_restricted.gif
	:align: center
	:width: 100%


Target Area
------------------
Controls the range of acceptable bounding-rectangle areas, as percentages of the screen. You can increase the minimum area to help filter-out stadium lights, and decrease the maximum value to help filter-out things like large displays near the field.

.. image:: https://thumbs.gfycat.com/HairyWarlikeCusimanse-size_restricted.gif
	:align: center
	:width: 100%


.. note:: The area slider is not linearly scaled, but quarticly scaled. This is done to provide extra precision near the lower-end of area values, where many FRC targets lie. The area of a square scales quadratically with its side length, but x^4 scaling provides even greater precision where it is needed.

Target Fullness
------------------
Fullness is the percentage of "on" pixels in the chosen contour's bounding rectangle. A solid rectangle target will have a near-1.0 fullness, while a U-shaped target will have a low fullness.

.. image:: https://thumbs.gfycat.com/AmazingUnfortunateFlyingfish-size_restricted.gif
	:align: center
	:width: 100%

Target Aspect Ratio
---------------------------
Aspect ratio is defined by the width of the bounding rectangle of the chosen contour divided by its height. A low aspect ratio describes a "tall" rectangle, while a high aspect ratio describes a "wide" rectangle. 


.. image:: https://thumbs.gfycat.com/BlankMatureAzurevase-size_restricted.gif
	:align: center
	:width: 100%

.. note:: The aspect ratio slider is also quadratically scaled.

Direction Filter
------------------
Rejects contours on the basis of their orientation. 

.. image:: https://thumbs.gfycat.com/SparklingConcernedCockatoo-size_restricted.gif
	:align: center
	:width: 100%

Smart Speckle Rejection
----------------------------
Rejects relatively small (as opposed to absolutely small w/ the area filter) contours that have passed through all other filters. This is essential if a target must remain trackable from short-range and long-range. 
This feature was introduced in the 2019 season to reject Limelight's LED reflections when robots were very close to targets.

.. image:: https://thumbs.gfycat.com/EachInsecureAustraliansilkyterrier-size_restricted.gif
	:align: center
	:width: 100%


Target Grouping
----------------------------
Controls target "grouping". Set to dual mode to look for "targets" that consist of two shapes, or tri mode to look for targets that consist of three shapes.

Smart Target Grouping can group a variable number of targets and reject outliers. It was added in 2022 to help track the upper hub target.

.. image:: https://thumbs.gfycat.com/HugeCraftyBear-size_restricted.gif
	:align: center
	:width: 100%

Intersection Filter (Dual Targets Only)
------------------------------------------
Rejects groups of contours based on how they would intersect if extended to infinity.

.. image:: https://thumbs.gfycat.com/ThunderousWholeDinosaur-size_restricted.gif
	:align: center
	:width: 100%

Smart Target Grouping
------------------------------------------

Automatically group targets that pass all individual target filters.
	* Will dynamically group any number of targets between -group size slider minimum- and -group size slider maximum-
.. image:: https://thumbs.gfycat.com/WetImmediateEarthworm-size_restricted.gif
		:align: center
		:width: 100%

Outlier Rejection
	* While group targets are more challenging than normal targets, they provide more information and opportunities for filtering. If you know that a goal is comprised of multiple targets that are close to each other, you can actually reject outlier targets that stand on their own.
	* You should rely almost entirely on good target filtering, and only use outlier rejection if you see or expect spurious outliers in your camera stream. If you have poor standard target filtering, outlier detection could begin to work against you!
.. image:: https://thumbs.gfycat.com/CoolQualifiedHedgehog-size_restricted.gif
		:align: center
		:width: 100%



----------

.. _Output:

Output
~~~~~~~~~~~

----------

This tab controls what happens during the last stage of the vision pipeline

Targeting Region
-------------------
Controls the point of interest of the chosen contour's bounding rectangle. By default, the tracking parameters tx and ty represent the offsets from your crosshair to the center of the chosen rectangle. You can use another option if a target changes in size, or is comprised of two targets that sometimes blend together.

.. image:: https://thumbs.gfycat.com/FakeThinAfricanbushviper-size_restricted.gif
	:align: center
	:width: 100%

Send Raw Corners?
-----------------------------------------
Set this control to "yes" to submit raw corners over network tables. Tune the number of corners submitted by adjusting the "Contour Simplification" value in the "Contour Filtering" page.

Send Raw Contours?
-----------------------------------------
Set this control to "yes" to submit raw contours over network tables. The top 3 passing contours will be submitted.

Crosshair Calibration
-------------------------
Controls the "origin" of your targeting values. Let's say a shooter on your robot needs to be calibrated such that it always points a bit left-of-center. You can line up your robot, click "calibrate," and all of your targeting values will be sent relative to your new crosshair. See the calibration page for more details!

.. image:: https://thumbs.gfycat.com/JauntyTerrificGreatwhiteshark-size_restricted.gif
	:align: center
	:width: 100%

-------------

.. _3D:

3D
~~~~~~~~~~~

----------

Experiment with PnP point-based pose estimation here.

.. image:: https://thumbs.gfycat.com/LeftHalfBluewhale-size_restricted.gif
	:align: center
	:width: 100%

Compute 3D
-------------------
Controls whether pose estimation is enabled. You must enable the 960x720 high-res mode for this to work.

Force Convex
-------------------
Use this option to select only the "outermost" corners of a target for SolvePnP.

Contour Simplification
-------------------
Use this option to remove small, noisy edges from the target.

Acceptable Error
-------------------
Limelight will only return a target if it passes a reprojection test with a certain score in pixels.

Goal Z-Offset
-------------------
Automatically the 3D Depth value of your target (Z-Axis).

.. image:: https://thumbs.gfycat.com/AcidicHonoredElephant-size_restricted.gif
	:align: center
	:width: 100%


Camera Matricies (Advanced Users)
-----------------------------------

.. tabs::

	.. tab:: Limelight 2 960x720

		.. code-block:: c++

			cameraMatrix = cv::Matx33d(
						772.53876202, 0., 479.132337442,
						0., 769.052151477, 359.143001808,
						0., 0., 1.0);
			distortionCoefficient =  std::vector<double> {
						2.9684613693070039e-01, -1.4380252254747885e+00,-2.2098421479494509e-03,
						-3.3894563533907176e-03, 2.5344430354806740e+00};

			focalLength = 2.9272781257541; //mm
			
	.. tab:: Limelight 1 960x720

		.. code-block:: c++

			cameraMatrix = cv::Matx33d(
					8.8106888208290547e+02, 0., 4.8844767170376019e+02,
					0., 8.7832357838726318e+02, 3.5819038625928994e+02,
					0., 0., 1.);
			distortionCoefficient =  std::vector<double> {
					1.3861168261860063e-01, -5.4784067711324946e-01,
					-2.2878279907387667e-03, -3.8260257487769065e-04,
					5.0520158005588123e-01 };
			
			focalLength = 3.3385168390258093; //mm
