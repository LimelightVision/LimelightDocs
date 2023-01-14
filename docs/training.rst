Training Your Own Neural Network
==============================================================

While it is possible to train custom object detection models, we don't believe students should spend a single second labelling training data by drawing boxes around objects. 
We have set up all the infrastructure necessary quickly train new models for teams, including a cloud GPU cluster and a human data labelling team.

Classifier models, on the other hand, can be trained with Teachable Machine by Google. It's as easy as dragging-and dropping images into your browser.

https://teachablemachine.withgoogle.com/




----------

Training a Classifier 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
----------
(DOCS WIP)

Navigate to https://teachablemachine.withgoogle.com/

Click "Getting Started" and "Standard image model"

Add as many classes as necessary, but remember to keep it simple.

Upload a broadly diverse set of images per class, click the "Train Model" button.

Test the model using your laptop's webcam.

Once satisfied export the model as a Tensorflow Lite EdgeTPU Model.

Limelight OS 2023.1 will enable the "upload" button for custom models!

----------


----------

Training a Detector 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
----------
(DOCS WIP)

----------