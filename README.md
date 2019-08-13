# usb-sensors-linux
Cloned from https://github.com/tuxedo0801/usb-sensors-linux


# Introduction
Instructions how to install the airsensor utility on Linux.

# Tested on
* Linux Mint 19

# Details

## Requirements
The *Applied Sensors Indoor Air Monitor* USB sensor, also sold as *Voltcraft CO-20* at Conrad and Rehau *Raumluftfühler*

## LibUSB-dev
Install LibUSB-dev, which is needed when compiling the airsensor utility.

```
$ sudo apt-get install libusb-dev
```

# Compile tempsensor from sources

## Install required software
The 'build-essential' package is needed to be able to compile the source code.

```
$ sudo apt-get install build-essential
```

## Download the source code and compile

Download the latest version from github, no released version is packaged yet.

```
$ git clone https://github.com/skevas/usb-sensors-linux.git
$ cd airsensor
```

Compile the source code

```
$ gcc -o airsensor airsensor.c -lusb
```

## Test
You can now execute the airsensor and see if you get readings, as seen on most Linux installations it seems that you must be root user, or add a udev rule to make the device read/write.

To run with root access

```
$ sudo ./airsensor -o
2013-04-28 14:55:56, VOC: 450, RESULT: OK
$
```

Without root access it will print following

```
$ ./airsensor -o
2013-04-28 14:55:19, Error: claim failed with error:  -1
$
```

## Make Udev rule

Insert a udev rule to get read/write access to the device from normal user.

Create and edit following file:

```
$ sudo vi /etc/udev/rules.d/99-usb.rules
```

Add following lines:

```
SUBSYSTEM=="usb", ATTR{idVendor}=="03eb", ATTR{idProduct}=="2013", MODE="0666"
```

Save and reboot (or reload the udev rules). The device should be writable and give you results. 

Example, without sudo

```
$ ./airsensor -o
2013-04-28 15:08:55, VOC: 450, RESULT: OK
$
```

Please see readme for more usage instructions

# Troubleshooting

## Error: claim failed with error: -1

If you get following issue when running air sensor:

```
$ ./airsensor -o
2013-04-28 14:55:19, Error: claim failed with error:  -1
$
```

It means that you have no access to the USB device, two solutions;

a) use sudo, eg ```sudo ./airsensor```

b) create the udev rule as above
