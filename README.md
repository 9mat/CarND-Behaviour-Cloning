# CarND-Behaviour-Cloning

## Introduction
This is third project from the Udacity Nanodegree on Self-driving Car (CarND).
The purpose is to teach a (simulated) car to drive on an empty track and
maintain itself in the middle of the track.
A covolutional neural network is to be employed, taking in a photo from
the front camera of the car and outputing the desired steering angle.

My implementation follows closely this [nVidia paper](http://images.nvidia.com/content/tegra/automotive/images/2016/solutions/pdf/end-to-end-dl-using-px.pdf).

## Data collection, normalization and augmentation
### Data collection
A sample data set was provided by Udacity, presumbly by having a human constroling
the simulated car. Additional data could be collected by similar method (simulated driving)
and could potentially improve the performance, but due to time constraint, I restrict
myself to the the set of those given 8036 samples, coupled with substantial data augmentation 
described below.

### Data normalization
The images, originally in RGB, were converted to YUV color space, and the three color 
channels were then recentered and rescaled to the min-max values of -0.5 and 0.5.

One third of the top and one fifth of the bottom of the each image were cropped away 
as they contained no relevant information about the position of the car or the tracks.

Lastly, the images were resized to 200x66 to reduce the dimensionality of the network.
200x66 was the input size chosen in the nVidia paper.

### Data spliting
As I employed an apdaptive optimizer for training (Adam), there were few hyper-parameters 
to tune. In fact, I left all the hyper-paramters at their default/typical values and did
no substatial tuning at all (except for the trail and error of different netword designs
and dimensionality). Thus, the data were only splitted into training set and test set, with
25% of the original data reversed for testing.

### Data augmentation
Additional data were generated using samples from the training sets with the following 
transformation: random horizontal flip, random brightness adjustment, random vertical
and horizontal translation, and the use of photos from the left and right cameras.

1. Flip: The image was flip horizontally and the sign of steering angle was reversed
2. Additional camera: randomly choose from the 3 available camera; if the left camera
was used, the steering angle would be adjusted up by 0.25, and -0.25 if the right
camera was used
3. Horizontal shift: to mimic small shift in the position of the car on the track;
the steering angle also need to be adjusted by 0.00
