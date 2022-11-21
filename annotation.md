___
Project Team 8 -> Contributor: AKASHDEEP SINGH - working solo
___
#### DEXTR Summary
Deep learning techniques have revolutionized the field
of computer vision since their explosive appearance in the
ImageNet competition [34], where the task is to classify im-
ages into predefined categories, that is, algorithms produce
one label for each input image. Image and video segmen-
tation, on the other hand, generate dense predictions where
each pixel receives a (potentially different) output classifi-
cation. Deep learning algorithms, especially Convolutional
Neural Networks (CNNs), were adapted to this scenario by
*First two authors contributed equally
removing the final fully connected layers to produce dense
predictions.

One of the most common ways to perform weakly su-
pervised segmentation is drawing a bounding box around
the object of interest. However, in order to
draw the corners of a bounding box, the user has to click
points outside the object, drag the box diagonally, and ad-
just it several times to obtain a tight, accurate bounding box.
This process is cognitively demanding, with increased error
rates and labelling times.These extreme points be-
long to the top, bottom, left-most and right-most parts of the
object. Extreme-clicking annotations by definition provide
more information than a bounding box; they contain four
points that are on the boundary of the object, from which
one can easily obtain the bounding-box. We use extreme
points for object segmentation leveraging their two main
outcomes: the points and their inferred bounding box. DEXTR is trained on PAS-
CAL 2012 Segmentation for 100 epochs or on COCO 2014
training set for 10 epochs. The learning rate is set to 10−8,
with momentum of 0.9 and weight decay of 5 ∗ 10−4. A
mini-batch of 5 objects is used for PASCAL, whereas for
COCO, due to the large size of the database, we train on
4 GPUs with an effective batch size of 20. Training on
PASCAL takes approximately 20 hours on a Nvidia Titan-
X GPU, and 5 days on COCO. Testing the network is fast,
requiring only 80 milliseconds.

The pipeline of
DEXTR can also be used in the frame of interactive seg-
mentation from points. We work on the case where
the user labels the extreme points of an object, but is nev-
ertheless not satisfied with the obtained results. The natural
thing to do in such case is to annotate an extra point (not
extreme) in the region that segmentation fails, and expect
for a refined result. Given the nature of extreme points, we
expect that the extra point also lies in the boundary of the
object.
To simulate such behaviour, we first train DEXTR on a
first split of a training set of images, using the 4 extreme
points as input. For the extra point, we infer on an image of
the second split of the training set, and compute the accu-
racy of its segmentation. If the segmentation is accurate (eg.
IoU ≥ 0.8), the image is excluded from further processing.
In the opposite case (IoU < 0.8), we select a fifth point in
the erroneous area. To simulate human behaviour, we per-
turbate its location and we train the network with 5 points
as input. Results presented in Section 4.6 indicate that it is
possible to recover performance on the difficult examples,
by using such interactive user input. DEXTR is able to gen-
erate high-quality class-agnostic masks given only extreme
points as input. The resulting masks can in turn be used
to train other deep architectures for other tasks or datasets,
that is, we use extreme points as a way to annotate a new
dataset with object segmentations. In this experiment we
compare the results of a semantic segmentation algorithm
trained on either the ground-truth masks or those gener-
ated by DEXTR (we combine all per-instance segmenta-
tions into a per-pixel semantic classification result).
Specifically, we train DEXTR on COCO and use it to
generate the object masks of PASCAL train set, on which
we train Deeplab-v2, and the PSP head as the se-
mantic segmentation network. To keep training time man-
ageable, we do not use multi-scale training/testing. We
evaluate the results on the PASCAL 2012 Segmentation valsets.

We use ResNet-101 as the backbone architecture, and compare two different alternatives. The
first one is a straightforward fully convolutional architecture
(Deeplab-v2) where the fully connected and the last two
max pooling layers are removed, and the last two stages are
substituted with dilated (or atrous) convolutions. This keeps
the size of the prediction in reasonable limits (8× lower than
the input). We also tested a region-based architecture, similar to Mask R-CNN, with a re-implementation of the
ResNet-101-C4 variant, which uses the fifth stage (C5)
for regressing a mask from the Region of Interest (RoI), together with the re-implementation of the RoI-Align layer.
In the first architecture,
the input is a patch around the object of interest, whereas in
the latter the input is the full image, and cropping is applied
at the RoI-Align stage. Deeplab-v2 performs +3.9% better.
We conclude that the output resolution of ResNet-101-C4
(28×28) is inadequate for the level of detail that we target.
Bounding boxes vs. extreme points: We study the performance of Deeplab-v2 as a foreground-background clas-
sifier given a bounding box compared to the extreme points.
In the first case, the input of the network is the cropped image around the bounding box plus a margin of 50 pixels

We have presented DEXTR, a CNN architecture for
semi-automatic segmentation that turns extreme clicking
annotations into accurate object masks; by having the four
extreme locations represented as a heatmap extra input
channel to the network. The applicability of our method is
illustrated in a series of experiments regarding semantic, in-
stance, video, and interactive segmentation in five different
datasets; obtaining state-of-the-art results in all scenarios.
DEXTR can also be used as an accurate and efficient mask
annotation tool, reducing labeling costs by a factor of 10
##### Link to Google Drive with the COCO annotations and the images: https://drive.google.com/drive/folders/16ICf06_6CRzy6VHEISJoWZdGuXgtYvfK?usp=sharing
