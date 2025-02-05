# Library for EVA II PRO WiFi Smart Dehumidifier appliance
[![PyPI](https://img.shields.io/pypi/v/midea_python_client.svg)](https://pypi.org/project/midea_python_client/)
[![](https://img.shields.io/pypi/pyversions/midea_python_client.svg)](https://pypi.org/project/midea_python_client/)
[![](https://img.shields.io/pypi/l/midea_python_client.svg)](https://pypi.org/project/midea_python_client/)
[![](https://img.shields.io/pypi/wheel/midea_python_client.svg)](https://pypi.org/pypi/midea_python_client/)
[![](https://img.shields.io/pypi/status/midea_python_client.svg)](https://pypi.org/pypi/midea_python_client/)
[![](https://img.shields.io/pypi/implementation/midea_python_client.svg)](https://pypi.org/pypi/midea_python_client/)
[![<100kB](https://img.shields.io/github/languages/code-size/dellorogiulio/midea_python_client.svg)](https://github.com/dellorogiulio/midea_python_client)
[![<100kB](https://img.shields.io/github/repo-size/dellorogiulio/midea_python_client.svg)](https://github.com/dellorogiulio/midea_python_client)
[![Build status](https://travis-ci.com/dellorogiulio/midea_python_client.svg?branch=master)](https://travis-ci.com/dellorogiulio/midea_python_client)
[![Known Vulnerabilities](https://snyk.io/test/github/dellorogiulio/midea_python_client/badge.svg?targetFile=requirements.txt)](https://snyk.io/test/github/dellorogiulio/midea_python_client?targetFile=requirements.txt)
[![HitCount](http://hits.dwyl.io/dellorogiulio/midea_python_client.svg)](http://hits.dwyl.io/dellorogiulio/midea_python_client)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=5E7ULVFGCGKU2&source=url)


Original Author: Andrea Barbaresi =2018, 2019=
Licence: GPLv3


This repo contains the python package ***midea_python_client*** that implements a client-side library to connect to the Web API provided by Midea/Inventor, in order to remotely control an **EVA II PRO WiFi Smart Dehumidifier device**.

Info about the dehumidifier appliance can be found [here.](https://www.inventorappliances.com/dehumidifiers/eva-ii-pro-wi-fi-20l)

You can buy the smart dehumidifier appliance (WiFi version) on Amazon (the two links below contain my referral code):
* [Amazon.co.uk](https://www.amazon.co.uk/gp/product/B07665CCSM/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=barban0d-21&creative=6738&linkCode=as2&creativeASIN=B07665CCSM&linkId=a7408b12a09679586e1816acc3c9d74b)
* [Amazon.it](https://www.amazon.it/gp/product/B075486X31/ref=as_li_tl?ie=UTF8&camp=3414&creative=21718&creativeASIN=B075486X31&linkCode=as2&tag=barban03-21&linkId=33e8ff818aaa4b45f0c320e6661773b2)


Target devices
--------------
Even though the library has been designed to generically address any kind of existing MIDEA devices, ***please note that at the moment the implemented functionalities work on the dehumidifier appliance only (0xA1 type devices).***

If you are interested in developing code that is able to control Midea/Inventor Air Condition systems (0xAC type devices), you can have a look at ***midea-air-condition*** Ruby&Rails library by **Balazs Nadasdi** available [here.](https://github.com/yitsushi/midea-air-condition)


Prerequisites
-------------
In order to control the EVA II PRO WiFi Smart Dehumidifier appliance using the provided Python library, first of all it is necessary to download and install the official App, in order to register a valid user to the cloud platform (a valid email address is required). 
The official companion Apps are available on Google's and Apple's App Stores:
* [Google Play](https://play.google.com/store/apps/details?id=com.inventor)
* [Apple Store](https://itunes.apple.com/gr/app/invmate-ii/id1109243423)

Once connected with valid credentials (i.e. email address and password), your home device has to be added to the list of configured devices using the App (please refer to the manual of the official App to accomplish this task).

Once having a valid registered user and the home device configured, you can start to use the python library instead of the offical App to control the device via Internet (both the client when the library is installed and the home device should be connected to the Internet).


Installation
------------
Install from PyPi using [pip](http://www.pip-installer.org/en/latest), a package manager for
Python.
```
pip install midea_python_client
```
Don't have pip installed? Try installing it, by running this from the
command line:
```
$ curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python
```
Or, you can [download the source code (ZIP)](https://github.com/dellorogiulio/midea_python_client/zipball/master) and then run:
```
python setup.py install
```
You may need to run the above commands with ``sudo``.


Getting started
---------------
Minimal steps to use the library in your python code are reported below:

**Step 1: Include the python package**
```python
from midea_python_client import MideaClient
```
**Step 2: Instantiate the MideaClient object**

Using clear-text password:
```python
client = MideaClient("user.example@gmail.com", "myPassword", "")
```
Using password's sha-256 hash:
```python
client = MideaClient("user.example@gmail.com", "", "76549b827ec46e705fd03831813fa52172338f0dfcbd711ed44b81a96dac51c6")
```
**Enable logging (optional):**

You can enable logging by setting the 'verbose' parameter to True (default is False) in the MideaClient constructor. 
Set 'debug' parameter to True in order to log debugging messages too (default is False).
Set 'logfile' string parameter to a full-path filename in order to make the library log messages into a file instead of using the console (default).
E.g.:
```python
_email = "user@example.com"
_password = "passwordExample"
_sha256password = ""
_verbose = True		#Enable logging
_debug = False		#Disable debug messages
_logfile = ""		#Log to console (default)
client = MideaClient(_email, _password, _sha256password, _debug, _verbose, _logfile)
```
**Step 3: Activate a new session by logging in**
```python
res = client.login()
if res == -1:
  print "Login error: please check log messages."
else:
  sessionId = client.current["sessionId"]
```
**Step 4: Get the target deviceId by retrieving the list of configured appliances**
```python
appliances = {}
appliances = client.listAppliances()
for a in appliances:
  print "[id="+a["id"]+" type="+a["type"]+" name="+a["name"]+"]"
```
**Step 5: Send commands to control the target device**
Get the device state:
```python
res = client.get_device_status(deviceId)
if res == 1:
  print client.deviceStatus.toString()
```
Power-on:
```python
res = client.send_poweron_command(deviceId)
if res:
  print client.deviceStatus.toString();
```
Power-off:
```python
res = client.send_poweroff_command(deviceId)
if res:
  print client.deviceStatus.toString();
```
Set Ion on:
```python
res = client.send_ion_on_command(deviceId)
if res:
  print client.deviceStatus.toString();
```
Set Ion off:
```python
res = client.send_ion_off_command(deviceId)
if res:
  print client.deviceStatus.toString();
```
Set fan speed:
```python
if speed > 0 and speed < 100:
  res = client.send_fan_speed_command(deviceId, speed)
  if res:
    print client.deviceStatus.toString();
```
Set target humidity:
```python
if hum >= 30 and hum <= 70:
  res = client.send_target_humidity_command(deviceId, hum)
  if res:
    print client.deviceStatus.toString();
```
Set operation mode:
```python
if mode > 0 and mode < 5:
  res = client.send_mode_command(deviceId, mode)  #set Mode (1:TARGET_MODE, 2:CONTINOUS_MODE, 3:SMART_MODE, 4:DRYER_MODE)
  if res:
    print client.deviceStatus.toString();
```
Set updated status (usefull to update multiple attributes at one):
```python
status =client.get_device_status(deviceId)  #get current status
#Update status
status.ionSetSwitch = 1   #Ion on
status.setMode = 1        #target-mode
res = self._client.send_update_status_command(self._device["id"], status)
if res:
  print client.deviceStatus.toString();
```

Client example
--------------
This repo also contains a fully working client (***dehumi_control.py***) that demonstrates how to use the ***midea_python_client*** library in order to control the EVA II PRO WiFi Smart Dehumidifier appliance via a Command Line Interface.

To use the client, the email address of a registered user and the associated password have to be provided via command line parameters (either clear-text password or password's sha-256 hash has to be provided using the '-p' or '-s' options):
```
# python dehumi_control.py  -h
Usage:dehumi_control.py -e <email_address> -p <cleartext password> -s <sha256_password> -l <logfile> [-h] [-v] [-d]
```

Home Assistant custom-component
-------------------------------
***[NEW]*** A custom integration for Home Assistant platform (version 0.96.0 or newer) can be found at [Home Assistant Custom Integration for EVA II PRO WiFi Smart Dehumidifier appliance by Midea/Inventor](https://github.com/barban-dev/homeassistant-midea-dehumidifier) repository. 


Internals 
---------
***You can skip this part if you are not interested in technical details concerning the format of the API messages used by the library.***

Official companion Apps for Android and IOS platforms are based on the midea-SDKs made available by Midea Smart Technology Co., Ltd.:
* [ios-sdk](https://github.com/midea-sdk-org/ios-sdk)
* [android-sdk](https://github.com/midea-sdk-org/android-sdk)

According to the SDK's documentation, "MideaSDK is a software develop kit maintained by MSmart. You can develop your own APP, Smart Hardware or Smart TV based on this SDK to control the smart appliances produced by Midea."

Official documentation for the open API can be found here (chinese language only):
https://github.com/midea-sdk/midea-sdk.github.io/tree/master/api

Apart Androd and IOS platforms, no other environment is currently officially supported. In order to develop the client-side library for all the platform supporting Python, I used a Man-In-The-Middle Web Proxy as a packet sniffer to understand the basics on the API messages exchanged between the offical Android client and the Midea cloud Server.

Web API server can be reached via ```https://mapp-appsmb.com/<endpoint>``` (POST web requests shoud be used).

A brief description of the most relevant endpoints follows:

```/v1/user/login/id/get``` endpoint with 'loginAccount' parameter is used to get 'loginId' parameter (different for each session).

```/v1/user/login``` endpoint with 'password' parameter is used to perform the login ('accessToken' and 'sessionId' parameters are returned). The password parameter sent by the client is SHA-256 hash of a string derived from 'loginId', 'password' and 'appKey' parameters.

```/v1/appliance/user/list/get``` endpoint is used to retrieve the list of configured devices together withh all the associated parameters ('name', 'modelNumber', 'activeStaus', 'onlineStatus', etc.)

```/v1/appliance/transparent/send``` endpoint with the 'order' parameter is used to control the home device (the 'reply' parameter is returned). Both the 'order' and 'reply' parameters are AES encryted; the encryption/decryption key used by AES is derived from the 'APP_key' parameter (constant string) and the 'accessToken' parameter returned when logging in. The revelant part of code used for the encription and decryption tasks can be found in the **MideaSecurity** class in **midea_security.py** file.

For Further Studies (FFS)
-------------------------
At the moment, the client-side Python library can control a dehumidifier appliance by sending API messages to the cloud Server that talks to the home device. Both the client and the home device need Internet access in this cloud-to-cloud scenario. 
The possibility to control the home device locally (i.e. the possibility to let the client to send API messagges directly to the home device) when both the client and the home device are associated to the same WiFi network is FFS.

How to contribute
-----------------
If you can code in Python and you are interested in improving and expanding this work, feel free to clone this repo. Drop me a line if you wish to merge your modifications in my repo too.

Disclaimer
----------
Besides owning an EVA II PRO WiFi device, I have no connection with Midea/Inventor company. This library was developed for my own personal use and shared to other people interested on Internet of Things systems and domotic platforms. This software is provided as is, without any warranty, according to the GNU Public Licence version 3.

Donations
---------
If this project helps you to reduce time to develop your code, you can make me a donation.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=5E7ULVFGCGKU2&source=url)

