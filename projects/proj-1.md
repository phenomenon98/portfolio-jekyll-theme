---
layout: post
title: 'Smart Bin for Garbage Segregation'
---

We produce unbelievable amounts of garbage and still do not have proper measures in place to deal with it. The amount of waste generated has tripled since 1960, leading to devastating effects for the environment. Segregating waste for further processing is the first major bottleneck in the recycling process. This task is usually carried out by hand. Automating it would greatly help reduce the cost and time taken to recycle items.

This project aims to solve the fundamental problem of waste segregation using **Deep Learning**. We want to develop Computer Vision technologies that can classify the category that individual garbage pieces belong to. This can then be implemented in smart waste disposal systems, either in the form of smart bin that immediately sorts garbage into the category it belongs, or as a additional module in existing garbage processing facilities.
<br><br><br>
### Work Done So Far


#### Literature Review
 Looked into existing smart waste disposal systems and identified potential improvements that could be made. Shortlisted various Image processing and Deep Learning techniques that may help us.

### Obtained Initial Dataset
 Dataset was btained from the trashnet Github repository (https://github.com/garythung/trashnet). Contains images split into 6 different classes: glass, paper, cardboard, plastic, metal, and other trash . Currently, the dataset consists of 2527 images: <br>
 * 501 glass 
 * 594 paper 
 * 403 cardboard 
 * 482 plastic 
 * 410 metal 
 * 137 trash 

 The pictures were taken by placing the object on a white poster-board and using sunlight and/or room lighting . The pictures have been resized down to 512 x 384. The devices used were Apple iPhone 7 Plus, Apple iPhone 5S, and Apple iPhone SE. 

### Model v1
We attempted training our dataset on existing Neural architectures such as VGG19. However due to computational constraints this approach was soon abandoned. We tried training the data on simple CNNs built from scratch but the accuracy was too low.

### Model v2
We decided to use Transfer Learning as the primary approach for designing our model. Used Resnet50 pretrained on Imagenet and used Pytorch to retrain the final layer on our dataset. This gave us a very good test accuracy of 94%. This could be improved further with a better pretrained model and by finetuning our hyperparameters.
!["Confusion Matrix"]("projects/proj-1/cm.png")
<!--
	add f1 score
-->
### Model Deployment
Deployed our model to a simple [website](https://recycle.onrender.com) that can be used for demonstration and testing. 
<!---
pic of ui
-->

### Designing a Smart Bin Prototype
<br>

The high level idea is that it should look and work like a regular garbage bin. 
#### Hardware Used
* Raspberry Pi 3B
* 720p Camera Module
* Servo motors
* Connecting Cables
* Battery Pack

#### Working

When a user drops a waste object into the smart bin, it lands on a staging area. The staging area has a white background and is illuminated to have similar background conditions as the dataset. Motion is detected by a Python script running on the Raspberry Pi. This is done by taking low resolution images continously and comparing the images for differences(only the green channel is compared as it has the highest amount of information). If the difference is above a threshold value, an image is captured. This image is then analyzed by the model which classifies it into one of 6 categories. This information is passed on to the servo motors which tilt the staging platfom and guide the garbage into a containment area. Simaltaneously, the image and the corresponding label is uploaded to a database. This is done to augment the training data, enabling the model to become more accurate the more it is used.

#### Model v3

Running our model on the Raspberry Pi to classify images in real-time was impractical. The Pi would heat up and would take too long to predict the class of an image. Due to computational constraints we had to sacrifice accuracy for speed and created a new model by performing Transfer Learning on MobileNet v2. We then exported this to a Ternsorflow Lite model. This resulted in our model classifying our images almost instantaneously, although accuracy was affected. Hopefully accuracy can be improved with hyperparameter tuning and dataset augmentation.

<!--
	gif of it working
	accuracy and f1
-->

#### Dataset Augmentation

After every classification the image, coresponding label and a timestamp is sent to a Firebase database. This allows us to retrain our model on images taken by our camera in our lighting and background conditions, which should significantly improve our accuracy.

### Use in conveyor belt