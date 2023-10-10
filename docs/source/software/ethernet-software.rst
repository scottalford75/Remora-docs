Ethernet Setup
========

Configure Ethernet Port
------------

Remora Ethernet firmware has a fixed IP adderess of 10.10.10.10, and it is looking for the host IP at address 10.10.10.11, so it is required that you configure your host computer with a static IP address. 

The following example applies to the Raspberry Pi, but the process will be similar for other Debian-like OS 

To set the stapic IP on the Raspberry Pi :

1. Open DHCP configuration in terminal

.. code-block::

	sudo nano /etc/dhcpcd.conf

2.  Navigate the file to the section " Example static IP configuration " , uncomment this section (by removing the #), modify it as follows, then save and exit nano (ctrl X, then Y then enter)

.. code-block::

	interface eth0
	static ip_address=10.10.10.11/24


3. Reboot your Raspberry Pi


4. After Reboot, verify your IP address was changed by typing the following into a terminal, it should return back "10.10.10.11" 

.. code-block::

	hostname -I





