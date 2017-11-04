# ueyesrc

## What it is

This is a source element for GStreamer 1.0 for live acquisition from a camera that uses the 
ueye SDK from IDS (https://en.ids-imaging.com/downloads.html).

## Comments
- Contains a maxframerate property, that will limit the frame rate pushed to the pipeline. The frame rate can get smaller 
 if the exposure time is long, but if the exposure time is short the frame rate may be limited at the given value.

- It supports just colour of one type, BGR 24-bit

# Set up instruction
## Building
Building on linux is tested using the autogen script.
Tested with ueyesdk-setup-4.40-usb-amd64 on Ubuntu 12.04 64 bit.

First run autogen.sh

	$ chmod a+x autogen.sh
	$ ./autogen.sh

This will use autotools to setup the dev environment, and complete with the line:
"Now type 'make' to compile this module." You can do that by using make:

	$ sudo make install 
	
will put install the lo file for use with GStreamer, in `/usr/local/lib/gstreamer-1.0`
To use this in a pipeline you need to tell gstreamer where to find the `.lo` file.
use:

	$ export GST_PLUGIN_PATH=/usr/local/lib/gstreamer-1.0

See the `INSTALL` file for advanced setup.

To import into the Eclipse IDE, use "existing code as Makefile project", and the file EclipseSymbolsAndIncludePaths.xml is included here
to import the library locations into the project (Properties -> C/C++ General -> Paths and symbols).

# Libraries

Download and install the ueye SDK.
Example install script:

	cd ~/Downloads/LinuxCameraDrivers/uEye/
	MACHINE_TYPE=`uname -m`
	if [ ${MACHINE_TYPE} == 'x86_64' ]; then
		echo x64
		sudo sh ueyesdk-setup-4.40-usb-amd64.gz.run
	else
		echo x86
		sudo sh ueyesdk-setup-4.40-usb-i686.gz.run
	fi
	sudo /etc/init.d/ueyeusbdrc start
	sudo apt-get update
	sudo apt-get install -y libqt4-qt3support

The file `src/Makefile.am` contains pointers to where with ueye includes and libraries are expected to be.
Headers: /usr/include
Libraries: /usr/lib

Example commands for installing the GStreamer development libraries on Ubuntu 12.04 

	sudo add-apt-repository -y ppa:gstreamer-developers/ppa
	sudo apt-get update
	sudo apt-get install -y dpkg-dev devscripts
	sudo apt-get install -y gstreamer1.0*
	sudo apt-get install -y libgstreamer-plugins-base1.0-dev
	echo "export GST_PLUGIN_PATH=/usr/local/lib/gstreamer-1.0" >> ~/.profile
	sudo apt-get install -y build-essential libgtk-3-dev

# Sample ueyesrc pipelines

	gst-launch-1.0 ueyesrc ! videoconvert ! xvimagesink
	gst-inspect-1.0 ueyesrc

# Locations

Gstreamer plugin locations:
`/usr/lib/i386-linux-gnu/gstreamer-1.0`
`/usr/local/lib/gstreamer-1.0`









