# Fast Robots @ Cornell, Spring 2023

[Return to main page](index.md)

# Lab 2: Bluetooth

## Objective
The purpose of this lab is to establish communication between your computer and the Artemis board through the Bluetooth stack. 
We will be using Python inside a Jupyter notebook on the computer end and the Arduino programming language on the Artemis side.
We will also establish a framework for sending data via Bluetooth that will be useful in future labs.

## Prelab
Please read up on this [fantastic summary](https://www.Arduino.cc/en/Reference/ArduinoBLE) of Bluetooth Low Energy (BLE).

### Computer Setup

#### Install Python
You will need to install Python 3 and pip. If you already have them installed, please make sure you have the latest releases (Python >= 3.9 and pip >= 21.0). 

<details>
  <summary><strong>How to check for versions?</strong></summary>
    In a Command Line Interface (CLI), run the below commands to check for Python and pip versions:
    <blockquote>
      <p>Some Windows users may need to use `python` instead of `python3`</p>
    </blockquote>
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>python3 --version<br>python3 -m pip --version</code></pre></div></div>
</details><br>

<div class="tab">
  <button class="tablinks 1 active" onclick="openTab(event, 'L1', '1')">Linux/FreeBSD</button>
  <button class="tablinks 1" onclick="openTab(event, 'W1', '1')">Windows</button>
  <button class="tablinks 1" onclick="openTab(event, 'M1', '1')">macOS</button>
</div>

<div id="L1" class="tabcontent 1" style="display: block">
  <p><strong>Minimum Requirements:</strong> Linux kernel 4.15+ and bluez 5.48+</p>
  <p>Simply use your package manager to install python3 and pip :).</p>
</div>

<div id="W1" class="tabcontent 1">
  <p><strong>Minimum Requirements:</strong> Windows 10</p>
  <ol>
    <li>Install/Upgrade <a href="https://www.python.org/downloads/">Python 3.10</a></li>
    <li>Install/Upgrade <a href="https://pip.pypa.io/en/stable/installation/">pip</a></li>
  </ol>
</div>

<div id="M1" class="tabcontent 1">
  <p><strong>Recommended OS: macOS 12.0 </strong></p>
  <blockquote>
      <p>It may work with (some) older versions. If you have issues with older versions, contact the teaching team. We will try out best to help you.</p>
  </blockquote>
  <ol>
    <li>Open a terminal and run the command: <code class="language-plaintext highlighter-rouge">python3</code>.</li>
    <li>This should bring up a xcode dialog box asking you to install the necessary software.</li>
  </ol>
</div>

