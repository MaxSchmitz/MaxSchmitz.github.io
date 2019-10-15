# Starting up headless raspberry pi

Get lastest version of [Rasbian](https://www.raspberrypi.org/downloads/raspbian/)

Get etcher [Etcher](https://www.balena.io/etcher/)

Flash OS image to SD card

enable ssh

touch /Volumes/boot/ssh

Get your your current network information from Keychain Access

subl /Volumes/boot/wpa_supplicant.conf

wpa_suppliant.conf contents:

country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="NETWORK-NAME"
    psk="NETWORK-PASSWORD"
}

If ejecting the disk complains cd ~ then eject

Ready to boot up raspberry pi

Remove previous keys

ssh-keygen -R raspberrypi.local

The -R option removes all keys belonging to hostname from a known_hosts file.  This option is useful to delete hashed hosts if this isn't your first time setting up a pi.

Login with defaults
ssh pi@raspberrypi.local

Change password
sudo raspi-config

Copy over public key
ssh-copy-id pi@raspberry.local




