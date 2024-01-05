marlin 2.1.2 octopus powerloss psu smart sensor bltouch firmware 

printer.cfg voor klipper tronxy x5sa 2ec330x330  vind u hier onder.gebruikte run out sensoren btt smart sensor 2.0 opgelet lees u eigen mcu nummer uit en schrijf de huidige in de printer cfg hier mee over 


klipper printer cfg. 

include fluidd.cfg]
[include mainsail.cfg]
# This file contains common pin mappings for the BigTreeTech Octopus.
# To use this config, the firmware should be compiled for the
# STM32F446 with a "32KiB bootloader" and a "12MHz crystal" clock reference.

# See docs/Config_Reference.md for a description of parameters.

[virtual_sdcard]
path: /home/gijs/printer_data/gcodes

[display_status]

[pause_resume]

[gcode_macro CANCEL_PRINT]
description: Cancel the actual running print
rename_existing: CANCEL_PRINT_BASE
gcode:
  TURN_OFF_HEATERS
  CANCEL_PRINT_BASE

[duplicate_pin_override]
pins: PA2, PF4

# Driver0
[stepper_x]
step_pin: PF13
dir_pin: PF12
enable_pin: !PF14
microsteps: 16
rotation_distance: 80
endstop_pin: !PG6
position_endstop: 0
position_max: 330
homing_speed: 50

# Driver1
[stepper_y]
step_pin: PG0
dir_pin: PG1
enable_pin: !PF15
microsteps: 16
rotation_distance: 80
endstop_pin: !PG9
position_endstop: 0
position_max: 330
homing_speed: 50

# Driver2
[stepper_z]
step_pin: PF11
dir_pin: !PG3
enable_pin: !PG5
microsteps: 16
rotation_distance: 16
endstop_pin: probe:z_virtual_endstop
position_min: -15
position_max: 395


# Driver3
# The Octopus only has 4 heater outputs which leaves an extra stepper
# This can be used for a second Z stepper, dual_carriage, extruder co-stepper,
# or other accesory such as an MMU
[stepper_z1]
step_pin: PG4
dir_pin: !PC1
enable_pin: !PA0
microsteps: 16
rotation_distance: 16



#...

########################
#      BL Touch        #  
########################

# A [probe] section can be defined instead with a pin: setting identical
# to the sensor_pin: for a bltouch
[bltouch]
control_pin: PB6
sensor_pin: PE9
x_offset: -43
y_offset: -6
#z_offset: 3.0
## uncomment these to speed up testing, though some BLTouchs wont work reliably (i.e. mine)
# stow_on_each_sample: false
# probe_with_touch_mode: true

## Inductive Probe, comment out if using BLTouch
# [probe]
# pin: ~!PC5
# x_offset: -40
# y_offset: -7
# # z_offset: 3.71
# z_offset: 4.54
# speed: 10.0
# samples: 3
# samples_result: median
# # sample_retract_dist: 3.0
# samples_tolerance: 0.01
# samples_tolerance_retries: 3

[safe_z_home]
home_xy_position: 205,165
speed: 150
z_hop: 10
z_hop_speed: 10

# [bed_screws]
# screw1: 5,5
# screw2: 165,5
# screw3: 325,5
# screw4: 5,325
# screw5: 165,325
# screw6: 325,325

[screws_tilt_adjust]
speed: 150
screw1: 45,12
screw2: 210,12
screw3: 350,12
screw4: 45,330
screw5: 210,330
screw6: 350,330

[bed_mesh]
speed: 150
probe_count: 5,5
mesh_min : 20,20
mesh_max : 300,300
# horizontal_move_z: 6
algorithm: lagrange

########################
#      Z Tilt          #  
########################

[z_tilt]
z_positions:-90,165
  420,165
points: 90,165
  327,165
speed: 200
horizontal_move_z: 10
retries: 5
retry_tolerance: .005


########################
#  Mesh Bed Leveling   #  
########################

