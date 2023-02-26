Best Practices
============================

Before An Event
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Download and backup all pipelines to your programming laptop.
* Download a copy of the latest Limelight image to your programming laptop.
* Record a list of your pipelines and their indices.
    * 1 - Dual Target Low
    * 2 - Dual Target High Cargo
* Add strain reliefs to all power and ethernet cables going to your LL.
* Consider hot-gluing all connections.
* Make sure you are using a dashboard (Smartdashboard, Shuffleboard) and not a web browser to view the stream while practicing and during events. Default web controls don't have the ability to auto-reconnect to streams after spurious disconnections, but SmartDashboard and Shuffleboard both have auto-reconnection built-in by default.


* Add a network switch to your robot to enable ethernet tethering while at an event and to avoid the second radio port. We recommend the `Branboxes SW-005 5 port Switch <https://www.amazon.com/BRAINBOXES-SW-005-Brainboxes-Unmanaged-Ethernet/dp/B07PRZ2R1P/>`_ 
* Use twisted Cat6 cables with stranded wires. We do not recommend the use of flat ethernet cables.
* We do not recommend use of the second radio port. Route all devices through your network switch if possible.
* Setup `Port Forwarding <https://docs.wpilib.org/en/latest/docs/networking/networking-utilities/portforwarding.html>`_ to enable Limelight communication while tethered to your robot over USB.
    * Forward ports 5800, 5801, 5802, 5803, 5804, and 5805

.. tabs::

    .. tab:: Java

        .. code-block:: java

                    import edu.wpi.first.wpiutil.net.PortForwarder;
                    @Override
                    public void robotInit() 
                    {
                        // Make sure you only configure port forwarding once in your robot code.
                        // Do not place these function calls in any periodic functions
                        for (int port = 5800; port <= 5805; port++) {
                            PortForwarder.add(port, "limelight.local", port);
                        }
                    }

    .. tab:: C++

        .. code-block:: c++
                
            #include <wpi/PortForwarder.h>
            void Robot::RobotInit 
            {
                for (int port = 5800; port <= 5805; port++) {
                    wpi::PortForwarder::GetInstance().Add(port, "limelight.local", port);
                }
            }
                    


During Event Calibration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Roll your robot to each target on the field.
    * Make sure your thresholding is working properly. Switch to the "threshold" view during this process (located under the image stream).
    * Roll your robot close to the target, and far away from the target. Ensure crosshairs are calibrated properly.
    * While far away from the target, rotate your robot left and right ~ 30 degrees to ensure that other targets will not be erroneously tracked.
    * See the tuning section below for more tuning tips. 
    * Ensure no other field / arena elements are being accidentally tracked. Check your area and aspect ratio filters if you are picking up arena lights.
    * Take snapshots of all targets and erroneous targeting. You can use these to tune your pipelines in the pits.


Pipeline Tuning
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Use the lowest exposure possible, and increase black level offset until field lights and LED reflections are removed from the image.
* Test your thresholding while far away and angled away from your target.
* Use 2019.7's "Smart Speckle Rejection" to filter unwanted LED reflections


Before Connecting to the Field
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Give your laptop a static IP configuration.
    * IP: 10.TE.AM.5
    * Subnet Mask: **255.0.0.0**
    * Gateway: 10.TE.AM.1
* Give your RIO a static IP configuration.
    * IP: "10.TE.AM.2"
    * Subnet Mask: **255.255.255.0** **<- NOTE THE DIFFERENCE HERE**
    * Gateway: 10.TE.AM.1
* Give your Limelights unique hostnames (if using multiple).
* Give your Limelights unique static IP configurations.
    * Always start with ".11" addresses and go upward. (10.9.87.11, etc.)
    * **The use of other addresses may cause your units to malfunction when connected to the FMS.**
    * IP: "10.TE.AM.11"
    * Subnet mask: **255.255.255.0**
    * Gateway: "10.TE.AM.1"

NOTE: Teams with zeros need to pay special attention:
    * Team 916 uses 10.9.16.xx,
    * Team 9106 uses 10.91.6.xx
    * Team 9016 uses 10.90.16.xx

Additional information: https://docs.wpilib.org/en/stable/docs/networking/networking-introduction/ip-configurations.html

Before Every Match
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Check all power and Ethernet cables going to your Limelights.
* Check all electrical connections for looseness and frayed wires.
* Check all mounting screws / zipties / tape.
* Observe ESD precautions at all times.

Bandwidth
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Some teams run two Limelights with two USB cameras while staying well under under the bandwidth limit. Follow the steps below to reduce bandwidth.
* Rather than using driver mode, create a "driver" pipeline. Turn down the exposure to reduce stream bandwidth.
* Using a USB camera? Use the "stream" NT key to enable picture-in-picture mode. This will dramatically reduce stream bandwidth.
* Turn the stream rate to "low" in the settings page if streaming isn't critical for driving.
* Use the 160x120 stream option introduced in 2019.7.

Troubleshooting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Try to access the stream at <IP>:5800 with a web browser. This should help you determine the root of your issues.
* Restart your dashboard
* Reboot your computer
* Reboot your robot if the field has been reset
* Broken Ethernet cables can be the cause of intermittent networking issues.
* Always use static IP configurations on the field.