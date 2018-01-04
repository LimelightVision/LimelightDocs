Using Multiple Pipelines
===============================

Limelight can store up to ten unique vision pipelines for different goals, different fields, or different robots. Change pipelines mid-match by changing the "pipeline" value in NetworkTables.

To edit multiple pipelines, you must first check the "Ignore NetworkTables Index" checkbox in the web interface. This will force the robot to temporarily allow you to change the pipline index through the webinterface rather than through NetworkTables.

To download your pipelines for backups and sharing, simply click the "download" button next to your pipeline's name. To upload a pipeline, click the "upload" button.

Here's an example of a robot that utilizes two pipelines:


.. image:: https://thumbs.gfycat.com/UnfitLankyHadrosaurus-max-14mb.gif
	:align: center


The first pipeline is tuned to target single vertical stripes. The second pipeline is tuned to find a combo of two horizontal stripes. The code for this robot is available in the "Aim and Range" case study.

Notice that when the robot switches pipelines, the web interface auto-loads the new pipeline. 

