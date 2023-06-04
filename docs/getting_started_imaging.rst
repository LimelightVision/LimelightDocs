Imaging
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow this guide to update your Limelight Smart Camera to the latest version of LimelightOS.


.. _Downloads: https://limelightvision.io/pages/downloads
.. tabs::
	
	.. tab:: Limelight 3

		* Power-off your limelight.
		* Download the latest drivers, image and Balena Flash tool from from the Downloads_ Page.
		* Run a USB-USB-C cable from your laptop to your limelight. Your limelight wiill power on automatically
		* Run "Balena Etcher" as an administrator.
		* It may take up to 20 seconds for your machine to recognize the camera.
		* Select the latest .zip image in your downloads folder
		* Select a "Compute Module" device in the "Drives" menu
		* Click "Flash"
		* Once flashing is complete, remove the usb cable from your limelight.

	.. tab:: Limelight 2

		* Power off your Limelight.
		* Download the latest drivers, image and Balena Flash tool from from the Downloads_ Page.
		* Run a USB-MicroUSB cable from your laptop to your limelight. Your limelight will power on automatically.
		* Run "Balena Etcher" as an administrator.
		* It may take up to 20 seconds for your machine to recognize the camera.
		* Select the latest .zip image in your downloads folder
		* Select a "Compute Module" device in the "Drives" menu
		* Click "Flash"
		* Once flashing is complete, remove the usb cable from your limelight.

	.. tab:: Limelight 1

		.. image:: img/esd-susceptibility-symbol.gif
			:align: center
			:width: 64
			:height: 64
			
		.. warning:: Some versions of Limelight 1 are electrostatically sensitive around the micro-usb port.  To prevent damaging the port, ground yourself to something metal before you connect to the micro usb port.  This will ensure your personal static charge has been discharged.
	
		* Power off your Limelight.
		* Download the latest drivers, image and Balena Flash tool from from the Downloads_ Page.
		* Run a USB-MicroUSB cable from your laptop to your limelight.
		* Turn-on to your limelight.
		* Run "Balena Etcher" as an administrator.
		* It may take up to 20 seconds for your machine to recognize the camera.
		* Select the latest .zip image in your downloads folder
		* Select a "Compute Module" device in the "Drives" menu
		* Click "Flash"
		* Once flashing is complete, remove power from your limelight

.. warning:: Only connect the microUSB cable while imaging. Limelight enters a special flash mode while the microUSB cable is connected. You will not be able to access the web interface while Limelight is in flash mode.
