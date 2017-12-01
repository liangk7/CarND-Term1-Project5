# **Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)

[image1]: ./output_images/???.png "[title]"
[image2]: ./output_images/???.png "[title]"

#### Sources 
Udacity SDC Nanodegree: [CarND-Vehicle-Detection-P5](https://github.com/udacity/CarND-Vehicle-Detection)


---

## Writeup / README

This is the writeup for my Udacity Self-Driving Car Nanodegree Term 1 [Project 5 submission]() in accordance with the [rubric guidelines](https://review.udacity.com/#!/rubrics/513/view)


### Histogram of Oriented Gradients (HOG)

#### 1. HOG feature extraction

![alt text][image]

#### 2. Image classifier

![alt text][image]


### Sliding Window Search

#### 1. Selection of scales and window overlap

![alt text][image]

#### 2. Pipeline and classifier optimization

![alt text][image]


### Video Implementation

#### 1. Final video output

![alt text][image]

#### 2. Filtering out false positives ad combining overlapping boxes

![alt text][image]


---

### Discussion

#### 1. Implementation problems and pipeline shortcomings


