## Wiring for LEDs
This manual does NOT describe the wiring of LEDs.  
The reason is that additional circuitry and motherboard configuration are required to turn on the LEDs, and various configurations are possible depending on the individual printer environment.  
The design is intended to allow for LED installation, but it is not required. If you think it is necessary, please devise your own wiring.  
A simplified schematic is shown below, but the question is where to wire the 3.3V from?  
<img src="img/fig0.jpg" width="50%"><br>  
This 3.3V needs to be controllable by G-Code as I want to turn it on and off in conjunction with probe rod expansion and retraction.  
From what I have seen in the BTT Octopus schematic, it looks like the only PINs that can be used for such an application are probably Fan0-Fan5 (Fan6 and 7 cannot be used), or J37 for the Neopixel LED.  
Both PINs are 5v or higher, so to obtain 3.3V, either a step-down by regulator or leveling PWM output is required.  

## Parts Printing
Layer height 0.2mm, Extrusion width 0.4mm, Wall count 4, Top/Bottom layers 8, Infill 40%.  
All parts do not require support.  
Some parts have built-in supports, so remove them.  
ServoHornA.stl and ServoHornB.stl differ in thickness by 0.2 mm. Select the one that fits.  

## Assemble
Only procedures different from the VORON standard X-Cariagge are explained.  
<img src="img/fig1.jpg" width="70%"><br>
<img src="img/fig2.jpg" width="70%"><br>
<img src="img/fig3.jpg" width="70%"><br>
<img src="img/fig4.jpg" width="70%"><br>
<img src="img/fig5.jpg" width="70%"><br>
<img src="img/fig6.jpg" width="70%"><br>
<img src="img/fig7.jpg" width="70%"><br>
<img src="img/fig8.jpg" width="70%"><br>
<img src="img/fig9.jpg" width="70%"><br>
<img src="img/fig10.jpg" width="70%"><br>
<img src="img/fig11.jpg" width="70%"><br>
<img src="img/fig12.jpg" width="70%"><br>
<img src="img/fig13.jpg" width="70%"><br>
<img src="img/fig14.jpg" width="70%"><br>
<img src="img/fig15.jpg" width="70%"><br>

## Mounting on the X-axis
<img src="img/fig16.jpg" width="70%"><br>

## Wirering
<img src="img/fig17.jpg" width="70%"><br>
<img src="img/fig18.jpg" width="70%"><br>

## Configuration (sample)
~~~
#####################################################################
#   Retractable Probe Servo
#####################################################################
[servo RetractableProbe]
pin: PB6
initial_angle: 0
maximum_servo_angle = 180
minimum_pulse_width = 0.0005
maximum_pulse_width = 0.0024

[gcode_macro PROBE_OUT]
gcode:
    SET_SERVO SERVO=RetractableProbe ANGLE=180
    G4 P800

[gcode_macro PROBE_IN]
gcode:
    SET_SERVO SERVO=RetractableProbe ANGLE=0
    SET_SERVO SERVO=RetractableProbe WIDTH=0
    G4 P800

#####################################################################
#   Probe
#####################################################################
[probe]
pin: ^PB7
x_offset: -27.49
y_offset: 13.85
z_offset: 0
speed: 10.0
samples: 3
samples_result: median
sample_retract_dist: 3.0
samples_tolerance: 0.006
samples_tolerance_retries: 3
deactivate_on_each_sample: False
activate_gcode:
    PROBE_OUT
deactivate_gcode:
    PROBE_IN
~~~
