# Node Wrapper for YOLO/Darknet recognition framework

Darknet is [an open source neural network framework written in C and CUDA](https://github.com/pjreddie/darknet).
This project wraps this framework in a Node native addon.

- [darknet](http://pjreddie.com/darknet/)
- [YOLO (You only look once)](http://pjreddie.com/darknet/yolo/)

## Status

Currently, the wrapper implements a demo method to run a recognition from the webcam or a video file (equivalent to `./darknet detector demo cfg/coco.data cfg/yolo.cfg yolo.weights`).

## Prerequisites

This module requires OpenCV installed. First, you need to compile [this fork](https://github.com/OrKoN/darknet) of darknet with OpenCV support (optionally, with CUDA support):

```sh
git clone git@github.com:OrKoN/darknet.git
cd darknet
make OPENCV=1
make install # by default installed to /usr/local
```

After that you can process with the installation via NPM.

## Installation

```sh
npm i @moovel/darknet --save
```

## Usage

You need to download `cfg`, `data` folders and weight files you need to use from the darknet project.

Detect a video from camera or a file:

```js
const darknet = require('@moovel/darknet');

darknet.detect({
  cfg: './cfg/yolo.cfg',
  weights: './yolo.weights',
  data: './cfg/coco.data',
  cameraIndex: 0, // optional, default: 0,
  video: "./test.mp4", // optional, forces to use the video file instead of a camera
}, function(modified, original, detections) {
  /**

  modified - raw frame with detections drawn, bgr24 format
  original - raw frame, as captured by the webcam/video, bgr24 format,
  detections - array of detections

  Example detections:

  [ { x: 0.8602103590965271,
      y: 0.20008485019207,
      w: 0.13895535469055176,
      h: 0.39782464504241943,
      prob: 0.2408987432718277,
      name: 'tvmonitor' },
    ,
    { x: 0.26072466373443604,
      y: 0.4977818727493286,
      w: 0.10842404514551163,
      h: 0.22796104848384857,
      prob: 0.3290732204914093,
      name: 'person' },
    ,
    { x: 0.2568981349468231,
      y: 0.5765896439552307,
      w: 0.12322483211755753,
      h: 0.2544059157371521,
      prob: 0.2738085687160492,
      name: 'chair' },
    ,
    { x: 0.6593853235244751,
      y: 0.8188746571540833,
      w: 0.06210440397262573,
      h: 0.100614033639431,
      prob: 0.3225017189979553,
      name: 'clock' } ]

  */
});
```

Detect on a single image:

```js
const darknet = require('@moovel/darknet');

darknet.detectImage({
  cfg: './cfg/yolo.cfg',
  weights: './yolo.weights',
  data: './cfg/coco.data',
  image: './test.jpeg', // detect an image
}, function(modified, original, detections) {
  /**

  modified - raw frame with detections drawn, bgr24 format
  original - raw frame, as captured by the webcam/video, bgr24 format,
  detections - array of detections

  Example detections:

  [ { x: 0.8602103590965271,
      y: 0.20008485019207,
      w: 0.13895535469055176,
      h: 0.39782464504241943,
      prob: 0.2408987432718277,
      name: 'tvmonitor' },
    ,
    { x: 0.26072466373443604,
      y: 0.4977818727493286,
      w: 0.10842404514551163,
      h: 0.22796104848384857,
      prob: 0.3290732204914093,
      name: 'person' },
    ,
    { x: 0.2568981349468231,
      y: 0.5765896439552307,
      w: 0.12322483211755753,
      h: 0.2544059157371521,
      prob: 0.2738085687160492,
      name: 'chair' },
    ,
    { x: 0.6593853235244751,
      y: 0.8188746571540833,
      w: 0.06210440397262573,
      h: 0.100614033639431,
      prob: 0.3225017189979553,
      name: 'clock' } ]

  */
});
```

After that you can compress original frames with ffmpeg:

```sh
ffmpeg -f rawvideo -s 768x576 -pix_fmt bgr24 -i data.raw data.jpg
```

