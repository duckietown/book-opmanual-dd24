```{seo}
:description: Configure and verify the motor spinning direction of your Duckiedrone (DD24) using QGroundControl to ensure correct flight behavior.
:keywords: Duckiedrone motor configuration, DD24 motor direction, QGroundControl motor setup, SpeedyBee F405 v3, BetaflightX frame, ESC motor reversal
```

(dd24-motor-configuration)=
# Configure motor spinning direction

Now you have to set up the motor spinning direction on the flight controller in QGroundControl.

```{admonition} What you will need
:class: note

* A base station computer
* QGroundControl installed on your computer.
* Flight Controller with correct firmware and parameters.
* Your drone's motors and ESCs connected properly to the flight controller.
```


```{admonition} What you will get

* A Flight Controller configured for flight.
```

## 1. Connect Flight Controller to QGroundControl

- Connect your SpeedyBee F405 v3 flight controller to your computer using a USB cable.

- Open QGroundControl.

- Follow the instructions in the [flight controller initialization](qgroundcontrol-connection) to connect to the flight controller from your laptop.


## 2. Check Motor Outputs
```{caution}
Remove propellers for safety before proceeding with motor testing.
```

- Go to the `Motors` tab in QGroundControl. This section will allow you to test motor outputs individually.

    ```{figure} ../_images/fc-setup/motor_setup_tab.png
    Motor testing tab
    ```

- Test each motor by clicking on the individual motor numbers (1, 2, 3, 4). Ensure each motor spins in the correct direction according to the BetaflightX frame diagram:
    
    - Motor 1 (B): `Clockwise (CW)`
    - Motor 2 (A): `Counterclockwise (CCW)`
    - Motor 3 (C): `Clockwise (CW)`
    - Motor 4 (D): `Counterclockwise (CCW)`

    ```{figure} ../_images/fc-setup/reference_spinning_direction.png
    :name: betaflightx-spinning-dir

    BetaflightX frame type spinning directions
    ```

    ```{important}
    Notice that there is a different naming scheme between the motor numbering and the motor test tab:

        - The motor test tab uses **letters** (A, B, C, D)
        - The motor configuration parameters use numbers (1,2,3,4)
    ```

## 3. Reverse Motor Direction (if required)
If any motor is spinning in the wrong direction, you'll need to reverse its direction:
   
- Go to the `Parameters` tab.
- In the search bar, type `SERVO_BLH_RVMASK`.
- Click on it and select the motor (here the configurator refers to `Channels`, so motor `i` will correspond to `Channeli`) to reverse based on the [required direction](betaflightx-spinning-dir).

    ```{figure} ../_images/fc-setup/esc_reverse_direction.png
    :name: motor-reverse-configurator

    Configuration tab of the `SERVO_BLH_RVMASK` to reverse individual motors.
    ```

- Click on `Save` and, when prompted, reboot the flight controller.

## 4. Verify Motor Spin Direction
- After making any changes, retest each motor through the `Motors` tab in QGroundControl to ensure all motors are spinning in the correct direction.
- Make sure the motor directions match the BetaflightX frame configuration.

## 5. Save
- Once all motors are spinning in the correct direction, save the settings in QGroundControl.

This completes the setup of the flight controller.
