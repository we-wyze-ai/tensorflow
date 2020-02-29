# TensorFlow Lite C++ object detection demo

This example shows how you can load a pre-trained and converted
TensorFlow Lite model and use it to recognize objects in images.

Before you begin,
make sure you [have TensorFlow installed](https://www.tensorflow.org/install).

You also need to [install Bazel 26.1](https://docs.bazel.build/versions/master/install.html)
in order to build this example code. And be sure you have the Python `future`
module installed:

```
pip install future --user
```

## Build the example

First run `$TENSORFLOW_ROOT/configure`. To build for Android, set
Android NDK or configure NDK setting in
`$TENSORFLOW_ROOT/WORKSPACE` first.

Build it for desktop machines (tested on Ubuntu and OS X):

```
bazel build -c opt --cxxopt=-std=c++11 //tensorflow/lite/examples/object_detection:object_detection
```

Build it for Android ARMv8:

```
bazel build -c opt --cxxopt=-std=c++11 --config=android_arm64 \
  //tensorflow/lite/examples/object_detection:object_detection
```

Build it for Android arm-v7a:

```
bazel build -c opt --cxxopt=-std=c++11 --config=android_arm \
  //tensorflow/lite/examples/object_detection:object_detection
```

## Download sample model and image

You can use any compatible model, but the following MobileNet v1 model offers
a good demonstration of a model trained to recognize 1,000 different objects.

```
# Get model and labels
curl https://storage.googleapis.com/download.tensorflow.org/models/tflite/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip --output /tmp/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip && unzip /tmp/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip -d /tmp/
```

```
# Get an test image
curl https://cdn.cnn.com/cnnnext/dam/assets/200226172016-01-trump-coronavirus-presser-0226-exlarge-169.jpg --output /tmp/person.jpg && convert /tmp/person.jpg /tmp/person.bmp
```

## Run the sample

```
bazel-bin/tensorflow/lite/examples/object_detection/object_detection \
  --tflite_model /tmp/detect.tflite \
  --labels /tmp/labelmap.txt \
  --image /tmp/person.bmp
```

You should see results like this:

```
Loaded model /tmp/detect.tflite
resolved reporter
invoked
average time: 42.769 ms
Total number of labels: 91
Total number of detection: 10
0.730469 31:tie | (0.476074, 0.601423, 1.002, 0.69705)
0.71875 0:person | (0.0237119, 0.265134, 0.984543, 0.92416)
0.6875 31:tie | (0.589167, 0.123395, 0.978664, 0.203761)
0.6875 0:person | (0.107211, -0.00242768, 0.993042, 0.337987)
0.425781 31:tie | (0.601196, 0.203068, 0.984585, 0.283434)
0.367188 31:tie | (0.604902, 0.101701, 0.976361, 0.183348)
0.355469 81:refrigerator | (-0.00655219, 0.725905, 0.959322, 0.99002)
0.332031 71:tv | (0.0109962, 0.0674582, 0.483878, 0.955279)
0.300781 31:tie | (0.59719, 0.111391, 0.723824, 0.157441)
```

See the `object_detection.cc` source code for other command line options.
