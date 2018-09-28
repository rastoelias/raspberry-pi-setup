
# Raspberry Pi setup

Setting up a Raspberry Pi 3/3B+ without keyboard, mouse and ethernet cable.

## What you will need

### Hardware

* [Raspberry Pi 3/3B+](https://www.raspberrypi.org/products/)
* Power supply
* Formatted Micro SD card ([there are some guidelines that should be followed](https://www.raspberrypi.org/documentation/installation/sd-cards.md))
* Micro SD card reader

### Software / tools

* SD Memory Card Formatter 
* Image writing tool (e.g.: [Etcher](https://etcher.io))
* OS for Raspberry Pi (choose one)
    * [Raspbian stretch with desktop](https://www.raspberrypi.org/downloads/raspbian/)
    * [Raspbian stretch lite](https://www.raspberrypi.org/downloads/raspbian/)
* Terminal (Mac OS)

## Prepare SD card

1. Insert your formatted SD Card into your SD card reader.
2. Download the [Raspbian Lite disk image](https://www.raspberrypi.org/downloads/raspbian/).
3. Unzip the file and run **Etcher**.
4. Select the unzipped disk image file and destination (your SD card), click **Flash**, and wait until Etcher flashes the image.
5. When the process is finished Etcher will automatically unmount the SD card, so you need to remove and then insert your SD card to your reader again.

## Enable SSH

1. Create an empty `ssh` file in the root level of your SD card.
    ```
    cd /Volumes/boot
    touch ssh
    ```

## WiFi configuration

1. Clone this project.
    ```
    git clone https://github.com/rastoelias/raspberry-pi-setup.git raspberry-pi-setup
    ```
2. Cd to the root folder of this project.
    ```
    cd raspberry-pi-setup
    ```
3. Copy the `example-wpa_supplicant.conf` file to `wpa_supplicant.conf`
    ```
    cp example-wpa_supplicant.conf wpa_supplicant.conf
    ```
4. Open the `wpa_supplicant.conf` file in your preferred editor.
    ```
    nano wpa_supplicant.conf
    ```
5. Edit `country` (Use the [2 letter country abbreviation](https://en.wikipedia.org/wiki/ISO_3166-1) in CAPS: GB, US, etc..), the `ssid` and the `psk`, then save the file.
    ```
    country=YOUR_COUNTRY_ISO_CODE
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    network={
        ssid="YOUR_NETWORK_NAME"
        psk="YOUR_PASSWORD"
        key_mgmt=WPA-PSK
    }
    ```
6. Copy this file to the root of your sd card.
    ```
    cp wpa_supplicant.conf /Volumes/boot/wpa_supplicant.conf
    ```
7. Safely eject the card from your PC.

## Get it running

1. Insert an SD card with OS installed to the slot on your Raspberry Pi.
2. Plug the power supply connector.
3. Wait to boot. It takes quite a bit of time.

## SSH into your Raspberry Pi
To connect to your Raspberry Pi from another machine you need to know the **IP address of your Raspberry Pi** device, the Raspbian **username**, and **password**. The default username for raspberry is `pi` and the password is `raspberry`.

To find your Raspberry Pi IP address, open a web browser and enter the IP Address of your router (often printed on a label on your router) e.g. `http://192.168.1.1`. Enter the administrative login information to authenticate and access the admin settings. Find the list of connected devices (Depending on your router, the information about connected devices could be listed under many different headings.) There may be more connected devices on your network, but you should be abble to recognize your Raspberry Pi device.

1. Open a terminal on your Mac.
2. Run `RASPBIAN-USERNAME@YOUR-RASPBERRY-PI-IP`
    ```
    ssh pi@192.168.1.9
    ```
3. You will be prompted for the password for the **pi** user.
4. Type in your password, and if you did that correctly, you are in.


## Troubleshooting

### Q: I get a Warning trying to connect to the Pi
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
...
```
**A:** You probably reinstalled your Pi, but your Mac remember the old SSH key. You have to generate a new SSH key. To do that, run `ssh-keygen -R YOUR-RASPBERRY-PI-IP` on your Mac terminal.
```
ssh-keygen -R 192.168.1.9
```
