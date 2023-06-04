Basic Programming
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For now, we just need to get data from the camera to your robot. Limelight posts targeting data to Network Tables at 100hz. The default update rate for NetworkTables is 10hz, but Limelight automatically overwrites it to allow for more frequent data transfer.

To get started, we recommend reading four values from the "limelight" Network Table as frequently as possible. Our code samples will show you exactly how to do this. The offsets to your target (in degrees) are sent as "tx" and "ty". These can be used to turn your robot, turn a turret, etc. The target's area, sent as "ta", may be used a rough indicator of distance to your target. Area is a value between 0 and 100, where 0 means that your target's hull area is 0% of the total image area, and 100 means that your target's hull fills the entire image. The rotation or "skew" of your target is returned as "ts". If all values are equal to zero, no targets exist.

In addition, you may control certain features by setting values in NetworkTables. See the complete NT API here: :doc:`networktables_api`

Read the following from the "limelight" table

=========== =====================================================================================
tv		Whether the limelight has any valid targets (0 or 1)
----------- -------------------------------------------------------------------------------------
tx	 	Horizontal Offset From Crosshair To Target (-27 degrees to 27 degrees)
----------- -------------------------------------------------------------------------------------
ty 		Vertical Offset From Crosshair To Target (-20.5 degrees to 20.5 degrees)
----------- -------------------------------------------------------------------------------------
ta 		Target Area (0% of image to 100% of image)
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

		Don't forget to add these imports:

		.. code-block:: java

			import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
			import edu.wpi.first.networktables.NetworkTable;
			import edu.wpi.first.networktables.NetworkTableEntry;
			import edu.wpi.first.networktables.NetworkTableInstance;


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

		
	.. tab:: LabView

		.. image:: img/Labview_10.png

	.. tab:: C++
		
		Don't forget to add these #include directives:

		.. code-block:: c++

			#include "frc/smartdashboard/Smartdashboard.h"
			#include "networktables/NetworkTable.h"
			#include "networktables/NetworkTableInstance.h"
			#include "networktables/NetworkTableEntry.h"
			#include "networktables/NetworkTableValue.h"
			#include "wpi/span.h"

		.. code-block:: c++

			std::shared_ptr<nt::NetworkTable> table = nt::NetworkTableInstance::GetDefault().GetTable("limelight");  
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
