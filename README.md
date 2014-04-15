## ARDrone Embedded Client

This is a simple UDP client that runs on the ARDrone's embedded Linux system, based on [ARDrone-Client by Conor Branagan](https://github.com/conorbranagan/ARDrone-Client).

This program is distributed in the hope that it will be useful, but is provided AS IS with ABSOLUTELY NO WARRANTY; The entire risk as to the quality and performance of the program is with you. Should the program prove defective, you assume the cost of all necessary servicing, repair or correction. In no event will any of the developers, or any other party, be liable to anyone for damages arising out of the use or inability to use the program. 

####How to Install:

1. (Optional) Compile for ARM GNU/Linux using an ARM cross-compiler
	- `$ arm-g++ -o "DroneClient" DroneClient.cpp`
2. Copy compiled executable onto ARDrone using FTP. 
	- Boot up ARDrone
	- Connect to ARDrone ad-hoc network
	- FTP into 192.168.1.1 as anonymous user/password using FTP GUI or command line
	- Copy ARDrone executable to ARDrone
3. Run client on ARDrone
	- Open a Terminal/Shell
	- Telnet into the ARDrone and make DroneClient executable
		- `$ telnet 192.168.1.1`
		- `$ cd /data/video`
		- `$ chmod +x DroneClient`
	- Run the Drone Client: `$ ./DroneClient`

		
####Using the Drone Client:
- There are two types of use for the Drone client. 

1. Simple command line input by the user. Example:
	- `$ takeoff`
	- `$ pitch -.3`
	- `$ gaz .2`
	- `$ pitch 0`
	- `$ gaz 0`
	- `$ land`
	
2. ARDrone scripts which follow the same format as the command line input but also include a **wait** command. An example script **sample.ard**
is included here as well. To execute the script you simply add it as an argument:
	- `$ ./DroneClient sample.ard`


####How it works:
The ARDrone is designed to receive UDP packets on port 5556. The packets can contain one or more AT commands which the Drone will then
read and react to. For the native client to run, you simply take this same idea but send the packets to port 5556 on localhost. The Drone
sees this as the same as any remote client sending AT commands and thus the client works as it should.

####Build Instructions for Linux (Ubuntu)
The C++ file needs to be compiled with a ARM GNU/Linux cross-compiler or you may use the already compiled DroneClient executable (may be out of date).    I 

Here's how I got the code to build code that ran on an ARDrone 2.0, built on an Ubuntu VM based on `ubuntu-12.04.4-desktop-amd64.iso`.

######Toolset
Install the free `Sourcery G++ Lite 2011.03-41` ARM toolset

* I used the IA32 GNU/Linux Installer
  * (after downloading, chmod the file to be executable, then run it)
  * The installer required me to install 32bit support - libc6-i386 (My Ubuntu was x64)
  * The installer asked to use a specific shell - instructions were provided to do this.
  * I chose to install into `/opt/CodeSourcery`
  * Liberal use of `sudo` was require in the steps above.

######Build
`$ /opt/CodeSourcery/Sourcery_G++_Lite/bin/arm-none-linux-gnueabi-g++ -pthread -o "DroneClient" DroneClient.cpp`
	
