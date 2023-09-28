Overview
========

Why Remora?

Remora was primarily developed to use LinuxCNC for 3D Printing, where customising or adding features to existing firmwares is not simple or straight forward. 

LinuxCNC, however, is highly configurable and customisable. It has a truely compliant G-code interpetor and one of the best open source trajectory planners available. Making it an ideal software for those wanting to develop custom machines, or CNC implementations.

In a 3D printer application a traditional desktop PC, or even a Mini-ITX based system is nearly as large as the printer itself. And this was in fact how the author first started to contol a Prusa i3 clone with LinuxCNC. Converting the original printer controller board to a Modus RTU with a custom firware for IO, thermistor inputs and heater control. A desktop PC with a parallel port, Break out Board (BoB) and external stepper drivers.

This setup worked well and was the printer used to print the prototype Hypercube Evolution parts. But the setup was bulky and space hungry. Not ideal for a desktop 3D printer setup.

Why not use the existing 3D printer controller board with LinuxCNC? It has all of the IO, thermistor inputs, mosfet outputs and onboard stepper drivers needed for a CNC machine. So how to harness these for LinuxCNC running on a small single board computer, the Raspberry Pi? The development or Remora began...

The development of Remora started back in 2017 using a Raspberry Pi 3 and Arduino Due with a 32 bit RADDS board. This was first shared in Scott 3D's (aka Scotta) Hypercube Evolution introduction |video|.

.. |video| raw:: html

   <a href="https://www.youtube.com/watch?v=1rctCsUGnX8&t=2m45s" target="_blank"> video.</a>


As more and more 32 bit controller boards entered the market, the limitations of the Arduino framework became apparent. Development was moved onto the LPC17xx using PlatformIO and the Mbed framework. Current development is now ongoing using Mbed Studio and baremetal Mbed OS6. The newest addation to the Remora family, comes from by way of Ethernet based controller boards. Intended for Mach3, these controller boards make the ideal cantidate for a LinuxCNC controller board. 

We also see a new player enter the market,  Expatria Technologies Inc. based out of British Columbia, Canada. With boards designed specifically for Remora based CNC machines, Expatria offeres an ideal solution for a Remora based CNC. In addition to Remora specific boards, Expatria also contributes to the codebase. 

The outcome is an accessible and inexpensive LinuxCNC controller that provides all of the necessary hardware interfacing for a CNC machine.

