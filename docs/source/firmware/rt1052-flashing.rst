RT1052 tutorial by ColdTurkey
========

Below is the tutorial developed by ColdTurkey and posted on LinuxCNC forum. Shared here while the docs are developed.

https://forum.linuxcnc.org/18-computer/44828-remora-ethernet-nvem-ec300-ec500-cnc-board?start=1640#295702

### DRAFT ONLY ###
#1 - Install Linuxcnc 2.9 on PC - THIS IS NOT A LINUXCNC TUTORIAL - Follow instructions on Linuxcnc site for ALL linuxcnc related questions.
#2 - Updating to 6.3RT Kernel (Optional)
#3 - Fix latency issues with Realtek NIC and install WIFI drivers (Work in progress. Optional)
#4 - Install ProbeBasic (Optional)
#5 - Installing PYOCD and TFTPY
#6 - Setup debug probe
#7 - Flash firmware to EC500 (RT1052)
#8 - Install Ethernet component
#9 - Flash CONFIG to EC500
### Please report errors to Cold Turkey ###

#1# DOWNLOAD AND INSTALL THE LINUXCNC ISO #

#1 Open Console

sudo apt update
sudo dpkg -P raspi-firmware
sudo apt upgrade
sudo apt-get install geany ethtool grub-customizer usbutils git

#2# Upgrade Kernel to 6.3RT OPTIONAL ##
#2 download linux-headers and linux-image from here: https://drive.google.com/drive/folders/1NzQIHnf9M_cHzuZCqSldVFGschOOxaER
#2 Unzip files to folder.
#2 Open terminal in that folder and run the code below (note: pressing (tab) lets linux autocomplete the line for you to save typing)

sudo dpkg -i linux-headers(tab)
sudo dpkg -i linux-image(tab)
sudo grub-customizer

#2 Using grub customizer window: Delete the kernels that aren't 6.3 (should have two 6.3 kernels left) OR Make the new 6.x kernel the default. Eg move to the first entry and then REBOOT

#2 Check our new kernel is the default - Open Console

uname -v

#2 Should give result like: #2 SMP PREEMPT_RT Wed Dec 14 00:49:27 AEST 2022 (Example Only)

uname -r

#2 Should give result like: 6.3.0-rt5-linuxcnc-rt (Example Only)

#3# FOR REALTEK NIC ETHERNET LATENCY ISSUES - UNFINISHED WORK IN PROGRESS - OPTIONAL ###
#3# For Realtek usb WIFI adapters - skip first part of step 3 - Tested - Working only with some dongles - OPTIONAL
#3 open console

ip a

#3 Look for altname (should look like enp0s31f6)

sudo ethtool -i (name here)

#3 CHECK DRIVER and FIRMWARE MATCH = SOMETHING LIKE r8169 (IF NOT THEN CONTINUE)

sudo geany /etc/sources.list

#3 Comment out CD or DVD #

sudo apt-get update
sudo apt-get install (driver you need)-dkms
(EXAMPLE) sudo apt-get install r8168-dkms
sudo reboot

#3 CHECK AGAIN

sudo ethtool -i (name)

#3 Get Realtek Wifi working - Tested - Works with some of my usb wifi dongles but not all - Make sure you have usbutils installed from step #1
lsusb
sudo apt-cache search firmware-realtek
sudp apt install firmware-realtek
sudo git clone "https://github.com/RinCat/RTL88x2BU-Linux-Driver.git" /usr/src/rtl88x2bu-git
sudo sed -i 's/PACKAGE_VERSION="@PKGVER@"/PACKAGE_VERSION="git"/g' /usr/src/rtl88x2bu-git/dkms.conf
sudo dkms add -m rtl88x2bu -v git
sudo dkms autoinstall
sudo reboot

#4# INSTALL PROBE BASIC - OPTIONAL ####
#4 Open Console

sudo apt install python3-qtpyvcp

sudo apt install python3-probe-basic

#5# PYOCD and TFTPY #####
#5 Open Console

sudo apt install pipx
pipx ensurepath

#5 Restart Console

pipx install pyocd
pipx ensurepath
pip3 install tftpy --break-system-packages

#5 Restart Console

#6# SETUP DEBUG PROBE TO WORK WITH PYOCD ######
#6Plug in debug probe and open a terminal

pyocd list

#6#  If you can see your probe then skip step 6 and continue with installing remora > step 7   
#6# If you can't see your probe then we need to create or modify the *.rules in the udev folder: Download these rulesets: https://github.com/pyocd/pyOCD/tree/main/udev
#6# Navigate to your downloaded folder and open a console
#6# With your debug probe plugged run the following - Make sure you have usbutils installed in step #1

lsusb

#6# Identify your probe: CMAS, CMSIS, STLINK, etc. Identify Address (EXAMPLE: Bus001 Device007)
#6# Run the following in terminal with correct device address (EXAMPLE: Bus001 Device007)

udevadm info -a -p $(udevadm info -q path -n /dev/bus/usb/001/007)

#6# Read through generated data and find the following fields and copy their "values". ("numbers" below as example only)
#6# SUBSYSTEM=="usb"
#6# ATTR{idVendor}=="6666"
#6# ATTR{idProduct}=="9930
#6# Press q when you have what you want to quit the search
#6# In the folder with the rules you downloaded, find the right ruleset (STLINK, etc) open in notepad
#6# If you cannot find a rule with values matching your values then add it to the bottom
#6# (EXAMPLE) SUBSYSTEM=="usb", ATTR{idVendor}=="6666", ATTR{idProduct}=="9930"
#6# Open Terminal in folder containing your new modified rules and run

sudo cp *.rules /etc/udev/rules.d
sudo udevadm control --reload-rules
sudo reboot

#6# Open terminal - with debug probe plugged in run

pyocd list

#6# You should now see your debug probe.

#7# FLASHING REMORA FIRMWARE TO EC500 ##
#7# DOWNLOAD REMORA CPPMAIN FROM GIT##
#7 Plug your debug probe into the EC500 header pins and power the EC500
#7 Open console in the downloaded folder containing Remora Firmware and run

pyocd flash remora-rt1052-3.1.2.bin --target mimxrt1050_quadspi

#7 Once finished wait a 10 seconds and then power cycle the EC500 (IMPORTANT NOTE: POWER CYCLING THE EC500 TOO FAST CAN CORRUPT IT REQUIRING FIRMWARE REFLASH, ALWAYS WAIT A FEW SECONDS BEFORE POWER DOWN AND POWER UP)

#8# INSTALL ETH COMPONENT
#8 Find downloaded components folder containing  remora-eth.3.0.c and open a console here - run

sudo halcompile --install remora-eth-3.0.c

#9# Flashing a CONFIG to the EC500 ##
#9 Plug in ethernet from PC to EC500 - Set IP address manually with range of (10.10.10.*) DO NOT USE (10.10.10.10)
#9 With EC500 powered, navigate to your folder containing your CONFIG (usually your machine config folder) and open console in the location - run

ping 10.10.10.10

#9  You should be seeing results from the EC500
#9 Ctrl-C to close ping - run

python3 upload_config.py config.txt

## FINISHED ##

## If you run your HAL and have an error - Check your .hal is loading the eth3.0 component and not the remora.nv component##