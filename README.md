# Tvheadend with oscam in raspberry3

Install raspberry pi os 32 bits Desktop

Adjust language English and us keyboard at begining installation
or run
```
piwiz
```
Connect to network and update software

Open a ssh connection

Install Dependencies:

```
sudo apt install gettext
sudo apt-get install libssl-dev
sudo apt install cmake
sudo apt install build-essential git pkg-config libssl-dev bzip2 wget libavahi-client-dev zlib1g-dev libavcodec-dev libavutil-dev libavformat-dev libswscale-dev libavresample-dev gettext cmake libiconv-hook-dev liburiparser-dev debhelper libcurl4-gnutls-dev python-minimal
sudo apt-get install libpcre3-dev
sudo apt-get install libdvbcsa-dev
```

Download Tvheadend
```
git clone https://github.com/tvheadend/tvheadend.git
cd tvheadend
git checkout d492091de8231ca25ac4b4f682da7d32f3d6f44f
```
Or find the last commit installing gitk (visual)
```
sudo apt-get install gitk
```

Configure tvheadend enabling dvbcsa and constcw for PVU and enable bundle for ffmpeg transcoding
```
./configure --enable-dvbcsa --enable-constcw  --enable-bundle
```

Compiling:
```
sudo AUTOBUILD_CONFIGURE_EXTRA=--disable-libx265\ --disable-pie\ --disable-libvpx  ./Autobuild.sh
```

Go to ~ and look for package package, install with the following line: (substitute whatever is in front version.
```
sudo dpkg -i tvheadend_4.3-1895~gd492091de_armhf.deb
```

it will show:
```
 | Tvheadend is accessed via a world reachable web interface (assuming your  │ 
 │ host is reachable from the internet). To protect from malicious use you   │ 
 │ must create an initial account to be able to login to Tvheadend. This     │ 
 │ account can not be changed nor deleted from within Tvheadend itself. If   │ 
 │ you want to change the superuser account you need to reconfigure the      │ 
 │ Tvheadend package. 
```
Pick administrator username:
by example: admin
Administrador password: trinity

Next:
```
 │ After installation Tvheadend can be accessed via HTTP on port 9981. From  │ 
 │ this machine you can point your web-browser to http://localhost:9981/.    │ 
 │                                                                           │ 
 │ If you want to completely remove configuration, use your package          │ 
 │ managers --purge option, e.g, apt-get remove --purge tvheadend* 
```

Follow Directions give password Enter Tvheadend 
```
http://192.168.1.82:9981
```

Install Kodi
```
sudo apt-get install kodi
```
Install hts tvheadend client
```
sudo apt-get install kodi-pvr-hts
```
Install IPTV for ffmpg transcoding
```
sudo apt-get install kodi-pvr-iptvsimple
```

### Install OSCAM

```
cd /home/pi
git clone https://github.com/oscam-emu/oscam-patched
cd oscam-patched
git checkout oscam11546-emu798
```

for options:
```
./config.sh | more
```

Compile
```
make USE_LIBCRYPTO=1
cd Distribution
sudo mv oscam-1.20_svn11546-798-arm-linux-gnueabihf /usr/local/bin/oscam
```
Create directories
```
cd ~
mkdir .oscam
cd oscam
mkdir tmp
```

Create file
```
vim ~/.oscam/oscam.conf
```
And Insert the following
```
# oscam.conf generated automatically by Streamboard OSCAM 1.20-unstable_svn SVN r11392
# Read more: http://www.streamboard.tv/svn/oscam/trunk/Distribution/doc/txt/oscam.conf.txt

[global]
logfile                       = stdout
fallbacktimeout               = 3
bindwait                      = 5
initial_debuglevel            = 64
nice                          = -1
maxlogsize                    = 200
waitforcards                  = 0
preferlocalcards              = 1
lb_mode                       = 1
lb_save                       = 100
lb_savepath                   = /home/pi/.oscam/oscam.stat
lb_auto_timeout_t             = 26000

[cache]

[streamrelay]
stream_relay_enabled          = 0

[dvbapi]
enabled                       = 1
au                            = 1
pmt_mode                      = 4
listen_port                   = 9001
delayer                       = 60
ecminfo_type                  = 4
user                          = tvheadend
read_sdt                      = 1
extended_cw_api               = 1
boxtype                       = pc

[webif]
httpport                      = 8888
httprefresh                   = 2
httppollrefresh               = 2
httpallowed                   = 127.0.0.1,192.168.0.0-192.168.254.254
```

Create the file:
```
vim ~/.oscam/oscam.server
```

And Insert the following

```
[reader]
label                         = emulator
protocol                      = emu
device                        = emulator
caid                          = 090F,0500,1801,0604,2600,FFFF,0E00,4AE1,1010
detect                        = cd
disablecrccws_only_for        = 0E00:000000
ident                         = 0D00:0000C0;0D02:0000A0;0500:023800,021110,007400;0E00:000000;1801:000000;1010:000000
group                         = 1
emmcache                      = 2,0,2,0
emu_auproviders               = 0E00:000000
auprovid                      = 000E00

[reader]
label                         = SoftCam.Key
protocol                      = constcw
device                        = /home/pi/.oscam/SoftCam.Key
caid                          = 0D00,0D02,090F,0500,1801,0604,2600,FFFF,0E00,4AE1,1010
ident                         = 1801:000000,001101;0604:000000;2600:000000;FFFF:000000;0E00:000000;4AE1:000000;1010:000000
group                         = 1

```

Create the file:
```
vim ~/.oscam/oscam.services
```

And insert the following:
```
# oscam.services generated automatically by Streamboard OSCAM 1.20_svn SVN r11441
# Read more: http://www.streamboard.tv/svn/oscam/trunk/Distribution/doc/txt/oscam.services.txt

[afn]
caid                          = 0E00
provid                        = 000000
srvid                         = 0004,0005,0006,0007,0008,0009,012C,0098,0096,04B1,03E9,04B2,00AD,00B7,00B9,00AF,019E,0193,019D,0192,0191,004A,004C,009D,00A0,004B,0097,000F,0047,0048,0003
```

Create the file:
```
vim ~/.oscam/oscam.user
```

And insert the following:
```
[account]
user                          = tvheadend
pwd                           = tvheadend
monlevel                      = 4
au                            = 1
group                         = 1,2,3,4,5,6,7,8,9,11,12,13,14,15,16,17,18,19,21,22,23,24,25,26,27,28,29,31,32,33,34,35,36,37,38
```
