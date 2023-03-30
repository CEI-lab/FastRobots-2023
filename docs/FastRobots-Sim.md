# Fast Robots @Cornell, Spring 2023

[Return to main page](index.md)

# Simulation tool used for Labs 10-12

## Objective
Follow these directions to setup and use your simulation environment. You will learn how to control your virtual robot and use the live plotting tool.

## Simulation Environment
The simulation environment consists of three components:

### 1. Simulator
The simulator contains a wheeled robot equipped with laser range finder(s), similar to the physical robot and its ToF sensors. The simulated world is defined in a .yaml configuration file.

#### Odometry
*Odometry* is the use of data from onboard sensors to estimate change in position over time. The relative changes recorded by the sensors (typically IMU and/or wheel encoders) are integrated over time to get a pose estimate of the robot. It is used in robotics by mobile robots to estimate their position relative to a starting location. This method is sensitive to errors due to the integration of velocity measurements over time to give position estimates.

The virtual robot provides simulated IMU data that mimics a typical real robot and this data is integrated over time to give you the odometry pose estimate. (In your real robot, we know that accelerometer data is too noisy to estimate forward motion, but you can consider using the gyroscope to estimate turns, and doing forward motion open loop. More on this in future labs!)

#### Ground Truth
In robotics, *ground truth* is the most accurate measurement available. New methods, algorithms and sensors are often quantified by comparing them to a ground truth measurement. Ground truth can come either directly from a simulator, or from a much more accurate (and expensive) sensor.

For the virtual robot, ground truth is the exact position of your virtual robot within the simulator.

#### Virtual Robot
The virtual robot does has a (simulated) constant speed controller and odometry pose estimation built in. It essentially mimics an idealized version of the real robot.

### 2. Plotter
The 2d plotting tool is a lightweight process that allows for live asynchronous plotting of multiple scatter plots using Python. The Python API to plot points is described in the Jupyter notebook. It allows you to plot the odometry and ground truth poses. It also allows you to plot the map (as line segments) and robot belief in future labs. Play around with the various GUI buttons to familiarize yourself with the tool; you want to be more familiar with the tools before you get into the future labs. 

### 3. Controller
You will be programming the controller in Python to perform various functions on your virtual robot. We provide you with a Python API which, among other things, provides a minimal control interface for the robot in the simulator. It allows you to:
  - Get robot odometry pose
  - Get the sensor data
  - Move the robot


## Computer Setup
### WSL
WSL users should continue to use WSL (for Bluetooth reliability) and should follow the
Linux instructions when applicable. The provided WSL distribution comes with Python 3.9
and works without upgrading to a later Python version.

### Upgrade Python
This is a reminder to upgrade python (>= 3.9) and pip (>= 21.0).
> NOTE: You may notice different versions when using `python` and `python3` commands; use the one that has the latest version and use pip in that version of python. For example, if `python3` has the latest version of python on your computer, use pip as `python3 -m pip`. Make sure you use the right python command for all the steps detailed in this documentation.

<details>
  <summary><strong>How to check for versions?</strong></summary>
    In a Command Line Interface (CLI), run the below commands to check for Python and pip versions:
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>python --version<br>python -m pip --version</code></pre></div></div>
</details><br>

<ol>
  <li>Install/Upgrade <a href="https://www.python.org/downloads/">Python 3.9</a></li>
  <li>Install/Upgrade <a href="https://pip.pypa.io/en/stable/installation/#upgrading-pip">pip</a></li>
</ol>

#### Install Python Dependencies
If you just upgraded your Python version and used an older version in Lab2, then the virtual environment created in Lab 2 will no longer be compatible. Inside your project folder, delete the directory with the name of your virtual environment (**FastRobots_ble**). Verify it is actually deleted! Now, follow instructions in [Lab2](Lab2.md) to setup your new virtual environment and reinstall the packages from Lab 2.

### Install pip packages
1. Activate your virtual environment.
2. Install dependencies
  ```bash
  python -m pip install numpy pygame pyqt6 pyqtgraph pyyaml ipywidgets colorama
  ```
  > Replace `python` with `python3` if `python3` points to the latest version.

