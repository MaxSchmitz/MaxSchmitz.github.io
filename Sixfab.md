# Connect to Internet with SixFab App Shield

## Software setup
`sudo apt-get install raspberrypi-kernel-headers`

`ls /usr/src`

gives us 4.19.75


`wget https://raw.githubusercontent.com/sixfab/Sixfab_RPi_3G-4G-LTE_Base_Shield/master/tutorials/QMI_tutorial/qmi_install.sh`


shutdown to unplug shield completely from pi

`sudo shutdown -h now`

run the installer script

`sudo ./qmi_install.sh`

Press any key to reboot

after reboot shutdown pi
`sudo shutdown -h now`

Plug in shield and power back up

After reboot please follow commands mentioned below
go to /home/pi/files/quectel-CM and run sudo ./quectel-CM -s [YOUR APN]

```
cd ~/files/quectel-CM
sudo ./quectel-CM -s hologram
```
If you get the following output then you need to power on the shield with the pwr button on the shield:
```
Cannot find qmichannel((null)) usbnet_adapter((null)) for Quectel modules
```

You should see something like this:
```
~/files/quectel-CM $ sudo ./quectel-CM -s hologram
[10-16_16:31:36:670] WCDMA&LTE_QConnectManager_Linux&Android_V1.1.45
[10-16_16:31:36:671] ./quectel-CM profile[1] = hologram///0, pincode = (null)
[10-16_16:31:36:673] Find /sys/bus/usb/devices/1-1.2 idVendor=2c7c idProduct=0296
[10-16_16:31:36:673] Find /sys/bus/usb/devices/1-1.2:1.4/net/wwan0
[10-16_16:31:36:673] Find usbnet_adapter = wwan0
[10-16_16:31:36:673] Find /sys/bus/usb/devices/1-1.2:1.4/usbmisc/cdc-wdm0
[10-16_16:31:36:673] Find qmichannel = /dev/cdc-wdm0
[10-16_16:31:36:691] cdc_wdm_fd = 7
[10-16_16:31:36:710] Get clientWDS = 1
[10-16_16:31:36:712] Get clientDMS = 1
[10-16_16:31:36:714] Get clientNAS = 1
[10-16_16:31:36:718] Get clientUIM = 1
[10-16_16:31:36:720] Get clientWDA = 1
[10-16_16:31:36:723] requestBaseBandVersion BG96MAR02A10M1G
[10-16_16:31:36:732] requestGetSIMStatus SIMStatus: SIM_READY
[10-16_16:31:36:733] requestSetProfile[1] hologram///0
[10-16_16:31:36:748] requestGetProfile[1] hologram///0
[10-16_16:31:36:750] requestRegistrationState2 MCC: 310, MNC: 410, PS: Attached, DataCap: LTE
[10-16_16:31:36:752] requestQueryDataCall IPv4ConnectionStatus: DISCONNECTED
[10-16_16:31:36:756] requestRegistrationState2 MCC: 310, MNC: 410, PS: Attached, DataCap: LTE
[10-16_16:31:36:770] requestSetupDataCall WdsConnectionIPv4Handle: 0x833bbc10
[10-16_16:31:36:774] requestQueryDataCall IPv4ConnectionStatus: CONNECTED
[10-16_16:31:36:779] change mtu 1500 -> 1460
[10-16_16:31:36:779] ifconfig wwan0 up
[10-16_16:31:36:794] busybox udhcpc -f -n -q -t 5 -i wwan0
udhcpc: started, v1.30.1
No resolv.conf for interface wwan0.udhcpc
udhcpc: sending discover
udhcpc: sending select for 10.170.90.191
udhcpc: lease of 10.170.90.191 obtained, lease time 7200
Too few arguments.
Too few arguments.
[10-16_16:33:55:029] requestRegistrationState2 MCC: 310, MNC: 410, PS: Attached, DataCap: LTE
```

Now check to make sure internet connection works

`ifconfig wwan0`

`ping -I wwan0 -c 5 8.8.8.8`

Hopefully everything is working by this point.

## Now we can add Auto connect

`cd ~`

`wget https://raw.githubusercontent.com/sixfab/Sixfab_RPi_3G-4G-LTE_Base_Shield/master/tutorials/QMI_tutorial/install_auto_connect.sh`

permissions

`sudo chmod +x install_auto_connect.sh`

run install script

`sudo ./install_auto_connect.sh`

check to make sure qmi_reconnect service is active

`sudo systemctl status qmi_reconnect.service`

should have a green active


## now its time to set up hologram tunnel to access pi remotely

go to [holgram.io](hologram.io) configure device for spacebridge

download the space bridge client

add api key and set remote port 22 and local port 5000

`ssh pi@localhost -p 5000`
