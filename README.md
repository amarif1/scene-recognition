# SCENE RECOGNITION USING LOCAL AND GLOBAL DESCRIPTORS


**Approach** : We make use of global feature descriptors as well as 
local feature descriptors simoulteniously to represent each image, to 
improve recognition accuracy. Our basic steps are as as follows

**1. Feature Extraction** : From each image, we first extract DAISY 
features [1] (similar to SIFT features) corresponding to the keypoints 
detected from the image. We make use of the skimage library 
(scikit-image) [2] for feature extraction. Corresponding to each 
keypoint in the image, there would be a DAISY descriptor with dimensions 
controlled by parameters step size (distance between descriptor sampling 
points) and radius (describing size of the scanning area). We also 
extract a standard HOG descriptor corresponding to the image at a 
diffrent granularity (by making use of the parameters pixels_per_cell, 
cells_per_block and orientations), effectively allowing us to choose 
features at different scales.


**2. Encoding** : We make use of the local DAISY features corresponding 
to each keypoints to do the encoding. We use the standard "bag of visual 
words" concept here. Concretely, we apply K-means algorithm to quantize 
DAISY features into 'K' clusters to form "visual words". K becomes the 
size of our vocabulary. Corresponding to each image, we form a histogtam 
with 'K' as the dimensionality, by using this vocabulary.


**3. Pooling** : We make use of a 2-level pooling scheme. From the DAISY 
features, we form a histogram by representing frequency of each visual 
word in each image. We do this by taking each key point in the image and 
looking up the cluster_id corresponding to that DAISY descriptor and 
incrementing the count corresponding to that bin in our histogram. This 
procedure is effectively a "sum pooling". We do an L2 normalization of 
the resulting histogram to form a "DAISY histogram feature". For the 2nd 
level pooling, we take the HOG global descriptor corresponding to each 
image and do an L2 normalization followed by a concatenation with the 
corresponding DAISY histogram feature


**4. Classification** : We make use of standard SVM classifier with 
various kernels. We utilize the sckit-learn (sklearn) library [3] for 
the SVM implementations. We do crossvalidation by randomly splitting the 
dataset into a training and testing set. We build the "visual 
vocabulary" as well as training feature vectors from the training split. 
We report overall accuracy, confusion-matrix as well as standard 
information-retrival statistics such as precision, recall and f-measure.




#### REFERENCES ########

[1] Tola, Engin, Vincent Lepetit, and Pascal Fua. "A fast local 
descriptor for dense matching." Computer Vision and Pattern Recognition, 
2008. CVPR 2008. IEEE Conference on. IEEE, 2008.

[2] Van der Walt, Stefan, et al. "scikit-image: image processing in 
Python." PeerJ 2 (2014): e453.

[3] Pedregosa, Fabian, et al. "Scikit-learn: Machine learning in 
Python." Journal of Machine Learning Research 12.Oct (2011): 2825-2830.