[bed_mesh]
speed: 200
horizontal_move_z: 6
mesh_min: 45, 45
mesh_max: 280, 280
probe_count: 5, 5

###########################
#  SCREWS_TILT_CALCULATE  #  
###########################

[screws_tilt_adjust]
screw1: 90,45
screw1_name: left front
screw2: 90,285
screw2_name: left back
screw3: 330,285
screw3_name: right back
screw4: 330,45
screw4_name: right front
speed: 100
horizontal_move_z: 5
screw_thread: CCW-M3


########################
#      Extruder        #  
########################

# Driver4 (Extruder 0 / T0)
[extruder]
step_pin: PF9
dir_pin: PF10
enable_pin: !PG2
full_steps_per_rotation: 200
microsteps: 16
rotation_distance: 14.797544011113  
nozzle_diameter: 0.400
filament_diameter: 1.750
heater_pin: PA2 # HE0
sensor_pin:  PF4 # T0
sensor_type: EPCOS 100K B57560G104F
control: pid
pid_Kp: 16.165
pid_Ki: 0.623
pid_Kd: 104.872
min_temp: 0
max_temp: 260
min_extrude_temp: 180
max_extrude_only_distance: 150
pressure_advance: 0.60
pressure_advance_smooth_time: 0.080

[filament_switch_sensor switch_sensor]
switch_pin: gpio_PG13
pause_on_runout: False
runout_gcode:
  PAUSE # [pause_resume] is required in printer.cfg
  M117 Filament switch runout
insert_gcode:
  M117 Filament switch inserted

[filament_motion_sensor encoder_sensor]
switch_pin: gpio_12
detection_length: 2.88
extruder: extruder
pause_on_runout: False
runout_gcode:
  PAUSE # [pause_resume] is required in printer.cfg
  M117 Filament encoder runout
insert_gcode:
  M117 Filament encoder inserted

# Driver5 (Extruder 1 / T1)
[extruder1]
step_pin: PC13
dir_pin: PF0
enable_pin: !PF1
full_steps_per_rotation: 200
microsteps: 16
rotation_distance: 15.377839854686
nozzle_diameter: 0.400
filament_diameter: 1.750
heater_pin: PA2 # HE0
sensor_pin:  PF4 # T0
sensor_type: EPCOS 100K B57560G104F
control: pid
pid_Kp: 16.165
pid_Ki: 0.623
pid_Kd: 104.872
min_temp: 0
max_temp: 260
min_extrude_temp: 180
max_extrude_only_distance: 150
pressure_advance: 0.60
pressure_advance_smooth_time: 0.080

[filament_switch_sensor switch_sensor]
switch_pin: gpio_PG13
pause_on_runout: False
runout_gcode:
  PAUSE # [pause_resume] is required in printer.cfg
  M117 Filament switch runout
insert_gcode:
  M117 Filament switch inserted

[filament_motion_sensor encoder_sensor]
switch_pin: gpio_14
detection_length: 2.88
extruder: extruder
pause_on_runout: False
runout_gcode:
  PAUSE # [pause_resume] is required in printer.cfg
  M117 Filament encoder runout
insert_gcode:
  M117 Filament encoder inserted



[gcode_macro T0]
gcode:
SYNC_EXTRUDER_MOTION EXTRUDER="extruder" MOTION_QUEUE="extruder"
SYNC_EXTRUDER_MOTION EXTRUDER="extruder1" MOTION_QUEUE=""
#SYNC_EXTRUDER_MOTION EXTRUDER="extruder2" MOTION_QUEUE=""
#SYNC_EXTRUDER_MOTION EXTRUDER="extruder3" MOTION_QUEUE=""
#SYNC_EXTRUDER_MOTION EXTRUDER="extruder4" MOTION_QUEUE=""

