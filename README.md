**Vehicle Detection**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier.
* Implement a sliding-window technique and use the trained classifier to search for vehicles in images.
* Run the pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: /vehicles/GTI_Far/image0000.png
[image2]: /output_images/out_8.png
[image3]: /output_images/out_9.png
[image4]: /non-vehicles/Extras/extra1.png
[image5]: /output_images/out_1.png
[image6]: /output_images/out_2.png
[image7]: /output_images/out_46.png
[image8]: /output_images/out_47.png
[image9]: /output_images/out_48.png
[image10]: /output_images/out_49.png
[image11]: /output_images/out_53.png
[image12]: /output_images/out_54.png

### Histogram of Oriented Gradients (HOG)

#### 1. Explanation of how I extracted HOG features from the training images.

The code for this step is contained in the second code cell of the IPython notebook called `extract_features()`.  

Since the images in the training data are in .png format, I loaded them in using matplotlib.image so that they are already normalized. This means that we do not need an additional normalization step when training the classifier. After reading in the images, I converted the features to the YCrCb color channel since it produced the best results. I then extracted the HOG features using the function `get_hog_features`. Below are examples of HOG feature extraction from both vehicle and non vehicle images.  

Original Vehicle Image  
![alt text][image1]  

HOG Image  
![alt text][image2]  

YCrCb Transformation  
![alt text][image3]  

Original Non-Vehicle Image  
![alt text][image4]  

HOG Image  
![alt text][image5]  

YCrCb Transformation  
![alt text][image6]  


#### 2. Explanation of how I settled on my final choice of HOG parameters.

I used all 3 channels with 18 orientations, 8 pixels per cell and 2 cells per block. This configuration allowed for the best balance between accuracy and speed. 

#### 3. Description of how I trained a classifier using my selected HOG features.  

The code for this step is contained in the second code cell of the IPython notebook called `train()`.  

First, each image in the training set was converted to a horizontal feature vector. Then the dataset was split between between training and testing data with 20% of the data being used for the test set. An SVM classifier was used to train the classifier.  

### Sliding Window Search

#### 1. Description of how I implemented a sliding window search and how I decided what scales to search and how much to overlap window.  

The code for this step is contained in the second code cell of the IPython notebook called `search_windows()`.  

The sliding window technique was used to detect vehicles. First, each image is passed into the function `gethotwindows()` that detects vehicles in the image. Since the upper half of each image mostly contains the sky, only the bottom half of the image is used to search for vehicles. As for the x axis, information from the previous image is stored so that we minimize the range over which we search for cars in the x axis. I used a window size of 75 x 75 with an overlap of 80%. After that, the HOG features are extracted and pushed into the classifier that determines whether a vehicle is present or not. 


#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

The search was done on half of the image, which helped to optimize performacne. Here are some example images:  


![alt text][image7]  
![alt text][image8]  
![alt text][image9]  
![alt text][image10]  
![alt text][image11]  
![alt text][image12]  

---

### Video Implementation

#### 1. Link to your final video output. 
Here's the [link to my video result](https://github.com/AmanMander123/VehicleDetectionAndTracking)


#### 2. Description of how I implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

The code for this step is contained in the second code cell of the IPython notebook.  

First, a heatmap for each of the dectections is created using the function `add_heat()`. A moving list of the last 10 heatmap images is generated and updated. At the final output heatmap, an average of the last 10 heatmaps is created which helps us reduce false positives. A detection in one image but not in any others gets averaged out. Then in order to ensure that we do not get any false positives, the heatmap is thresholded to allow output where we see an overlap of at 2 heatmaps.   


---

### Discussion

#### 1. Brief discussion on any problems / issues I faced in your implementation of this project.  Where will the pipeline likely fail?  What could I do to make it more robust?

Although it is good, the vehicle detection piepline is still not perfect. Sometimes it takes several seconds to detect a car that is present in the images. Other times, it mistakenly classifies 2 vehicles as 1 when they are close to each other. The processing time to create the output video is also very long, which makes it impractical for real-time applications. In order to make it more robust, a better classifier such as neural nets can be used.  

