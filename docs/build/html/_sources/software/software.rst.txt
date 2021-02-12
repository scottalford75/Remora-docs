Software
========

In this section of the Remora documentation, the installation, setup and configuration of LinuxCNC and the Remora component are covered.


LinuxCNC basics
---------------

LinuxCNC is well documented, but there are some key concepts of LinuxCNC that are important to know so that we can understand why the Remora component was developed and how it off loads tasks that a traditional parallel port stepper based LinuxCNC setup would do.


LinuxCNC task threads
~~~~~~~~~~~~~~~~~~~~~

In LinuxCNC, a thread is a list of functions that runs at specific intervals as part of a realtime task. When a thread is first created, it has a specific time interval (period), but no functions. Functions are added to the thread, and these will be executed in order every time the thread runs. In a traditional stepper configuration, two threads a created:

* Base Thread - a short period (high frequency) thread that is used for software step generation.
* Servo Thread - a longer period (lower frequency) thread tha is used for calculating motor positions, checking IO and logic functions

The Remora component allows the Base Thread tasks to be off loaded to the controller board


Hardware Abstraction Layer (HAL)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




LinuxCNC installation
---------------------




Remora installation
~~~~~~~~~~~~~~~~~~~



LinuxCNC configuration
----------------------




E-Stop Loop
~~~~~~~~~~~

To alert and stop LinuxCNC if SPI communication with the controller board cannot be estabilished, or if it is lost during operation, the Remora component is included as part of the LinuxCNC E-Stop loop. The HAL configuration is shown below.

.. code-block::

    # estop loopback, SPI comms enable and feedback

    net user-enable-out     <= iocontrol.0.user-enable-out      => remora.SPI-enable
    net user-request-enable <= iocontrol.0.user-request-enable  => remora.SPI-reset
    net remora-status       <= remora.SPI-status                => iocontrol.0.emc-enable-in


Diagramatically this is shown in the following figure.

.. image:: images/hal-estop.png


.. note::

    TODO - include the ability to have a physical E-Stop wired to the controller board. 
	* E-Stop to be a normally closed switch
	* Json config to turn feature on
	* new module needed to monitor input pin and halt SPI on E-Stop activation (pin going low)