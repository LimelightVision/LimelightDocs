Frequently Asked Questions
===============


Does Limelight support Active POE?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The Limelight camera only supports passive POE.  Active POE requires a handshake between the sending and receiving POE devices.  Limelight is designed to work with a passive POE injector.  Just supply 12V by wiring the passive POE injector right into your PDP.

Should I plug the Limelight into our VRM (Voltage Regulator Module)?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The Limelight camera is designed to be connected to the main robot battery.  It contains its own voltage regulators. 

My robot has extreme voltage swings while driving, will this damage my Limelight?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We specifically designed the Limelight camera to handle the "noisy" power available on a typical FRC robot.  It contains the voltage regulators needed to generate the voltages it needs to operate as long its input voltage is above 6V.   

Will the Limelight LEDs dim when our robot's voltage drops?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We designed the LED array on the Limelight to produce a consistent brightness all the way down to 7.5V.  So your tracking should remain consistent even when your robot motors are putting a heavy load on the battery.  This can be an important feature for robots with high-traction drivetrains that aim by turning in place.