[[Relevant xkcd]](https://xkcd.com/1654/)

#### Virtual Environment
A virtual environment is a Python environment such that the Python interpreter, libraries and scripts installed into it are isolated from those installed in other virtual environments, and (by default) any libraries installed in a “system” Python, i.e., one which is installed as part of your operating system.

##### Install venv
Run the following commands in a Command Line Interface (CLI):
1. Install venv
    ```python
    python3 -m pip install --user virtualenv
    ```
2. Navigate to your project directory (folder).
    > Place all your Python scripts and Jupyter notebooks within this directory.
3. Create a new virtual environment named "FastRobots_ble"
    ```python
    python3 -m venv FastRobots_ble
    ```
    > venv will create a virtual Python installation in the newly created **FastRobots_ble** directory.


##### Using a virtual environment
For any lab using Python scripts (or Jupyter notebooks), you will first need to activate your virtual environment.

1. Activate the virtual environment
    > This will only work from the project folder (the one containing the **FastRobots_ble** folder).

    <div class="tab">
    <button class="tablinks 2 active" onclick="openTab(event, 'L2', '2')">Linux/FreeBSD</button>
    <button class="tablinks 2" onclick="openTab(event, 'W2', '2')">Windows</button>
    <button class="tablinks 2" onclick="openTab(event, 'M2', '2')">macOS</button>
    </div>

    <div id="L2" class="tabcontent 2" style="display: block">
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>source FastRobots_ble/bin/activate</code></pre></div></div>
    </div>

    <div id="W2" class="tabcontent 2">
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>.\FastRobots_ble\Scripts\activate</code></pre></div></div>
    </div>

    <div id="M2" class="tabcontent 2">
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>source FastRobots_ble/bin/activate</code></pre></div></div>
    </div>

2. Your CLI prompt should now have the prefix **(FastRobots_ble)**.
3. To deactivate the virtual environment, run the following command:
    ```bash
    deactivate
    ```
    > Your CLI prompt should no longer display the prefix.

##### Install Python Packages
1. Activate your virtual environment:
    > Your CLI prompt should now have the prefix **(FastRobots_ble)**.
2. Install packages:
    ```bash
    pip install numpy pyyaml colorama nest_asyncio bleak jupyterlab
    ```
**NOTE**: In Windows, you may see an error while installing some of these packages. Follow the download link in the error message to download and install (the unnecessarily huge) C++ build tools.

### Lab 2 Codebase

1. Download and unzip the [codebase](https://cornell.box.com/s/aivj9ad3uv74lmpvxz8s64aamgz6azt1) into your project directory.
2. Copy the ```ble_python``` directory into your project directory.

```
ble_robot-1.1
├── ble_arduino
|   ├── ble_arduino.ino - Arduino code that will be compiled and run on the Artemis board
|   ├── BLECStringCharacteristic.h - class definition used to send and receive data
|   ├── EString.h - class definition for Extended String used to easily manipulate character arrays
|   └── RobotCommand.h - class definition used to extract values of different data types from the robot command string sent to the Artemis board
└── ble_python
    ├── __init__.py
    ├── base_ble.py
    ├── ble.py
    ├── cmd_types.py - definition of command type mappings to integers
    ├── connections.yaml - assigning UUIDs to different data to send/receive
    ├── demo.ipynb - main Jupyter Notebook to run commands
    ├── logs
    |   ├── __init__.py
    └── utils.py
```

### Start Jupyter Server
We will be using Jupyter notebooks to write Python code. Before you can open a Jupyter notebook, you need to start the Jupyter server. **If the Jupyter server is stopped, you will not be able to run/open/modify any Jupyter notebooks.**

1. In a CLI, navigate to your project directory and activate your virtual environment.
2. Start the Jupyter server
    > The Jupyter server can only access notebooks from the directory it was started in.

    ```bash
    jupyter lab
    ```
    > macOS users may have to use `Jupyter lab` (capital J).
3. A browser tab should open up with JupyterLab.
   > If no window opens up, click on the URL displayed in the CLI.

You will be writing most of your Python code using this browser window on Jupyter notebooks. For students proficient in Python, you may choose to write some of your modules in Python script files and import them into your Jupyter notebook.

If you are new to JupyterLab, please go through the [Introduction to JupyterLab](https://cei-lab.github.io/FastRobots-2023/tutorials/jupyter_notebooks.html) tutorial (available under the [Tutorials](https://cei-lab.github.io/FastRobots-2023/tutorials/) page).

### Read Through Codebase
Please read through the codebase (in particular, the ```demo.ipynb``` Jupyter notebook) to understand the different functions that you will be using in this lab.

### Artemis Board Setup

#### Install ArduinoBLE
Install **ArduinoBLE** from the library manager (Tools -> Manage Libraries...) in the Arduino IDE.

#### Burn Artemis Codebase
1. Load and burn the sketch **ble_arduino.ino** into the Artemis board from the directory **ble_arduino** in the codebase.
    > Make sure you change the serial baud rate to 115200 bps.
2. The Artemis board should now print its MAC address.

** **End of Prelab** **
<hr>

## Code Summary 

The Python and Artemis packages provide you with the base code necessary to establish a communication channel between your computer and the Artemis board through BLE. 

### Bluetooth Library Limitations

Though a characteristic value can be up to 512 bytes long (according to the Bluetooth Core Specification), the ArduinoBLE library limits the maximum size to 255 bytes. We are using a more conservative size limitation of 150 bytes. The provided Python codebase throws an error if you attempt to send data that is larger than the member variable ArtemisBLEController.max_write_length (150 bytes). On the Arduino side, the macro MAX_MSG_SIZE (defined in EString.h) is used to set the character array sizes used in various classes.

### ```ble_arduino``` – Processing Commands on the Artemis Board

1. **BLE UUIDS**
* These are the Universally Unique Identifiers. This helps differentiate the different kinds of data that you’d want to send between the Artemis and your computer. 
* In order to generate new UUIDs to use, run these lines in your Jupyter Notebook and copy them to the appropriate places.
 ```
   from uuid import uuid4
   uuid4()
 ```

2. **BLEService**
* Used to set advertised local name and service, add BLE characteristics, and add BLE service
* See setup in ```ble_arduino.ino``` to see how to use BLEService for our purposes, or [this page](https://docs.arduino.cc/retired/archived-libraries/CurieBLE#bleservice-class) for more examples
* If you add more UUIDs/characters (different kinds of data to transmit/receive), don’t forget to add the characteristic to the BLEService

3. **BLExCharacteristic**
* These are constructors for handling different types of data provided by ArduinoBLE. Please read [this page](https://docs.arduino.cc/retired/archived-libraries/CurieBLE#blecharacteristic-class) for more details on types, syntax, parameters and examples.
* These three types are what we recommend, and should be sufficient for this class (but feel free to use other types if you feel the need):
  * BLEFloatCharacteristic
  * BLEIntCharacteristic
  * BLECStringChracteristic – this is not provided by ArduinoBLE but defined in BLECStringCharacteristic.h
* For receiving data on the Artemis, only use BLECStringCharacteristic as it defines a writable string GATT characteristic for communicating robot commands
* Relevant functions:
  * Constructor – parameters for constructors of relevant characteristics are found at the top of ```ble_arduino.ino```,  or [here](https://docs.arduino.cc/retired/archived-libraries/CurieBLE#bleservice-class) for all the characteristics
  * For transmitting characteristics:
    * ```writeValue( value );``` – value to transmit to your computer; ensure that the data type matches the characteristic type you use
  * For receiving characteristics:
    * ```written();``` – returns 1 if value has been written to this UUID by another BLE device
    * ```value();``` and ```valueWritten();``` – returns a uint8 array containing the BLE characteristic value, and the length of the uint8 array; used in ```set_cmd_string()``` function in RobotCommand.h

4. **RobotCommand**
* This is a class defined in RobotCommand.h
* RobotCommand is used when handling a robot command that the Artemis receives, of the string format “\<cmd_type\>:\<value1\>\|\<value2\>\|\<value3\>\|...”
  * \<cmd_type\> is an integer
  * \<value\> can be a float (or double), integer, or string literal
* Used in the function handle_command() in ```ble_arduino.ino```.
* Relevant functions:
  * Constructor – instantiates object; takes a string (character array) to specify the delimiters used to tokenize the robot command string e.g. ```RobotCommand robot_cmd(":|");``` where “:\|” is the delimiter
  * ```set_cmd_string( value, valueWritten );``` – set the command string from the characteristic value received for the RobotCommand object instantiated
  * ```get_command_type( cmd_type );``` – returns 1 if successful; sets ```cmd_type``` as the integer value sent by the other device
  * ```get_next_value( val );``` – returns 1 if successful; extracts the next value from the command string and assigns it to ```val```; ensure that the type of val is what is expected (by ensuring you handle ```cmd_type``` properly in case statements in ```handle_command()```)

5. **EString (Enhanced String)**
* Used to easily manipulate character arrays, defined in EString.h
* The header provides getter and setter functions to convert between a character array and an EString object
* We recommend using this when transmitting strings from the Artemis to your computer
* See case PING in ```handle_command()``` in ```ble_arduino.ino``` for an example on how to use EString
* Relevant functions:
  * ```clear();``` – empties the contents of the character array
  * ```append();``` – append a float, double, int, character array, or string literal to the EString
  * ```c_str();``` – returns the character array

6. **enum CommandTypes**
* This enumerates the variables in CommandTypes e.g. “PING” has the value 0, “SEND_TWO_INTS” has the value 1
* This is used in the case statements in ```handle_command()```

7. **handle_commmand()**
* This is the function that handles commands received by your computer by using a switch statement to determine what action to take depending on the command received ([what is a switch statement?](https://www.geeksforgeeks.org/switch-statement-cc/))
  
## Instructions 

<img src="Figs/UnderConstruction.png" width="500">

In order to test your robot's sensors more effectively, it is
critical to have a working wireless debugging system. The
following tasks will ensure that you can receive timestamped
messages from the Artemis board.

1. Send an *ECHO* command with a string value from the computer to the Artemis board, and receive an augmented string on the computer.
    > For example, the computer sends the string value "HiHello" to the Artemis board using the ECHO command, and the computer receives the augmented string "Robot says -> HiHello :)" from a read GATT characteristic.
2. Add a command GET_TIME_MILLIS which makes the robot reply
write a string such as "T:123456" to the string characteristic.
3. Setup a notification handler in Python to receive the string value (the **BLEStringCharactersitic** in Arduino) from the Artemis board. In the callback function, extract the
time from the string.
4. Add a command GET_TEMP_5s which sends an array
of five timestamped internal die temperature readings using
a string array, taken once per second for five seconds.
For example, you could send temperatures in
pairs as "T:06050|C:28.545|T:07082|C:28.537" (and so on until)
five timestamped temeratures are sent. Remember the
characteristic size limit! Add processing code to your
Python notification handler.
5. Add a command GET_TEMP_5s_RAPID which sends an array of
five seconds worth of rapidly sampled temperature data.
You should try to send at least fifty temperature points.
Remember the characteristic size limit, and add processing
code to your Python notification handler.
6. Working with strings introduces significant latency, so
real-time communication is not practical. Your robot needs
to handle all real-time processing on-board, and communicate
results in chunks (hopefully while safely stopped).
The Artemis board has 384 kB of RAM. Approximately how much
data can you store to send without running out of memory?
Discuss the limitations in the form of "5 seconds of 16-bit
values taken at 150 Hz."

7. Optional: You can try sending plain byte arrays over a
suitably-typed Bluetooth characteristic. You could also
explore sending character arrays that represent bytes, such
as base64-encoded bytes.

<!--
6. In your report, briefly explain the difference between the two approaches:
   1. Receive a float value in Python using **receive_float()** on a characteristic that is defined as **BLEFloatCharactersitic** in the Arduino side
   2. Receive a float value in Python using **receive_string()** (and subsequently converting it to a float type in Python) on a characteristic that is defined as a **BLECStringCharactersitic** in the Arduino side
   -->

## Additional tasks for 5000-level students
  
Perform an analysis on the communication performance.
> TIP: Use the notification handler for this. If you find a better way, include your approach in the write-up.

1. **Effective Data Rate And Overhead**: Send a message from the computer and receive a reply from the Artemis board. Note the respective times for each event, calculate the data rate
for 5-byte replies and 120-byte replies. Do many short
packets introduce a lot of overhead? Do larger replies help 
to reduce overhead? You may also test additional reply sizes.
Please include at least one plot to support your write-up.

2. **Reliability**: What happens when you send data at a higher rate from the robot to the computer? Does the computer read all the data published (without missing anything) from the Artemis board? Include your answer in the write-up.

## Write-up
To demonstrate that you've successfully completed the lab, please upload a brief lab report (<600 words), with code snippets (not included in the word count), photos, and/or videos documenting that everything works and what you did to make it happen.


