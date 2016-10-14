# Atmel SAMD MicroPython Firmware Build Machine

Vagrant-based virtual machine to build MicroPython for Atmel SAMD-based boards.

## Installation & Usage

1.  Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

2.  Install [Vagrant](https://www.vagrantup.com/downloads.html).

3.  Clone or download this repository to a directory.

4.  In a terminal navigate to the directory and provision the VM by running:

        vagrant up

    The very first time this command is run it will take some time (~30 minutes)
    to download the operating system and setup the MicroPython build toolchain.

5.  After the VM is provisioned and up use the `vagrant ssh` command to enter
    the VM.  Once inside the VM the latest Atmel SAMD MicroPython source will
    be in the `~/source/micropython/atmel-samd` directory.  You can enter this folder
    and compile the firmware by running:

        cd ~/micropython/atmel-samd
        make BOARD=feather_m0_bluefruit_le

6.  By default the VM will automatically grab USB devices that use the Adafruit M0
    bootloader (i.e. double press reset button on the board to make its LED pulse
    slowly on and off).  The VM includes the BOSSA tool to flash firmware and can be used
    with a command like (assuming the board is visible to the VM under the /dev/ttyACM0
    device):
    
        sudo bossac -p ttyACM0 -U true -e -w -v -R /path/to/firmware.bin
    
    Note the -e option will erase the chip's flash and any files on the MicroPython filesystem!
    
    Change the path to the firmware to the .bin file that was compiled.  These are typically
    located in the build- directory for each board like build-feather_bluefruit_m0_le/firmware.bin.

7.  To copy files between the VM and host computer use the  `/vagrant` folder
    inside the VM.  Any files in the `/vagrant` folder will be synchronized with
    the directory where the Vagrantfile is located on the host computer.

8.  You can exit the VM at any time with the `exit` command.

    **Note even after exiting the VM will continue to run in the background!**
    Use the `vagrant halt` command to stop the VM.

    Once halted to enter the VM again use the `vagrant up` and then `vagrant ssh`
    commands again.

See more [details on Vagrant usage here](https://www.vagrantup.com/docs/getting-started/).
