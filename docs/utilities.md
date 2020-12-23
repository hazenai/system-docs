# Utilities / Snippets

## Gstreamer pipelines For Image Cache
- **For H.265**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink
```
An Example of password protected RTSP url can be `rtsp://admin:hazen123@192.168.1.54:554/Streaming/Channels/101/`
- **For H.264**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph264depay ! h264parse ! omxh264dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink
```
- **For JPEG Encoding**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph264depay ! h264parse ! omxh264dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! jpegenc ! image/jpeg ! appsink
```


## Converting OD Engine From `.onnx` to `.trt`
- For static engine
```
/usr/src/tensorrt/bin/trtexec --onnx=/workspace/yolov3_torch1.onnx --saveEngine=./yolov3_torch1.trt --fp16 --verbose --workspace=2048
```
- For dynamic shape engine
```
/usr/src/tensorrt/bin/trtexec --onnx=./engine/epoch99sim.onnx --saveEngine=./engine/epoch99sim.trt --fp16 --verbose --workspace=2028 --explicitBatch --optShapes=input:1x3x256x448 --minShapes=input:1x3x256x448 --maxShapes=input:128x3x256x448
```

## Frequently used `docker` and `docker-compose` commands
- To start all services
```
docker-compose --compatibility up -d
```
- To Shutdown the system
```
docker-compose --compatibility down --volume
```
- To Restart single or multiple service
```
docker-compose --compatibility restart <service_name1> <service_name2> ...
```
For Example to restart image cache service:
```
docker-compose --compatibility restart image-cache
```
- To Stop a service
```
docker-compose --compatibility stop <service_name>
```
- To Start a service
```
docker-compose --compatibility start <service_name> 
```
- To see logs of a service
```
docker logs -f <container_name>
```
- To see status of services
```
docker ps
```
- To see system resource usage of services
```
sudo docker stats
```