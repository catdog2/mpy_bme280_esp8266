# This fork

I created this to raise a [PR](https://github.com/catdog2/mpy_bme280_esp8266/pull/9)
against the master. Unfortunately the master seems to have been abandoned. My PR and
other apparently good ones have been ignored. Please read the PR's epecially
[this bugfix](https://github.com/catdog2/mpy_bme280_esp8266/pull/11).

# README #

This is a driver for the Bosch BME280 temperature/pressure/humidity sensor, for use with MicroPython on ESP8266 boards. It is also compatible with the BMP280 which provides the same interface but temperature + pressure only.

### About the BME280 ###

The Bosch BME280 Environmental Sensor is a combined temperature, pressure and humidity sensor. It can communicate via I2C or SPI; this driver uses I2C.

See the datasheet at https://www.adafruit.com/datasheets/BST-BME280_DS001-10.pdf for details.

### Using the library ###

Copy `bme280.py` onto the board (e.g. using webrepl_cli.py). Then:

``` python
import machine
import bme280

i2c = machine.I2C(scl=machine.Pin(5), sda=machine.Pin(4))
bme = bme280.BME280(i2c=i2c)

print(bme.values)
```

#### Detailed usage ####

The `values` property is a convenience function that provides a tuple of human-readable string values to quickly check that the sensor is working. In practice, the method to use is `read_compensated_data()` which returns a `(temperature, pressure, humidity)`-tuple:

* `temperature`:  the temperature in hundredths of a degree celsius. For example, the value 2534  indicates a temperature of 25.34 degrees.
* `pressure`: the atmospheric pressure. This 32-bit value consists of 24 bits indicating the integer value, and 8 bits indicating the fractional value. To get a value in Pascals, divide the return value by 256. For example, a value of 24674867 indicates 96386.2Pa, or 963.862hPa.
* `humidity`: the relative humidity. This 32-bit value consists of 22 bits indicating the integer value, and 10 bits indicating the fractional value. To get a value in %RH, divide the return value by 1024. For example, a value of 47445 indicates 46.333%RH.

The BME280 constructor takes an optional `address` argument. BME280 devices are
on I2C addresses 0x76 or 0x77. If you do not provide an address the driver will
scan for a device and use it, raising an exception if no device is found.
