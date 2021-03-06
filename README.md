## Traffic Sign Classification

Overview
---
In this project, I have used a modified LeNet convolutional neural network to classify traffic signs from a [German dataset](http://benchmark.ini.rub.de/?section=gtsrb&subsection=dataset). 

Project Goals
---
* Dataset summary and visualization.
* Design, train and test a LeNet Architecture.
* Use the model to make predictions on new images & analyze the softmax probabilities of the new images.


### Dataset summary and visualization

Here is the statistics of the dataset. A 80-20% train-validation split is taken.

* Number of training examples   = 31367(80.0%)
* Number of validation examples = 7842(20.0%)
* Number of testing examples    = 12630
* Input Image data shape        = (32, 32, 3)
* Number of classes             = 43

Plotting the distribution, we can observe that both in training and validation set, the % of images per class is roughly the same.
<p align="center">
    <img src="./Distribution.PNG" alt="Image" />
</p>

### Lenet architecture implementation 
#### Data augmentation
As a first step, images from the initial training & validation sets are resized using the given coordinate markers using the function below. NOTE: I have kept this step optional in this project.
```python
def resize(features,labels,coord,size):
```
Then a section of images (3%) per class are shifted in 4 different directions (up, down, left, right) by 3 pixels. NOTE: "Reflect" mode is used as padding during shift operation.NOTE: I have kept this step optional in this project.
```python
def shift_augment(x,shiftval,y):
```
Finally a 80-20% (train-validation) split is obtained.

#### Preprocessing

As part of preprocessing, the images are subjected to the following operations before training the network:

* RGB to Gray scale conversion (In this project, I have kept this step optional)
* Normalization

#### Lenet Architecture
<p align="center">
    <img src="./architecture.PNG" alt="Image" />
</p>

The architecture consists of 2 convolutional layers and 3 fully connected layers. Note the use flattening operation at the end of the 2nd convolutional layer.
A batch size of 128, total number of epochs = 10 & a learning rate of 0.001 are chosen for training the network.

* Layer 1: Convolutional. Input = 32x32x1. Output = 28x28x6.
    * Pooling. Input = 28x28x6. Output = 14x14x6.
* Layer 2: Convolutional. Input = 14x14x6.Output = 10x10x16.
    * Pooling. Input = 10x10x16. Output = 5x5x16.
    * Flatten. Input = 5x5x16. Output = 400.
* Layer 3: Fully Connected. Input = 400. Output = 120.
* Layer 4: Fully Connected. Input = 120. Output =  84.
* Layer 5: Fully Connected. Input =  84. Output =  43.


Running the validation dataset with the above trained model yielded about 97.7% accuracy @ the 10th Epoch.
```python
EPOCH 10 ...
Validation Accuracy = 0.977
Model saved
```

Running the above model on the test dataset yielded about 88.6% accuracy.
```python
INFO:tensorflow:Restoring parameters from ./lenet
Test Accuracy = 0.886
```

#### Testing the model with new images.

The following images are tested with the model. The difficulty in classifying each image is discussed in bold against each image.

<p align="center">
    Speed Limit 20 Km/h <b> - Overwritten text</b>
    <p align="center">
    <img src="./Test_images/20.png" alt="Image" width="50" height="50" /></p>
</p>
<p align="center">
    Stop <b> - Occluded by snow</b>
    <p align="center">
    <img src="./Test_images/stopsnow.PNG" alt="Image" width="50" height="50" /></p>
</p>
<p align="center">
    Ahead Only <b> - Rectangular boundary - not included in training set. Image shifted to right.</b>
    <p align="center">
    <img src="./Test_images/aheadonlyboundshift.PNG" alt="Image" width="50" height="50" /></p>
</p>
<p align="center">
    Bumpy road <b> - Easy. Background object: Post</b>
    <p align="center">
    <img src="./Test_images/bumpy.png" alt="Image" width="50" height="50" /></p>
</p>
<p align="center">
    Roadwork  <b> - Blurred image</b>
    <p align="center">
    <img src="./Test_images/roadwork.png" alt="Image" width="50" height="50" /></p>
</p>
<p align="center"> 
    Traffic signals <b> - Easy</b>
    <p align="center">
    <img src="./Test_images/signals.png" alt="Image" width="50" height="50" /></p>
</p>   

#### Test accuracy
```python
Overall Test accuracy = 100%
```
In the following figures, we can observe the top 5 predictions with their softmax probability scores. All 6 images have been classified correctly.

Though classified correctly, 
* low score observed for Stop sign input as the image is occluded by snow.
* low score observed for Speed limit (20 Km/h) due to overwritten text across the image.
* high score is observed for Ahead only sign even if the image is shifted to right and has a rectangular boundary (training image had a   circular boundary)

<p align="center">
    Speed Limit 20 Km/h
    <p align="center">
    <img src="./Test_images/20_accuracy.PNG" alt="Image"  /></p>
</p>
<p align="center">
    Stop
    <p align="center">
    <img src="./Test_images/snowstop_accuracy.PNG" alt="Image" /></p>
</p>
<p align="center">
    Ahead Only
    <p align="center">
    <img src="./Test_images/aheadonlyboundshift_accuracy.PNG" alt="Image" /></p>
</p>
<p align="center">
    Bumpy road
    <p align="center">
    <img src="./Test_images/bumpyroad_accuracy.PNG" alt="Image"  /></p>
</p>
<p align="center">
    Roadwork 
    <p align="center">
    <img src="./Test_images/roadwork_accuracy.PNG" alt="Image"  /></p>
</p>
<p align="center">
    Traffic signals 
    <p align="center">
    <img src="./Test_images/trafficsignals_accuracy.PNG" alt="Image"  /></p>
</p>   
