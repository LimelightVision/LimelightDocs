Getting Started
===============

This page will cover the content on the official website's "Getting Started" page.

* :ref:`mounting`
* :ref:`wiring`
* :ref:`programming`

.. _mounting:

Mounting
~~~~~~~~~~~~~~~~~~~

Use four 1 1/4" 10-32 screws with nylock nuts to mount your Limelight. 

.. _wiring:

Wiring
~~~~~~~~~~~

.. note:: Limelight takes a 12V input, but is built to function down to 6V. Its LED's have a constant brightness down to 9.6v in the current beta iteration.

Standard Wiring
---------------
* Do not run wires to your VRM.
* Run two wires from your limelight to a slot on your PDP.
* Add a 5A breaker to the same slot on your PDP.
* Run an ethernet calbe from your Limelight to your robot radio.

Power-over-Ethernet (PoE) Wiring
--------------------------------
.. note:: PoE allows you to add both power and network connectivity to your Limelight via an Ethernet cable.
* Ensure that your Limelight's power jumper is set to the "E" position.
* Interface a passive `Passive PoE Injector <http://amzn.to/2he36Dp/>`_. to your PDP.
* Add a 5A breaker to the same slot on your PDP.
* Run an ethernet cable from your Limelight to your passive POE injector.

.. _programming:

Basic Programming
~~~~~~~~~~~
For now, we will grab Limelight's tracking info from Network Tables. Here's an overview of what Limelight posts to Network Tables:
*i

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
