Mks-Sbase
=========

The MKS-SBASE has a level shifter in the SPI circuit and therefore the EXP1 connector cannot be used. However adding an onboard jumper allows the J7 header to be used instead. Solder a jumper between pin 4 on U11 to pin 2 on the J7 header.

Wiring
------

Wiring requires the following components:

* 100mm Female-Female Dupont ribbon jumper
* 6 way (1x6) Dupont connector
* 8 way (2x4) Dupont connector
* 1 way Dupont connector

.. image:: ../_static/MksSbase-wiring-diag.png
    :align: center

.. image:: ../_static/MksSbase-wiring-diag2.png
    :align: center

Images
------

.. image:: ../_static/MksSbase.jpg

.. image:: ../_static/MksSbase-jumper-top.jpg

.. image:: ../_static/MksSbase-jumper-bottom.jpg

.. figure:: ../_static/MksSbase-wiring.jpg
    
	Mks-Sbase installed with Raspberry Pi. Note optional serial connection for debugging.


