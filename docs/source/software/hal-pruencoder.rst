PRUencoder HAL Component
=======================

The PRUencoder HAL component is used with the Remora HAL component in conjunction with the encoder firmware module. This is what converts the raw counts from the hardware MCU and turns them into something useful for motion control.

| Note : Remora PRUencoder is NOT the same as LinuxCNC encoder. It is important to understand the difference between them. 

Connecting the encoder
----------------------

The AB channel of the encoder is connected to the GPIO of the Remora hardware. The connections are done in the remora config.txt file. Refer to your boards hardware section and the configuration section for more information. 
The encoders are counted in the Remora firmware, and the raw count information is sent to LinuxCNC in the form of a float, remora.PV . The Z/Index signal is connected to a GPIO (refer to your hardware section) and configured in LinuxCNC as a remora input. 
The PRUencoder takes that information and turns it into hal pins used in for things like closed loop motion control and rigid tapping. 

Sample Configuration Files
---------------------------
Sample LinuxCNC configuration files can be found in the Remora/LinuxCNC/ConfigSamples/remora-closed-loop directory.
To copy all the samples into your LinuxCNC configuration for experimentation or customizing do the following in a terminal window:

.. code-block::

	cp -a ~/linuxcnc/Remora/LinuxCNC/ConfigSamples/* ~/linuxcnc/configs

When you next start LinuxCNC you will find these items under the "My Configurations" node of the LinuxCNC Configuration Selector window.


Understanding the LinuxCNC configuration 
----------------------------------------

The following sections give some further details regarding the purpose and function of the different portions of the configuration files.


Loading realtime components
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Along with the standard realtime components (kinematics and motion module) the Remora component needs to be loaded. This will expose the pins.


.. code-block::


	loadrt PRUencoder names=encoderS0

position_scale
~~~~~~~~~~~~~~~~~~~~~~
	
| Encoder Cycles Per Rev, total number of quadratic encoder cycles per revolution


.. code-block::
	
	
	# 4096 x 4 = 16384
	setp encoderS0.position-scale 16384 

| Alternately, you can convert your scale number to be an INI variable, and edit the number from within your INI file

.. code-block::
	
	
	# HAL file
	setp encoderS0.position-scale [ENCODER_S0]ENCODER_SCALE

	# INI file
	[ENCODER_S0]
	ENCODER_SCALE = 16384
	




phaseZ
~~~~~~~~~~~~~~~~~~~~~~
	
| Connecting the remora.input pin to the PRUencoder z phase


.. code-block::

	
	net encoder-phaseZ <= remora.input.15 => encoderS0.phase-Z

	

raw_count
~~~~~~~~~~~

| Connecting the raw PV count coming from the remoraPRU to the PRUencoder
.. code-block::

	net encoder-count <= remora.PV.5 => encoderS0.raw_count


index-enable
~~~~~~~~~~~

| Used for threading operations in LinuxCNC 
| Connects the PRUencoder pin to the LinuxCNC Spindle0 motion pin


.. code-block::

	net spindle-index-enable encoderS0.index-enable <=> spindle.0.index-enable


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

The servo thread is used to communicate with the controller board and perform motion calculations. Functions are added in the order of execution. 

.. code-block::

    # add the remora and motion functions to threads

	addf PRUencoder.capture-position servo-thread
