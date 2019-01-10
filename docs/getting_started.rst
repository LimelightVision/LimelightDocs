Getting Started
===============

This page will cover the content on the official website's "Getting Started" page.

* :ref:`mounting`
* :ref:`wiring`
* :ref:`imaging`
* :ref:`networking`
* :ref:`programming`

.. _mounting:

Mounting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tabs::
	
	.. tab:: Limelight 2

		Use four 1 1/2" 10-32 screws and nylock nuts to mount your Limelight.

		.. image:: img/LL2DrawingSmall.png

	.. tab:: Limelight 1

		Use four 1 1/4" 10-32 screws and nylock nuts to mount your Limelight.
		
		.. image:: img/LL1DrawingSmall.png

.. _wiring:

Wiring
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: Limelight takes a 12V input, but is built to function down to 6V. Its LEDs have a constant brightness down to 7V.


.. tabs::
	
	.. tab:: Standard Wiring

		* Do not run wires to your VRM.
		* Run two wires from your limelight to a slot on your PDP (NOT your VRM).
		* Add any breaker (5A, 10A, 20A, etc.) to the same slot on your PDP.
		* Run an ethernet cable from your Limelight to your robot radio.

	.. tab:: Power-over-Ethernet (PoE) Wiring

		.. note:: PoE allows you to add both power and network connectivity to your Limelight via an Ethernet cable. 
		.. warning:: This is not standard 44V PoE - this is why you must use a passive injector with 12V.
		* (LIMELIGHT 1 ONLY) Ensure that your Limelight's power jumper is set to the "E" position.
		* Connect a passive `Passive PoE Injector <http://www.revrobotics.com/rev-11-1210/>`_ to your PDP (NOT your VRM).
		* Add any breaker (5A, 10A, 20A, etc.) to the same slot on your PDP.
		* Run an ethernet cable from your Limelight to your passive POE injector.

.. _imaging:

Imaging
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: Limelight will receive software updates for improvements and features to accomodate the game if necessary.
.. _Downloads: https://limelightvision.io/pages/downloads
.. tabs::
	
	.. tab:: Limelight 2

		* Do not use a Windows 7 machine.
		* Remove power from your limelight.
		* Download the latest drivers, flasher tool, and image from from the Downloads_ Page.
		* Install the Balena Etcher flash tool.
		* Run a USB-MicroUSB cable from your laptop to your limelight.
		* Run "Balena Etcher".
		* It may take up to 20 seconds for your machine to recognize the camera.
		* Select the latest .zip image in your downloads folder
		* Select an "RPI" device in the "Limelights" menu
		* Click "Flash"
		* Once flashing is complete, remove power from your limelight

	.. tab:: Limelight 1

		* Do not use a Windows 7 machine.
		* Remove power from your limelight.
		* Download the latest drivers, flasher tool and image from from the Downloads_ Page.
		* Install the Balena Etcher flash tool.
		* Run a USB-MicroUSB cable from your laptop to your limelight.
		* Apply power to your limelight.
		* Run "Balena Etcher".
		* It may take up to 20 seconds for your machine to recognize the camera.
		* Select the latest .zip image in your downloads folder
		* Select an "RPI" device in the "Limelights" menu
		* Click "Flash"
		* Once flashing is complete, remove power from your limelight

.. warning:: Only connect the microUSB cable while imaging. Limelight enters a special flash mode while the microUSB cable is connected. You will not be able to access the web interface while Limelight is in flash mode.

.. _networking:

Networking Setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We highly reccomend following the static IP instructions for reliability during events.

.. tabs::

	.. tab:: Static IP Address (Recommended)

		* Install bonjour from the Downloads_ Page.
		* Power-up your robot, and connect your laptop to your robot's network.
		* After your Limelight flashes its LED array, navigate to http://limelight.local:5801. This is the configuration panel.
		* Navigate to the "Settings" tab on the left side of the interface.
		* Enter your team number and press the "Update Team Number" button.
		* Change your "IP Assignment" to "Static".
		* Set your Limelight's IP address to "10.TE.AM.11".
		* Set the Netmask to "255.255.255.0".
		* Set the Gateway to "10.TE.AM.1".
		* Click the "Update" button.
		* Power-cycle your robot.
		* You will now be access your config panel at http://10.TE.AM.11:5801, and your camera stream at http://10.TE.AM.11:5800

	.. tab:: Dynamic IP Address (Not recommended)

		* Install bonjour from the Downloads_ Page.
		* Power-up your robot, and connect your laptop to your robot's network.
		* After your Limelight flashes its LED array, navigate to http://limelight.local:5801. This is the configuration panel.
		* Navigate to the "Settings" tab on the left side of the interface.
		* Enter your team number and press the "Update Team Number" button.
		* Change your "IP Assignment" to "Automatic".
		* Click the "Update" button.
		* Power-cycle your robot.
		* You can continue be access your config panel at http://limelight.local:5801, and your camera stream at http://limelight.local:5800

