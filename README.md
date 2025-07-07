# pwm_fan_pax_norte
Conversion of Pax Norte fan to work in esphome


Instead of the included HR202 analog humidity sensor, I connected the SHT45 digital sensor to the sda: GPIO17 scl: GPIO04 pins.

The RGB led is connected to pins GPIO18 - Red, GPIO19 - Green, GPIO21 - Blue.

The TEMT6000 analog light sensor is connected to GPIO34.

The PWM output for controlling the motor power is GPIO22.

The PPM input for reading the motor RPM is GPIO16.

The controller has four operating modes - "Auto", "Duty", "Manual", "Burst"

The revolutions for each mode are installed individually via sliders.

The thresholds of humidity for changing revolutions in automatic mode are set separately.

Indication of operating modes: green - on duty mode, blue - manual mode, red - burst, flashing blue - automatic increased humidity.

When the slider changes for the speed for manual mode, switching into manual mode after 10 minutes switch to automatic mode.


![Main board](https://github.com/KostuaD/pwm_fan_pax_norte/blob/main/PAX_NORTE.png#:~:text=PAX_NORTE.png)

Pins on main board:

1 - 3.3V
2 - TX
3 - RX
4 - GND
5 - GPIO0
6 - EN