[gcode_macro T1]
gcode:
SYNC_EXTRUDER_MOTION EXTRUDER="extruder" MOTION_QUEUE=""
SYNC_EXTRUDER_MOTION EXTRUDER="extruder1" MOTION_QUEUE="extruder"
#SYNC_EXTRUDER_MOTION EXTRUDER="extruder2" MOTION_QUEUE=""
#SYNC_EXTRUDER_MOTION EXTRUDER="extruder3" MOTION_QUEUE=""
#SYNC_EXTRUDER_MOTION EXTRUDER="extruder4" MOTION_QUEUE=""


[delayed_gcode STARTUP_GCODE]
initial_duration: 0.1
gcode:
{% set svv = printer.save_variables.variables %}
{% if svv.active_tool != "" %}
{ svv.active_tool }
{% else %}
T0
{% endif %}
{ action_respond_info("Active Tool: " + svv.active_tool) }
set_pressure_advance extruder=extruder advance=0.60 smooth_time=0.08
set_pressure_advance extruder=extruder1 advance=0.60 smooth_time=0.08
#set_pressure_advance extruder=extruder2 advance=0.60 smooth_time=0.08
#set_pressure_advance extruder=extruder3 advance=0.60 smooth_time=0.08
#set_pressure_advance extruder=extruder4 advance=0.60 smooth_time=0.08


# Driver6
#[extruder2]
#step_pin: PE2
#dir_pin: PE3
#enable_pin: !PD4
#heater_pin: PB10 # HE2
#sensor_pin: PF6 # T2
#...

#[filament_switch_sensor material_2]
#switch_pin: PG14

# Driver7
#[extruder3]
#step_pin: PE6
#dir_pin: PA14
#enable_pin: !PE0
#heater_pin: PB11 # HE3
#sensor_pin: PF7 # T3
#...

#[filament_switch_sensor material_3]
#switch_pin: PG15

[heater_bed]
heater_pin: PA1
sensor_pin: PF3 # TB
sensor_type: ATC Semitec 104GT-2
#control: watermark
control: pid
pid_Kp: 72.003
pid_Ki: 1.616
pid_Kd: 801.935
min_temp: 0
max_temp: 130


[fan]
pin: PA8

[heater_fan fan1]
pin: PE5

[heater_fan fan2]
pin: PD12

#[heater_fan fan3]
#pin: PD13

#[heater_fan fan4]
#pin: PD14

#[controller_fan fan5]
#pin: PD15

