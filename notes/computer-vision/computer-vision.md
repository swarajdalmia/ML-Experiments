# Computer Vision 
[Source](https://d2l.ai/chapter_computer-vision/index.html)

Many applications in the area of computer vision are closely related to our daily lives, now and in the future, whether medical diagnostics, driverless vehicles, camera monitoring, or smart filters. In recent years, deep learning technology has greatly enhanced computer vision systems’ performance. It can be said that the most advanced computer vision applications are nearly inseparable from deep learning.

## Image Augmentation 
Large-scale datasets are prerequisites for the successful application of deep neural networks. Image augmentation technology expands the scale of training datasets by making a series of random changes to the training images. 

Another way to explain image augmentation is that randomly changing training examples can reduce a model’s dependence on certain properties, thereby improving its capability for generalization. For example, we can crop the images in different ways, so that the objects of interest appear in different positions, reducing the model’s dependence on the position where objects appear. We can also adjust the brightness, color, and other factors to reduce model’s sensitivity to color. 

- Flipping and cropping : Flipping up and down is not as commonly used as flipping left and right. 
- Changing the color : We can change four aspects of the image color: brightness, contrast, saturation, and hue. 

Typically we can overlay the above 3 methods randomly. However these are only the simplest image augmentation techniques. 

## Fine-tuning 

Assume we want to identify different kinds of chairs in images and then push the purchase link to the user. One way to do that would be contruct a dataset of tens of thousands, however that is very timetaking and often expensive. 

Another solution is to apply transfer learning to migrate the knowledge learned from the source dataset to the target dataset. For example, although the images in ImageNet are mostly unrelated to chairs, models trained on this dataset can extract more general image features that can help identify edges, textures, shapes, and object composition. In this section, we introduce a common technique in transfer learning: fine tuning.

- Pre-train a neural network model
- Create a new neural network model, i.e., the target model. This replicates all model designs and their parameters on the source model, except the output layer. 
- Add an output layer whose output size is the number of target dataset categories to the target model, and randomly initialize the model parameters of this layer.
- Train the target model on a target dataset, such as a chair dataset. We will train the output layer from scratch, while the parameters of all remaining layers are fine-tuned based on the parameters of the source model.

#### Learning Rates
Because the model parameters in features are obtained by pre-training on the ImageNet dataset, it is good enough. Therefore, we generally only need to use small learning rates to “fine-tune” these parameters. In contrast, model parameters in the member variable output are randomly initialized and generally require a larger learning rate to learn from scratch. Assume the learning rate in the Trainer instance is  n  and use a learning rate of  10n  to update the model parameters in the member variable output.


## Object Detection and bounding boxes
However, in many situations, there are multiple targets in the image that we are interested in. We not only want to classify them, but also want to obtain their specific positions in the image. In the next few sections, we will introduce multiple deep learning models used for object detection. Before that, we should discuss the concept of target location. 

- Bounding Box : In object detection, we usually use a bounding box to describe the target location.The main outline of the image has to be within the bounding box. 

## Anchor Boxes
Object detection algorithms usually sample a large number of regions in the input image, determine whether these regions contain objects of interest, and adjust the edges of the regions so as to predict the ground-truth bounding box of the target more accurately.
Different models may use different region sampling methods. Here, we introduce one such method: it generates multiple bounding boxes with different sizes and aspect ratios while centering on each pixel. These bounding boxes are called anchor boxes.

The number of anchor boxes = im\_h * im\_w * num of boxes per pixel. 

Given that there are so many anchor boxes how do we measure that the given anchor box does a good job omcpared to the ground truth bounding box. A measure called Jaccard index is used which calculated the intersection of the bounding boxes to the ratio of the union(IoU - intersection over union). 

#### Labeling Training Set Anchor Boxes
In the training set, we consider each anchor box as a training example. In order to train the object detection model, we need to mark two types of labels for each anchor box: first, the category of the target contained in the anchor box (category) and, second, the offset of the ground-truth bounding box relative to the anchor box (offset). 

In object detection, we first generate multiple anchor boxes, predict the categories and offsets for each anchor box, adjust the anchor box position according to the predicted offset to obtain the bounding boxes to be used for prediction, and finally filter out the prediction bounding boxes that need to be output.

When predicting, we can use non-maximum suppression (NMS) to remove similar prediction bounding boxes, thereby simplifying the results.

## Single Shot Multibox Detection
This is covered in another note.

## Region-based CNNs (R-CNNs)
This is covered in another note. 

## Semantic Segmentation and the Dataset
Semantic regions label and predict objects at the pixel level rather than use bounding boxes. In the computer vision field, there are two important methods related to semantic segmentation: image segmentation and instance segmentation. Instance segmentation is also called simultaneous detection and segmentation. This method attempts to identify the pixel-level regions of each object instance in an image. In contrast to semantic segmentation, instance segmentation not only distinguishes semantics, but also different object instances. If an image contains two dogs, instance segmentation will distinguish which pixels belong to which dog.

In the semantic segmentation field, one important dataset is Pascal VOC2012. 

## Fully Convolutional Networks

A fully convolutional network (FCN) [Long et al., 2015] uses a convolutional neural network to transform image pixels to pixel categories. 
The fully convolutional network first uses the convolutional neural network to extract image features, then transforms the number of channels into the number of categories through the  1×1  convolution layer, and finally transforms the height and width of the feature map to the size of the input image by using the transposed convolution layer.

## Neural Style Transfer 
we will discuss how we can use convolution neural networks (CNNs) to automatically apply the style of one image to another image, an operation known as style transfer [Gatys et al., 2016]. 

#### Technique 
Content and style input images and composite image produced by style transfer.  First, we initialize the composite image. For example, we can initialize it as the content image. This composite image is the only variable that needs to be updated in the style transfer process, i.e., the model parameter to be updated in style transfer. Then, we select a pre-trained CNN to extract image features. These model parameters do not need to be updated during training. The deep CNN uses multiple neural layers that successively extract image features. We can select the output of certain layers to use as content features or style features. 

The loss functions used in style transfer generally have three parts: 1. Content loss is used to make the composite image approximate the content image as regards content features. 2. Style loss is used to make the composite image approximate the style image in terms of style features. 3. Total variation loss helps reduce the noise in the composite image. Finally, after we finish training the model, we output the style transfer model parameters to obtain the final composite image.


