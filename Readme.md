# FM Radio on Raspberry Pi
These tools require an RTL chipset based DVB-T dongle and strong radio signals. There are a bunch of scripts to listen to FM directly using the sox play command ... or as an IP feed from the IP address of the Raspberry Pi.

[Required Hardware DVB-T or RTL_SDR Stick](https://www.amazon.com/dp/B01B4L48QU/ref=sspa_dk_hqp_detail_aax_0?psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUExTkU4VUY2UjRVU1kyJmVuY3J5cHRlZElkPUEwODk1ODU0OUEyTTJOMlFUMDlDJmVuY3J5cHRlZEFkSWQ9QTAwODg2NzIxWkdBOTA1RElNMktIJndpZGdldE5hbWU9c3BfaHFwX3NoYXJlZCZhY3Rpb249Y2xpY2tSZWRpcmVjdCZkb05vdExvZ0NsaWNrPXRydWU=)

Here is some general information about the hardware and software:

- https://cromwell-intl.com/open-source/raspberry-pi/sdr-getting-started.html
- https://photobyte.org/using-the-raspberry-pi-as-an-rtl-sdr-dongle-server/
- https://github.com/AlbrechtL/rtl_fm_streamer

## IPRadio
- To find your Pi's Ethernet address use "ifconfig eth0"
- To find your Pi's WiFi address use "ifconfig wlan0"
- To listen to 90.3FM over your network http://your-pi-ip:2346/90300000

## Clone the repo

```
$ cd ~/
$ git clone https://github.com/duracell80/RTLUtils.git
$ cd RTLUtils

$ sudo chmod +x *.sh
```

## Install RTL_FM
To install only the SDR tools without a streamer
```
$ cd ~/RTLUtils
$ ./install.sh
```
After reboot and knowing a local FM station's frequency and already knowing your Pi is outputting sound ...

```
$ cd ~/RTLUtils
$ sudo chmod +x *.sh
$ ./tune_fm.sh 103.3M
```
## Install RTL_FM with FM Server (FM over IP)
To install the SDR with a streamer
```
$ cd ~/RTLUtils
$ sudo chmod +x *.sh
$ ./install-streamer.sh
$ sudo reboot

From your PC or Phone's browser via port 2346 for FM station 103.3 ...
http://your-pi-ip:2346/103300000/2
```

This could be useful if you're wanting to listen to an FM station on your phone maybe in VLC without having to use Internet data to do so, using your Raspberry Pi as a conduit. Maybe your local FM station doesn't carry sporting events on their online stream.

To listen to a local FM radio station over your local LAN after reboot and knowing the IP address of the Pi and a frequency to tune to such as 103.3; Open the URL in a web browser or VLC player such as ... http://192.168.1.180:2346/95500000 for 95.5FM. Add these as URL's to M3U or PLS files to use within Mopidy, Moode, Volumio or any other MPD player. Note as stated by rtl_fm_streamer's creator, this is a one dongle, one station, one client deal. It's not possible to tune to the same station from multiple devices at the same time. Switching between listening devices I have found it beneficial to reboot the pi.

### Starting and Stopping the Radio Server
```
$ sudo service rtl_fm stop
$ sudo service rtl_fm start
```

### Reading the Status of the Radio Server
```
$ sudo service rtl_fm status

rtl_fm.service - RTL FM Server, turns local broadcast FM stations into online radio $
   Loaded: loaded (/etc/systemd/system/rtl_fm.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2020-06-07 07:54:30 BST; 2min 25s ago
 Main PID: 972 (rtl_fm_streamer)
    Tasks: 8 (limit: 2200)
   Memory: 13.9M
   CGroup: /system.slice/rtl_fm.service
           └─972 /usr/local/bin/rtl_fm_streamer

Jun 07 07:56:23 raspberrypi rtl_fm_streamer[972]: Start streaming on frequency: 103300000 Hz [stereo: 0]
Jun 07 07:56:23 raspberrypi rtl_fm_streamer[972]: Tuned to 103780000 Hz.
Jun 07 07:56:23 raspberrypi rtl_fm_streamer[972]: Mono demodulation
Jun 07 07:56:23 raspberrypi rtl_fm_streamer[972]: Waiting for connection...
Jun 07 07:56:24 raspberrypi rtl_fm_streamer[972]: Error sending stream: "Broken pipe". Close the connection!
Jun 07 07:56:46 raspberrypi rtl_fm_streamer[972]: Connected
Jun 07 07:56:46 raspberrypi rtl_fm_streamer[972]: Start streaming on frequency: 103300000 Hz [stereo: 0]
Jun 07 07:56:46 raspberrypi rtl_fm_streamer[972]: Tuned to 103780000 Hz.
Jun 07 07:56:46 raspberrypi rtl_fm_streamer[972]: Mono demodulation
Jun 07 07:56:47 raspberrypi rtl_fm_streamer[972]: Waiting for connection...

```

You can tune to a stereo stream, depending on the capability of your Pi. Contact the rtl_fm_streamer maintainer. Knowing your local FM frequencies is very helpful and I recommend putting together a playlist of the stations you want to tune to, in this way you could mix local FM broadcasts with online streams in one playlist file. It is possible to listen to FM bands beyond what a normal HiFi can recieve. You could try SDR# on Windows to explore your local signals. In the US it might be possible to tune to a NOAA broadcast.

[Lookup FM Frequencies Here](https://www.fmlist.org)
