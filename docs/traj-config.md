# Trajectory COnfiguration

## Changing System stream to rtsp stream
#### Prerequisites
- A working h264 or h265 encoded rtsp video stream url.
#### Changing Config
- Change `camid` key in `service_configs/image_cache/config.yaml` to gstreamer pipeline with your rtsp stream as source. For Example:
For H265 encoded stream 
```
camid: "rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"
```
- If there is any ALPR image-cache being used. Also change it's `camid` key in `service_configs/image_cache_alpr/config.yaml`.
- Restart overview and alpr image-cache services.
```
docker-compose --comptibility restart image-cache image-cache-alpr
```


## Configuring Violation regions and rules