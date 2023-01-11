(ADVANCED) 3D AprilTags
==============================================================

(DOCS WIP)

There are three levels of 3D AprilTag tracking in Limelight OS:

* Point of interest tracking (Easy to use, requires zero code changes, compatible with "tx" and "ty")
* Full 3D Tracking
* Robot Localization

Point-of-Interest Tracking
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

--------------------------------------------

Point-of-Interest tracking allows you to define a 3D point of interest relative to an AprilTag.

Let's say you are trying to target a field feature that is 6 inches to the left and 2 inches behind an AprilTag. You can simply define that point of interest
in the web interface, and then track this 3D point using tx and ty as if it existed as a real-world target.(DOCS WIP)
 

Full 3D Tracking
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

--------------------------------------------

Full 3D tracking is accessible though the "camtran" networktables array and through the json results output. In the "visualizer" section on the "Advanced" tab,
you will find several different visualizers that will help you understand the purpose of each of the available transforms in the json dump. In general,
the most useful transforms will be "Camera Transform in Target Space", and "Robot Transform in Target Space". See the coordinate system doc for more details.(DOCS WIP)


Robot Localization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

--------------------------------------------

If your Limelight's robot-space pose has been configured in the web ui, and a field map has been uploaded via the web ui, then the robot's location in field space
will be available via the "botpose" networktables key. (DOCS WIP)

