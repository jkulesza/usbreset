# Linux USB Device Reset Application

This application can be used to perform resets on USB devices.  This README describes the steps to compile, run, and then automate execution of this application to reset a USB wireless adapter with a great degree of authority than is available using `ifdown wlan0`/`ifup wlan0`.

Credit to Alan Stern for developing this program, as retrieved from: http://marc.info/?l=linux-usb&m=121459435621262&w=2

The following sequence is adequate to execute the program:

1. Compile the program:

   `cc usbreset.c -o usbreset`

2. Set the permissions on the executable:

   `chmod +x usbreset`

3. Get the Bus and Device ID of the USB device you want to reset:

   ```
   lsusb
   ```
   which provides output:

   ```
   Bus 001 Device 007: ID 050d:945a Belkin Components F7D1101 v1 Basic Wireless Adapter [Realtek RTL8188SU]
   Bus 001 Device 003: ID 0403:6014 Future Technology Devices International, Ltd FT232H Single HS USB-UART/FIFO IC
   Bus 001 Device 002: ID 0bc2:5031 Seagate RSS LLC FreeAgent GoFlex USB 3.0
   Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
   Bus 002 Device 002: ID 046d:c06b Logitech, Inc. G700 Wireless Gaming Mouse
   Bus 002 Device 003: ID 413c:2106 Dell Computer Corp. Dell QuietKey Keyboard
   Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
   ```
   where the device of interest is on Bus 001 identified as Device 007 (our USB wireless adapter).


4. Execute the program with sudo privilege or as the root user, making necessary substitution for with the `Bus` and `Device` ids as found by running the lsusb command:

   ```sudo ./usbreset /dev/bus/usb/Bus/Device```

   or in our case

   ```sudo ./usbreset /dev/bus/usb/001/007```

   Now, we can automate execution with the following inline Perl command (assuming that `usbreset` is in the $PATH):

5. Execute or embed in a script:

    ```echo $(lsusb | grep "Wireless Adapter");  wifipath=$( lsusb | grep "Wireless Adapter" | perl -nE "/\D+(\d+)\D+(\d+).+/; print qq(\$1/\$2)")  sudo usbreset /dev/bus/usb/$wifipath```

    which was inspired by the code provided by *knb* at http://askubuntu.com/questions/645/how-do-you-reset-a-usb-device-from-the-command-line
