NVMPG HAL Component
=======================

The NVMPG LinuxCNC HAL component is used with the Remora Ethernet HAL component in conjunction with the NVEM CNC pendant Remora firmware. This component is used to interface with the NVMPG, by sending position data to the pendant, and commands back to LinuxCNC.

Sample Configuration Files
---------------------------
Sample LinuxCNC configuration files can be found in the remora-nv-mpg.hal , in your Remora Ethernet config file, where supported. 




Understanding the LinuxCNC configuration 
----------------------------------------

The following sections give some further details regarding the purpose and function of the different portions of the configuration files.


Loading realtime components
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Along with the standard realtime components the NVMPG needs to be loaded as well as a LinuxCNC encoder module

| Note: The NVMPG configuration utilzies the LinuxCNC encoder module for the MPG. This is NOT the same as Remora PRUencoder. 


.. code-block::


	loadrt nvmpg
	loadrt encoder num_chan=1

Parameters
~~~~~~~~~~~~~~~~~~~~~~
	
| The NVMPG has some parameters that need configuration. The update-freq  parameter is used to set the how often the positions are updated,
| The mpg-x1-inc is used to set the jogging incraments on the pendant. 


.. code-block::
	
	# NVMPG setup

	setp nvmpg.update-freq 4
	setp nvmpg.mpg-x1-inc 0.001
	


comms-status
~~~~~~~~~~~~~~~~~~~~~~
	
| Connecting the Remora status to the NVMPG status


.. code-block::

	
		net remora-status 	=> nvmpg.comms-status 

	

AXIS-pos
~~~~~~~~~~~

| Hal pin used to connect the Axis position, to the NVMPG so it can display current position

.. code-block::

	net mpg-xpos		halui.axis.x.pos-relative 	=> nvmpg.x-pos
	net mpg-ypos		halui.axis.y.pos-relative 	=> nvmpg.y-pos
	net mpg-zpos		halui.axis.z.pos-relative 	=> nvmpg.z-pos


AXIS-select
~~~~~~~~~~~

| Pin connected to the Axis Jog select on the pendant
| Used for connecting to LinuxCNC to indicate the axis to jog. 


.. code-block::

		# The Axis select inputs
	net mpg-x axis.x.jog-enable <= nvmpg.x-select
	net mpg-y axis.y.jog-enable <= nvmpg.y-select
	net mpg-z axis.z.jog-enable <= nvmpg.z-select


position
~~~~~~~~~~~~~~~~~~~~~~
	
| Position Outout from PRUencoder
| For Spindle, this is conneted to spindle.n.revs


.. code-block::

	
	net spindle-position encoderS0.position => spindle.0.revs
	

	
velocity
~~~~~~~~~~~~~~~~~~~~~~
	
| Encoder Velocity, output in RPS/velocity from PRUencoder


.. code-block::

	
	net spindle-velocity-raw <= encoderS0.velocity =>  spindle.0.speed-in





Adding functions to threads
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

    # add the remora and motion functions to threads

	addf nvmpg.update 				servo-thread
	addf encoder.update-counters	servo-thread
	addf encoder.capture-position	servo-thread
