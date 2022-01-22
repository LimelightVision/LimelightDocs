Using Python to create Custom OpenCV Vision Pipelines
====================================================

.. Summary

Limelight has successfully exposed a large number of students to some of the capabilities of computer vision in robotics. With python scripting, teams can now take another step forward by writing their own image processing pipelines.

.. image:: https://thumbs.gfycat.com/SpotlessGlisteningCygnet-size_restricted.gif
    :align: center
    :width: 100%


Limelight handles the hardware, camera interfacing, networking, streaming, and basic image pre-processing. All you need to do is write one python function called runPipeline().
    * One of the most important features we offer is the one-click crosshair. The crosshair, dual crosshair, tx, ty, ta, ts, tvert, and all other standard limelight NetworkTables readings will automatically latch to the contour you return from the python runPipeline() function.
    * Write your own real-time visualizations, thresholding, filtering, and bypass our backend entirely if desired.
        * Limelight’s python scripting has access to the full OpenCV and numpy libraries.
        * Beyond access to the image, the runPipeline() function also has access to the “llrobot” NetworkTables number array. Send any data from your robots to your python scripts for visualization or advanced applications (One might send IMU data, pose data, robot velocity, etc. for use in python scripts)
        * The runPipeline function also outputs a number array that is placed directly into the “llpython” networktables number array. This means you can bypass Limelight’s crosshair and other functionality entirely and send your own custom data back to your robots.
        * Python scripts are sandboxed within our c++ environment, so you don’t have to worry about crashes. Changes to scripts are applied instantly, and any error messages are printed directly to the web interface.

Minimal Limelight Python Script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

    import cv2
    import numpy as np

    # runPipeline() is called every frame by Limelight's backend.
    def runPipeline(image, llrobot):
        # convert the input image to the HSV color space
        img_hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
        # convert the hsv to a binary image by removing any pixels 
        # that do not fall within the following HSV Min/Max values
        img_threshold = cv2.inRange(img_hsv, (60, 70, 70), (85, 255, 255))
    
        # find contours in the new binary image
        contours, _ = cv2.findContours(img_threshold, 
        cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    
        largestContour = np.array([[]])

        # initialize an empty array of values to send back to the robot
        llpython = [0,0,0,0,0,0,0,0]

        # if contours have been detected, draw them 
        if len(contours) > 0:
            cv2.drawContours(image, contours, -1, 255, 2)
            # record the largest contour
            largestContour = max(contours, key=cv2.contourArea)

            # get the unrotated bounding box that surrounds the contour
            x,y,w,h = cv2.boundingRect(largestContour)

            # draw the unrotated bounding box
            cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,255),2)

            # record some custom data to send back to the robot
            llpython = [1,x,y,w,h,9,8,7]  
    
        #return the largest contour for the LL crosshair, the modified image, and custom robot data
        return largestContour, image, llpython
