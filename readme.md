# Gstreamer examples

## Requirements

* Docker Engine

## Build

```sh
docker pull openvino/ubuntu20_data_runtime:latest
```


## Run

```sh
docker run -it --rm \
    -e DISPLAY=$DISPLAY \
    --net=host \
    -v ~/Videos:/videos \
    -v ~/projects/gstreamer-fiuba/models:/models \
    openvino/ubuntu20_data_runtime
```

## Basic pipelines

```sh
gst-inspect-1.0
```

```sh
gst-launch-1.0 videotestsrc ! fakesink
```

```sh
gst-launch-1.0 videotestsrc ! ximagesink
```

Display some frame rate related analytics:

```
gst-launch-1.0 videotestsrc ! fpsdisplaysink video-sink=xvimagesink sync=true
```

A shorter form:

```
gst-launch-1.0 videotestsrc ! fpsdisplaysink sync=true
```

## Processing pipelines

Median filter:

```
gst-launch-1.0 videotestsrc ! videomedian filtersize=9 ! fpsdisplaysink sync=true
```
Video transformation

```
gst-launch-1.0 -v videotestsrc ! circle ! videoconvert ! fpsdisplaysink sync=true
```

Edge detection:
```
gst-launch-1.0 -v videotestsrc ! edgetv ! videoconvert ! autovideosink
```

Gaussian Filtering

```
gst-launch-1.0 -v videotestsrc ! gaussianblur sigma=7 ! videoconvert ! autovideosink
```
Fish eye camera:

```
 gst-launch-1.0 -v videotestsrc ! fisheye ! videoconvert ! autovideosink
```

### Video processing

Read and display:

```
gst-launch-1.0 filesrc location=/videos/vehicles.mp4 ! decodebin ! videoconvert ! ximagesink
```

Median Filter:

```
gst-launch-1.0 filesrc location=/videos/vehicles.mp4 ! decodebin ! videomedian filtersize=5 ! videomedian filtersize=9 ! fpsdisplaysink sync=true
```

Object detection:

```
gst-launch-1.0 filesrc location=/videos/vehicles.mp4 ! \
               decodebin ! \
               gvadetect model=/models/person-vehicle-bike-detection-2000.xml ! \
	           gvawatermark ! \
               videoconvert ! \
               fpsdisplaysink sync=true
```

Object detection and tracking:

```
gst-launch-1.0 filesrc location=/videos/vehicles.mp4 ! \
               decodebin ! \
               gvadetect model=/models/person-vehicle-bike-detection-2000.xml ! \
               gvatrack tracking-type=short-term ! \
	           gvawatermark ! \
               videoconvert ! \
               fpsdisplaysink sync=true
```


## Advanced plugins

* [cameraundistort
](https://gstreamer.freedesktop.org/documentation/opencv/cameraundistort.html?gi-language=c#cameraundistort)

* [geometric transformations](https://gstreamer.freedesktop.org/documentation/geometrictransform/index.html?gi-language=c)

* [simplevideomarkdetect](https://gstreamer.freedesktop.org/documentation/videosignal/simplevideomarkdetect.html?gi-language=c)

* [Canny edge detector](https://gstreamer.freedesktop.org/documentation/opencv/edgedetect.html?gi-language=c#edgedetect)

* [gvaelements (dl streamer)](https://dlstreamer.github.io/elements/elements.html)

## Plugins List

* [gstreamer plugins](https://gstreamer.freedesktop.org/documentation/plugins_doc.html?gi-language=c)