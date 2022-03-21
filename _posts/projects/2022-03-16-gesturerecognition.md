---
layout: project
permalink: /:title/
category: projects

meta:
  keywords: "Deep Learning, Computer Vision, Pytorch"

project:
  title: "Deep Learning for Gesture Recognition"
  type: "Jekyll"
  url: "https://github.com/javtges/"
  logo: "/assets/images/projects/gesture/fist_gesture.png"
  tech: "Deep Learning, Computer Vision, Pytorch"


images:
  - image:
    url: "/assets/images/projects/gesture/fist_gesture.png"
    alt: "Depth and color fist gesture"
  - image:
    url: "/assets/images/projects/gesture/dataset_image.png"
    alt: "Infrared image from a dataset"
---


<p> <a href="https://youtu.be/BBnZKu9_Neg"> target="_blank">Drone control via gesture</a> </p><br>

<p>Gestures are a common and especially alluring method of human-computer interaction. For decades media has depicted science-fictional computer wizards manipulating computers and robots with a swipe of a hand, as if a digital interface was tangible and could be grabbed and dragged about.
<br>
However, in practice, dynamic gesture recognition is a surprisingly difficult task, which I approach in this project. Starting as a total beginner in deep learning, I experimented with and implimented multiple common methods of recognizing dynamic hand gestures.</p>
<br>


<p>Time-series data struggles with a fundemental challenge in live inference tasks - that is, the problem of segmentation. In a live video stream, there's no definite beginning or end of the gesture, and the time the gesture takes to be performed can be variable.</p>
<br>
<p>
Various solutions to this temporal classificaion problem exist in the deep learning space, most prominently recurrent neural networks (RNNs) which take multiple inputs - a more traditional input for a CNN layer, and a previous state of the RNN. This is used in models such as LTSMs, and specific loss functions have been developed for use with these networks, such as CTC: Connectionist Temporal Classification loss, all of which were experimented with in this project.</p>
<br>

<p>
Two different 3DCNNs, used for classifying video rather than a 2DCNN which is used on images, were implimented on Pytorch and trained on the nvGesture dataset. The two networks that were implimented were [C3D](https://arxiv.org/abs/1412.0767) and [Resnet 2+1d](https://arxiv.org/pdf/1711.11248.pdf). Both networks extract spatiotemporal features, useful for classifying gestures.</p>

<br>

<p>
While using C3D to emulate the results in a [2016 paper by NVIDIA](https://research.nvidia.com/sites/default/files/pubs/2016-06_Online-Detection-and/NVIDIA_R3DCNN_cvpr2016.pdf) to classify gestures in the nvGesture dataset yielded training results were not as expected, this was still a great introduction to Pytorch by attempting to solve a difficult problem. </p> <br>

<p> Additionally, a parallelized 1DCNN was implimented to classify dynamic hand gestures in the [DHG-14 dataset](http://www-rech.telecom-lille.fr/shrec2017-hand/), using the XYZ coordaintes of skeletal points on a hand as detected through softwares such as Mediapipe. This included a real-time infrence wrapper around [this network](https://github.com/guillaumephd/deep_learning_hand_gesture_recognition). While training and validation accuracies were strong, this suffers from the same segmentation problem as before. </p>

<br>

<p>

Finally, a dataset of static images of hands in an infrared camera was used to classify static gestures from a [leap motion camera](https://www.kaggle.com/gti-upm/leapgestrecog). Given the nature of the images, this can be approximated by depth data too, with background removal. A 2D CNN was implimented in Pytorch, with parsing of the dataset files courtesy of [kaggle](https://www.kaggle.com/kageyama/keras-hand-gesture-recognition-cnn/notebook). The implimented classifier works on single frames in a video stream, and is accurate enough to achieve high performance in real time with a framerate of 30 FPS.

</p> <br>

<p>
Controlling the Drone </p> <br>

<p>
This project uses a Dji Tello Edu drone, as it provides an easy-to-interface software development kit. Commands that can be sent to the drone are listed [here](https://djitellopy.readthedocs.io/en/latest/tello/). Each gesture will correspond to a movement in this SDK. </p> <br>

<p>
The final demonstration as of March 17th uses the Depth/Infrared Classifier, to differentiate between 10 static hand gestures in a depth or infrared video stream. While training and validation accuracy exceeds 99% on the training dataset, the real-time recongition capability is signifigantly diminished - being able to reliably differentiate between four or five of the classes.
</p> <br>
<p>
This has to do with the input datatype, where the hands in the dataset are clearly segmented opposed to more loosely segmented hands in the live video. (Show picture of dataset image) This is improved by removing pixels with depth values greater than a threshold of 0.5m, though can still stand to be improved.
</p> <br>
<p>
Controlling the drone is accomplished by sending commands to the drone corresponding to the gesture detected, provided the decision proability is greater than a threshold of 0.7. Once a gesture is found, the next N frames are ignored.
</p> <br>