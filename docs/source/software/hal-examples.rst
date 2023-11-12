HAL Examples
=============

Stepper motor basic example
+++++++++++++++++++++++++++++++++++

.. code-block::

	# Joint 0 setup

	setp remora.joint.0.scale 		[JOINT_0]SCALE
	setp remora.joint.0.maxaccel 	[JOINT_0]STEPGEN_MAXACCEL


	net xpos-cmd 		<= joint.0.motor-pos-cmd 	=> remora.joint.0.pos-cmd  
	net j0pos-fb 		<= remora.joint.0.pos-fb 	=> joint.0.motor-pos-fb
	net j0enable 		<= joint.0.amp-enable-out 	=> remora.joint.0.enable


This is a simple hal configuration for basic position mode stepgen

Note: sections with labels such as "[JOINT_0]xxxx" means the value is found in the Linuxcnc ini file under [JOINT_0]

For further refinement of your stepper configuration, or if you are having issues with stepper position, it is recomended to add these sections to your Linuxcnc hal/ini file. These represent values for the internal stepgenerator and can be used to smooth out motion.

.. code-block::

	setp remora.joint.0.pgain [JOINT_0]P_GAIN
	setp remora.joint.0.ff1gain [JOINT_0]FF1_GAIN
	setp remora.joint.0.deadband [JOINT_0]DEADBAND

Stepper motor Closed Loop example
+++++++++++++++++++++++++++++++++++

.. code-block::

	# remora joint control to velocity mode
	loadrt remora_rpspi ctrl_type=v
	# load the PRU encoder module 
	loadrt PRUencoder names=encoderJ0
	# load pid controller for joint0
	loadrt pid names=j0pid

	# add PRUencoder and PID to funtions
	addf PRUencoder.capture-position servo-thread
	addf j0pid.do-pid-calcs servo-thread

	# Joint 0 setup

	setp remora.joint.0.scale 		[JOINT_0]SCALE
	setp remora.joint.0.maxaccel 	[JOINT_0]STEPGEN_MAXACCEL
	setp encoderJ0.position-scale	[JOINT_0_ENCODER]ENCODER_SCALE

	net j0enable 		<= joint.0.amp-enable-out 	=> remora.joint.0.enable
	net j0enable 									=> j0pid.enable
	net encoderJ0-count 							=> encoderJ0.raw_count
	net j0pos-fb 		<= encoderJ0.position 		=> j0pid.feedback
	net j0pos-fb 									=> joint.0.motor-pos-fb
	net j0pos-cmd 		<= joint.0.motor-pos-cmd 	=> j0pid.command
	net j0pid-output 	<= j0pid.output 			=> remora.joint.0.vel-cmd

	setp j0pid.Pgain 		[JOINT_0]P
	setp j0pid.Igain 		[JOINT_0]I
	setp j0pid.Dgain 		[JOINT_0]D
	setp j0pid.bias 		[JOINT_0]BIAS
	setp j0pid.FF0 			[JOINT_0]FF0
	setp j0pid.FF1 			[JOINT_0]FF1
	setp j0pid.FF2 			[JOINT_0]FF2
	setp j0pid.deadband 	[JOINT_0]DEADBAND

	# Remora Process Value (PV) feedbacks
	# link the encoder PV to the config.txt
	net encoderJ0-count <= remora.PV.0

Note: sections with labels such as "[JOINT_0]xxxx" means the value is found in the Linuxcnc ini file under [JOINT_0]

| The example above is for setting up a stepgen joint with velocity mode to run a closed loop stepper motor, please refer to the example configuration under linuxcnc/configs/remora/remora-closed-loop


PWM to 0-10v spindle control simple
+++++++++++++++++++++++++++++++++++

.. code-block::

	#spindle DAC 0-10 control
		loadrt scale count=1
		addf scale.0 servo-thread
		setp scale.0.gain 1 #this will make m3 s1000 give 100% output and m3 s100 10%
		net spindle-speed-scale spindle.0.speed-out => scale.0.in
		net spindle-speed-abs scale.0.out => abs.0.in
		net spindle-speed-DAC abs.0.out  => remora.SP.3
	
| In the above example we create a scale and give it a value of 1
| then we link the spindle speed to the scale input
| Then we must pass the scale value into a abs as the spindle value can go negative. linux cnc uses negative to define spindle direction and we need to avoid this as the abs will always return a positive number.
| Lastly we take the abs out and link it to the remora.sp.3
| remora needs a value between 0-100 for the pwm gen.



