# Utilities / Snippets

## Gstreamer pipelines For Image Cache

- **For H.265 (Resizing frames to 1920x1080)**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! appsink
```
An Example of password protected RTSP url can be `rtsp://username:password@192.168.1.90:554/Streaming/Channels/101/`

- **For H.264 (Resizing Frames to 1920x1080)**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph264depay ! h264parse ! omxh264dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! appsink
```

- **H264 with JPEG Encoding Enabled and Resizing**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph264depay ! h264parse ! omxh264dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! jpegenc ! image/jpeg ! appsink
```
- **H265 with JPEG Encoding Enabled and Resizing**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! jpegenc ! image/jpeg ! appsink
```

- **H264 Without Resizing,Formating and JPEG Encoding**
```
rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw ! appsink
```
> Testied On Verra Rudi-NX (Jan 5, 2020)

- **Stream from Video File, with H264, Resizing Frame and JPEG Encoding**
```
filesrc location=/workspace/video.mp4 ! qtdemux ! h264parse ! omxh264dec ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080 ! jpegenc ! image/jpeg ! appsink
```

- **Stream from Video File, with H264 and JPEG Encoding**
```
filesrc location=/workspace/video.mp4 ! qtdemux ! h264parse ! omxh264dec ! nvvidconv interpolation-method=1 ! video/x-raw ! jpegenc ! image/jpeg ! appsink
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

- To pull images from docker registry
```
docker-compose pull
```
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

## `systemctl` commands

systemctl is part of systemd which is a service managment tool responsible for starting services on boot and controlling the state of these services later on.
Following services are being run as Systemd services and systemctl will be used to control them.

- `docker`
- `sshtunnel`

**Check status of a service**
```
sudo systemctl status <service>
```

**Restart a service**
```
sudo systemctl restart <service>
```

**Stop a service**
```
sudo systemctl stop <service>
```

**Enable service to be started on boot**
```
sudo systemctl enable <service>
```
