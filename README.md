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

Configure tvheadend enabling dvbcsa and constcw for PVU and enable bunble for ffmpeg transcoding
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

