Case Study: Estimating Distance
===============================
* :ref:`Fixed-Angle-Camera`
* :ref:`Using-Area`

.. Fixed-Angle-Camera:
Using a Fixed Angle Camera:
~~~~~~~~~~~~~~~~~~~~~~~~~~~
If your vision tracking camera is mounted on your robot in such a way that its angle between the ground plane and its line of sight does not change then you can use this technique to very accurately calculate the distance to a target.  You can then use this distance value to either drive your robot forward and back to get into the perfect range or you can use it to adjust the power of a shooting mechanism.  

See the below diagram.  In this situation all of the variables are known: the height of the target (h2) is known because it is a property of the field.  The height of your camera above the floor (h1) is known and its mounting angle is known (a2).  The limelight (or your vision system) will tell you the value of angle a2.

.. image:: img/DistanceEstimation.jpg
	:align: center

We can solve for d using the following equation:

tan(a1+a2) = (h2-h1) / d

d = (h2-h1) / tan(a1+a2)

When using this technique it is important to choose the mounting angle of your camera carefully.  You want to be able to see the target both when you're too close and too far away.  You also do not want this angle to change so mount it securely since you'll have to adjust all of your numbers if it moves.

If you are having trouble figuring out what the angle a1 is, you can also use the above equation to solve for a1.  Just put your robot at a known distance (measuring from the lens of your camera) and solve the same equation for a1.

In the case where your target is at nearly the same height as your camera, this technique is not useful.

.. Using-Area:
Using Area to Estimate Distance:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

WIP

