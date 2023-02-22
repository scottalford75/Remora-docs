Understanding the Remora Config file
====================================

The Remora config file has many options for the user customize for their specific use cases.
We will cover each module and how to configure it.



| There are 2 ways to currently customize a config file.
| The first is to open it in a text editor. Notepad++ preferred.
| The second is to use a tool
| https://github.com/aaronCNC/Remora-GUI-Config-Tool



Board
-----
This definition is a user note only and is not used by the firmware. Examples

.. code-block::

    "Board": "BIGTREETECH SKR v1.3 & v1.4"
	"Board": "MKS SBASE v1.3"

Pin
-----
Pin names and definations need to match your boards schematics and pin naming structure. The LPC176x based boards use pins with an n.nn naming scheme. STM32 based boards use a Px_n naming scheme.  Examples

.. code-block::

    "Board": "BIGTREETECH SKR v1.3 & v1.4"
   		"Pin":				"1.31"
	
    
    "Board": "SKR v2.0 & Octopus"
   		"Pin":				"PC_4"
	


MCP4451
-------
| If Board is set to "MKS SBASE v1.3"
| You will need the following for the digital pots to set the motor current.

.. code-block::

	{
	"Thread": "On load",
	"Type": "MCP4451",
	"Comment": "Digipot for joints/Axis 0 - 3",
		"I2C SDA pin": "0.0",
		"I2C SCL pin": "0.1",
		"I2C address": 0,
		"Max current": 2.0,
		"Factor": 113.33,
		"Current 0": 0.8,
		"Current 1": 0.8,
		"Current 2": 0.8,
		"Current 3": 0.8
	},
	{
	"Thread": "On load",
	"Type": "MCP4451",
		"Comment": "Digipot for joints/Axis 4 - 7",
		"I2C SDA pin": "0.0",
		"I2C SCL pin": "0.1",
		"I2C address": 2,
		"Max current": 2.0,
		"Factor": 113.33,
		"Current 0": 0.8,
		"Current 1": 0.8,
		"Current 2": 0.8,
		"Current 3": 0.8
	},

**Current 0-3** (0.0-2.0) The value enter here sets the current in amps. Note the comment on the above the first entry is for the first 4 joints/Axis (X,Y,Z,E0) and the second is for the last 4 joints/Axis (E1,E2,E3,E4)

TMC stepper
-----------
| This allows Remora to adjust the parameters of TMC based stepper drivers.

.. code-block::

	{
	"Thread": "On load",
	"Type": "TMC stepper",
		"Comment":			"X - Joint 0 TMC driver",
		"Driver": 			"2209",
		"RX pin": 			"1.10",
		"RSense":			0.11,
		"Current":			800,
		"Microsteps":		16,
		"Stealth chop":		"on",
	},
	
| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **Driver:**	(2208,2209) Sets the TMC driver chip type
| **RX pin:** Sets the RX pin for communication with the driver
| **RSense:** (sense resistor value in ohms) This should match the drivers sense resistor
| **Current:** (0-2500) This sets the driver current in milliamps 
| **Microsteps:** (1-256) Sets the micro steps for the driver
| **Stealth chop:** (on, off) Enabled or disables stealth chop for silent operation

Stepgen
-------
This module defines the axis and how it is connected to the main board and LinuxCNC.

.. code-block::

    {
	"Thread": "Base",
	"Type": "Stepgen",
		"Comment":			"x axis",
		"Joint Number":			0,
		"Step Pin":			"2.2",
		"Direction Pin":		"2.6",
		"Enable Pin":			"2.1"
	},

| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **Joint Number:** (0-8) This is where you link the join/axis to LinuxCNC. This number must match what is set in your hal file.
| **Step/Direction/Enable Pins:** These are user set pin to connect to your motor driver.


Digital Pin
-----------
This module can create an input or output. This is useful for things like home and limit switches or controlling relays and such.

.. code-block::

    {
	"Thread": "Servo",
	"Type": "Digital Pin",
		"Comment":			"spindle enable",
		"Pin":				"2.5",
		"Mode":				"Output",
		"Modifier":			"Pull None",
		"Invert":			"False",
		"Data Bit":			0
	},

| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **Pin:** What pin the output or input is connected to.
| **Mode:** (Output, Input) sets the digital pin mode
| **Modifier:** ("Pull None" "Pull Up" "Pull Down" "Open Drain") This sets the internal resistor for the connected pins
| **Invert:** (True, False) This inverts the state of the pin
| **Data Bit:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7 
| when "Mode:" is set as "Output" you can set this to any number 0-7 but do not use the same number twice. This give the user 8 total unique outputs.
| when "Mode:" is set as "Input" you can set this to any number 0-7 but do not use the same number twice. This give the user 8 total unique Inputs. (this is shared with encoders)
	
PWM
---
This module create a PWM output. this can be used to control lasers, fans, spindles ect.

.. code-block::

    {
	"Thread": "Servo",
	"Type": "PWM",
		"Comment":			"PWM0",
		"SP[i]":			0,
		"PWM Pin":			"1.24",
		"PWM Max":			256,
		"Hardware PWM":			"True",
		"Variable Freq":		"True",
		"Period SP[i]":		1,
		"Period us":			200
	},

| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **SP[i]:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7 only use each number once for a total of 8 (this is shared with RCServo)
| **PWM Pin:** What pin the PWM output is connected to. if Hardware PWM is set true only use the pins listed for your board in the hardware section
| **PWM Max:** (0-256) sets the max output for the PWM. This is useful for driving a 6V load with a 12V source just set it to 128 for the max output to be half.	
| **Hardware PWM:** (True, False) This enables hardware PWM, it will limit what pins you can use but in return will give better and more adjustable PWM signals.
| **Variable Freq:** (True, False) This enables variable PWM feq only if hardware PWM is set to True
| **Period SP[i]:** (1-20) This allows the user to change the length of the pulse only if hardware PWM is set to True
| **Period us:** (200-20000) This allows the user to set the freq timing only if hardware PWM is set to True. 20000=50Hz 

| Note: Hardware PWM pins vary between boards, refer to your boards hardware section for pin details. 
RCServo
-------

.. code-block::

    {
	"Thread": "Base",
	"Type": "RCServo",
		"Comment":			"servo",
		"Servo Pin":			"2.0",
		"SP[i]":			7
	},

| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **Servo Pin:** What pin the Servo output is connected to
| **SP[i]:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7 only use each number once for a total of 8 (this is shared with PWM)

QEI
---
| This is a pin dedicated hardware quadrature encoder module for high speed encoders useful for spindles or very high resolution encoders. 
| Note: QEI varies between boards, refer to your boards hardware section for pin details. 

.. code-block::

    {
	"Thread": "Servo",
	"Type": "QEI",
		"Comment":			"Spindle encoder",
		"Modifier":			"Pull Up",
		"PV[i]":			0,
		"Data Bit":			7,
		"Enable Index":			"True"
	},

| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **Modifier:** ("Pull None" "Pull Up" "Pull Down" "Open Drain") This sets the internal resistor for the connected pins
| **PV[i]:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7 only use each number once for a total of 8 (this is shared with Encoder and Temperature)
| **Data Bit:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7. 
| This is shared pool with digital pin input. and only is needed if "Enable Index" is set to "True"
| **Enable Index:** (True, False) This enables the index pulse on the encoder. if your encoder only has a and b set this to false

Encoder
-------
This is a software encoder module for low to mid speed encoders useful for axis and servo motors and has max input of 30KHz.

.. code-block::

    {
	"Thread": "Base",
	"Type": "Encoder",
		"Comment":			"X encoder",
		"ChA Pin":			"1.22",
		"ChB Pin":			"1.20",
		"Modifier":			"Pull Up",
		"PV[i]":			1,
		"Data Bit":			6,
		"Index Pin":			"1.18"
	},

| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **ChA,ChB:** What pin the encoder is connected to.
| **Index Pin:** What pin the index pulse connected to. If this is set to "" with no value index is disabled. 
| **Modifier:** ("Pull None" "Pull Up" "Pull Down" "Open Drain") This sets the internal resistor for the connected pins
| **PV[i]:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7 only use each number once for a total of 8 (this is shared with QEM and Temperature)
| **Data Bit:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7. This is shared pool with digital pin input. and only is needed if "Enable Index" is not set to ""


Temperature
-----------
This is a thermistor module for sensing temperatures. useful for 3d printers and CNC machine spindle max temp

.. code-block::

    {
	"Thread": "Servo",
	"Type": "Temperature",
		"Comment":			"temp0",
		"PV[i]":			"2",
		"Sensor":			"thermistor",
			"thermistor":
			{
				"Pin":		"0.23",
				"beta":		5,
				"r0":		10000,
				"t0":		200
			}
	},

| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **PV[i]:** (0-7) This is where you link the module to LinuxCNC and can be set to a number between 0-7 only use each number once for a total of 8 (this is shared with Encoder and QEM)
| **Sensor:** (thermistor) only option 
| **Pin:** What pin the thermistor is connected to.
| **beta, r0, t0:** These are the values of the thermistor.

Switch
---------
The switch can turn on and off a pin based on the value of a thermistor or other module with a PV[i]

.. code-block::

    {
	"Thread": "Servo",
	"Type": "Switch",
		"Comment":			"temp0 fan",
		"Pin":				"2.3",
		"Mode":				"On",
		"PV[i]":			2,
		"SP":				25
	},
	
| **Comment:** is just for the user to give a custom name to keep track of what it is set to and has no effect on the machine.
| **Pin:** What pin the switch is connected to.
| **Mode:** (On, Off) what action to take when SP is reached
| **PV[i]:** what module to watch. IE if this is for a cooling fan set this the same as the thermistor.
| **SP:** The set point value for when the switch should activate.

Blink
---------
This will turn a pin on and off useful for leds

.. code-block::

    {
	"Thread": "Servo",
	"Type": "Blink",
		"Pin":				"1.18",
		"Frequency":		2
	},
	
| **Pin:** What pin the blink is connected to.
| **Frequency:** (1-20000) sets the Frequency the pin will cycle from on to off.

Motor Power
---------
The Motor Power module can turn on a pin at startup with no user interaction. This is required for boards with motor power hardware that needs an io to be turned on. For boards like the SKRv2 this is required for the motors to recive power. Example

.. code-block::

   {
	"Thread": "On load",
	"Type": "Motor Power",
	"Comment": "Enable motor power SKR2",
	"Pin": "PC_13"
	}


Reset Pin
---------
The reset pin allows for a controller board reset initiated from LinuxCNC. The Pin can be freely assigned to any free pin on the controller board, but it is hard coded to pin 25 on the Raspberry Pi.

.. code-block::

    {
	"Thread": "Servo",
	"Type": "Reset Pin",
		"Comment":			"Reset pin",
		"Pin":				"1.31"
	}
