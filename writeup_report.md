#**Traffic Sign Recognition** 

## Solution to German Traffic Sign Classification project


---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[pc]: ./projectGraphsAndImages/pieChart.png "Relative sizes of Training, Validation and Test sets"
[fdi]: ./projectGraphsAndImages/fdi.png "Frequency Distribution of Training, Validation and Test sets"
[fdr]: ./projectGraphsAndImages/fdr.png "Relative Distribution of Training, Validation and Test sets"
[nsigns]: ./projectGraphsAndImages/newsigns.png "New Test Signs from Web"

[aftertraining]: ./projectGraphsAndImages/aftertraining.png "Validation and Training Accuracies at each epoch"

[1.png]: ./newSignImages/1.png
[2.png]: ./newSignImages/2.png
[3.png]: ./newSignImages/3.png
[4.png]: ./newSignImages/4.png
[5.png]: ./newSignImages/5.png
[6.png]: ./newSignImages/6.png

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

## Preprocessing

For all images in training, validation and test sets, following normalization is used so that value of each colour channel for each pixel varies from -1.0 to 1.

X = (X-128.0)/128.0

## Model Architecture

I have used a model very similar to that in LeNet lab solution, however, my solution differs in count of neurons in different layers, and unlike LeNet lab solution, I have used dropout for regularization.

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

Here is a link to my [project code](https://github.com/swarnimsoni/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb) along with its HTML version. My project write up is in [writeup_report.md](https://github.com/swarnimsoni/CarND-Traffic-Sign-Classifier-Project/blob/master/writeup_report.md)
nn
### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy library to calculate summary statistics of the traffic
signs data set:

* Number of training examples   = 34799
* Number of validation examples = 4410
* Number of testing examples    = 12630
* Image data shape              = (32, 32, 3)
* Number of classes             = 34799

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a pie chart showing relative sizes of training, validation and testing sets.

![Relative sizes of training, validation and testing sets][pc]

Following graph shows count of each class in training, validation and testing sets:

![Class counts][fdi]

Probability distribution of each class in training, validation and testing sets is quite same. Hence testing and validation sets are good representatives of training set

![Relative distribution][fdr]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I normalized the image data because so that the optimizer can run smoothly and for numerical stability.


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         	|     Description	        		| 
|:---------------------:|:---------------------------------------------:| 
| Input         	| 32x32x3 RGB image				| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x20 	|
| RELU			|						|
| Droput		| 0.75						|
| Max pooling	      	| 2x2 stride,  outputs 14x14x30 		|
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 10x10x30 	|
| RELU			|						|
| Droput		| 0.75						|
| Max pooling	      	| 2x2 stride,  outputs 5x5x30			|
| Flatten		| outputs 750  	       				|
| Fully Connected Layer	| outputs 250					|
| RELU			|						|
| Droput		| 0.75						|
| Fully Connected Layer	| outputs 100					|
| RELU			|						|
| Droput		| 0.75						|
| Final Fully Connected Layer	| outputs 43					|


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used adam optimizer. Following are the hyper paramters:
* learning rate : 0.01
* Epochs : 20
* Dropout rate : 0.75
* Batch size : 128

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

It took me a while to figure out correct number of layers, size of each layer, dropout rate and no. of epochs to achieve 93%+ accuracy on validation set. It involved lots of experiments.

My initial architecture was taken straight from LeNet lab solution, however, it had many limitations e.g. it could only handle grayscale images and number of layers and their sizes were not enough to accurately classify traffic sign. Therefore I had to increase sizes of each layer. And I also observed that the model was overfitting (since validation accuracy was lesser than training accuracy), hence I applied dropout also.

My final model results were:
* training set accuracy of 99.9%
* validation set accuracy of 96.7% 
* test set accuracy of 95.3%


### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are the six German traffic signs that I found on the web:

Speed limit (30km/h)
![Speed limit (30km/h)][1.png]

Bumpy road
![Bumpy road][2.png]

Ahead only
![Ahead only][3.png]

No vehicles
![No vehicles][4.png]

Go straight or left
![Go straight or left][5.png]

General caution
![General caution][1.png]


The most misclassified signs were the speed sign, probably because it was not easy for the model to figure out what is the speed written on the sign (e.g. 50, 80 etc).


#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        	| 
|:---------------------:|:---------------------------------------------:| 
| Speed limit (30km/h)  |         Speed limit (30km/h)			|
| Bumpy road      	|         Bumpy road 				|
| Ahead only            |         Ahead only				|
| No vehicles    	|         No vehicles				|
| Go straight or left   |         Go straight or left			|
| General caution       |         General caution 			|


The model was able to correctly guess 6 of the 6 traffic signs, which gives an accuracy of 100%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 16-17th cell of the Ipython notebook. 

Following are the predictions and their corresponding probabilities for each new web image. My model was able to correctly classify each new image.


Speed limit (30km/h):

| Probabilities        |     Predicted labels				 |
|:---------------------:|:---------------------------------------------:|
| 1.0                  | Speed limit (30km/h)				 |
| 1.6371764388622978e-08 | End of all speed and passing limits		 |
| 1.0936039851472401e-09 | Speed limit (80km/h) |
| 7.00144164866856e-10 | Speed limit (50km/h) |
| 2.359038520083345e-10 | End of speed limit (80km/h) |

Bumpy road:

| Probabilities        |     Predicted labels |
|:---------------------:|:---------------------------------------------:|
| 0.9997866749763489   |           Bumpy road |
| 0.0002128941414412111 |    Bicycles crossing |
| 4.209207986605179e-07 |          No vehicles |
| 1.9457909061770806e-08 |            Road work |
| 6.915800820905815e-09 |    Children crossing |

Ahead only:

| Probabilities        |     Predicted labels |
|:---------------------:|:---------------------------------------------:|
| 1.0                  |           Ahead only |
| 1.2349278544210307e-10 | Go straight or right |
| 5.813883039275727e-14 |     Turn right ahead |
| 2.5894293558871052e-14 | End of no passing by vehicles over 3.5 metric tons |
| 2.2983336086623667e-14 | Speed limit (60km/h) |


No vehicles:

| Probabilities        |     Predicted labels |
|:---------------------:|:---------------------------------------------:|
| 0.9536492824554443   |          No vehicles |
| 0.02079581283032894  |           Keep right |
| 0.016727346926927567 | Speed limit (30km/h) |
| 0.003337420057505369 | Speed limit (70km/h) |
| 0.0019288352923467755 | Dangerous curve to the left |

Go straight or left:

| Probabilities        |     Predicted labels |
|:---------------------:|:---------------------------------------------:|
| 0.9999997615814209   |  Go straight or left |
| 1.8720216132805945e-07 |      Traffic signals |
| 2.3480199118353084e-08 | Roundabout mandatory |
| 1.921543990590635e-08 |           Ahead only |
| 1.8783556932078227e-09 |            Keep left |

General caution:

| Probabilities        |     Predicted labels |
|:---------------------:|:---------------------------------------------:|
| 1.0                  |      General caution |
| 1.9183195216554018e-18 |      Traffic signals |
| 8.115583761067746e-19 |          Pedestrians |
| 1.691646267405164e-25 |            Road work |
| 3.457907683087756e-26 | Right-of-way at the next intersection |