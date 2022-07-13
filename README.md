# Vegetable Classifier

This project was for the Professional Master's Program Computer Vision course taught by Professor Stan Birchfield at the University of Washington.

## Introduction

Can a Neural Network classify images that are more than just a picture on a white background?? After poking around a few simple datasets, I noticed that many of the datasets that were achieving very high accuracy with SOTA CNNs were simply different images on white backgrouds, so I asked myself the question: can a network classify images correctly that are much more generalized. To do this, I chose my objects to be different vegetables, namely Bell Pepper, Broccoli, Cauliflower, Cucumber, and Mushroom. I also ran a second question of interest which was can the network still perform at top notch when two of the vegetables look the same, ie adding Zuchinni to the list of vegetables. 

In the end results I found that the network did very well in classifying generalized images when the vegetables look differently. The network was able to classify correctly 93.85% of the time when zuchinni was not in the dataset, but began to fail when two similar vegetables were present, ie correct classification dropped to 88.04% of the time with zuchinni in the dataset, with the cucumber class taking the biggest hit down to 60% accuracy. 

## Dataset

Since this was a novel idea of building a vegetable classifier that was trained and tested on data that was more generalized, I had to build the generalized dataset. This began with finding images online of the 6 different vegetabes with different backgrounds, some with other objects in the images, some sliced or diced, and some simple background images. I gathered 15 images of each vegetable and then wrote some data augmentation functions for the images, shown below is an example snippet of some of the vegetables used for the original data for this project. 

<img width="614" alt="Screen Shot 2022-07-12 at 8 36 53 PM" src="https://user-images.githubusercontent.com/72525765/178645292-5b36d329-42f7-46cf-83dd-88d9b8ac5e1c.png">

The augmentation function that I created randomly applied a variety of augmentations including: flip, flop, rotate, brighten/darken, gaussian noise, or smoothen/sharpen. An example of the augmented data is below: 

<img width="200" alt="Screen Shot 2022-07-12 at 8 42 29 PM" src="https://user-images.githubusercontent.com/72525765/178645818-37b630ca-360a-45d6-be6b-1c9095423c13.png"> <img width="200" alt="Screen Shot 2022-07-12 at 8 42 47 PM" src="https://user-images.githubusercontent.com/72525765/178645853-8542cd8c-c855-4dec-b24d-92e42d2b2772.png">

## Models

To make sure I was getting the best results I could while keeping in mind training time, I opted to do Transfer Learning with a pretrained model of ResNet-34.

<img width="165" alt="Screen Shot 2022-07-12 at 9 18 29 PM" src="https://user-images.githubusercontent.com/72525765/178649732-7e1049b1-38d6-4a18-8e45-9f5d555e05a7.png">

In V1 of the project (the classification problem with 5 vegetables that look different), I did three increasingly deeper layer unfreezing and retraining. I did this by first unfreezing just the last fully connected layer of ResNet-34, then unfreezing and retraining with layer4 and then also layer3. This gave me better and better results for my particular classification dataset. 

V2 of the project was the classification problem with the 6 vegetables, ie zuchinni was added to answer the question can the network still perform well with two of the vegetables being very visually similar. For this problem I simply unfroze the fully connected, layer4 and layer3 weights of a pretrained ResNet-34 network and retrained.

## Training

Transfer Learning: the method of taking a pretrained model on a dataset like ImageNet, and fine tuning the model to test well on your dataset. This works well with very large networks as training times can be exceedingly long for complex networks. 

In my example I was performing the training process on the unfrozen layers incrementally. In the below graph which is for V1, Case 0 refers to only the fully connected layer unfrozen, where Case 1 and 2 refer to the increasingly deep unfreeze process of layer4 and layer3, with all previous unfrozen layers still unfrozen. As we can see the model with the lowest loss recorded in training was the deepest unfreeze retrain, Case 2. This shoudl come as no surprise that the model that gets retrained the most performs the best -- assuming we don't consider the possibility of overfitting!

<img width="305" alt="Screen Shot 2022-07-12 at 9 30 12 PM" src="https://user-images.githubusercontent.com/72525765/178650998-f224bffb-2284-4acb-bc03-a4603c971dea.png">

