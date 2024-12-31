# Automation-Script-for-OS-Installation
This is an automated script for installing ParrotOS in your computer.


Steps to follow:


Ensure Python 3.x is installed on your system. 


Set Up Your IDE. Use any Python IDE or text editor.


Create a New File and paste the Code:


Install Required Tools:

Ensure wget and dd are installed on your system:

Linux/Mac: These are typically pre-installed.

Windows: Install them using a package manager like Chocolatey or use WSL (Windows Subsystem for Linux).



Run the script with bash


Permissions: You'll need root/sudo access for:

Running dd to write to the USB.

Mounting or copying files to the USB.


Testing Advice:

Use a spare USB drive to avoid accidental data loss.

The script will overwrite data on the selected USB device, so double-check the device path (e.g., /dev/sdb).