[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32f446xx_47005E000451373330333137-if00
# CAN bus is also available on this board

[printer]
kinematics: corexy
max_velocity: 300
max_accel: 3000
max_z_velocity: 5
max_z_accel: 100

[heater_bed]
heater_pin: PA1
sensor_pin: PF3 # TB
sensor_type: ATC Semitec 104GT-2
#control: watermark
#control: pid

min_temp: 0
max_temp: 130

[fan]
pin: PA8

[heater_fan fan1]
pin: PE5

[heater_fan fan2]
pin: PD12

#[heater_fan fan3]
#pin: PD13

#[heater_fan fan4]
#pin: PD14

#[controller_fan fan5]
#pin: PD15

[mcu]
serial: /dev/serial/by-id/usb-Klipper_stm32f446xx_200046001450535556323420-if00
# CAN bus is also available on this board

[printer]
kinematics: corexy
max_velocity: 300
max_accel: 3000
max_z_velocity: 5
max_z_accel: 100

########################################
# TMC2209 configuration
########################################
[tmc2209 stepper_x]
uart_pin: PC4
diag_pin: PG6
run_current: 0.800


[tmc2209 stepper_y]
uart_pin: PD11
diag_pin: PG9
run_current: 0.800


[tmc2209 stepper_z]
uart_pin: PC6
diag_pin: PG10
run_current: 0.650
stealthchop_threshold: 999999


[tmc2209 stepper_z1]
uart_pin: PC7
diag_pin: PG11
run_current: 0.650
stealthchop_threshold: 999999

[tmc2209 extruder]
uart_pin: PF2
run_current: 0.800
stealthchop_threshold: 999999

[tmc2209 extruder1]
uart_pin: PE4
run_current: 0.800
stealthchop_threshold: 999999

#[tmc2209 extruder2]
#uart_pin: PE1
#run_current: 0.800
#stealthchop_threshold: 999999

#[tmc2209 extruder3]
#uart_pin: PD3
#run_current: 0.800
#stealthchop_threshold: 999999

########################################
# TMC2130 configuration
########################################

#[tmc2130 stepper_x]
#cs_pin: PC4
#spi_bus: spi1
##diag1_pin: PG6
#run_current: 0.800
#stealthchop_threshold: 999999

#[tmc2130 stepper_y]
#cs_pin: PD11
#spi_bus: spi1
##diag1_pin: PG9
#run_current: 0.800
#stealthchop_threshold: 999999

#[tmc2130 stepper_z]
#cs_pin: PC6
#spi_bus: spi1
##diag1_pin: PG10
#run_current: 0.650
#stealthchop_threshold: 999999

#[tmc2130 stepper_]
#cs_pin: PC7
#spi_bus: spi1
##diag1_pin: PG11
#run_current: 0.800
#stealthchop_threshold: 999999

#[tmc2130 extruder]
#cs_pin: PF2
#spi_bus: spi1
#run_current: 0.800
#stealthchop_threshold: 999999

#[tmc2130 extruder1]
#cs_pin: PE4
#spi_bus: spi1
#run_current: 0.800
#stealthchop_threshold: 999999

#[tmc2130 extruder2]
#cs_pin: PE1
#spi_bus: spi1
#run_current: 0.800
#stealthchop_threshold: 999999

#[tmc2130 extruder3]
#cs_pin: PD3
#spi_bus: spi1
#run_current: 0.800
#stealthchop_threshold: 999999

[board_pins]
aliases:
    # EXP1 header
    EXP1_1=PE8, EXP1_2=PE7,
    EXP1_3=PE9, EXP1_4=PE10,
    EXP1_5=PE12, EXP1_6=PE13,    # Slot in the socket on this side
    EXP1_7=PE14, EXP1_8=PE15,
    EXP1_9=<GND>, EXP1_10=<5V>,

    # EXP2 header
    EXP2_1=PA6, EXP2_2=PA5,
    EXP2_3=PB1, EXP2_4=PA4,
    EXP2_5=PB2, EXP2_6=PA7,      # Slot in the socket on this side
    EXP2_7=PC15, EXP2_8=<RST>,
    EXP2_9=<GND>, EXP2_10=PC5

# See the sample-lcd.cfg file for definitions of common LCD displays.



#[neopixel my_neopixel]
#pin: PB0

#*# <---------------------- SAVE_CONFIG ---------------------->
#*# DO NOT EDIT THIS BLOCK OR BELOW. The contents are auto-generated.
#*#
#*# [bed_mesh default]
#*# version = 1
#*# points =
#*# 0.050000, 0.025000, 0.020000, 0.020000, 0.035000
#*# -0.005000, -0.070000, -0.120000, -0.060000, -0.080000
#*# -0.045000, -0.100000, -0.090000, -0.085000, -0.105000
#*# -0.145000, -0.185000, -0.195000, -0.200000, -0.185000
#*# -0.035000, -0.115000, -0.120000, -0.100000, -0.040000
#*# tension = 0.2
#*# min_x = 45.0
#*# algo = lagrange
#*# y_count = 5
#*# mesh_y_pps = 2
#*# min_y = 45.0
#*# x_count = 5
#*# max_y = 280.0
#*# mesh_x_pps = 2
#*# max_x = 280.0
#*#
#*# [extruder]
#*#
#*# [heater_bed]
#*#
#*# [bltouch]
#*# z_offset = 2.080
#*#
#*# [extruder1]
#*#