PWM to 0-10v spindle control with inverted lincurve compensation.
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code-block::

	#spindle DAC 0-10 control
		loadrt lincurve personality=9
		addf lincurve.0 servo-thread
		loadrt scale count=1
		addf scale.0 servo-thread
		setp scale.0.gain 1 #this will make m3 s1000 give 100% output and m3 s100 10%
		net spindle-speed-scale spindle.0.speed-out => scale.0.in
		net spindle-speed-abs scale.0.out => abs.0.in
		net spindle-speed-DAC abs.0.out  => lincurve.0.in
		#Lincurve compensation
		setp lincurve.0.x-val-00 10
		setp lincurve.0.y-val-00 100
		setp lincurve.0.x-val-01 100
		setp lincurve.0.y-val-01 98
		setp lincurve.0.x-val-02 200
		setp lincurve.0.y-val-02 90
		setp lincurve.0.x-val-03 300
		setp lincurve.0.y-val-03 81
		setp lincurve.0.x-val-04 400
		setp lincurve.0.y-val-04 69
		setp lincurve.0.x-val-05 500
		setp lincurve.0.y-val-05 59
		setp lincurve.0.x-val-06 600
		setp lincurve.0.y-val-06 48.6
		setp lincurve.0.x-val-07 700
		setp lincurve.0.y-val-07 39.6
		setp lincurve.0.x-val-08 800
		setp lincurve.0.y-val-08 29.9
		setp lincurve.0.x-val-08 900
		setp lincurve.0.y-val-08 20.8
		setp lincurve.0.x-val-08 1000
		setp lincurve.0.y-val-08 12.6
		net spindle-corrected lincurve.0.out => remora.SP.3
	
| This is almost the exact same as above but adds a lincurve component to fix for the non linear PWM to 0-10v control board selected, it also fixes a problem of the cnc break out board logic being reversed.
| Such that without the lincurve 0%pwm would give out 10V(max spindle speed) and 100% pwm would give out 0V
| We take the abs out and pass it into lincurve then the table in lincurve takes a value X and replaces it with value Y and scale any value between the points.
| in the above any value between 0-10 for spindle speed gives 100 as the output thus the logic is inverted
| in the above any value between 1000 or more for spindle speed gives 12.6 
| For more info about lincurve
| https://linuxcnc.org/docs/html/man/man9/lincurve.9.html
| 
| This was tuned via a scope watching the values and making the table such that the output would be roughly linear.
	
Spindle control and coolant signal outputs
++++++++++++++++++++++++++++++++++++++++++++

.. code-block::

	# outputs
		net coolant-flood <= iocontrol.0.coolant-flood
		net spindle-on => remora.output.00
		net spindle-ccw => remora.output.01
		net coolant-flood   => remora.output.02
		


QEI Encoder without index
++++++++++++++++++++++++++++

.. code-block::

	# Initialize the encoder (spindle)
	loadrt PRUencoder names=encoder.0
	addf PRUencoder.capture-position servo-thread
	setp encoder.0.position-scale 1200.000000 #6
	# connect the hal encoder to linuxcnc
	net encoder-count <= remora.PV.2 => encoder.0.raw_count
	
| This example we add the encoder module to the linux cnc servo thread
| Then define/set the encoder with its pulse per revolution, example: 300pulse per rev encoder x4 for being a quadrature encoder equals 1200.
| Then we link the "encoder-count" to the remora PV value and pass it all into encoder.0.raw_count (the PV value will be what ever you set in the config tool/file)

QEI Encoder with index
++++++++++++++++++++++++++++++

.. code-block::

	# Initialize the encoder (spindle)
	loadrt PRUencoder names=encoder.0
	addf PRUencoder.capture-position servo-thread
	setp encoder.0.position-scale 1200.000000 #6
	# connect the hal encoder to linuxcnc
	net encoder-count <= remora.PV.2 => encoder.0.raw_count
	net encoder-phaseZ <= remora.input.07 => encoder.0.phase-Z
	
| This example we add the encoder module to the linux cnc servo thread
| Then define/set the encoder with its pulse per revolution, example: 300pulse per rev encoder x4 for being a quadrature encoder equals 1200.
| Then we link the "encoder-count" to the remora PV value and pass it all into encoder.0.raw_count (the PV value will be what ever you set in the config tool/file)
| Finally we link encoder-phaseZ to the remora input that has the index pulse connected and pass it to encoder.0.phase-Z.


Endstops + home switches
+++++++++++++++++++++++++++++++

.. code-block::

	# end-stops
	net X-min 	remora.input.00 	=> joint.0.home-sw-in joint.0.neg-lim-sw-in
	net Y-min 	remora.input.02 	=> joint.1.home-sw-in joint.1.neg-lim-sw-in
	net Z-min 	remora.input.04 	=> joint.2.home-sw-in joint.2.neg-lim-sw-in

| In the above example we are sending the value of the input to both the home-sw-in and neg-lim-sw-in
| The advantage to this is we can save on pins and simplify the machine wiring
