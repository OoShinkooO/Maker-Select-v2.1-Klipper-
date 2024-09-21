# Maker-Select-v2.1-Klipper-
A Very Basic Configuration Setup for a Maker Select v2.1 running Klipper


Changed Specs
 - Logic Board: SKR 3 EZ,
 - Drivers: ez2209 Drivers
 - Z-Axis Configured: Dual Z

  Contains my Configuration

## Cura General Configuration Settings

### Printer Settings

Start G-code:

    START_PRINT BED_TEMP={material_bed_temperature_layer_0} EXTRUDER_TEMP={material_print_temperature_layer_0}

End G-code:

    END_PRINT

### PLA Settings

Extruder Temperature: 210  
Bed Temperature: 65  

### Profile Settings

Clone Draft (.2mm)  
Change Print Speed:  60mm/s  
Retraction Distance: .75mm  
Retraction Speed: 40mm/s


## Initial Start Process

One thing to keep in mind, this is a general guideline process that I follow.  It's probably not without faults as I am new, so please consult with the Klipper Documentation.

### Temperature Calibration

#### Temperature Calibration Description

Do this first in order to ensure you don't either blow out a mosfet, or possibly have something catch on fire.

Make sure you Calibrate it for the target temperature.

#### Process

1. Perform PID Tune (Extruder)  
2. Perform PID Tune (Bed)  
3. Save Settings  

        PID_CALIBRATE HEATER=extruder TARGET=210
        PID_CALIBRATE HEATER=heater_bed TARGET=65
        SAVE_CONFIG

### End-Stop Calibration

#### End-Stop Calibration Description

#### End-Stop Calibration Process

1. Open Klipper UI
2. Go to Machine Settings (it should have /config in the url)
3. Click on the Endstops, they should all be open.
4. Hold down X and refresh.
    - If it says "Triggered" you're good.  OTherwise verify Wiring.
5. Hold down Y and refresh.
    - If it says "Triggered" you're good.  OTherwise verify Wiring.

#### BLTouch Calibration

1. Open Klipper UI
2. Launch command and verify the triggering Pin moves down

        BLTOUCH_DEBUG COMMAND=pin_down

3. Launch the next set of commands and verify the output is "probe: open"

        BLTOUCH_DEBUG COMMAND=touch_mode
        QUERY_PROBE

4. Push and hold the probe pin up and then run the following command to ensure it says "probe: TRIGGERED"

        QUERY_PROBE

5. After this, run the following command to put pin back to original state:

        BLTOUCH_DEBUG COMMAND=pin_up

6. Next Attempt to Home the printer by running the following command.  Trigger the Z-Probe by hand to ensure it's working before triggering by bed as a safety percaution.

        G28

### Stepper Motor Calibration

#### Stepper Motor Calibration Description

Do this to make sure the stepper motor moves in the right direction, and the enable pin is configured properly.  Additionally, it's important to ensure both Z (Left Stepper Motor with fron of printer facing you) and Z1 (Right Stepper Motor with front of Printer Facing you) are orientated properly.  Otherwise Gantry leveling may not work.

- If Homing worked on previous Step, you only need to ensure z and z1 are working properly.

#### Stepper Motor Calibration Process

1. Stepper Motor Buzz (X Stepper Motor)
2. Stepper Motor BUzz (y Stepper Motor)
3. Stepper Motor Buzz (z stepper Motor)
4. Stepper Motor Buzz (z1 Stepper Motor)  

### Generate Mesh

#### Generate Mesh Description

#### Generate Mesh Process

1. Clear Bed Mesh and Z-Offset
2. Generate Mesh
3. Save Settings

    BED_MESH_CLEAR  
    BED_MESH_CALIBRATE  
    SAVE_CONFIG  

### Calculate Z-Offset

#### Calculate Z-Offset Description

While this process is not really specific, I basically adjusted the Z-offset for what I initially had.  I used Feeler gauages to know how much I was off by. Larger number means extruder is much lower than probe trigger.  It's the amount in millimeters the z has to go lower.  To big a number and the Nozzle will crash into the bed.  Save this number under the following section:

> [bltouch]  
> ...  
> ...  
> z_offset: 1.784

#### Calculate Z-Offset Process

1. Home Printer
2. Go to the location the Z-Offset was derived
3. Go to Z0 and use a feeler gauge to determine the depth.
4. Add that depth to value in z_offset:

        G28
        G0 X100 Y100
        G0 Z2
        G0 Z0  

HAPPY Printing
