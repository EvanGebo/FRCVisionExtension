
# FRC Vision Extensions
This repository is going to store my notes on trying to run some custom code on FRC's official Raspberry Pi image. For general notes on how to setup and use the Pi and on how our vision system worked for the short-lived Infinite Recharge game, see [this repository](https://github.com/The-Charge/Vision2020). Also, for the custom HTML dashboard I'll be using, see [this repository](https://github.com/FRCDashboard/FRCDashboard).

## Setup

### PuTTY & File Transfer
I used the [PuTTY](https://www.putty.org/) software to connect to the Pi and use the command line remotely. I also used [PSCP](https://it.cornell.edu/managed-servers/transfer-files-using-putty) to transfer files from my computer to the Pi.

### File Structure
The FRC image stores its main files in three locations: `/home/pi`, which is the home directory of the Pi; `/usr/local/frc`; and `/usr/local/frc-static`. However, the only files that we need to worry about are stored in `/home/pi`. Of these, `runCamera`is the most important file. When opened in a text editor(`nano runCamera`) , you can see that it prints to the console, sleeps 5 seconds, and finally runs a python file. What Python file it runs depends on whether you have it set to built in streaming, Python example, or an uploaded Python file. If a Python file is uploaded, than that file is created in the same directory with the name `uploaded.py`. By default, the Pi's file-system is in read-only so you won't be able to change anything. To remedy this run `sudo mount -o remount,rw /`. If that doesn't work, than look at [this post](https://www.raspberrypi.org/forums/viewtopic.php?t=149196) on the Raspberry Pi forums.

Next step: getting Python to work right. In the same directory, if you run `python --version` you'll see that it's `Python 2.7.16`. However, the file we uploaded is for Python 3, so there's clearly a difference here. In `runCamera`, the last line is `exec /usr/bin/python3 uploaded.py`, so there is an installation of Python 3 at that location. If you run `/usr/bin/python3 --version` instead it returns `Python 3.7.3`, which is correct. To install any third party libraries, you need `pip`. This should be installed by default as `pip3`, but if it's not it can be installed by `apt install python3-pip`. You may need to preface these with `sudo` if they don't work.

## Projects

### Facial Detection
Facial detection is only finding the face, not identifying who it is or features about it.

#### Haar Cascade
I followed [this tutorial](https://towardsdatascience.com/face-detection-in-2-minutes-using-opencv-python-90f89d7c0f81) for most of the code for this. You'll need to upload the main fail as normal, but also transfer `haarcascade_frontalface_default.xml` to `\home\pi` on the Pi using PSCP.
