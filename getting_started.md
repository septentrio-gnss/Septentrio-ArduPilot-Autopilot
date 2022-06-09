# Getting started with ArduPilot

## How to connect latest AsteRx OEM receivers with Pixhawk 4 using Robotics Interface board?

The steps below detail how to integrate a Pixhawk 4 with the latest generation AsteRx GNSS and INS receivers using a
Robotics Interface Board (RIB). In this example, the setup is done in a lab environment using an AsteRx-i3 D receiver
with RIB, without installing it on an actual UAS (Unmanned Aerial System). Please note that the procedure is same for
AsteRx-i3 S and AsteRx-m3 receivers (with RIB kits).

The following materials are required for the integration:
* Windows Laptop/PC
* AsteRx GNSS/INS OEM receiver
* RIB
* Pixhawk 4
* CBL_UAS_44 pin to Auto-pilot cable (Septentrio PN 215947)
* Latest Mission Planner version installed on any Laptop/PC
* Dual-frequency GNSS antenna(s)
* Adapters and cables to interface with MMCX connectors
* 2 * Micro-USB cable


The integration encompasses the following steps:

### Step 1:
Make sure the receiver is powered with at least 3.3V. You can use the micro USB connector or the open ended supply (
labeled "PWR & GND") on the 44 pin cable for this.

### Step 2:
Connect one or two GNSS antennas to the external antenna ports on the AsteRx-i3 D board as shown in Figure 1.

![](getting_started_assets/rtaImage.jpeg "External Antenna ports")


### Step 3:
Now connect the 44-pin cable to the AsteRx-i3 D board on RIB and connect the 6 pin JST GH connector to the UART & I2C B
port on the Pixhawk 4 as shown in Figure 2.

![](getting_started_assets/rtaImage%20(1).jpeg "AsteRx-i3 D board on RIB and Pixhawk 4 connected through Cable")

### Step 4:
On a Windows Laptop/PC, download and install Mission Planner from the Ardupilot Website. The drivers for the Pixhawk 4
will be installed, along with the Mission Planner software.

### Step 5: 

#### Download the firmware
From the [releases](https://github.com/septentrio-gnss/Septentrio-ArduPilot-Autopilot/releases) in this repository, download the assets corresponding to your setup (usually a .apj file). (To build the code for a specific target board, download the Source code and follow the [instructions](https://ardupilot.org/dev/docs/building-the-code.html) in the ArduPilot documentation).
#### Connect autopilot to computer
Once you have installed a ground station on your computer, connect the autopilot using the micro USB cable.

#### Select the COM port
If using Mission Planner as the GCS, select the COM port drop-down in the upper-right corner of the window near the Connect button. Select AUTO or the specific port for your board. Set the Baud rate to 115200 as shown. Do not hit Connect just yet.
![](getting_started_assets/rtaImage%20(2).jpeg "Selection of Pixhawk 4 port in the Mission Planner")

#### Install firmware
In Mission Planner’s SETUP | Install Firmware screen click "Load custom firmware" and select the .apj file.

Mission Planner will try to detect which board you are using. It may ask you to unplug the board, press OK, and plug it back in to detect the board type.

If all goes well, you will see a status appear on the bottom right including the words: “erase…”, “program…”, “verify..”, and “Upload Done”. The firmware has been successfully uploaded to the board.

It usually takes a few seconds for the bootloader to exit and enter the main code after programming or a power-up. Wait to press CONNECT until this occurs.


### Step 6:
In Mission Planner, select the port corresponding to the Pixhawk 4 as shown in Figure 3 and then click connect (please
note that port enumeration will be different for different connections). Make sure the baudrate matches the one of the
receiver which is 115200 baud per default.

![](getting_started_assets/rtaImage%20(2).jpeg "Selection of Pixhawk 4 port in the Mission Planner")


### Step 7:
After successfully connecting to the Pixhawk 4, select the "CONFIG" tab from the top and open the Full Parameter List (
highlighted on the left in figure 4). Now search for "GPS_TYPE2" and set its value to 10 to select SBF as incoming data
format, it will select the AsteRx-i3 D receiver as the second GPS as shown in Figure 4.

![](getting_started_assets/rtaImage%20(3).jpeg "Setting second GPS parameters for input data as SBF")

Also make sure that the "SERIAL4_BAUD" parameter is set to 115 and that "SERIAL4_PROTOCOL" is configured as value 5 for
GPS as shown in Figure 5.
![](getting_started_assets/rtaImage%20(4).jpeg "Setting second GPS parameters for the serial port")

Finally, you can also switch off the Ardupilot automatic configuration by setting "GPS_AUTO_CONFIG" to 0 as shown below.
This is necessary as the current auto config doesn't work for this set-up.

![](getting_started_assets/rtaImage%20(5).jpeg "Disabling automatic GPS configuration by Ardupilot")

After applying all the above settings click on "Write Params" (on the right hand side of the screen in above figures) to
save the settings to memory.

### Step 8:
Now, open the receiver's webUI (or another interface) and define an SBF data stream on COM2 with an output rate of 10Hz
for AsteRx-m3 whereas 2Hz for INS OEM receivers i.e. AsteRx-i3 D Pro(+) and AsteRx-i3 S Pro+. In the webUI, you can do
this via NMEA/SBF Out as shown in figure 7. The recommended SBF messages are "PVTGeodetic", "DOP", "VelCovGeodetic"
and "ReceiverStatus".
![](getting_started_assets/rtaImage%20(6).jpeg "Setting an Output SBF stream on COM2")

After this, you can save the current configuration in the non-volatile memory of the receiver to make sure that the
receiver does not lose its configuration after power cycling. It can be done by going Admin > Configurations and copying
the "current" config to "boot" as shown below.
![](getting_started_assets/rtaImage%20(7).jpeg "Saving current configuration in non-volatile memory of the receiver")


### Step 9:
Now the Pixhawk 4 should be able to receive SBF data through serial COM2 port of the receiver. On the Flight Data
screen, the GPS2 status should now be displayed, along with a location indicator on the map. In this case, the screen
reports 3D Fix for GPS2 to indicate a standalone solution as shown in Figure 9.
![](getting_started_assets/rtaImage%20(8).jpeg "GPS2 status displayed in Mission Planner")


To read more about injecting RTK corrections using Mission Planner, please go to this [article](https://customersupport.septentrio.com/s/article/How-to-inject-RTK-Corrections-into-an-AsteRx-UAS-receiver-via-Mission-Planner).