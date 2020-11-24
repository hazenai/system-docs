# Basic Installation Guide for Seatbelt and Mobile phone System Deployment

## Prerequisites

* Edge device is flashed with Nvidia `JetPack 4.4.0`
* Internet

## Edge Script Installtion

* Power Up your edge device and connect keyboard, mouse, monitor etc.
* Open terminal `CTRL + ALT + T`.
* Check if `python3` is installed on device `python3 --version`. If not [follow this link to install](https://phoenixnap.com/kb/how-to-install-python-3-ubuntu)
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
* Uncompress the downloaded file `tar -xvzf AutoDeployment-1.2.0.tar.gz`
* Change directory `cd AutoDeployment-1.2.0/edge/`. Check if `install.py` and `dependencies.yaml` is present.

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

## Running the services

* Move to following directory `cd $SD_CARD_MOUNTPOINT/hazen-test/imagetars/$COMPANY_NAME/`. For example, in this case, I will move to
`cd /media/sdcard/hazen-test/imagetars/hazenclient/`
* Run the following command `sudo cp -r ./sbmp/* ./`
* Now, Run the services `sudo ./run-services.sh`

