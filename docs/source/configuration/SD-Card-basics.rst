Setting up micro SD card for Remora
===================================

Setting up the micro SD card:
The sample config files are located: main/Firmware/ConfigSamples
The pre compiled firmware: main/Firmware/FirmwareBin

1. Insert Micro SD card into your PC
2. Format micro SD card to FAT
3. Copy the firmware.bin & config.txt file into the root of the SD card.
4. Eject micro SD card from your PC and insert into your mainboard.
5. Power on or if already power on hit the reset button on your main board and wait 10 seconds.

The firmware only needs to be loaded once, but the config.txt is stored on the SD card, and needs to remain in the controller board.  

To check if everything worked remove the micro sd card from the main board and insert it back into your pc. the firmware.bin file should be renamed firmware.cur if the system flashed correctly. Some boards may rename the loaded firmware to another name. For example, the bootloader for the Fysetc Spider will rename the firmware to old.bin . 

- Note: Some boards are picky about what SD card will work. This is more present in boards with SPI SD protocol. If you have followed all the steps correctly, but your firmware has not changed to firmware.cur, old.bin, etc, it might be necessary to try with another SD card. Some SDHC cards might also not be properly supported on some boards. 
