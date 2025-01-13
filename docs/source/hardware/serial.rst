Remora Serial Communication
===========================

| All Remora boards have a serial UART output option available for config verifaction and debugging. When having an issue, it is useful to connect over UART to diagnoise a problem. Each board will have a differnt hardware configuration for connecting to it, refer to the hardware section for board specific connections. 

| On a successful boot, Remora firmware will output a number of things over UART :
    - Comms status
    - Config details
    - RemoraPRU Status

Connecting to board
--------------------

| To recieve information from the board, you need to install a Serial Communication Terminal, like cutecom or minicom. 
| To connect to the board, select your channel, on an RPi this might be dev/AMA0 , and set your baud rate to 11560.
| Reset your board when you are connected, and see what the output is from the board. Here is an example of a serial output for Remora

.. code-block::