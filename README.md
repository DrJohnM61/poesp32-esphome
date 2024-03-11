These are the instructions to get an M5Stack poesp32 running on ESPHOME and setting the UART2 pins TX and RX to switch a relay.

Device: <https://shop.m5stack.com/products/esp32-ethernet-unit-with-poe>

Edit the `M5Stack_poesp32.yaml` file and rename the switches to whatever you want to call them.
Change the API to one that you generate (you can use <https://generate.plus/en/base64> and set the length to 32).

Ensure that we have the latest version of ESPHome, Python, and other packages[^1]:

```shell-session
brew update
brew upgrade
```

Update `pip`:

```shell-session
python3 -m pip install --upgrade pip
```

Update ESPHome:

```shell-session
pip3 install -U esphome
```

Disassemble the POESP32 and connect the pins on the board to the USB serial adaptor (with the USB disconnected).

Ensure that RX -> TX and TX - RX.

Plug in the USB while holding/strapping the G0 pin to the top of the ESP32 chip. This will put it into bootloader mode.

Note the usb device that the M5Stack POESP32 is plugged into. If you see more than one device, unplug and re-run the command to see what the new device name is.

```shell-session
ls /dev/ | grep cu.usbserial
```

To build the ESP device with the coded YAML template, run the following command:

```shell-session
esphome run M5Stack_poesp32.yaml
```

Select the correct USB interface found in the first step.

> [!CAUTION]
> Don't wipe a wrong device plugged into the computer.

You should see progress: compile followed by writing the data to the device. If the device is not connected properly, then the load will fail. Check the connection and try again.

Make note of the encryption password for adding the device to home assistant.

Unplug the USB and plug in the POE ethernet cable. You should see the lights blinking. Go to Home Assistant and the device should be shown as a new device under the left-hand "settings" menu.

[^1]: These instructions are for macOS and require [Homebrew](https://brew.sh/).
