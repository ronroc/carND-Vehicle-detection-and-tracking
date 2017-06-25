# carND-Vehicle-detection-and-tracking
This repo contains submissions made for project 5 as part of Udacity self driving car nanodegree 
## **Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)

[image2]: ./examples/HOG_example.png
[image4]: ./examples/sliding_windows.png
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[video1]: ./project_video.mp4

Histogram of Oriented Gradients (HOG)

1. Explain how (and identify where in your code) you extracted HOG features from the training images.

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the RGB color space and HOG parameters of orientations=6, pixels_per_cell=(8) and cells_per_block=(2):


![alt text][image2]

2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and my final choice of parameters is shown below

* color_space = 'YCrCb'  # Can be RGB, HSV, LUV, HLS, YUV, YCrCb
* orient = 9  # HOG orientation angles
* pix_per_cell = 8
* cell_per_block = 2
* hog_channel = "ALL"  # aka 1st feature channel. Can be 0, 1, 2, or "ALL"
* spatial_size = (32, 32)  # Spatial binning dimensions
* hist_bins = 32  # Number of histogram bins
* spatial_feat = True  # Spatial features on or off
* hist_feat = True  # Histogram features on or off
* hog_feat = True  # HOG features on or off

3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using HOG features and color features. Code is shown in cell block (19)

Sliding Window Search

1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

After calculating params such as number of windows, number of pixels per step and span of the region to be searched , a list of window positions are created which is then iterated over.  Codes for the appraoch which I have used are in code blocks 6, 7, 9 of the notebook


2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
---

Video Implementation

1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./edit_project_video.mp4)


2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

Here are six frames and their corresponding heatmaps:

![alt text][image5]

Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



Discussion

* Most of the code in this  project follows the code from the classroom sessions . I plan to improve further on these codes by implemeting better functionality and making the pipeline more robust

* Even though output from computer vision techniques looks promising I hope to improve it further using Object detection techniques from deep learning
