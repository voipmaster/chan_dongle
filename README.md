Installation
Chan_dongle runs only in asterisk 1.6.2 and asterisk 1.8. Asterisk 1.4 is not supported.

Asterisk Installation
There is a lot of very good howto's explaining Asterisk installation. Take a look at this brief summary (Debian), but you can take a look at 1, 2 or 3 (in spanish).

Note that we don't need Dahdi channel to run chan_dongle, so it can be avoided.

Prerequisites
Be careful with iptables and selinux. If needed, edit /etc/selinux/config and write:

SELINUX=disabled
SELINUXTYPE=targeted
Then, disable iptables

iptables -F
iptables-save > /etc/iptables.up.rules
or configure it to allow SIP (port 5060) and RTP (port 10000-20000) if you plan to use these protocols.

Now upgrade your system

apt-get update
apt-get upgrade
And install packages needed for compilation of asterisk

apt-get install linux-headers-`uname -r` gcc g++ make libnewt-dev libncurses5-dev openssl libssl-dev zlib1g-dev
Asterisk Installation
Download source files and untar. Asterisk 1.6 is explained, but 1.8 looks similar.

wget http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-1.6.2.13.tar.gz
tar zxvf asterisk-1.6.2.13.tar.gz
If you will use FreePBX, then install asterisk-addons

wget http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-addons-1.6.2.2.tar.gz
tar zxvf asterisk-addons-1.6.2.2.tar.gz
Now compile

cd asterisk-1.6.2.13
make clean && ./configure --disable-xmldocs && make && make install && make config
If needed

make samples
Install chan_dongle
Obtain source code
If you installed asterisk as described here, go ahead. Anyway, check some dependencies like automake and autoconf. Also must have asterisk sources and development tools like make, gcc and so on.

SVN Method
svn checkout https://github.com/bg111/asterisk-chan-dongle/trunk/ dongle-read-only
cd dongle-read-only
aclocal && autoconf && automake -a
Compressed package
wget https://github.com/bg111/asterisk-chan-dongle/archive/master.zip
unzip master.zip
cd asterisk-chan-dongle-master
aclocal && autoconf && automake -a
Set Build options
./configure
configure options:

--enable-debug
--disable-debug
--enable-manager
--disable-manager
--enable-apps
--disable-apps
--with-asterisk=/path_to_source/asterisk.h
usefull environment variables

DESTDIR
CFLAGS
LDFLAGS
Examples

DESTDIR=“/usr/lib/asterisk/modules” ./configure
./configure –with-asterisk=/usr/src/asterisk-1.6.2.13/include
CFLAGS=“-I /usr/src/asterisk-1.6.2.13/include” ./configure
./configure --enable-debug
./configure --disable-apps --disable-manager
CFLAGS=“-O0 -g” ./configure
CFLAGS=“-Os” ./configure
default is

./configure --disable-debug --enable-apps --enable-manager
DESTDIR searching in /usr/lib/asterisk/modules /usr/local/lib/asterisk/modules /opt/local/lib/asterisk/modules
and

asterisk.h searching in ../include /usr/include /usr/local/include /opt/local/include
Building
For compile and build source to module

make
Installation
For install property configured and build module:

sudo make install
Module Configuration
Once installed, copy example file dongle-read-only/etc/dongle.conf to /path_to_asterisk_config/ (i.e. /etc/asterisk/). Configure it as desired and start asterisk. To load, stop or restart chan_dongle:

CLI>module load chan_dongle.so
CLI>module unload chan_dongle.so
CLI>module reload chan_dongle.so
or much easy

CLI>module reload now
CLI>module reload gracefully
CLI>module reload convenient