I also generated Test Error curves for V1, these were done after training was completed. Since each model step was saved after each epoch of training, I was able to run the test data through each step of the training models and determine a sense of overfitting if applicable.

<img width="309" alt="Screen Shot 2022-07-12 at 9 33 12 PM" src="https://user-images.githubusercontent.com/72525765/178651312-3b916d1b-5fb8-4d7e-b240-13896d21f06c.png">

Here I included the training loss of the V2 network. This was interesting as it was a very smooth decline in loss and showed great promise for the effectiveness of this particular retraining.

<img width="306" alt="Screen Shot 2022-07-12 at 9 35 14 PM" src="https://user-images.githubusercontent.com/72525765/178651509-beb3d6dd-8836-475b-bb9a-64ca2966a032.png">

Test Error for V2 was also captured and shown here as it was an interesting discovery to see that the test error was so volatile between epochs of retraining. 

<img width="301" alt="Screen Shot 2022-07-12 at 9 37 02 PM" src="https://user-images.githubusercontent.com/72525765/178651697-e622563d-e200-4328-9c4a-70ee717c121e.png">

## Results

Before any transfer learning occured, I ran my test images through the network to see what kind of generalized images / vegetables would test well on the pretrained ResNet-34 model. It turns out that the classes performed drastically different depending on V1 vs V2. The results can be seen here:

<img width="317" alt="Screen Shot 2022-07-12 at 9 24 26 PM" src="https://user-images.githubusercontent.com/72525765/178650366-f203b1f5-0f99-4a45-9013-f260c1a919e4.png"> <img width="302" alt="Screen Shot 2022-07-12 at 9 24 42 PM" src="https://user-images.githubusercontent.com/72525765/178650392-9e33bfa7-cb84-4d7e-8f8b-5c5fadfedd48.png">

### V1
After some transfer learning, I present the results below with some analysis on them. As we can see below the accuracy across all categories slowly increases the deeper the network is retrained. Accuracy went from 83.67% - 93.85% with the three cases of transfer learning. We can note that the category that performed the best was the Mushroom class, where the model also did not perform as well on Bell Peppers. It could be concluded that the variation in the Mushroom pictures is less than the variation in the bell pepper images, causing a harder classification problem for the network with Bell Peppers. 

<img width="292" alt="Screen Shot 2022-07-12 at 9 38 43 PM" src="https://user-images.githubusercontent.com/72525765/178651857-b616f39f-35ba-4328-abe1-da35246d2995.png">
<img width="304" alt="Screen Shot 2022-07-12 at 9 39 14 PM" src="https://user-images.githubusercontent.com/72525765/178651909-b0b62d3e-6d92-4dfc-a963-a85a8e8e3016.png">
<img width="308" alt="Screen Shot 2022-07-12 at 9 39 28 PM" src="https://user-images.githubusercontent.com/72525765/178651926-bf37eaef-637d-4334-985a-8e8e9a1e4eae.png">

### V2
With the addition of Zuchinni as a vegetable in the dataset I hypothesized that the network would perform worse overall, with the majority of decrement in accuracy being from either the cucumber or the zuchinni class. This hypothesis was correct as the addition of the zuchinni actually took the accuracy of the cucumber class from 96.93% to 61.84%, a huge decrement in class accuracy from V1 case 2.

<img width="310" alt="Screen Shot 2022-07-12 at 9 45 45 PM" src="https://user-images.githubusercontent.com/72525765/178652589-b6c3028b-4029-424b-af47-2a6b820119f3.png">

To answer my original questions: yes, neural networks can classify images that are more general, and the addition of objects that have very similar visual attributes causes a major decrement in model performance.

## To run
Download veggie_classifier.ipynb and put into the same directory as data/images/ (unzip data/images.zip into data/images)
Launch Jupyter Notebook via anaconda prompt and run the code from start to finish for all generated/desired results.

Note: the code takes from 3-6 hours to run depending on the hardware of your computer. There is not cuda checking since I was not able to use cuda. 
      To speed the performance of the CNN training and testing up make use of cuda and gpu if possible!
