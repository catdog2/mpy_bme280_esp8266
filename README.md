# README #

This is a driver for the Bosch BME280 temperature/pressure/humidity sensor, for use with MicroPython-based boards.

### About the BME280 ###

The Bosch BME280 Environmental Sensor is a combined temperature, pressure and humidity sensor. It can communicate via I2C or SPI; this driver uses I2C.

See the datasheet at https://www.adafruit.com/datasheets/BST-BME280_DS001-10.pdf for details.

### Using the library ###

Use ftp to copy `bme280.py` to the `flash` or `flash/lib` directory on the board. Then:

``` python
import machine
import bme280

i2c = machine.I2C(0, pins=('GP11', 'GP10'))
bme = bme280.BME280(i2c=i2c)

print(bme.temperature, bme.pressure, bme.humidity)
```

#### Detailed usage ####

The `temperature`, `pressure` and `humidity` properties are convenience functions that provide human-readable string values to quickly check that the sensor is working. In practice, the methods to use are:

* `get_temperature()`: returns the temperature in hundredths of a degree celsius. For example, the value 2534  indicates a temperature of 25.34 degrees.
* `get_pressure()`: returns the atmospheric pressure. This 32-bit value consists of 24 bits indicating the integer value, and 8 bits indicating the fractional value. To get a value in Pascals, divide the return value by 256. For example, a value of 24674867 indicates 96386.2Pa, or 963.862hPa.
* `get_humidity()`: returns the relative humidity. This 32-bit value consists of 22 bits indicating the integer value, and 10 bits indicating the fractional value. To get a value in %RH, divide the return value by 1024. For example, a value of 47445 indicates 46.333%RH.