Sync files

rsync -avz --ignore-existing host:/path/to/folder /path/to/folder

Arduino

<pre>
$ stty -F /dev/ttyACM0 speed 1200
$ stty -F /dev/ttyACM0 speed 115200
$ avrdude -p m32U4 -P /dev/ttyACM0 -c avr109 -U flash:w:atreus62.hex
</pre>

Bossa

https://github.com/pda/arduino-zero-without-ide

sudo apt-get install build-essential

sudo apt-get install libreadline-dev wx3.0-headers libwxgtk3.0-0v5 libwxgtk3.0-dev
