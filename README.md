Lab Report - 7

DC MOTOR CONTROL USING ADC AND PWM

WITH TEMPERATURE SENSING AND LCD INTERFACE

Sowmya Balaji

Electronic Systems Engineering, IISc

1. Objective

The objective of this experiment is to control the speed and direction of a DC motor using analog
inputs from a potentiometer or a temperature sensor. The TM4C123GH6PM microcontroller
continuously samples these inputs using its 12-bit analog-to-digital converter (ADC). The
potentiometer reading determines the duty cycle of a 1 kHz pulse-width-modulated (PWM) signal,
while the temperature input allows the speed command to be scaled or overridden when heat
from a soldering iron is detected. A 16x2 character LCD displays the ADC raw value, temperature
reading, and calculated duty cycle percentage in real time.

2. Hardware & GPIO Configuration

The system uses the TM4C123GH6PM LaunchPad, a potentiometer, a temperature sensor, a DC
motor driver, and a 16x2 character LCD. The potentiometer wiper and temperature-sensor output
connect to separate ADC-capable analog inputs, with their voltages kept within the ADC input
range. PF2 is configured for its M1PWM7 alternate function, driven by PWM module 1, Generator
The PWM output connects to the motor driver's enable or speed-control input, while GPIO
outputs select the direction through the driver. The motor is powered through the driver rather
than directly from a GPIO pin, and the controller, sensor, and driver share a common ground.

3. ADC Sampling and Analog Input Processing

The ADC repeatedly samples the potentiometer and temperature-sensor channels. Each 12-bit
conversion produces a raw code between 0 and 4095. The corresponding input voltage is
calculated as V_in = (ADC_raw / 4095) x V_ref, where V_ref is the ADC reference voltage. The
potentiometer reading is normalized to form the requested speed command. The
temperature-channel voltage is converted to temperature using the selected sensor's offset and
sensitivity. Averaging successive readings can reduce fluctuations, giving a steadier motor
command and a readable LCD display.

4. PWM Speed Control and Motor Direction

PWM module 1, Generator 3 produces a 1 kHz signal on PF2 (M1PWM7), corresponding to a
period of 1 ms. The potentiometer-based duty command is calculated as Duty (%) = (ADC_pot /
4095) x 100. Increasing the duty cycle increases the average voltage applied through the driver
and generally increases motor speed. Direction is selected using the motor driver's direction
inputs according to the analog-input control logic. The PWM command is reduced to zero before
reversing direction, and the driver's input truth table is followed to avoid conflicting drive states.

5. Temperature-Based Control and LCD Output

The temperature reading is evaluated continuously alongside the potentiometer input. When the
sensor detects heating from a soldering iron, the temperature-control logic scales or overrides the
potentiometer-derived speed command. The final duty command is limited to the valid 0-100%
range before updating the PWM output. A defined threshold and hysteresis can prevent rapid
switching between normal and temperature-controlled operation near the transition point. The
exact temperature conversion and control thresholds depend on the sensor and the chosen
control settings. The 16x2 LCD presents the potentiometer ADC raw value, measured
temperature, and final calculated duty cycle using compact labels across its two rows. Only
changed fields are refreshed at a suitable interval to limit flicker while maintaining real-time
feedback.

6. Conclusion

The experiment combines ADC acquisition, hardware PWM generation, motor-driver direction
control, and LCD interfacing in a single embedded system. The potentiometer provides an
adjustable speed request, while the temperature sensor introduces a heat-dependent modification
of that request. The 1 kHz PWM output on PF2 controls the motor through its driver, and the LCD
makes the analog readings and applied duty cycle visible. This arrangement demonstrates how
sensed analog quantities can be converted into a continuously updated actuator command with
clear user feedback.
