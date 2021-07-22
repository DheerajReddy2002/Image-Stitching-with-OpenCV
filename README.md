# Image Panorama Stitching with OpenCV

Image stitching is one of Computer Vision's most successful applications. It is now difficult to locate the mobile phone or image processing API without this feature.

In this work, we'll discuss how to do Python and OpenCV photo stitching. Given a couple of photos in a similar location, we aim to 'stitch' them into a panoramic picture scene.
We go through some of the best-known computer vision techniques in this essay. Including:

* Detection of Keypoint
* Local Descriptors Invariant (SIFT, SURF, etc)
* Coinciding feature
* RANSAC homography estimate
* Outlook Warping


![im1](https://user-images.githubusercontent.com/59975618/126669177-f709b578-b3b0-49e1-b083-d0a3e1f3ea88.png)

Detect and remove features
We want to stitch them together to create a panoramic scene, given the photos like the ones above. It is crucial to note that both pictures have a common region to share.

In addition, even if the images differ in one or more of the following features, our solution must be robust:

Space position scaling angle
Device capture
The initial stage is to extract certain essential elements and interesting characteristics. However, these traits must have specific characteristics.


But what if we turn a picture, then? We would have difficulty in this case because corners are not invariant. That means the previously identified corner might become a line if we zoom in to a picture!


In a nutshell, we need to be able to rotate and scale. There, strong techniques are introduced such as SIFT, SURF, and ORB.

## Descriptors and key points.

Methods such as SIFT and SURF attempt to solve corner algorithms constraints. Corner detector methods often utilize a kernel of defined size to find areas of interest on pictures (corners). It's easy to see that this kernel may go too tiny or too large if you resize a picture.

Methods like SIFT are using Gaussian difference to solve this restriction (DoD). DoD should be applied for copies of the same picture that are different sized. It also uses the next pixel information to locate and improve important spots and descriptions.

We must start by loading 2 pictures, a query picture, and a training picture. Initially, the essential points and descriptors were extracted from both of them. In one stage we may achieve this by utilizing the method OpenCV detect and compute(). Note that we require a keypoint detector instance and the descriptor object to utilize detect and compute(). This may include ORB, SIFT, SURF, etc. In addition, we transform the pictures to grayscale prior to feeding AndCompute().

On both the query and the train picture we execute detect and compute(). We have a number of important points and descriptions for the pictures at this stage. When SIFT is the extractor of the feature, it will return for each key point a 128-dimensional feature vector. When you choose SURF, we obtain a Vector of 64 dimensions. Some features retrieved using SIFT, SURF, BRISK, and ORB are shown in the following photos.


![im2](https://user-images.githubusercontent.com/59975618/126669424-05659f75-6ebe-421a-8e67-1408641f00cd.png)


![im3](https://user-images.githubusercontent.com/59975618/126669557-8cd40143-6004-4fbf-8cb8-98748cc5f6d7.png)


![im4](https://user-images.githubusercontent.com/59975618/126669621-e6c64ebe-d430-4a11-975a-e8dce8123e10.png)


![im5](https://user-images.githubusercontent.com/59975618/126669702-c8502c7e-4ec5-4dbf-8df5-f629c633d70d.png)

## Matching feature

We have many characteristics from both photographs, as we can see. Now we want to compare both attributes and stick to the more like-minded pairings.

OpenCV requires a Matcher object for functional matching. Two tastes are explored here:

Matcher KNN Brute Force (k-Nearest Neighbors)

The Matcher BruteForce (BF) performs what its name says exactly. In view of two sets of characteristics (Figure A & Figure B) individual characteristics of set, A are compared with all characteristics of set B. The Euclidean distance between 2 locations is computed by default. By default.

## Testing ratio

The authors of the SIFT article offer a technique called a ratio test to ensure that the features given by KNN are well comparable. In principle, we iterate and conduct a distance test on each pair provided by KNN. If the gap between f1 and f2 is within a particular ratio, we will maintain it for each pair of features (f1, f2), otherwise, we will throw it off. The ratio value must also be selected by hand.

Essentially, proportion testing is the same as the BruteForce Matcher cross-checking option. Both ensure that a few of identified traits are in fact as near as comparable as possible.


![im6](https://user-images.githubusercontent.com/59975618/126669867-d517533b-9d4c-4d63-9391-d7a4dacbb579.png)


![im7](https://user-images.githubusercontent.com/59975618/126669970-6720d43f-f7b4-421d-827e-9ed78f987cbd.png)

The Matcher algorithm, however, will provide both pictures with the best (similar) collection of characteristics. Now we must take these points and create the transformations matrix, based on their matching points, that can sew the 2 pictures together.


The matrix of homography is known as such a transformation. Homography is a 3x3 matrix for numerous purposes, such as estimating a camera posture, correction of perspective, and picture stitching. 2D transformation is the homography. The plane (picture) maps points to another plane. See how we get it.


The image seen below is panoramic. The consequence is a few artifacts, as we can see. In particular, we can notice certain difficulties relating to lighting circumstances and the edge effects on the picture borders. In order to equalize intensity, we can ideally carry out post-processing procedures. The outcome would probably appear more realistic.

