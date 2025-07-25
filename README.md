# pwm_fan_pax_norte
Conversion of Pax Norte fan to work in esphome

# ESPHome PAX Ventilator Controller

This ESPHome configuration is for an ESP32-based smart fan controller for PAX ventilators.  
It is highly customizable via substitutions, features robust humidity-based automation, and includes a debouncing mechanism to prevent frequent speed switching due to rapid sensor fluctuations.


Instead of the included HR202 analog humidity sensor, I connect the SHT45 digital sensor to the sda: GPIO17 scl: GPIO04 pins.

The RGB led is connected to pins GPIO18 - Red, GPIO19 - Green, GPIO21 - Blue.

The TEMT6000 analog light sensor is connected to GPIO34.

The PWM output for controlling the motor power is GPIO22.

The PPM input for reading the motor RPM is GPIO16.

The controller has four operating modes - "Auto", "Duty", "Manual", "Burst"

The revolutions for each mode are installed individually via sliders.

The thresholds of humidity for changing revolutions in automatic mode are set separately.

Indication of operating modes: green - on duty mode, blue - manual mode, red - burst, flashing blue - automatic increased humidity.

When the slider changes for the speed for manual mode, switching into manual mode after 10 minutes switch to automatic mode.

Added support for an external humidity sensor. If the external humidity is below 60%, then nothing changes. 

If the external humidity is within 60-70%, then 15% is added to the on-off thresholds. 

If the external humidity is greater than 70%, then the automatic mode is disabled and the Duty mode is enabled.

Record this sensor using the number.set_value action in Home Assistant to the sensor.{device_name}_current_humidity.


![Main board](https://github.com/KostuaD/pwm_fan_pax_norte/blob/main/PAX_NORTE.png#:~:text=PAX_NORTE.png)

Pins on main board:

1 - 3.3V, 
2 - TX, 
3 - RX, 
4 - GND, 
5 - GPIO0, 
6 - EN

---

## Features

- **Fan Modes**: Auto (humidity-driven), Duty (low/quiet), Manual (user-set, limited time), Burst (max speed, limited time)
- **Humidity Control**: Adjustable ON/OFF thresholds, with correction for high outdoor humidity
- **Debounce Logic**: Prevents fan speed toggling when humidity changes rapidly (configurable interval)
- **Time Schedules**: Switches to Duty mode at night and back to Auto in the morning
- **Indicators**: RGB LED shows current mode/status
- **Sensors**: Humidity, temperature, light, RPM, WiFi, uptime, external humidity input
- **User Controls**: Sliders for speed and thresholds, mode selector, reboot button
- **Web/API/OTA**: Local web server control, Home Assistant/ESPHome API, secure OTA

---

## Hardware Connections

| Signal           | Pin        | Substitution       |
|------------------|------------|--------------------|
| I2C SDA          | GPIO17     | `${i2c_sda}`       |
| I2C SCL          | GPIO04     | `${i2c_scl}`       |
| Fan PWM          | GPIO22     | `${fan_pwm_pin}`   |
| Red LED          | GPIO18     | `${red_led_pin}`   |
| Green LED        | GPIO19     | `${green_led_pin}` |
| Blue LED         | GPIO21     | `${blue_led_pin}`  |
| Fan RPM (pulse)  | GPIO16     | `${fan_rpm_pin}`   |
| Illuminance (ADC)| GPIO34     | `${illuminance_adc_pin}` |

---

## Customization (Substitutions)

You can easily change the following parameters at the top of the YAML:

- `room_name`, `device_name`
- GPIO assignments for all sensors, LEDs, and outputs
- Default humidity thresholds (`humidity_on_default`, `humidity_off_default`)
- Fan speed timer durations (`fan_mode_manual_time`, `fan_mode_burst_time`)
- Humidity debounce interval (`debounce_time`)

---

## How Humidity Debounce Works

When humidity changes, the fan state is only recalculated if at least `${debounce_time}` (default: 60s) has passed since the last change.  
This prevents fan speed from flickering due to sensor jitter or rapid environment changes.

---

## Usage Tips

- Use the web interface or ESPHome/Home Assistant to monitor and control all settings.
- Adjust sliders for humidity thresholds and fan speeds to fit your needs.
- "Manual" and "Burst" modes automatically revert to "Auto" after the set time.
- The RGB LED color and effect indicate the current fan mode.
- If "External Humidity" goes above 70%, the system will force Duty mode to avoid bringing in humid air.

---

## Comments & Structure

- All code comments are in English for clarity.
- All user-adjustable variables are at the top in the `substitutions:` section.
- Each section is clearly documented.

---

## Example: Setting Debounce

To change debounce interval to 2 minutes for humidity-triggered speed changes, set at the top:

```yaml
debounce_time: "120000"  # 2 minutes in milliseconds
```

Default is `"60s"` (60,000 ms).

---

## OTA and API

- For secure OTA and Home Assistant integration, provide your keys/passwords in `secrets.yaml`.

---

## Requirements

- ESP32 (board: `esp32dev`)
- Compatible PAX fan (PWM-controlled)
- SHT4x I2C temperature/humidity sensor
- (optionally) Illuminance sensor, RPM sensor, RGB LED

---

**Enjoy smart, silent, and energy-efficient ventilation!**
