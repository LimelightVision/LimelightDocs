Best Practices
============================

Before An Event
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Download and backup all pipelines to your programming laptop.
* Download a copy of the latest Limelight image to your programming laptop.
* Record a list of your pipelines and their indecies.
    * 1 - Dual Target Low
    * 2 - Dual Target High Cargo

During Event Calibration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Roll your robot to various targets around the field.
    * Make sure your thresholding is working properly. Switch to the "threshold" view during this process (located under the image stream). 
    * Ensure no other field / arena elements are being accidentally tracked. Check your area and aspect ratio filters if you are picking up arena lights.
    * Take snapshots of all targets and erroneous targeting. You can use these to tune your pipelines in the pits.

Before Connecting to the Field
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
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

Additional information: https://wpilib.screenstepslive.com/s/currentCS/m/troubleshooting/l/319135-ip-networking-at-the-event

Bandwidth
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Some teams run two Limelights with two USB cameras while staying well under under the bandwidth limit. Follow the steps below to reduce bandwidth.
* Rather than using driver mode, create a "driver" pipeline. Turn down the exposure to reduce stream bandwidth.
* Using a USB camera? Use the "stream" NT key to enable picture-in-picture mode. This will dramatically reduce stream bandwidth.
* Turn the stream rate to "low" in the settings page if streaming isn't critical for driving.