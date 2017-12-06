Theory
===============
* :ref:`Vision-Targets`
* :ref:`Thresholding`
* :ref:`Contour-Filtering`
* :ref:`Output`

.. Vision-Targets:
Vision Targets
~~~~~~~~~~~~~~
The FRC game designers often place reflective "vision targets" on the field in strategic locations.  These vision targets are usually made out of retro-reflective tape.  Usually the major scoring elements of the field will have vision targets that can be used to automatically aim.  Below you can see two examples of some of the vision targets from the 2016 and 2017 FRC games.

.. image:: img/VisionTargetExamples.jpg
	:align: center

These retro-reflective vision targets have a very useful property: when light is shined at them, it will reflect directly back to the light source.  This is why the Limelight camera has bright green LEDs surrounding its camera lens.  By setting the camera exposure very low while emitting a bright green light toward the target, we can aquire an image that is mostly black with a bright green vision target.  This makes the job of aquiring the target relatively easy.

Here you can see an example of the type of image you want to get.  Notice how almost all of the detail in the image is gone due to the low exposure setting but the retro-reflective tape stands out brightly.

.. image:: img/VisionTargetExample2.jpg
	:align: center

.. _Thresholding:

Thresholding
~~~~~~~~~~~

----------

Thresholding is the next critical component of most FRC vision tracking algorithms. It is the act of taking an image, and throwing away any pixels that aren't in a specific color range. The result of thresholding is generally a one-dimensional image in which a pixel is either "on" or "off.  Thresholding works very well on images that are captured using the above strategy (low exposure, very dark image with a brightly illuminated vision target)

The Limelight camera does thresholding in the HSV (Hue-Saturation-Value) colorspace.  You may be used to thinking of colors in the RGB (Red-Green-Blue) colorspace.  HSV is just another way of representing color similar to the way cartesian coordinates or polar coordinates can be used to describe positions.  The reason we use the HSV colorspace is that the Hue can be used to very tightly select the green color that the limelight leds output.  

.. image:: img/HSVImage.png
	:align: center

It is critical to adjust your thresholding settings to eliminate as much as you can from the image (other than the vision targets).  You will get the best results if you optimize each stage of your vision pipeline before moving to the next stage.  Sometimes things like ceiling lights or windows in an arena can be difficult to remove from the image using thresholding which brings us to our next stage.

TODO:  Add image of a thresholded target here
 
----------

.. _Contour-Filtering:

Contour Filtering
~~~~~~~~~~~

----------

After thresholding, the Limelight vision pipeline generates a set of contours for the image.  A contour in is a curve surrounding a contiguous set of pixels.  Sometimes things like ceiling lights, arena scoreboards, windows and other things can make it past the thresholding step.  This is where contour filtering becomes useful.  The goal is to eliminate any contours which we know are not the target we are interested in.  

The first and easiest countour filter is to ignore any contours which are smaller than what our vision target looks like from our scoring distance.  Anything smaller than that size is obviously something farther away and should be ignored.  This is called area filtering.

The FRC vision targets often have some geometric property that can be exploited to help us filter contours.  For example, if the vision target has a wide aspect ratio, we can filter out any contours that are not wide:

.. image:: img/Target1.jpg 
	:align: center

However, keep in mind that your camera may be looking at the target from an odd angle.  This can drastically affect the aspect ratio of its contour.  Be sure to test your settings from a variety of angles to ensure that you do not filter too aggressively and end up ignoring the vision target!

.. image:: img/Target3.jpg
	:align: center

This next image target is very interesting.  It is one of the best designed vision targets in FRC (in my opinion).  Limelight automatically calculates a value called the **fullness** of a contour.  **Fullness** is the ratio between the pixel area of the contour to its convex area.  This particular shape has a very low fullness and you almost never see any ceiling lights, windows, etc with such a low fullness.  So you can very effectively filter out the unwanted contours if your vision target looks like this one.

.. image:: img/Target0.jpg 
	:align: center

Limelight has many options for filtering contours.  You can use these options along with what you know about the geometry properties of the particular vision target you are trying to track.

From Pixels to Angles
~~~~~~~~~~~~~~~~~~~~~
The end result of the vision pipeline is a pixel location of the best contour in the image.  For most games, we can just aim at the center of the contour.  Sometimes it is also useful to aim at the top-center or some other point but essentially we have a pixel coordinate for where we want to aim.  In order to compute the angles to this target, we need to use a little bit of trigonometry.

(WIP)
