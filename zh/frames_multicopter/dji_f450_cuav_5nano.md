# DJI FlameWheel 450 + CUAV V5 nano Build

This topic provides full instructions for building the kit and configuring PX4 using *QGroundControl*.

Key information

- **Frame:** DJI F450
- **Flight controller:** [CUAV V5 nano](../flight_controller/cuav_v5_nano.md)
- **Assembly time (approx.):** 90 minutes (45 minutes for frame, 45 minutes autopilot installation/configuration)

![Finished setup](../../assets/airframes/multicopter/dji_f450_cuav_5nano/f450_cuav5_nano_complete.jpg)

## Bill of materials

The components needed for this build are:
- Flight controller: [CUAV V5 nano](https://store.cuav.net/index.php?id_product=98&id_product_attribute=0&rewrite=cuav-new-pixhack-v5-nano-small-flight-controller-for-ardupilot-px4-drone-parts-free-shipping-whole-sale-&controller=product&id_lang=1):
  - GPS: [CUAV NEO V2 GPS](https://store.cuav.net/index.php?id_product=97&id_product_attribute=0&rewrite=cuav-new-ublox-neo-m8n-gps-module-with-shell-stand-holder-for-flight-controller-gps-compass-for-pixhack-v5-plus-rc-parts-px4&controller=product&id_lang=1)
  - Power Module
- Frame: [DJI F450](https://www.amazon.com/Flame-Wheel-Basic-Quadcopter-Drone/dp/B00HNMVQHY)
- Propellers: [DJI Phantom Built-in Nut Upgrade Propellers 9.4x5](https://www.masterairscrew.com/collections/all-products/products/dji-phantom-built-in-nut-upgrade-propellers-in-white-mr-9-4x5-prop-set-x4-phantom)
- Battery: [Turnigy High Capacity 5200mAh 3S 12C Lipo Pack w/XT60](https://hobbyking.com/en_us/turnigy-high-capacity-5200mah-3s-12c-multi-rotor-lipo-pack-w-xt60.html?___store=en_us)
- Telemetry: [Holibro Transceiver Telemetry Radio V3](https://shop.holybro.com/transceiver-telemetry-radio-v3_p1103.html)
- RC Receiver: [FrSky D4R-II 2.4G 4CH ACCST Telemetry Receiver](https://www.banggood.com/FrSky-D4R-II-2_4G-4CH-ACCST-Telemetry-Receiver-for-RC-Drone-FPV-Racing-p-929069.html?cur_warehouse=GWTR)
- Motors: [DJI E305 2312E Motor (960kv,CW)](https://www.amazon.com/DJI-E305-2312E-Motor-960kv/dp/B072MBMCZN)
- ESC: Hobbywing XRotor 20A APAC Brushless ESC 3-4S For RC Multicopters


In addition, we used an FrSky Taranis controller. You will also need zip ties, double-sided tape, a soldering iron.

The image below shows both frame and electronic components.

![All components used in this build](../../assets/airframes/multicopter/dji_f450_cuav_5nano/all_components.jpg)


## 硬件

### 框架

This section lists all hardware for the frame.

| 参数描述                                              | Quantity |
| ------------------------------------------------- | -------- |
| DJI F450 Bottom plate                             | 1        |
| DJI F450 Top plate                                | 1        |
| DJI F450 legs with landing gear                   | 4        |
| M3*8 screws                                       | 18       |
| M2 5*6 screws                                     | 24       |
| Velcro Battery Strap                              | 1        |
| DJI Phantom Built-in Nut Upgrade Propellers 9.4x5 | 1        |

![F450 frame components](../../assets/airframes/multicopter/dji_f450_cuav_5nano/f450_frame_components.png)


### CUAV v5 nano Package

This section lists the components in the CUAV v5 nano package.

| 参数描述                      | Quantity (Default Package) | Quantity (+GPS Package) |
| ------------------------- | -------------------------- | ----------------------- |
| V5 nano flight controller | 1                          | 1                       |
| DuPont Cable              | 2                          | 2                       |
| I2C/CAN Cable             | 2                          | 2                       |
| ADC 6.6 Cable             | 2                          | 2                       |
| SBUS Signal Cable         | 1                          | 1                       |
| IRSSI Cable               | 1                          | 1                       |
| DSM Signal Cable          | 1                          | 1                       |
| ADC 3.3 Cable             | 1                          | 1                       |
| Debug Cable               | 1                          | 1                       |
| Safety Switch Cable       | 1                          | 1                       |
| Voltage & Current Cable   | 1                          | 1                       |
| PW-Link Module Cable      | 1                          | 1                       |
| Power Module              | 1                          | 1                       |
| SanDisk 16GB Memory Card  | 1                          | 1                       |
| 12C Expansion Board       | 1                          | 1                       |
| TTL Plate                 | 1                          | 1                       |
| NEO GPS                   | -                          | 1                       |
| GPS Bracket               | -                          | 1                       |


### Electronics

| 参数描述                                                  | Quantity |
| ----------------------------------------------------- | -------- |
| CUAV V5 nano                                          | 1        |
| CUAV NEO V2 GPS                                       | 1        |
| Holibro Telemetry                                     | 1        |
| FrSky D4R-II 2.4G 4CH ACCST Telemetry Receiver        | 1        |
| DJI E305 2312E Motor (800kv,CW)                       | 4        |
| Hobbywing XRotor 20A APAC Brushless ESC               | 4        |
| Power Module(Included in the CUAV V5 nano package)    | 1        |
| Turnigy High Capacity 5200mAh 3S 12C Lipo Pack w/XT60 | 1        |


### Tools needed

The following tools are used in this assembly:

- 2.0mm Hex screwdriver
- 3mm Phillips screwdriver
- Wire cutters
- Precision tweezers
- Soldering iron


![Required tools](../../assets/airframes/multicopter/dji_f450_cuav_5nano/required_tools.jpg)

## 组装

Estimated time to assemble is approximately 90 minutes (about 45 minutes for the frame and 45 minutes installing the autopilot and configuring the airframe.

1. Attach the 4 arms to the bottom plate using the provided screws.

   ![Arms to bottom plate](../../assets/airframes/multicopter/dji_f450_cuav_5nano/1_attach_arms_bottom_plate.jpg)

1. Solder ESC (Electronic Speed Controller) to the board, positive (red) and negative (black).

   ![Solder ESCs](../../assets/airframes/multicopter/dji_f450_cuav_5nano/2_solder_esc.jpg)

1. Solder the Power Module, positive (red) and negative (black).

   ![Solder power module](../../assets/airframes/multicopter/dji_f450_cuav_5nano/3_solder_power_module.jpg)

1. Plug in the motors to the ESCs according to their positions.

   ![Plug in motors](../../assets/airframes/multicopter/dji_f450_cuav_5nano/4_plug_in_motors.jpg)

1. Attach the motors to the corresponding arms.

   ![Attach motors to arms (white)](../../assets/airframes/multicopter/dji_f450_cuav_5nano/5a_attach_motors_to_arms.jpg) ![Attach motors to arms (red)](../../assets/airframes/multicopter/dji_f450_cuav_5nano/5b_attach_motors_to_arms.jpg)

1. Add the top board (screw into the top of the legs).

   ![Add top board](../../assets/airframes/multicopter/dji_f450_cuav_5nano/6_add_top_board.jpg)

1. Add damping foam to the *CUAV V5 nano* flight controller.

   ![Damping foam](../../assets/airframes/multicopter/dji_f450_cuav_5nano/7a_attach_cuav5nano.jpg) ![Damping foam](../../assets/airframes/multicopter/dji_f450_cuav_5nano/7b_attach_cuav5nano.jpg)

1. Attach the FrSky receiver to the bottom board with double-sided tape.

   ![Attach FrSky receiver with double-sided tape](../../assets/airframes/multicopter/dji_f450_cuav_5nano/8_attach_frsky.jpg)

1. Attach the telemetry module to the vehicle’s bottom board using double-sided tape.

   ![Attach telemetry radio](../../assets/airframes/multicopter/dji_f450_cuav_5nano/9a_telemtry_radio.jpg) ![Attach telemetry radio](../../assets/airframes/multicopter/dji_f450_cuav_5nano/9b_telemtry_radio.jpg)

1. Put the aluminium standoffs on the button plate and attach GPS.

   ![Aluminium standoffs](../../assets/airframes/multicopter/dji_f450_cuav_5nano/10_aluminium_standoffs.jpg)

1. Plug in Telemetry (`TELEM1`), GPS module (`GPS/SAFETY`), RC receiver (`RC`), all 4 ESC’s (`M1-M4`), and the power module (`Power1`) into the flight controller. ![Attach peripherals to flight controller](../../assets/airframes/multicopter/dji_f450_cuav_5nano/12_fc_attach_periperhals.jpg)

:::note
The motor order is defined in the [Airframe Reference > Quadrotor x](../airframes/airframe_reference.md#quadrotor-x)
:::

设置完成！ The final build is shown below:

![Finished Setup](../../assets/airframes/multicopter/dji_f450_cuav_5nano/f450_cuav5_nano_complete.jpg)


## PX4 Configuration

*QGroundControl* is used to install the PX4 autopilot and configure/tune it for the frame. [Download and install](http://qgroundcontrol.com/downloads/) *QGroundControl* for your platform.

:::tip
Full instructions for installing and configuring PX4 can be found in [Basic Configuration](../config/README.md).
:::

First update the firmware, airframe, geometry and outputs:

- [固件](../config/firmware.md)
- [Airframe](../config/airframe.md)

:::note
You will need to select the *Generic Quadcopter* airframe (**Quadrotor x > Generic Quadcopter**).

  ![QGroundControl - Select Generic Quadcopter](../../assets/airframes/multicopter/dji_f450_cuav_5nano/qgc_airframe_generic_quadx.png)
:::

- [Actuators](../config/actuators.md)
  - Update the vehicle geometry to match the frame.
  - Assign actuator functions to outputs to match your wiring.
  - Test the configuration using the sliders.


Then perform the mandatory setup/calibration:

- [传感器方向](../config/flight_controller_orientation.md)
- [罗盘](../config/compass.md)
- [加速度计 Accelerometer](../config/accelerometer.md)
- [水平平面校准](../config/level_horizon_calibration.md)
- [无线电系统设置](../config/radio.md)
- [Flight Modes](../config/flight_mode.md)

:::note
For this build we set up modes *Stabilized*, *Altitude* and *Position* on a three-way switch on the receiver (mapped to a single channel - 5). This is the recommended minimal set of modes for beginners.
:::

Ideally you should also do:

- [电调（ESC）校准](../advanced_config/esc_calibration.md)
- [电池](../config/battery.md)
- [安全](../config/safety.md)


## 调试

Airframe selection sets *default* autopilot parameters for the frame. These may be good enough to fly with, but you should tune each frame build.

For instructions on how, start from [Autotune](../config/autotune.md).


## 视频

@[youtube](https://youtu.be/b0bKNdDqVHw)


## Acknowledgments

This build log was provided by the Dronecode Test Flight Team.
