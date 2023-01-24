# Fast Robots @ Cornell, Spring 2023

[Return to main page](index.md)

# Lab 2: Bluetooth

## Objective
The purpose of this lab is to establish communication between your computer and the Artemis board through the bluetooth stack. 
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
