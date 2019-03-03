https://github.com/asdil12/pymultimonaprs
https://osmocom.org/projects/rtl-sdr/wiki/Rtl-sdr
https://github.com/EliasOenal/multimon-ng/


git clone git://git.osmocom.org/rtl-sdr.git

cd rtl-sdr/

mkdir build

cd build

sudo apt-get install cmake

sudo apt-get install libusb-1.0-0-dev

cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON

make

sudo make install

sudo ldconfig

###

git clone https://github.com/EliasOenal/multimon-ng.git

cd multimon-ng/

mkdir build

cd build

cmake ..

# ignore pusle warning

make

sudo make install

###

git clone https://github.com/asdil12/pymultimonaprs.git

cd multimon-ng/

sudo apt-get install python-minimal

sudo apt-get install python-pip

sudo pip install setuptools

sudo python2 setup.py install

sudo vim /etc/pymultimonaprs.json

sudo systemctl enable pymultimonaprs.service

sudo systemctl start pymultimonaprs.service

sudo systemctl status pymultimonaprs.service
