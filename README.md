<h4>Ubuntu 20.04 Users NOTE:</h4>

Note that on Ubuntu 20.04 or newer PyQt4 is no longer available. A contributor has created his own modification to the code which uses PyQt5. You can try it at this link https://github.com/rfjohnso/kerberossdr

<h4>Change Log</h4>

* merged changes submitted from bradylangdale who was kind enough to submit a pull request to fix some errors that arose from updates to pyApril and pyQt5. The previous release relied on previous versions of those packages.
* The purpose of this branch is to get ksdr working in the latest kubuntu (and Ubuntu) based linux, which use different libraries (PyQt5 and libffi7). This branch is currently in testing status, but I have successfully gotten it working in both Ubuntu 20.04 LTS and kubuntu 20.04 LTS.
* This branch is meant for, and only works in, Ubuntu + kubunu versions 20.04 LTS. It does not work on older linux versions. 
* ***Note for smaller display areas: There seems to be an issue on machines that do not support higher display resolutions. The Kerberossdr window will not resize properly if the display doesn't support a resolution with a height above around 960 (e.g 1280x960 was the minimum I could get the whole window visible with). This shouldn't be a problem on most newer desktops and higher end laptops but since a lot of laptops only support 1366x768 this is an issue for those. While this issue is present in the original kerberossdr software on Ubuntu 18.04/PyQt4, the level of screen clipping is, it seems, more pronounced on 20.04/PyQt5 and you cant get to the "Start Processing button" where you could at least get to that on 18.04/PyQt4.***
* The installation instructions below have been updated for installing PyQt5 and libffi7, which are compatable with the new Linux releases instead of PyQt4 and libffi6. 
* A little background in case you run into problems so you will know what I did to get this working (if you want to go to the original rtl-sdr blog github repository and make the changes manually). Basically, the only changes needed in the Python code were to import PyQt5 where ever PyQt4 was imported  (using slightly different syntax than PyQt4, you cant just change PyQt4 to PyQt5, see  _GUI/hydra_main_window.py for an example). The files that needed the PyQt5 import changes were: _GUI/hydra_main_window.py, _signalProcessing/hydra_signal_processor.py and in _GUI/hydra_main_window_layout.py. Then, find/replaced all the instances of .setMargin(0) to .setContentsMargins(0,0,0,0) in the _GUI/hydra_main_window_layout.py file. Finally, I updated the install instructions to get rid of Pyqt4 and use PyQt5, and then changed libffi6 to libffi7.

* Selecting “Uniform Gain” will allow you to set the same gain value for all four receivers.
* The antenna spacing value (s, fraction of wavelength) is automatically calculated based on frequency and a user set antenna spacing (s’, meters). For circular arrays, just use the spacing between each antenna, the program will calculate the radius for you.
* I’ve added a button to the Web UI to enable the sync display and the noise source in one click. If the noise source or the sync display (or both) is enabled the button will disable both. This should make calibration less cumbersome on mobile devices.
* I've added CSS to the Web UI. This will allow for easy customization of the layout and adds a mobile friendly flare. Feel free to edit ./static/style.css to your liking.
* The graphs hurt less to look at.
* Fixed the backwards compass reading in JavaScript. This is just a bandaid. The reading should be fixed at the origin and the Android App updated to take the correct reading.


<h3>Please see the software tutorial at www.rtl-sdr.com/ksdr</h3>

<h2>KerberosSDR Demo Software</h2>

<h3>Installing the software</h3>

1. <h4>Install Dependencies via apt:</h4>

  `sudo apt update`<br>
  `sudo apt install python3-pip build-essential gfortran libatlas3-base libatlas-base-dev python3-dev python3-setuptools libffi-dev python3-tk pkg-config libfreetype6-dev php-cli wondershaper python3-pyqt5 libffi7`

2. <h4>Uninstall any preinstalled numpy packages as we want to install with pip3 to get optimized BLAS.</h4>

  `sudo apt remove python3-numpy`

3. <h4>Install Dependencies via pip3:</h4>

  `pip3 install numpy`<br>
  `pip3 install matplotlib`<br>
  `pip3 install scipy`<br>
  `pip3 install cairocffi`<br>
  `pip3 install pyapril`<br>
  `pip3 install pyargus`<br>
  `pip3 install pyqtgraph`<br>
  `pip3 install peakutils`<br>
  `pip3 install bottle`<br>
  `pip3 install paste`<br>

4. <h4>Install RTL-SDR-Kerberos Drivers</h4>

  Our Kerberos RTL-SDR driver branch contains code for slightly modified Osmocom RTL-SDR drivers that enable GPIO, disable dithering, and disable zerocopy buffers which seems to cause trouble on some ARM devices.

  `sudo apt-get install libusb-1.0-0-dev git cmake`<br>

  `git clone https://github.com/rtlsdrblog/rtl-sdr-kerberos`<br>

  `cd rtl-sdr-kerberos`<br>
  `mkdir build`<br>
  `cd build`<br>
  `cmake ../ -DINSTALL_UDEV_RULES=ON`<br>
  `make`<br>
  `sudo make install`<br>
  `sudo cp ../rtl-sdr.rules /etc/udev/rules.d/`<br>
  `sudo ldconfig`<br>

  `echo 'blacklist dvb_usb_rtl28xxu' | sudo tee --append /etc/modprobe.d/blacklist-dvb_usb_rtl28xxu.conf`

5. <h4>Reboot the Pi.</h4>

6. <h4>Test 4x Tuners</h4>

  At this stage we recommend first testing your four tuners with rtl_test. Open four terminal windows, or tabs, and in each window run "rtl_test -d 0", "rtl_test -d 1", "rtl_test -d 2" and "rtl_test -d 3". Ensure that each unit connects and displays no errors.
Install KerberosSDR Demo Software

7. <h4>Clone or unzip the software</h4>

  `git clone https://github.com/rfjohnso/kerberossdr`<br>
  `cd kerberossdr`<br>
  `sh setup_init.sh`

8. <h4>Now you can run the software by typing</h4>

  `./run.sh`

Full software tutorial at www.rtl-sdr.com/ksdr

TROUBLESHOOTING:

Edit the run.sh file and comment out the >&/dev/null parts on the last line to show any errors to the terminal.


This software was 95% developed by Tamas Peto, and makes use of his pyAPRIL and pyARGUS libraries. See his website at www.tamaspeto.com
