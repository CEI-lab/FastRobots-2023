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
|   ├── ble_arduino.ino
|   ├── BLECStringCharacteristic.h
|   ├── EString.h
|   └── RobotCommand.h
└── ble_python
    ├── __init__.py
    ├── base_ble.py
    ├── ble.py
    ├── cmd_types.py
    ├── connections.yaml
    ├── demo.ipynb - main Jupyter notebook
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
  
## Instructions 

<img src="Figs/UnderConstruction.png" width="500">


1. Send an *ECHO* command with a string value from the computer to the Artemis board, and receive an augmented string on the computer.
    > For example, the computer sends the string value "HiHello" to the Artemis board using the ECHO command, and the computer receives the augmented string "Robot says -> HiHello :)" from a read GATT characteristic.
2. Send three floats to the Artemis board using the *SEND_THREE_FLOATS* command and extract the three float values in the Arduino sketch.
3. Setup a notification handler in Python to receive the float value (the **BLEFloatCharactersitic** in Arduino) from the Artemis board. In the callback function, store the float value into a (global) variable such that it is updated every time the characteristic value changes. This way we do not have to explicitly use the **receive_float()** function to get the float value.
4. In your report, briefly explain the difference between the two approaches:
   1. Receive a float value in Python using **receive_float()** on a characteristic that is defined as **BLEFloatCharactersitic** in the Arduino side
   2. Receive a float value in Python using **receive_string()** (and subsequently converting it to a float type in Python) on a characteristic that is defined as a **BLECStringCharactersitic** in the Arduino side

## Additional tasks for 5000-level students
  
Perform an analysis on the communication performance.
> TIP: Use the notification handler for this. If you find a better way, include your approach in the write-up.

1. **Effective Data Rate**: Send a message from the computer and receive a reply from the Artemis board. Note the respective times for each event, calculate the data rate and include at least one plot to support your write-up.
2. **Reliability**: What happens when you send data at a higher rate from the robot to the computer? Does the computer read all the data published (without missing anything) from the Artemis board? Include your answer in the write-up.

## Write-up
To demonstrate that you've successfully completed the lab, please upload a brief lab report (<600 words), with code snippets (not included in the word count), photos, and/or videos documenting that everything works and what you did to make it happen.


