# Utilities / Snippets

### Gstreamer pipelines For Image Cache
- For H.265
```
rtspsrc location=rtsp://admin:hazen123@192.168.1.54:554/Streaming/Channels/101/ latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink
```
- For H.264
```
rtspsrc location=rtsp://admin:hazen123@192.168.1.54:554/Streaming/Channels/101/ latency=0 drop-on-latency=true ! rtph264depay ! h264parse ! omxh264dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink
```
- For JPEG Encoding
```
rtspsrc location=rtsp://admin:hazen123@192.168.1.54:554/Streaming/Channels/101/ latency=0 drop-on-latency=true ! rtph264depay ! h264parse ! omxh264dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! jpegenc ! image/jpeg ! appsink
```


### Converting OD Engine From `.onnx` to `.trt`
- For static engine
```
/usr/src/tensorrt/bin/trtexec --onnx=/workspace/yolov3_torch1.onnx --saveEngine=./yolov3_torch1.trt --fp16 --verbose --workspace=2048
```
- For dynamic shape engine
```
/usr/src/tensorrt/bin/trtexec --onnx=./engine/epoch99sim.onnx --saveEngine=./engine/epoch99sim.trt --fp16 --verbose --workspace=2028 --explicitBatch --optShapes=input:1x3x256x448 --minShapes=input:1x3x256x448 --maxShapes=input:128x3x256x448
```