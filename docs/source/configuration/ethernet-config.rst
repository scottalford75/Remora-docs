Ethernet Config
========

Remora Ethernet Versions load their configuration file ,differently than Remora-SPI, via Trivial File Transfer Protocol or TFTP. This is done over Ethernet and written to the Flash memory of the Remora boards MCU. There is no SD card required. The configuration is loaded via a command line interface named "upload_config.py"

Loading the Configuration
-------------------------------------------------

1. Install TFTPy. This is a required package for the upload_config.py. In terminal install TFTPy with pip, and the following command:

.. code-block::


	 pip3 install tftpy


2. Your Remora config should be in this folder as well as the "upload_config.py" script. If they are not there you can find the script and sample configurations on the Remora github.  Your the config will be similar to Remora-SPI config. If you are unsure about what the config is or how to edit your config, refer to the section "Understanding the Remora Config file"

3. Open your linuxcnc config folder in terminal. The command below is an example of the path to your config. 

.. code-block::

	
	cd /home/pi/linuxcnc/configs/ your-remora-ethernet-config

4. After you have placed your config.txt and upload_config.py script, you can load your config to your remora board. Plug in your Remora-Ethernet board into your host computer and power it on. 

5. To load the config, enter the following in  terminal, in your config folder:

.. code-block::

	
	python3 upload_config.py your_config.txt

Replace "your_config.txt" with the name of your config file.

6. On successful load of your config, the terminal should respond with something similar to

.. code-block::

	
	Valid JSON config file, uploading to NVEM board
	Config file length (words) = 363
	Config file length (bytes) = 1449
	Remainder = 1
	Padding added =  [0, 0, 0]
	Config file length with padding (bytes) = 1452
	CRC-32 = 0x4c3f0233
