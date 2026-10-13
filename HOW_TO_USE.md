# PC-FX_devcart

This is a design of a development cart for the PC-FX.

## How To Use:

This Cartridge is intended to be used in conjunction with the Python scripts in the
[src/PC/](src/PC) folder

## Installing the Software

1. First, make sure that you have Python installed on your machine.
2. Create a working folder to hold the scripts, and copy them from this repository's src/PC/ folder into that working folder.

## Initial Startup and Identification

Before using the Devcart in the PC-FX, you should do the following (while not plugged in to the PC-FX):

1. On the first connection, the computer will create a virtual COM port on the host computer, which will be the device name required for communication via the python scripts.
2. Find out the virtual COM port's name.
   - On Windows, use Device Manager, and look under "Ports (COM & LPT)", and make a list if there are already devices there. Then insert the devcart, and check for a device with a name starting with "COM".  In this picture, the device is "COM33": [!Device Manager](images/Windows_device_list.jpg)
   - On MacOS, open a terminal, and type 'ls -l /dev/tty.\*', then plug in the devcart and do it again. The new device is the devcart; in this paicture, it is "/dev/tty.usbmodem101":  [!MacOS](images/MacOS_devices.jpg)


## Using the Cart in the PC-FX

The FX-Devcart contains circuitry which communicated with the PC-FX, but it does not connect to the
PC-FX's power supply; it takes its power from the USB connection, supplied by your host computer.

**NOTE**: DO NOT PLUG the Devcart into your PC-FX while the power is on - either the PC-FX power, or the USB power.

1. While the PC-FX's power is off, plug the Devcart into the PC-FX *without the USB cable connected*.
2. Next, plug one end of a USB cable into the Devcart, and the other end into your host computer. If the Devcart is inserted into the PC-FX and not powered by the USB, it will not retain any storage, and will effectively be inert from the PC-FX's point of view.
3. Some data transfers are required to take place before powering on the PC-FX; see below.

### Transferring SAV data from Computer to PC-FX:

With the cartridge in the FX-BMP port and connected to the PC, you can move game save data
images to the external cartridge memory, and subsequently move game save files from there to the
console internal memory), using the file manager portion of the boot ROM.

It is important to remember that the devcart does not retain data when the power (from USB)
is removed, so don't try to use the dev cart for any permanent storage.

Running this command will transmit that data from the host PC. It is best to transfer the data
before the PC-FX is turned on.

```
Usage: python putbmp.py <input_file> [COM port]
```

Example (Windows):
```
python putbmp.py zeroigar.bmp COM33
```

Once the data is transferred, you can power up the PC-FX, and use the file manager to move individual
save files to/from the internal memory.


### Transferring SAV data from PC-FX to Computer:

With the cartridge in the FX-BMP port and connected to the PC, you can:
1. Turn on the PC-FX
2. Use the file manager portion of the boot ROM to format the external cartridge memory, and
3. Move game save files to it (from the console internal memory).

Running this command will ithen transmit that data to the host PC.

```
Usage: python getbmp.py <output_file> [COM port]
```
Example (Windows):
```
python getbmp.py zeroigar.bmp COM33
```

It is important to remember that the devcart does not retain data when the power (from USB)
is removed, so don't try to use the dev cart for any permanent storage.


### Booting a Homebrew program:

This will probably be the most-commonly-used program for PC-FX developers, for testing
their programs on real hardware by booting into them.

```
Usage: python boot.py <input_file> [COM port]
```

With the PC-FX off and the development cartridge inserted into the FX-BMP port, connect
a PC to the development cart and send a boot program.  Once sent, the Python script
will print:
```
You can boot the PC-FX now
```
...At which point, booting the PC-FX will start the program.

NOTE: Programs larger than 128KB can also be downloaded to the PC-FX; in those cases,
the initial boot program is a program which will continue to request blocks of data 
until program download is complete, and automatically invoke the downloaded program.

I have included a homebrew program at [src/homebrew/blox](src/homebrew/blox) for testing.

