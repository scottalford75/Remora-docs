LinuxCNC configuration
======================

Loading realtime components
---------------------------


E-Stop Loop
-----------

To alert and stop LinuxCNC if SPI communication with the controller board cannot be estabilished, or if it is lost during operation, the Remora component is included as part of the LinuxCNC E-Stop loop. The HAL configuration is shown below.

.. code-block::

    # estop loopback, SPI comms enable and feedback

    net user-enable-out     <= iocontrol.0.user-enable-out      => remora.SPI-enable
    net user-request-enable <= iocontrol.0.user-request-enable  => remora.SPI-reset
    net remora-status       <= remora.SPI-status                => iocontrol.0.emc-enable-in


Diagramatically this is shown in the following figure.

.. image:: ../_static/hal-estop.png


.. note::

    TODO - include the ability to have a physical E-Stop wired to the controller board. 
	* E-Stop to be a normally closed switch
	* Json config to turn feature on
	* new module needed to monitor input pin and halt SPI on E-Stop activation (pin going low)