.. This is a comment. Mutli-line notes, warnings, admonitions in general need indented lines after the first line
.. note:: Q. Why do we reccommend a static IP? 

	A. First, it shaves multiple seconds off Limelight's boot time. Second, teams have historically had issues with DHCP assignment and mDNS responders on actual FRC fields and with event radio firmware.
.. note:: Q. How do I reset the IP address? 

	A. After your Limelight has booted, hold the config button on the front face of the camera until the LEDs start blinking. Power-cycle your robot, and your Limelight will have an automatically-assigned IP address.


.. _programming:

Basic Programming
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For now, we just need to get data from the camera to your robot. Limelight posts targeting data to Network Tables at 100hz. The default update rate for NetworkTables is 10hz, but Limelight automatically overwrites it to allow for more frequent data transfer.

To get started, we recommend reading four values from the "limelight" Network Table at at least 100hz. Our code samples will show you exactly how to do this. The offsets to your target (in degrees) are sent as "tx" and "ty". These can be used to turn your robot, turn a turret, etc. The target's area, sent as "ta", may be used a rough indicator of distance to your target. Area is a value between 0 and 100, where 0 means that your target's hull area is 0% of the total image area, and 100 means that your target's hull fills the entire image. The rotation or "skew" of your target is returned as "ts". If all values are equal to zero, no targets exist.

In addition, you may control certain features by setting values in NetworkTables. 

Read the following from the "limelight" table

=========== =====================================================================================
tv		Whether the limelight has any valid targets (0 or 1)
----------- -------------------------------------------------------------------------------------
tx	 	Horizontal Offset From Crosshair To Target (-27 degrees to 27 degrees)
----------- -------------------------------------------------------------------------------------
ty 		Vertical Offset From Crosshair To Target (-20.5 degrees to 20.5 degrees)
----------- -------------------------------------------------------------------------------------
ta 		Target Area (0% of image to 100% of image)
----------- -------------------------------------------------------------------------------------
ts 		Skew or rotation (-90 degrees to 0 degrees)
----------- -------------------------------------------------------------------------------------
tl 		The pipeline's latency contribution (ms) Add at least 11ms for image capture latency.
=========== =====================================================================================

-------------------------------------------------

Write the following to the "limelight" table

=========== =====================================================================================
ledMode		Sets limelight's LED state
----------- -------------------------------------------------------------------------------------
0	 	use the LED Mode set in the current pipeline
----------- -------------------------------------------------------------------------------------
1 		force off
----------- -------------------------------------------------------------------------------------
2 		force blink
----------- -------------------------------------------------------------------------------------
3 		force on
=========== =====================================================================================

=========== =====================================================================================
camMode		Sets limelight's operation mode
----------- -------------------------------------------------------------------------------------
0	 	Vision processor
----------- -------------------------------------------------------------------------------------
1 		Driver Camera (Increases exposure, disables vision processing)
=========== =====================================================================================


=========== =====================================================================================
pipeline	Sets limelight's current pipeline
----------- -------------------------------------------------------------------------------------
0 .. 9		Select pipeline 0..9
=========== =====================================================================================


.. tabs::
	
	.. tab:: Java

		.. code-block:: java

			NetworkTable table = NetworkTableInstance.getDefault().getTable("limelight");
			NetworkTableEntry tx = table.getEntry("tx");
			NetworkTableEntry ty = table.getEntry("ty");
			NetworkTableEntry ta = table.getEntry("ta");
			
			//read values periodically
			double x = tx.getDouble(0.0);
			double y = ty.getDouble(0.0);
			double area = ta.getDouble(0.0);

			//post to smart dashboard periodically
			SmartDashboard.putNumber("LimelightX", x);
			SmartDashboard.putNumber("LimelightY", y);
			SmartDashboard.putNumber("LimelightArea", area);

		Don't forget to add these imports:

		.. code-block:: java

			import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
			import edu.wpi.first.networktables.NetworkTable;
			import edu.wpi.first.networktables.NetworkTableEntry;
			import edu.wpi.first.networktables.NetworkTableInstance;

	.. tab:: LabView

		Drag the below image into LabView to automatically generate the starter code for Limelight. We are working with teams to generate more detailed LabView Docs for 2019.
		NOTE: Prepend a "/" to all NT values. Instead of "limelight/tx", use "/limelight/tx". 

		.. image:: img/Labview_10.png

	.. tab:: C++

		.. code-block:: c++

			std::shared_ptr<NetworkTable> table = NetworkTableInstance::GetDefault().GetTable("limelight");  
			double targetOffsetAngle_Horizontal = table->GetNumber("tx",0.0);
			double targetOffsetAngle_Vertical = table->GetNumber("ty",0.0);
			double targetArea = table->GetNumber("ta",0.0);
			double targetSkew = table->GetNumber("ts",0.0); 

	.. tab:: Python

		.. code-block:: python

		    from networktables import NetworkTables
		    
		    table = NetworkTables.getTable("limelight")
		    tx = table.getNumber('tx',None)
		    ty = table.getNumber('ty',None)
		    ta = table.getNumber('ta',None)
		    ts = table.getNumber('ts',None) 
