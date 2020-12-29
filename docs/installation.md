# Basic Installation Guide for Trajectory and SBMP System Deployment

## Prerequisites

* Edge device is flashed with Nvidia `JetPack 4.4.0` (Make sure Jetson SDK Components are also installed)
* Internet
* Storage (at least 32GB)

## Edge Script Installtion

* Power Up your edge device and connect keyboard, mouse, monitor etc.
* Open terminal `CTRL + ALT + T`.
* Check if `python3` is installed on device `python3 --version`. If not [follow this link to install](https://phoenixnap.com/kb/how-to-install-python-3-ubuntu)
* Check if docker is installed and running `sudo docker info`. If docker is running, make sure it's `Server Version` is `19.03.6`
* Add current user to `docker` group and then reboot
```bash
sudo usermod -aG docker $USER
sudo reboot
```
* Check if nvidia-container-runtime is installed `nvidia-container-runtime --version`. If it's not installed, then install
```bash
# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
# nvidia-container-runtime installation
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit nvidia-container-runtime
sudo systemctl restart docker
```
* Download Edge Scripts `wget GET_LINK_FROM_ABDULLAH`
* Uncompress the downloaded file `tar -xvzf AutoDeployment-1.4.0.tar.gz`
* Change directory `cd AutoDeployment-1.4.0/edge/`. Check if `install.py` and `dependencies.yaml` is present.

## Running the Edge Script

* RUN the install script `sudo python3 install.py -d -p`. `-d` flag will install system dependencies like `docker, docker-compose` and `-p` flag will set power mode on jetson devices.
* Now, answer the questions asked in command line.
* Select storage disk. Choose SD card
```bash
NAME         SIZE      MOUNTPOINT       UUID
mmcblk0p1    14G       /                eda65433-df7c-4de9-8c4b-9ab2865b4f24
mmcblk1p1    234.7G    /media/sdcard    a10a85fe-a451-dd4c-b3b0-6862eb17eb56

Choose which disk to use for deployment[1-2]: 2

```  
* Select problem type
```bash
Select the problem you want to deploy:
0: Video Light Phase Detection
1: SeatBelt/MobilePhone
2: Trajectory
[0-2]: 1

```
* Type your company name
```bash
Please enter your company's name: hazenclient

```
* Select Power mode on Jetson devices. choose with id=2
```bash
################ Collecting Hardware Info #######################
## Power Model Selection on Jetson Devices ##
ID=0 NAME=MODE_15W_2CORE
ID=1 NAME=MODE_15W_4CORE
ID=2 NAME=MODE_15W_6CORE
ID=3 NAME=MODE_10W_2CORE
ID=4 NAME=MODE_10W_4CORE

Select Power Model. ID: 2

```
* Now, script will start installing system dependencies.
* After system dependencies installation, script will install required services. This process may take `20-30 mins`.

## Running the services (Trajectory Configuration)

* Move to following directory `cd $SD_CARD_MOUNTPOINT/hazen-test/imagetars/$COMPANY_NAME/`. For example, in this case, I will move to
`cd /media/sdcard/hazen-test/imagetars/hazenclient/`
* Run the following command `sudo cp -r ./traj/* ./`
* Make sure you have a working h264 or h265 encoded rtsp video stream url.
* Change `cam_id` key in `service_configs/image_cache/config.yaml` to gstreamer pipeline with your rtsp stream as source. For Example:
For H265 encoded stream 

```
cam_id: "rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"
```

* If there is any ALPR image-cache being used. Also change it's `camid` key in `service_configs/image_cache_alpr/config.yaml`.
* Configure Violation regions and rules
* Now, Run the services `sudo ./run-services.sh`

## Running the services (SBMP Configuration)

* Move to following directory `cd $SD_CARD_MOUNTPOINT/hazen-test/imagetars/$COMPANY_NAME/`. For example, in this case, I will move to
`cd /media/sdcard/hazen-test/imagetars/hazenclient/`
* Run the following command `sudo cp -r ./sbmp/* ./`
* Open `image_cache` config. `sudo nano service_configs/image_cache/config.yaml`
* If your have a working h264 or h265 encoded rtsp video stream url and want to use that then change `cam_id` key in `service_configs/image_cache/config.yaml` to gstreamer pipeline with your rtsp stream as source. For Example:
For H265 encoded stream, following gstreamer pipeline will work (Also note: this pipeline is resizing frames to 1920x1080)

```
cam_id: "rtspsrc location=<rtsp_url> latency=0 drop-on-latency=true ! rtph265depay ! h265parse ! omxh265dec  disable-dpb=true ! nvvidconv interpolation-method=1 ! video/x-raw, width=1920, height=1080, format=(string)BGRx ! appsink"
```
* If you are using rtsp stream with gstreamer pipeline, update `encoder` key. Change its value from `ffmpeg` to `gstreamer`
* If you are using rtsp stream, update `delay_for_file_ms` key. Make its value `0`. Because rtsp stream has built-in delay between frames. This value is used while running from video files and adding a
delay to adjust the fps.
* save(CTRL+S) changes in image-cache config and exit (CTRL+X)
* Open `object_detector` config for changes and update `trt_preprocessor_input_dims`. This take frames dimensions in following format [height, width, channels]. If you have not resized the frames
in gstreamer pipeline in `image_cache`, then get frame  dimensions (resolution) from camera settings. If you have resized frame in gstreamer pipeline, use them. In above example, we have resized frames 
to 1920x1080, so we will update it [1080, 1920, 3].
* Save and exit od config.
* Configure Violation regions and rules
* Now, Run the services `sudo ./run-services.sh`
