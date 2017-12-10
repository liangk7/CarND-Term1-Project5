# **Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)

[image1]: ./output_images/fig1_classData.png "Dataset"
[image2]: ./output_images/fig2.1_HOG.png "HOG"
[image3]: ./output_images/fig2.2-1_HOGsample.png "HOG, cspace sample"
[image4]: ./output_images/fig2.2-2_HOGcspace.png "HOG, cspace variation"
[image5]: ./output_images/fig2.3-1_HOGsample.png "HOG, orient sample"
[image6]: ./output_images/fig2.3-2_HOGorient.png "HOG, orient variation"
[image7]: ./output_images/fig2.4-1_HOGsample.png "HOG, pix per cell sample"
[image8]: ./output_images/fig2.4-2_HOGppc.png "HOG, pix per cell variation"
[image9]: ./output_images/fig2.5-1_HOGsample.png "HOG, cell per block sample"
[image10]: ./output_images/fig2.5-2_HOGcpb.png "HOG, cell per block variation"
[image11]: ./output_images/fig3.1_windows.png "Search windows"
[image12]: ./output_images/fig3.2_carsBoxes.png "Car matches"
[image13]: ./output_images/fig4.1_heatmap.png "Heat map"
[image14]: ./output_images/fig4.2_threshmap.png "Threshold map"
[image15]: ./output_images/fig4.3_carsDetected.png "Detected cars"
[image16]: ./output_images/fig5_imagesCarsDetected.png "Detected cars in test images"

#### Sources 
Udacity SDC Nanodegree: [CarND-Vehicle-Detection-P5](https://github.com/udacity/CarND-Vehicle-Detection)


---

## Writeup / README

This is the writeup for my Udacity Self-Driving Car Nanodegree Term 1 [Project 5 submission]() in accordance with the [rubric guidelines](https://review.udacity.com/#!/rubrics/513/view)


### Histogram of Oriented Gradients (HOG)

#### 1. HOG feature extraction

The first step in this project is to convert the datasamples into HOG features with the `hog` method (part of the `skimage` library). Using a custom function `get_hog_features` we can view image results and test parameters such as:
* colorspace
* the number of orientations
* the number of pixels per cell
* the number of cells per block

![alt text][image2]

By observing the following colorspaces, we can select the best option:
* RGB, HLS, LUV, HLS, YUV, YCrCb
Let's move forward with **YUV**

![alt text][image3] 
![alt text][image4]

Next, taking a look at the number of orientations:
* 4, 8, 12, 16
Intuitively, the greater number of `orient` seems to refine our results. But computationally, **8** orientations seems to perform sufficiently.

![alt text][image5] 
![alt text][image6]

Moving forward to the number of pixels per cell:
* 2, 4, 8
The lowest `pix_per_cell` value yields a more accurate HOG feature image. But for computation purposes, **4** pixels per cell prove optimal.

![alt text][image7] 
![alt text][image8]

Lastly, the number of cells per block:
* 2, 4, 6, 8
The `cell_per_block` feature doesn't appear to have significant impact in the results, so I went along with **2**.

![alt text][image9] 
![alt text][image10]


#### 2. Image classifier
With the `vehicles` and `non-vehicles` dataset referenced in the CarND-Vehicle-Detection-P5 [github README](https://github.com/udacity/CarND-Vehicle-Detection), we can use the `get_hog_features` function to create an SVM model for classifying the images.

![alt text][image1]


### Sliding Window Search

#### 1. Selection of scales and window overlap
By looking at the provided sample images, we can make inference on relative car sizes and locations. Taking image patterns into consideration (assuming that the camera positioning and road height are constant), I used the following parameters:
* box sizes (x, y): (64, 64), (80, 80), (96, 96), (112, 112)
* overlap (x, y): (0.3, 0)

![alt text][image11]

#### 2. Pipeline and classifier optimization
The overall vehicle detection pipeline is as follows:
* sliding window search and svm classification of HOG features
* heatmap to highlight likeliest candidates
* threshold to eliminate false positives
* labels to identify object shapes
* boxes appended to original image

The `find_cars` utilizes the box parameters determined in the previous section, to make a classification prediction on the HOG features of the boxed pixels in the provided image. It is evident that overlapping boxes allow us to clearly pinpoint likely car objects, but too much of an overlap may also result in more false positives. Thus, I resorted to an overlap of `0` in the `y` direction, so error propagations would be limited to the `x` direction and also be less likely to develop with overlaps of bigger boxes.

![alt text][image12]

The `add_heat` function takes each of the boxes (classified as *cars* by the `find_cars` function) and appends them to a blank image space to depict the likelihood of cars (in the form of heat intensity).

![alt text][image13]

To reduce the number false positives, the `apply_threshold` function reducts the intensity of classified cars. This effectively eliminates misclassified objects.
After removing false positives, the use of the `labels` function (part of the `scipy` library) allows us to isolate the object boxes to redraw onto the raw image.

![alt text][image14]

Finally, we can examine the pipeline results on the original image. 

![alt text][image15]

The following is the pipeline shown on each test image.

![alt text][image16]


### Video Implementation

#### 1. Final video output
After refining the pipeline on the images, we can now try on video.
[Sample video](https://github.com/liangk7/CarND-Term1-Project5/blob/master/output_videos/test_video_out.mp4)

[Full video](https://github.com/liangk7/CarND-Term1-Project5/blob/master/output_videos/project_video_out.mp4)


#### 2. Filtering out false positives and combining overlapping boxes



---

### Discussion

#### 1. Implementation problems and pipeline shortcomings


#### 2. Future implementation and insights

