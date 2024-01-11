
--- 

## BOARD STARTUP PROCEDURE

-   Make sure the SD card is inserted.
-   Connect the board to the power supply.
-   Reset the board (using the reset button on the ST board. Later firmware versions will allow this procedure to be performed from the client software).

## CLIENT

Launch the execution of the software (python3) **cli_communication_board.py**.

###### Scanning and searching for ST board
The first time the software is run, it performs a Bluetooth scan of possible available nodes. After scanning is complete, the software presents a list of pre-configured ST boards with IT-I firmware. The user can select the ST board to connect to.

![[Pasted image 20230217164408.png]] 
On subsequent executions, the client software is automatically set, without scanning to find the ST board. If the user wants to connect to a different ST board, they can use the **_scan_** command, which repeats the scanning procedure and allows the choice of a different ST board.

![[Pasted image 20230217161647.png]]

###### Data Acquisition
The user can start a data acquisition session with the **_start_** command. Data acquisition can occur in 2 different modes:

1)  Transmission of data to the client software via Bluetooth Low Energy. In this mode data from only one sensor can be transmitted at a time. The command to be used is: 
                         **_start ble** \[sensor id] \[sensor stream decimation rate]_ 
    The above command creates a folder named with the date and start time of the acquisition, within which the file related to the selected sensor is inserted. ![[Pasted image 20230220110159.png]]
2) In this mode, it is possible to transmit sensor data to the software client. One to five sensors can be selected simultaneously using the following command:
                          **_start usb** \[ \[sensor id], sensor id], …. sensor id]] \[sensor stream decimation rate]\[(optional) data acquisition duration]_
    If data from multiple sensors need to be acquired, the indices must be separated by commas. The data is saved in binary files in the "acquisition" folder. The acquisition duration in seconds can also be included as a third parameter. In this case, the **_stop_** command should not be used to end the acquisition.
3)  Writing data to the SD card. In this case, data from all sensors can be acquired simultaneously. The command to be used is: 
                          **_start sd** \[ \[sensor id], sensor id], …. sensor id]] \[sensor stream decimation rates]_ 
    The above command creates a folder named _TPSH__\[acquisition number]_ in which the files related to the selected sensors and a JSON file containing the configuration of the sensors used for the acquisition are inserted. ![[Pasted image 20230220110312.png]]
    
sensor_id_ is the numerical index that identifies a sensor. These indices are the same as those used in the JSON format configuration file, which can be used to set parameters for each individual sensor. The index is different from the identifying code of the sensor since the latter is the alphanumeric name assigned to the sensor by the manufacturer. In both acquisitions, the files are named with the identifying code of the sensor. Each sensor is associated with a numerical index: 
			0 = IIS3DWB 
			2 = IIS2DH 
			4 = IMP34DT05 
			5 = ISM330DHCX 
			6 = LPS22HH 
			7 = IMP23ABSU

###### Data Acquisition Interruption
The user can end the data acquisition procedure (both BLE and on the SD card) using the **_stop_** command.

###### Downloading files saved on the SD card
It is possible to download files saved on the SD card (e.g., binary files containing data detected by a single sensor). The user can access the list of files present on the SD card using the **_download_** command, with which the client software displays the list of file names present on the board. The user can then select the file they want to download from the board to the PC using the arrow key.

###### Sensor Parameter Configuration
The board configuration is saved in a JSON file, which can be uploaded using the **_upload_file_** command. Only one configuration file is allowed on the board, however, the user can have different configuration files with different names, and the software will change the names accordingly during the upload. The user can download the board's sensor configuration at any time using the **_download_** command. Please note, however, that the file name is a standard name that is different from the name of the previously downloaded file.

###### Session termination
To terminate the client software session, use the **_close_** command.

###### Firmware update
Firmware update Updating the firmware of the board can be done in 2 ways:

1.  Using STM32CubeIDE

-   Connect the board and ST-Link to the PC. ![[Pasted image 20230329170238.png]]
-   Open the "File" dropdown menu. ![[Pasted image 20230329170846.png]]
-   Select "Open Projects from File Systems".
-   Choose the folder with the project. ![[Pasted image 20230329171005.png]]
-   Select "Finish". ![[Pasted image 20230329171342.png]]
-   Press the "Run" button after selecting the project.

2.  Using STM32CubeProgrammer

-   Connect the board and ST-Link to the PC.
![[Pasted image 20230329173516.png]]
-   Select the "ST-LINK" connection option. ![[Pasted image 20230329173304.png]] ![[Pasted image 20230329173443.png]]
-   Press the "Reset" button on the board and simultaneously press the "Connect" button in the STM32CubeProgrammer interface. ![[Pasted image 20230329173616.png]]
-   (First interface) Select the "Open file" panel and choose the binary firmware file that you want to load onto the board. ![[Pasted image 20230329173721.png]]
-   Press "Download".
-   Press "Disconnect".


###### Documentation for interpreting data transmitted by the board
The data obtained from the sensors (whether they are transferred via BLE or usb from the board to the client software, or saved locally on the board's sd) are presented in the _.dat_ file format.
In order to extract data correctly it is necessary to know the general structure of the data used in _.dat_ files, which is the following:

![[Pasted image 20230420093234.png]]
Note that this structure has been adopted for the data of all active sensors on the board.

Below are the diagrams relating to the structure used for data of all sensors active on the board; the value ranges of the raw data and any operations to be performed on them are also indicated. The parameters required for the raw data conversion are defined in the _DeviceConfig.json_ configuration file.

**Sensor 0 - IIS3DWB** and **Sensor 2 - IIS2DH**
![[Pasted image 20230420095837.png]]
Every single acceleration raw value (e.g. _acc_0_), since it is a 16-bit value, is in the range \[-32768 : 32767]. This datum must be converted into a value belonging to the range \[-FS : FS]; the FS parameter is defined in the DeviceConfig.json configuration file.

To obtain the acceleration value from the raw data (_value_) the following procedure can be used.

	leftSpan = 32767 - (-32768)
	rightSpan = FS - (-FS)

Data normalization with value in range \[-32768 : 32767]

	valueScaled = (value - (-32768)) / (leftSpan)

Conversion of the data with value in range \[0 :1] into a value belonging to the range \[-FS : FS] .

	acceleration value = -FS + (valueScaled * rightSpan)

**Sensor 4 - IMP34DT05**
![[Pasted image 20230420145211.png]]

**Sensor 5 - ISM330DHC**
![[Pasted image 20230420145310.png]]
Also in this case the conversion of the values ​​from the range \[-32768 : 32767] to the range \[-FS : FS] must be performed.

**Sensor 6 - LPS22HH**
![[Pasted image 20230420145550.png]]
The values ​​of the raw pressure data must be divided by 4096.
Raw temperature data values ​​must be divided by 100.

**Sensor 7 - IMP23ABSU**
![[Pasted image 20230420150527.png]]

