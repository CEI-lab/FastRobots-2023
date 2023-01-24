# Virtual Machine Setup

## Background
A virtual machine (VM) is an emulation of a computer system. It is a virtual environment that functions as a virtual computer system with its own CPU, memory, network interface, and storage, created on a physical hardware system.

For this course, you can use [VirtualBox](https://www.virtualbox.org), a virtualization software for running the virtual machine. It is a general-purpose full virtualizer and allows to run a virtual machine.

### Terminologies
- VM: Virtual Machine
- OS: Operating System
- HOST OS: The OS installed on your system
- Guest OS: The OS that runs in a VM. In our case, we installed the Ubuntu OS as the Guest OS
- FOSS: Full Option Science System
- VirtualBox Menu Bar: The menu bar in VirtualBox to configure the VM. [[Ref]](https://www.howtogeek.com/wp-content/uploads/2013/08/virtualbox-switch-to-seamless-mode.png)
- OVA: An OVA file is an Open Virtualization Appliance that contains a compressed, "installable" version of a virtual machine. When you open/import an OVA file it extracts the VM and imports it into whatever virtualization software you have installed on your computer. 

## Install VirtualBox
1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads). (~ 150 MB)
   > For MacOS users: If your installation fails, your OS might have blocked it. Check out this [guide](https://medium.com/@DMeechan/fixing-the-installation-failed-virtualbox-error-on-mac-high-sierra-7c421362b5b5) for a fix.

# Install the OS
1. Download the preconfigured Ubuntu VM from [here](https://cornell.box.com/s/wu61hcekvmdgb2lp9qe16fx1ddhgog9x).
   > The preconfigured Ubuntu 20.04 VM contains the necessary python packages installed. Since you are going to type all your code in an IDE or Jupyter lab, it doesn't really matter if you are unfamiliar with Linux.
   > md5sum: e2d488f1356492bf90bf555be2b60fa2
2. Click on *"File->Import Appliance"* in Virtual Box and select the downloaded .ova file.
3. In the next screen, select *"Generate new MAC address for all network adapters"* as the **MAC Address Policy**.
4. Adjust the CPU and RAM allocations. Change **CPU** to the maximum number of CPUs in your machine and **RAM** to something above 8192MB (8GB).
> If you are unsure of your system specs, set CPU=2 and RAM=4096 MB. You can change this easily in a more informative GUI later on (Refer [Tune your VirtualMachine](https://cei-lab.github.io/ECE4960-2022/tutorials/vm_setup.html#tune-your-virtualmachine)).
1. Click on Import.

> To install **Ubuntu 20.04 from scratch**, download the ISO from [here](https://ubuntu.com/download/desktop). You don't need a license for FOSS! :D

> You can install a Windows 10 VM, but you would have to setup everything from scratch. Obtain an educational license from Cornell OnThHub, download the installation media (.iso file), and install the OS in a virtual disk. Here is a [guide](https://www.groovypost.com/howto/windows-10-install-virtualbox/) that you can refer to setup a Windows 10 VM (some steps may vary).

## Start your Virtual Machine
Click on the virtual machine you just created in the left pane in VirtualBox and then click on *"Start"* in the right pane.

> [Here](https://help.ubuntu.com/lts/ubuntu-help/shell-introduction.html.en) is a visual walkthrough of Ubuntu 20.04.

### Troubleshooting
- If you see a black screen when using the preconfigured VM:
   1. Select the VM in the left pane in Virtualbox and click on Settings.
   2. In the settings window, select Display on the left pane.
   3. Change the *"Graphics controller"* to *"VboxVGA"*.
   4. Select OK
   5. Try starting your VM. 
   6. If it still fails, select a different option for the *"Graphics controller"* and retry.
- If you have issues starting up VirtualBox or the VM, you may need to enable Virtualization in your PC BIOS. [Here](https://bce.berkeley.edu/enabling-virtualization-in-your-pc-bios.html) is a good guide to help you with that.


## VirtualBox Guest Additions
This step installs device drivers and system applications that optimize the guest operating system for better performance and usability. It is **highly** recommended that you finish this step successfully.
1. Once the VM is running, from the VirtualBox menu bar select "Devices -> Insert Guest Additions CD Image". 
   > **NOTE:** *If you don't see the menu bar, press "Right Control/Command + Home" on your keyboard. Check out this [post](https://superuser.com/questions/1176587/i-hid-the-menu-bar-in-virtual-box-how-to-show-it-again) for reference.*
2. Click on *"Run"* in the dialog box in the VM. 
3. The guest additions install process is completed when the window displays "Guest Additions: "Press return to close the window...". Now, press the Return key.
4. Restart your VM. You can do this by clicking on the power button on the top right corner in the VM.


## Change VirtualBox display settings
1. Once rebooted, you may change the resolution of the virtual machine to match your computer screen accordingly.
2. You can also switch to full-screen mode by clicking on *"View-> Full-screen mode*" from the VirtualBox menu bar. You can experiment with the other two modes available in the View menu and figure out a workflow that works best for you.

> **NOTE:** *If you don't see the menu bar, press the Right Control/Command + Home button together on your keyboard. Check out this [post](https://superuser.com/questions/1176587/i-hid-the-menu-bar-in-virtual-box-how-to-show-it-again) for reference.*


## Setup a shared folder between the Guest OS and Host OS
You can set up a shared folder to share files/folders within it between your Host OS and VM.
1. From the menubar, click on *"Machine -> Settings"*. 
2. Select *"Shared Folders"* in the dialog box.
3. Select a *"Folder path"* in the host OS that you want to share with your VM. 
4. Select *"Auto Mount" and "Permanent"* and click on *"Ok"*.

You should now see a folder appear on the desktop. If you don't see this, open the file explorer and it should be listed on the left pane.

## Tune your VirtualMachine
The VM is allotted a portion of your system resources (CPU, RAM, GPU, etc). You can tune these parameters to improve the VM performance. The VM is already configured with the minimum recommended settings. This step is to ONLY increase the system resource allocations if needed.

1. Make sure your VM is shutdown. 
2. In VirtualBox, right click on your VM and click on Settings.
3. Click on *"System"* in the left pane.
4. In the *"Motherboard"* pane, increase the *"Base Memory"* slider to increase the allocated RAM.
5. In the *"Processor"* pane, increase the number of allocated processors and the CPU execution cap. You cannot allocate more processors than what your system has.
6. Select the *"Display"* option in the left pane.
7. In the *"Screen"* pane, increase the *"Video Memory"* and select *"Enable 3D Acceleration"*.
8. Make sure to press *"OK"* after making your changes.

## Saving the Machine State
Instead of shutting down your VM, you can "Save the Machine State". The next time you start your VM, it should resume from the previous state. 

## Working on Labs
> **NOTE:** The relevant python packages for BLE have been installed at the system level in the VM. You **do not need** to use virtual environments.

### Jupyter Lab
1. Open a terminal (4th icon from the top in the Ubuntu dock).
2. Navigate to the project directory (or the shared folder) containing the python project files.
   > You can open the shared directory, right-click in an empty area and select "Open in Terminal" to open a terminal in the current directory. This way you can run Jupyter Lab from this directory to access the shared files from your Host OS.
3. Run the following command in a terminal
      ```bash
      jupyter-lab
      ```

### BLE Setup
1. Plug in the external BLE USB adapter.
2. After starting the VM, click on *"Devices->USB"* from the VirtualBox menu bar.
3. Select the BLE USB adapter from the menu. Your BLE USB adapter should not be accessible in your VM.
   > It should have the word "Broadcom" in it.
   > If you notice errors in this step, unplug and replug the BLE USB adapter, and try again.
4. Make sure Bluetooth is switched on in your Ubuntu VM. Open the Settings app (5th icon from the top in the Ubuntu dock), select "*Bluetooth*" from the right pane and switch it on.
5. You will need to repeat these steps every time you start the VM.

### Workflow
Choose a workflow that works best for you.

#### Workflow A
Copy your base code files from your Host OS into your shared folder. This way, you can edit the python (.py) files in an IDE/code editor on the host OS and edit the jupyter notebooks (.ipynb) in the Guest OS using Jupyter Lab. The Host OS also has access to the screenshots, screen recordings and logs saved by the python notebook (in the Guest OS) in the shared folder.

#### Workflow B
You can use the Guest OS like any other complete OS. Login to the appropriate websites to read documentation, download codebases and upload your work. This workflow is best suited for systems where the VM runs smoothly.

# References:
1. [More VM Info](https://www.redhat.com/en/topics/virtualization/what-is-a-virtual-machine)
2. [Enable Viirtualization in your PC Bios](https://bce.berkeley.edu/enabling-virtualization-in-your-pc-bios.html)
3. [Guest Additions](https://www.tecmint.com/install-virtualbox-guest-additions-in-ubuntu/#:~:text=VirtualBox%20Guest%20Additions%20are%20a,and%20usability%20of%20guest%20systems.&text=Easy%20mouse%20pointer%20integration)
4. [Shared folders](https://helpdeskgeek.com/virtualization/virtualbox-share-folder-host-guest/)
5. [What is Ubuntu?](https://help.ubuntu.com/lts/installation-guide/s390x/ch01s01.html)
6. [More Virtualization Terms](https://www.globalknowledge.com/us-en/resources/resource-library/articles/virtualization-terms-you-should-know/#:~:text=Virtual%20Machine%20(VM),such%20as%20Windows%20or%20Linux)

<hr>
<hr>