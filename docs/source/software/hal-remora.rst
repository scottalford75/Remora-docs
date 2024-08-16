Remora HAL Component
=============

ctrl_type[JOINTS]
+++++++++++++++++++++++++++++++++++

.. code-block::

		loadrt remora-rpispi ctrl_type=p,v
	
| Option to configure the control type for individual joints 
| Options are "p" for Position type control and "v" for Velocity type control
| Default is p


chip_type
+++++++++++++++++++++++++++++++++++

.. code-block::

		loadrt remora-rpispi chip_type=STM
	
| Note: This option is made obsolete by seperating components based on chip type	
| Option to configure for the chip type used on your controller board. 
| Options are LPC and STM
| Default is LPC





SPI_clk_div
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code-block::

	loadrt remora-rpispi chip_type=STM SPI_clk_div=32 


	
| Option for setting the SPI clock divider
| Avaiable options are 
| 128	3.125MHz on RPI3
| 64 	6.250MHz on RPI3
| 32	12.5MHz on RPI3
| 16	 25MHz on RPI3
| Defaults are based on chip type. generally used for trouble shooting and fine tuning. 
	
PRU_base_freq
++++++++++++++++++++++++++++++++++++++++++++

.. code-block::

	
	loadrt remora-eth-3.0 PRU_base_freq=500000
	
| Option for setting the PRU frequency
| Default is 40000
| This will be based on your specific chip type and the frequency it runs
| This must match your MCU base thread frequency if you have changed it in your Remora config.txt file
		


*SPIenable;
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 

*SPIreset;
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 

*PRUreset;
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 

SPIresetOld;
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 

*SPIstatus;
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 

*stepperEnable[JOINTS];
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 



pos_mode[JOINTS];
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 

*pos_cmd[JOINTS];
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  pin: position command (position units)

*vel_cmd[JOINTS];
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| pin: velocity command (position units/sec)

*pos_fb[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  pin: position feedback (position units)

*count[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  pin: psition feedback (raw counts)

pos_scale[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| param: steps per position unit

freq[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  param: frequency command sent to PRU

*freq_cmd[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  pin: frequency command monitoring, available in LinuxCNC

maxvel[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  param: max velocity, (pos units/sec)

maxaccel[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	setp remora.joint.1.maxaccel 	[JOINT_1]STEPGEN_MAXACCEL


|  
|  param: max accel (pos units/sec^2)

*pgain[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	setp remora.joint.2.pgain [JOINT_0]P_GAIN


|    Remora internal stepgen Pgain value
| 

*ff1gain[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	setp remora.joint.0.ff1gain [JOINT_0]FF1_GAIN


|    Remora internal stepgen FF1gain value
| 

*deadband[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	setp remora.joint.1.deadband 0.01


|  Remora internal stepgen deadband value
| Defaults to 1 step

old_pos_cmd[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  previous position command (counts)

old_pos_cmd_raw[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
|  previous position command (counts)

old_scale[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| stored scale value

scale_recip[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| reciprocal value used for scaling

prev_cmd[JOINTS];
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| 

cmd_d[JOINTS]
+++++++++++++++++++++++++++++++

.. code-block::

	#example


|  
| command derivative

*setPoint[VARIABLES]
+++++++++++++++++++++++++++++++

.. code-block::

	remora.SP.4


|  Remora set point variables include PWM, and RC servo 
| 0..5

*processVariable[VARIABLES]
+++++++++++++++++++++++++++++++

.. code-block::

	remora.PV.1


|  Remora process variables include Encoder modules, thermister input, 
| 0..5

*outputs[DIGITAL_OUTPUTS]
+++++++++++++++++++++++++++++++

.. code-block::

	remora.output.05


|  Remora output pins 00...15
| 

*inputs[DIGITAL_INPUTS]
+++++++++++++++++++++++++++++++



.. code-block::

	remora.input.05


|  Remora input pins 00...15
| 


