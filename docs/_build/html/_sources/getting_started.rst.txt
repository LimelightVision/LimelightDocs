Getting Started
===============

This page will cover the content on the official website's "Getting Started" page.

* :ref:`mounting`
* :ref:`wiring`
* :ref:`networking`
* :ref:`programming`


.. _mounting:

Mounting
~~~~~~~~~~~~~~~~~~~

Use four 1 1/4" 10-32 screws with nylock nuts to mount your Limelight. 

.. _wiring:

Wiring
~~~~~~~~~~~

.. note:: Limelight takes a 12V input, but is built to function down to 6V. Its LEDs have a constant brightness down to 9.6v in the 6 LED beta iteration. In the 8 LED iteration, LEDs have constant brightness down to 7V.

Standard Wiring
---------------
* Do not run wires to your VRM.
* Run two wires from your limelight to a slot on your PDP.
* Add a 5A breaker to the same slot on your PDP.
* Run an ethernet calbe from your Limelight to your robot radio.

Power-over-Ethernet (PoE) Wiring
--------------------------------
.. note:: PoE allows you to add both power and network connectivity to your Limelight via an Ethernet cable. 
.. warning:: This is not standard 44V PoE - this is why you must use a passive injector with 12V.
* Ensure that your Limelight's power jumper is set to the "E" position.
* Interface a passive `Passive PoE Injector <http://amzn.to/2he36Dp/>`_. to your PDP.
* Add a 5A breaker to the same slot on your PDP.
* Run an ethernet cable from your Limelight to your passive POE injector.

.. _networking:

Networking Setup
~~~~~~~~~~~
While we reccomend a static IP address for reliability, some teams prefer dynamically assigned IP addresses.

Static IP Address (recommended for competition settings until further testing is completed)
---------
* Power-up your robot, and connect your laptop to your robot's network.
* After your Limelight flashes its LED array, navigate to http://limelight.local:5801. This is the configuration panel.
* Navigate to the "Networking" tab.
* Enter your team number.
* Change your "IP Assignment" to "Static".
* Set your Limelight's IP address to "10.TE.AM.11".
* Set the Netmask to "255.255.255.0".
* Set the Gateway to "10.TE.AM.1".
* Click the "Update" button.
* Power-cycle your robot.
* You will now be access your config panel at http://10.TE.AM.11:5801, and your camera stream at http://10.TE.AM.11:5800
.. This is a comment. Mutli-line notes, warnings, admonitions in general need indented lines after the first line
.. note:: Q. Why do we reccommend a static IP? 

	A. First, it shaves multiple seconds off Limelight's boot time. Second, teams have historically had issues with DHCP assignment and mDNS responders on actual FRC fields.
.. note:: Q. How do I reset the IP address? 

	A. After your Limelight has booted, hold the config button on the front face of the camera until the LEDs start blinking. Power-cycle your robot, and your Limelight will have an automatically-assigned IP address.

Dynamic IP Address
---------
* Power-up your robot, and connect your laptop to your robot's network.
* After your Limelight flashes its LED array, navigate to http://limelight.local:5801. This is the configuration panel.
* Navigate to the "Networking" tab.
* Enter your team number.
* Click the "Update" button.
* Power-cycle your robot.
* You can continue be access your config panel at http://limelight.local:5801, and your camera stream at http://limelight.local:5801

.. note:: While the camera has a NetBIOS name, we highly reccommend installing an mDNS responder such as Apple's Bonjour if you plan on using a Dynamic IP address.


.. _programming:

Basic Programming
~~~~~~~~~~~
For now, we just need to get data from the camera to your robot. Limelight posts targeting data to Network Tables at 100hz. The default update rate for NetworkTables is 10hz, but Limelight automatically overrides it to allow for more frequent data transfer.

To get started, we recommend reading four values from the "limelight" Network Table at at least 100hz. Our code samples will show you exactly how to do this. The offsets to your target (in degrees) are sent as "tx" and "ty". These can be used to turn your robot, turn a turret, etc. The target's area, sent as "ta", may be used a rough indicator of distance to your target. Area is a value between 0 and 100, where 0 means that your target's hull area is 0% of the total image area, and 100 means that your target's hull fills the entire image. The rotation or "skew" of your target is returned as "ts".

Java
---------
.. code-block:: java

	NetworkTables table = NetworkTable.getTable("limelight");
	float targetOffsetAngle_Horizontal = table.getNumber("tx");
	float targetOffsetAngle_Vertical = table.getNumber("ty");
	float targetArea = table.getNumber("ta");
	float targetSkew = table.getNumber("ts");

LabView
---------
Drag the below image into LabView to automatically generate the starter code for Limelight!

.. figure:: Labview_10.png
   :alt: LabView snippet for Limelight Smart Camera
   :align: left
   :figwidth: 100%

C++
-------
.. code-block:: c++

	std::shared_ptr<NetworkTable> table = 	NetworkTable::GetTable("limelight");
	float targetOffsetAngle_Horizontal = table->GetNumber("tx");
	float targetOffsetAngle_Vertical = table->GetNumber("ty");
	float targetArea = table->GetNumber("ta");
	float targetSkew = table->GetNumber("ts"); 

Python
---------
.. code-block:: python

    import pynetworktables
