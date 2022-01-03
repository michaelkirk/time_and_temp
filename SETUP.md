Adafruit has a walkthrough for setting up this sensor here:

https://learn.adafruit.com/adafruit-aht20/python-circuitpython

But there examples are for either arduino, or if you're using a raspberry pi
(like I am) to use circuit python. I haven't really been able to figure out
what circuit python is, but it seems to be an entire operating system and boot
loader (which maybe boots into an editor which is not what I want at all.. but
I could be confused but I might be misunderstanding)

Anyway, all I had to do was:

1. Enable ISC on your pi, via the config app

	sudo raspi-config

(from https://raspberrypi.stackexchange.com/questions/118828/adafruit-bonnet-adafruit-circuitpython-motorkit-return-valueerror-no-hardwar)


then you can talk to the circuit!

	sh> python
	python> # Create sensor object, communicating over the board's default I2C bus
	python> i2c = board.I2C()  # uses board.SCL and board.SDA
	python> sensor = adafruit_ahtx0.AHTx0(i2c)
 	python> print("\nTemperature: %0.1f C" % sensor.temperature)
	python> print("Humidity: %0.1f %%" % sensor.relative_humidity)

2. sudo adduser timeandtemp

3. add user timeandtemp to the "i2c" group

4. install files for hardware (as user timeandtemp)
	
	pip3 install adafruit-circuitpython-ahtx0

**Note:** I  couldn't get the above to succeed in venv, so I ended up installing outside of venv. Here's the error output I got when tryign to install within venv:

	python3 -m venv .env
	source .env/bin/activate
	pip3 install adafruit-circuitpython-ahtx0

	Using legacy 'setup.py install' for RPi.GPIO, since package 'wheel' is not installed.
Installing collected packages: pyusb, pyserial, sysv-ipc, rpi-ws281x, RPi.GPIO, pyftdi, Adafruit-PureIO, Adafruit-PlatformDetect, Adafruit-Blinka, adafruit-circuitpython-register, adafruit-circuitpython-busdevice, adafruit-circuitpython-ahtx0
    Running setup.py install for RPi.GPIO ... error
    ERROR: Command errored out with exit status 1:
     command: /home/pi/src/time_and_temp/.env/bin/python3 -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-q78v0udr/rpi-gpio_8abc690afaf34437a6165e400cb3f69a/setup.py'"'"'; __file__='"'"'/tmp/pip-install-q78v0udr/rpi-gpio_8abc690afaf34437a6165e400cb3f69a/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record /tmp/pip-record-ko3q0xr3/install-record.txt --single-version-externally-managed --compile --install-headers /home/pi/src/time_and_temp/.env/include/site/python3.9/RPi.GPIO
         cwd: /tmp/pip-install-q78v0udr/rpi-gpio_8abc690afaf34437a6165e400cb3f69a/
	...
  collect2: error: ld returned 1 exit status
    error: command '/usr/bin/arm-linux-gnueabihf-gcc' failed with exit code 1
    ----------------------------------------
ERROR: Command errored out with exit status 1: /home/pi/src/time_and_temp/.env/bin/python3 -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-q78v0udr/rpi-gpio_8abc690afaf34437a6165e400cb3f69a/setup.py'"'"'; __file__='"'"'/tmp/pip-install-q78v0udr/rpi-gpio_8abc690afaf34437a6165e400cb3f69a/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record /tmp/pip-record-ko3q0xr3/install-record.txt --single-version-externally-managed --compile --install-headers /home/pi/src/time_and_temp/.env/include/site/python3.9/RPi.GPIO Check the logs for full command output.

5. add cronjob to record time and temp data
