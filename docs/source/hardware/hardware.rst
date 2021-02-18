Hardware
========


.. toctree::
   :maxdepth: 2
   :caption: Controller Boards
   
   Mks-Sbase
   SKRV13
   SKRV14
   Re-ARM

In general any controller board that is based on an LPC1768 or LPC1769 microcontroller can be used, provided that an SPI interface is available. "Smoothieware" compatible controller boards are common and include:

* Mks-Sbase V1.3 - Tested. Requires on board jumper to allow the J3 header connector to be used for SPI communication
* Bigtreetech SKR V1.3 - Tested
* Bigtreetech SKR V1.4 - Under test
* Re-ARM - Under test




Raspberry Pi
------------

As the hard realtime requirements are offloaded onto the controller board, Remora can run on RPi 3B, RPi 3B+ and RPi 4B.




Wiring
------

Wiring is quite simple, requiring the following components:

* 100mm Female-Female Dupont ribbon jumper
* 10 way (2x5) Dupont connector
* 8 way (2x4) Dupont connector