## Install SSH Server on your VM
Install ssh server on your VM. Open a terminal and type:
```language
sudo apt install openssh-server -y
```
> If you are unable to open the VM GUI (some MacOS hosts crashes the VM when it tried to display its GUI Desktop), download the VM with openssh alreadly installed from this link.

## Setup Port Forwarding
In order to ssh into your Guest VM from your host, you need to setup some port forwarding rules. The default ports for SSH and jupyter are 22 and 8888, respectively.

1. In Virtualbox, right-click on "ECE4960" and select "Settings".
2. Select the "Network" tab and under the "Adapter 1" pane, expand the "Advanced" tab.
3. Click on "Port Forwarding" and a new row entry with the following information:

|            |           |
|------------|-----------|
| Name       | SSH       |
| Protocol   | TCP       |
| Host IP    | 127.0.0.1 |
| Port       | 2200      |
| Guest IP   | 10.0.2.15 |
| Guest Port | 22        |

4. Create another row entry:

|            |           |
|------------|-----------|
| Name       | jupyter   |
| Protocol   | TCP       |
| Host IP    | 127.0.0.1 |
| Port       | 8888      |
| Guest IP   | 10.0.2.15 |
| Guest Port | 8888      |

5. Click the button "OK" in the window.
6. Click the button "OK" in Settings window.


## SSH into your Ubuntu Guest VM:
1. To SSH into your Ubuntu Guest VM (with X11 Forwarding):
```language
ssh -p 2200 artemis@127.0.0.1
```
   

## Managing VM from the terminal
You can start a VM headless,(Use the small arrow next to the start button), power off and shutdown from Virtualbox. Virtualbox also provides a commandline interface.

- Start a VM:
    ```language
    VBoxManage startvm "ECE4960" --type headless
    ```
- Find the status of all the VMs:
    ```language
    VBoxManage list vms --long | grep -e "Name:" -e "State:"
    ```
- Return 1 is the VM "ECE4960" is running:
    ```language
    VBoxManage list runningvms | sed -r 's/^"(.*)".*$/\1/' | grep 'ECE4960' | wc -l
    ```
- Poewr off the VM:
    ```language
    VBoxManage controlvm ECE4960 poweroff
    ```
- Save the state of the VM:
    ```language
    VBoxManage controlvm ECE4960 savestate
    ```
- Shutdown the VM:
    ```language
    VBoxManage controlvm ECE4960 acpipowerbutton
    ```

# File Access
Setup a shared folder as instructed in the Lab 1(b): VM Setup. Once you ssh into your VM, you can access the files at:
> /media/<name_of_shared_folder>


## References:
1. https://bobcares.com/blog/virtualbox-ssh-nat/
2. https://stackoverflow.com/questions/38545198/access-jupyter-notebook-running-on-vm
3. https://superuser.com/questions/959567/virtualbox-windows-graceful-shutdown-of-guests-on-host-shutdown
