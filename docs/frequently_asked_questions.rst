Frequently Asked Questions
===============

Why is limelight using a low (320x240) resolution?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Using a lower resolution allows us to run the vision pipeline at a high framrate.  Limelight provides tracking data fast enough that you can use its outputs to drive a PID loop on your robot directly.  This makes integrating vision into your robot extremely simple; limelight is just another sensor!  Additionally, we have used even lower resolutions for the past two years in FRC.  In our final regional, our 2016 robot never mis-aimed a shot while using 160x120 resolution and aiming at one of the most difficult targets FRC has used to-date (the high goal in stronghold).  *All* of its shots were auto-aimed; it was impossible to manually aim that robot. 

What if the game calls for a different tracking algorithm?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We will update the onboard software for the 2018 season.

Why is there an extra usb port?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can add a usb camera to limelight to act as a secondary driver or backup camera. The second camera stream will automatically appear alongside the primary camera stream.  Limelight's video stream uses very little bandwidth so you can safely use this feature for a competitive advantage.

How do I view the video stream?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The video stream exists at http://<Limelight's IP address or limelight.local>:5800. Stream info is also posted to network tables so SmartDashbaord and Shuffleboard (test LV dashboard) will automatically find it.

Are the eight LEDs bright enough?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The total light output of the LEDs is 400 lumens, and the LED cones actually increase the illuminance (functional light) of the device. To compare, the common two-ring setup has a total output of roughly 250 lumens.

Does Limelight support protocols other than NetworkTables?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Limelight also streams tx, ty, ta, and ts over the Serial (UART) pins at the back of the enclosure.

Does Limelight support Active PoE?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Limelight only supports passive PoE.  Active POE requires a handshake between the sending and receiving POE devices.  Limelight is designed to work with a passive POE injector.  Just supply 12V by wiring the passive POE injector right into your PDP.

Should I plug Limelight into our VRM (Voltage Regulator Module)?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Limelight is designed to be connected to the main robot battery.  It contains its own voltage regulators. 

My robot has extreme voltage swings while driving, will this damage my Limelight?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We specifically designed Limelight to handle the "noisy" power available on a typical FRC robot.  It contains the voltage regulators needed to generate the voltages it needs to operate as long its input voltage is above 6V.   

Will Limelight's LEDs dim when our robot's voltage drops?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We designed the LED array on Limelight to produce a consistent brightness all the way down to 7.5V.  So your tracking should remain consistent even when your robot motors are putting a heavy load on the battery.  This can be an important feature for robots with high-traction drivetrains that aim by turning in place.
