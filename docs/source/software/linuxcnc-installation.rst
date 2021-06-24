LinuxCNC installation
=====================

Installing on Raspberry Pi OS:

1. Use Raspberry Pi Imager to install Raspberry Pi OS

Download the Raspberry Pi Imager from https://www.raspberrypi.org/software/ and flash Raspberry Pi OS 32bit version to your SD card.

2. Headless configuration

The following instructions allow the setup and configuration of the Raspberry Pi without needing an additional monitor, mouse and keyboard. If you do normal setup, jump straight to 3. LinuxCNC installation.

a) Add a wpa_supplicant.conf file into the /boot directory with your wifi settings enclosed in "".

.. code-block::

	ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
	update_config=1
	country=<Insert 2 letter ISO 3166-1 country code here>
	
	network={
	ssid="<Name of your wireless LAN>"
	psk="<Password for your wireless LAN>"
	key_mgmt=WPA-PSK
	}

b) Add an empty SSH file into the /boot directory to enable SSH
c) Insert the SD card and power up the RPi
d) Install PuTTY
e) Open a PuTTY session and use the default Host Name "raspberrypi" to connect to the RPi.
   or
   Open a command prompt and ping the RPi using the following command to find the Pi's IP address. This will ping 4 times.

.. code-block::

    ping raspberrypi -4
	
The ping will return the Raspberry Pi's IP address. This can then be used in PuTTY to make the SSH connection to the Pi.

Log into the RPi with username "pi" and the default password "raspberry".

f) Update the Raspberry Pi operating system

.. code-block::

    sudo apt-get update

g) To enable VNC, open the raspberry pi config editor and enable VNC.

.. code-block::

    sudo raspi-config
	
h) Change the Raspberry Pi's hostname to be unique on the network. In raspi-config
	i) Select System Options
	ii) Select Hostname and hit enter of select Ok
	iii) Enter a new hostname and hit enter
	
i) Fix the 'Cannot Currently Show the Desktop' Error. As we are connecting headless there is no display resolution set. We will add the following to the config.txt file located in the /boot directory on the SD card.

.. code-block::

	# uncomment to force a specific HDMI mode (this will force VGA)
	# 39	1360x768	60Hz	16:9
	hdmi_group=2
	hdmi_mode=39
	

3. Add the LinuxCNC Archive Signing Key to the apt keyring

.. code-block::

    sudo apt-key adv --keyserver hkp://keys.gnupg.net --recv-key 3cb9fd148f374fef

4. Add the apt repository

.. code-block::

    echo deb http://linuxcnc.org/ buster base 2.8-rtpreempt | sudo tee -a /etc/apt/sources.list.d/linuxcnc.list
	
5. Update the package list from linuxcnc.org

.. code-block::

    sudo apt-get update
	
7. Apply the work around for realtime kernel issue with memory greater than 3GB. Add the following line to /boot/config.txt

.. code-block::

    # limit total memory
    total_mem=3072

8. Install the realtime kernel

.. code-block::

    sudo apt-get install linux-image-4.19.71-rt24-v7l+
	
9. Install LinuxCNC development brach. Dev is needed to make halcompile available for the installation of components not included in the LinuxCNC distribution, ie Remora.

.. code-block::

    sudo apt-get install linuxcnc-uspace-dev
	
.. note::

    It is also possible to monitor the controller board via the optional serial interface. To enable this on the Raspberry Pi add the following to /boot/config.txt  
	
    enable_uart=1
	dtoverlay=pi3-miniuart-bt
	
    Install a serial terminal application such as CuteCom or MiniCom.
	
    port = /dev/ttyAMA0
    baud rate = 115200

    Wiring is between RPi pins 8 (TXD) and 10 (RXD) to an available uart pins on the controller board.