### Install Box2D package
#### Installing from a pip wheel
1. Activate your virtual environment.
2. Download the [pip wheel](https://github.com/CEI-lab/ECE4960-sim-release/releases/tag/v2_3_10) that matches your OS and Python version. For example, MacOS with
Python 3.9 would download the wheel with ```cp39-macosx```
in the filename. Remember that WSL uses Linux instructions.

3. Install <b>Box2D</b> from pip
  > Replace &lt;wheel file&gt; with the path to the
  wheel that you downloaded
  ```bash
  pip install <wheel file>
  ```
  Remember that WSL can access your Windows ```C:\``` drive
  as ```/mnt/c```.

4. If Box2D is installed successfully, start a python interpreter:
  ```bash
  python
  ```
  and then run the following code in the python interpreter:
  ```python
  import Box2D; print(Box2D.__version__)
  ```
5. If the above command displays "2.3.10", you are done! Skip the next section ("Installing from source").

#### Installing from source
Follow the instructions in this section IF AND ONLY IF **Box2D** was NOT successfully installed in the previous section.
<div class="tab">
    <button class="tablinks 2 active" onclick="openTab(event, 'L2', '2')">Linux/FreeBSD</button>
    <button class="tablinks 2" onclick="openTab(event, 'W2', '2')">Windows</button>
    <button class="tablinks 2" onclick="openTab(event, 'M2', '2')">macOS</button>
    </div>

  <div id="L2" class="tabcontent 2" style="display: block">
   1. Activate your virtual environment. <br>
   2. Download and install the latest version of <b>swig</b> with your package manager. For example, in a Debian distro:
   <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>sudo apt-get install build-essential python-dev swig python-pygame git</code></pre></div></div>
   3. Clone the git repository
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>git clone https://github.com/pybox2d/pybox2d</code></pre></div></div>
   4. Build and install the <b>pybox2d</b> library
  <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>python setup.py build
python setup.py install</code></pre></div></div>
  </div>

  <div id="W2" class="tabcontent 2">
   1. Start an elevated Powershell: Type <i>"Powershell"</i> in your start menu, rightclick on the <i>Windows PowerShell</i> icon, and click on <i>Run as Administrator</i>. <br>
   2. With PowerShell, you must ensure Get-ExecutionPolicy is not Restricted. <br>
   3. Set the execution policy: Run the following command,
  <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>Get-ExecutionPolicy</code></pre></div></div> 
  4. If the command returns <i>"Restricted"</i>, then run 
  <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>Set-ExecutionPolicy AllSigned</code></pre></div></div>
  5. Install <b>choco</b>: Run the following command
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))</code></pre></div></div>
  6. Close the powershell window. <br>
  7. Start a new elevated powershell window. <br>
  8. Install <b>swig</b>: Run the following command
   <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>choco install swig</code></pre></div></div>
  9. Install <b>git</b>: Run the following command
   <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>choco install git</code></pre></div></div>
  10. Close the powershell window. <br>
  11. Start a new elevated Powershell: Type <i>"Powershell"</i> in your start menu, rightclick on the <i>Windows PowerShell</i> icon, and click on <i>Run as Administrator</i>. <br>
  12. Activate your virtual environment. (If you have issues with execution priveleges in Powershell, check out this <a href="https://stackoverflow.com/questions/18713086/virtualenv-wont-activate-on-windows">solution</a>) <br>
  13. Clone the repo, and install the python package.
  <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>git clone https://github.com/pybox2d/pybox2d
cd pybox2d
python setup.py build
python setup.py install</code></pre></div></div>  
</div>

  <div id="M2" class="tabcontent 2">
    1. Install <b>brew</b>:
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"</code></pre></div></div>
    2. Install <b>swig</b>:
    <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>brew install swig</code></pre></div></div> 
    3. Activate your virtual environment. <br>
    4. Clone the repo, and install the python package.
  <div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>git clone https://github.com/pybox2d/pybox2d
cd pybox2d
python setup.py build
python setup.py install</code></pre></div></div>  
</div>

## Instructions
1. Download and extract the [simulation base code](https://github.com/CEI-lab/ECE4960-sim-release) into your project folder.
2. Download the notebook from [here](https://github.com/CEI-lab/ECE4960-lab10) and copy *lab10.ipynb* into the **notebooks** directory (inside the simulation base code directory).
3. Follow the instructions in the notebook to understand how to control your virtual robot and use the plotter.

## Tasks (We will do these as an in-class exercise)

1. Open Loop Control 
  - Make your robot follow a set of velocity commands to execute a "square" loop anywhere in the map.
  - Plot and analyze the ground truth and odometry of the robot.
  - What is the duration of a velocity command?
  - Does the robot always execute the exact same shape?
   
2. Closed Loop Control
    - Design a simple controller in your Jupyter notebook to perform a closed-loop obstacle avoidance.
    - By how much should the virtual robot turn when it is close to an obstacle?
    - At what linear speed should the virtual robot move to minimize/prevent collisions? Can you make it go faster?
    - How close can the virtual robot get to an obstacle without colliding?
    - Does your obstacle avoidance code always work? If not, what can you do to minimize crashes or (may be) prevent them completely?


