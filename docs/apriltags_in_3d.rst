(ADVANCED) 3D AprilTags
==============================================================

There are three levels of 3D AprilTag tracking in Limelight OS:

* Point of interest tracking (Easy to use, requires zero code changes, compatible with "tx" and "ty")
* Full 3D Tracking
* Robot Localization
* All 3D data is accessible via direct NetworkTables or JSON.

Point-of-Interest Tracking
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

--------------------------------------------

Point-of-Interest tracking allows you to define a 3D point of interest relative to an AprilTag.

Let's say you are trying to target a field feature that is 6 inches to the left and 2 inches behind an AprilTag. You can simply define that point of interest
in the web interface (in meters), and then track this 3D point using tx and ty as if it existed as a real-world target.

.. image:: https://thumbs.gfycat.com/CoarseUnluckyArchaeocete-size_restricted.gif
    :width: 100%
 

Full 3D Tracking
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

--------------------------------------------

Full 3D tracking is accessible though the "campose" networktables array and through the json results output. In the "visualizer" section on the "Advanced" tab,
you will find several different visualizers that will help you understand the purpose of each of the available transforms in the json dump. In general,
the most useful transforms will be "Camera Transform in Target Space", and "Robot Transform in Target Space". See the coordinate system doc for more details.(DOCS WIP)

.. image:: https://thumbs.gfycat.com/ImpressionableNaturalHen-size_restricted.gif
    :width: 100%

.. image:: https://thumbs.gfycat.com/FineColorlessBeardeddragon-size_restricted.gif
    :width: 100%

Robot Localization (botpose and MegaTag)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

--------------------------------------------

If your Limelight's robot-space pose has been configured in the web ui, and a field map has been uploaded via the web ui, then the robot's location in field space
will be available via the "botpose" networktables array (x,y,z in meters, roll, pitch, yaw in degrees). 

.. image:: https://thumbs.gfycat.com/ForthrightUnfinishedIridescentshark-size_restricted.gif
    :width: 100%

Our implementation of botpose is called MegaTag. If more than one tag is in view, it is resilient to individual tag ambiguities and noise in the image.
If all keypoints are coplanar, there is still some risk of ambiguity flipping.

* Green Cylinder: Individual per-tag bot pose
* Blue Cylinder: Old BotPose
* White Cylinder: MegaTag Botpose

.. image:: https://thumbs.gfycat.com/ConfusedQuerulousLiger-size_restricted.gif
    :width: 100%

Notice how the new botpose (white cylinder) is extremely stable compared to the old botpose (blue cylinder). You can watch the tx and ty values as well.

This is not restricted to planar tags. It scales to any number of tags in full 3D and in any orientation. Floor tags and ceiling tags work perfectly.

Here’s a diagram demonstrating one aspect of how this works with a simple planar case. 
The results are actually better than what is depicted, as the MegaTag depicted has a significant error applied to three points instead of one point. 
As the 3D combined MegaTag increases in size and in keypoint count, its stability increases.

.. image:: https://downloads.limelightvision.io/documents/MEGATAG.png


Using WPILib's Pose Estimator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The latest images for Limelight publish targeting latency and capture latency in milliseconds. You can access them with the "tl" and "cl" NT keys, or with LimelightHelpers.getLatency_Pipeline() and LimelightHelpers.getLatency_Capture() if you are using Limelight Lib. You can also get the combined latency by accessing the 7th value in the botpose array.

pseudocode for the "latency" component of WPILib' addVisionMeasurement():

Timer.getFPGATimestamp() - (tl/1000.0) - (cl/1000.0)
or
Timer.getFPGATimestamp() - (botpose[6]/1000.0)


Configuring your Limelight's Robot-Space Pose
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

--------------------------------------------

LL Forward, LL Right, and LL Up represent distances along the Robot's forward, right, and up vectors if you were to embody the robot. (in meters).
LL Roll, Pitch, and Yaw represent the rotation of your Limelight in degrees. You can modify these values and watch the 3D model of the Limelight change in the 3D viewer.
Limelight uses this configuration internally to go from the target pose in camera space -> robot pose in field space.  
