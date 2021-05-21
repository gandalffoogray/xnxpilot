***Warning***
=======================
Do not waste time & money on this if you are not familiar with linux and openpilot, debug skill required. 

---

Table of Contents
=======================

* [What is openpilot?](#what-is-openpilot)
* [About this](#about-this)
* [To do](#to-do)
* [Hardware preparation](#hardware-preparation)
* [Software preparation](#software-preparation)
* [Installation](#installation)
* [Openpilot patch](#openpilot-patch)
* [Credits](#credits)

---

What is openpilot?
------
[openpilot](http://github.com/commaai/openpilot) is an open source driver assistance system. Currently, openpilot performs the functions of Adaptive Cruise Control (ACC), Automated Lane Centering (ALC), Forward Collision Warning (FCW) and Lane Departure Warning (LDW) for a growing variety of supported [car makes, models and model years](#supported-cars). In addition, while openpilot is engaged, a camera based Driver Monitoring (DM) feature alerts distracted and asleep drivers.

About this
------
This project is to showcase how to run openpilot on Nvidia Jetson Xavier NX with minimal changes to openpilot.

The video was recorded on commit [22cf2e6440ca004994f30b7b9e8d0c20de35c52a](https://github.com/commaai/openpilot/tree/22cf2e6440ca004994f30b7b9e8d0c20de35c52a) on 17/05/2021 (v0.8.4). 

<table>
  <tr>
    <td><a href="https://youtu.be/ubxSSLWqyt8" title="YouTube" rel="noopener"><img src="http://i3.ytimg.com/vi/ubxSSLWqyt8/hqdefault.jpg"></a></td>
  </tr>
</table>

---

To do
------
- [x] Create build scripts.
- [ ] Add patch samples/tutorials.
- [ ] Add sensor support.
- [ ] Tuning tutorials.
- [ ] On road tests.

---

Hardware preparation
------
- [Nvidia Jetson Xavier NX](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-xavier-nx/)
- 32GB+ microsd card (UHS 3 speed minimum)
- [Waveshare IMX 219-83 Stereo Camera](https://www.waveshare.com/IMX219-83-Stereo-Camera.htm)
- [comma.ai Black Panda](https://comma.ai/shop/products/panda) (or white/grey panda but require more code customization.)
---

Software preparation
------
- Jetpack 4.5.1

---

Installation
------

1) [Install ubuntu 18.04 / Jetpack 4.5.1 on to sdcard](https://developer.nvidia.com/embedded/learn/get-started-jetson-xavier-nx-devkit)
- While installing, use username "**openpilot**".
2) clone this repo to your home directory (e.g. ```cd ~/ && git clone https://github.com/efinilan/xnxpilot.git```)
3) run: ```cd ~/xnxpilot/ && ./1_install.sh``` wait for reboot.
4) run: ```cd ~/xnxpilot/ && ./2_install.sh``` wait for reboot.
5) run: ```cd ~/xnxpilot/ && ./3_install.sh``` wait for reboot.
6) Completed, this should be the minimal configuration to run openpilot on Jetson.

---

Openpilot patches
------
This is based on the openpilot master branch and is constantly changing, so there is no patch that works all the time.
Here I will explain why these files are changed so you can update the patches in the future.

**Notes**: All the sample patches below are based on commit [22cf2e6440ca004994f30b7b9e8d0c20de35c52a](https://github.com/commaai/openpilot/tree/22cf2e6440ca004994f30b7b9e8d0c20de35c52a) on 17/05/2021 (v0.8.4).


#### patches/1_SConstruct.diff
This file patches ```SConstruct``` file in the openpilot root folder.
Here we:
1) Create a new ```arch``` called ```linuxarm64``` so the reset of the modules will not mis-identify your board as ```aarch64``` or ```larch64``` used by comma.ai's board.
2) Setup the right environment path/variables, since it's not ```aarch64``` or ```larch64```, the default configuration uses x64/x86 architecure, however, jetson is arm64 (aarch64) based, so we have to utilize a few libraries from ```larch64``` configuration.

#### patches/2_selfdrive-crash.py.diff
This file patches ```selfdrive/crash.py``` so it does not use sentry.io library
Noted openpilot has plan to deprecate sentry.io service so the file may not exist in the future.
Here we:
1) keep all the function/method and simply ```pass``` the logic.
2) remove everything related to sentry sdk


**To Be Continued**

---

Credits
------
- Sid for his [installer script](https://discord.com/channels/660951518014341124/697074382018707507/841722618150780988)
- [dragonpilot Community](https://github.com/dragonpilot-community/dragonpilot/)
- [RetroPilot Community](https://discord.gg/fGUuASVZKg)
- [Unofficial OpenPilot Community](https://discord.gg/Mrf8FwfWSr)