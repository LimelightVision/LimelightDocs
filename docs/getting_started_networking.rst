.. _Downloads: https://limelightvision.io/pages/downloads

Networking Setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We highly recommend following the static IP instructions for reliability during events.

Follow these steps before starting:

* Go to add/remove programs in windows, and search for "bonjour"
* How many items do you see?
	* If there are two (2) items with "bonjour" in their names, uninstall "bonjour print services"
	* If there are no (0) items with "bonjour" in their names, install bonjour from our Downloads_ page.
* Reboot your robot and computer.
* Download the  `Limelight Finder Tool <https://limelightvision.io/pages/downloads/>`_
* Follow the steps listed below.

.. tabs::

	.. tab:: Static IP Address (Recommended)

		* Follow the bonjour-related instructions above.
		* Power-up your robot, and connect your laptop to your robot's network.
		* After your Limelight flashes its LED array, open the Limelight Finder Tool and search for your Limelight or navigate to http://limelight.local:5801. This is the configuration panel.
		* Navigate to the "Settings" tab on the left side of the interface.
		* Enter your team number and press the "Update Team Number" button.
		* Change your "IP Assignment" to "Static".
		* Set your Limelight's IP address to "10.TE.AM.11".
		* NOTE: Teams with zeros need to pay special attention:
		* Team 916 uses 10.9.16.xx,
		* Team 9106 uses 10.91.6.xx
		* Team 9016 uses 10.90.16.xx
		* Set the Netmask to "255.255.255.0".
		* Set the Gateway to "10.TE.AM.1".
		* Click the "Update" button.
		* Give your roboRIO the following static IP address: "10.TE.AM.2"
		* Power-cycle your robot.
		* You will now be access your config panel at http://10.TE.AM.11:5801, and your camera stream at http://10.TE.AM.11:5800

	.. tab:: Dynamic IP Address (Not recommended)

		* Follow the bonjour-related instructions above.
		* Power-up your robot, and connect your laptop to your robot's network.
		* After your Limelight flashes its LED array, open the Limelight Finder Tool and search for your Limelight or navigate to http://limelight.local:5801. This is the configuration panel.
		* Navigate to the "Settings" tab on the left side of the interface.
		* Enter your team number and press the "Update Team Number" button.
		* Change your "IP Assignment" to "Automatic".
		* Click the "Update" button.
		* Power-cycle your robot.
		* You can continue be access your config panel at http://limelight.local:5801, and your camera stream at http://limelight.local:5800

.. This is a comment. Mutli-line notes, warnings, admonitions in general need indented lines after the first line
.. note:: Q. Why do we recommend a static IP? 

	A. First, it shaves multiple seconds off Limelight's boot time. Second, teams have historically had issues with DHCP assignment and mDNS responders on actual FRC fields and with event radio firmware.  
	
	We recommend setting static IP addresses on your robo-rio and driverstation as well.  The networking settings to use 
	on all of these devices can be found near the bottom half of this web page:
	https://docs.wpilib.org/en/stable/docs/networking/networking-introduction/ip-configurations.html
	
	
.. note:: Q. How do I reset the IP address? 

	A. After your Limelight has booted, hold the config button on the front face of the camera until the LEDs start blinking. Power-cycle your robot, and your Limelight will have an automatically-assigned IP address.
	
	.. image:: img/limelight_reset.png
			:align: center
			:height: 180
			

* If the above steps do not fix the problem, install Angry IP scanner and find the address for your limelight.
* Go to <limelightaddress>:5801, and give your limelight a .11 static IP.
* From this point onward, you can rely on the static IP to access the page.
