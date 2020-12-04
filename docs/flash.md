# Guidlines to flash edge devices

### [Axiomtek eBOX800-900-FL (Jetson TX2)](https://www.axiomtek.com/Default.aspx?MenuId=Products&FunctionId=ProductView&ItemId=24809&C=eBOX800-900-FL&upcat=350)

* Arrange a Host device (running ubuntu)
* Download `Linux_for_Tegra_JP4.4_87900_T004.tar.gz` from [here](https://www.asuswebstorage.com/navigate/a/#/s/AB36002AD03841239C868974B66619E0Y) on host device.
* Extract the downloaded file `tar -xvzf Linux_for_Tegra_JP4.4_87900_T004.tar.gz`.
* change directory `cd Linux_for_Tegra_JP4.4_87900_T004/Linux_for_Tegra`.
* Turn upside down the Axiomtek TX2 box and remove screws.
* Connect the micro usb cable from micro usb port on axiomtek TX2 (on CN7) to usb port on host device.
* Make the TX2 in recovery mode. See the process [here](https://www.axiomtek.com/Download/download/eBOX800-900-FL/eBOX800-900-FL%20User's%20Manual%20VA1_12-17-2018.pdf)
* On host device, run `sudo ./flash.sh -r jetson-tx2 mmcblk0p1`.
* Wait till flashing is down.
* Remove tx2 from recovery mode and enable it to boot from internal eMMC by rebooting.
