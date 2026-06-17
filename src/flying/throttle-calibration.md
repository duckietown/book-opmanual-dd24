```{needget}
*   A drone that arms and whose motors spin in the correct direction
*   Safety goggles
---
*   A throttle midpoint calibrated to your drone so it hovers instead of climbing uncontrollably
```

(throttle-calibration)=
# Throttle Calibration

If your drone climbs uncontrollably on takeoff and triggers a `Safety Failure: too high` disarm,
the throttle midpoint needs calibration. No two drones hover at exactly the same throttle value.
Motor efficiency, propellers weight, and battery charge all affect it, so the default midpoint (`1600`)
might be too high. Without tuning it to your build, the drone will shoot upward faster
than the controller can correct.


## Finding and setting your hover throttle

1.  SSH into your drone and open `scripts/pid_class.py`:

    ```bash
    nano ~/catkin_ws/src/pidrone_pkg/scripts/pid_class.py
    ```

2.  Go to **line 173**. You will see:

    ```python
    cmd_t = self.throttle.step(error.z, time_elapsed)
    ```

    Add a hardcoded test value on the line directly below:

    ```python
    cmd_t = 1400  # test value
    ```

    Save and exit with `Ctrl+S`, then `Ctrl+X`.

3.  Restart the FC and PID nodes (`` `1 `` and `` `2 ``), arm the drone (`;`), and press `t` to takeoff.

    ```{warning}
    Keep your hand near `spacebar` to disarm the drone immediately if needed.
    ```

    Observe what the drone does:

    *   If it **climbs**, the value is too high. Lower it by `25`.
    *   If it **sinks or does not lift**, the value is too low. Raise it by `25`.

    Repeat until the drone hovers. For the DD24 with Kv 2300 motors, the hover throttle
    typically falls in the **1450–1475** range.

4.  Once you have found the hover throttle, note the value and remove the hardcoded line you
    added in step 2. The PID controller will take over throttle again.

5.  Now go to **line 81** in `pid_class.py`. You will see:

    ```python
    d_range=(-40, 40), midpoint=1600)
    ```

    Set `midpoint` to your hover throttle value. For example, if you found `1473`:

    ```python
    d_range=(-40, 40), midpoint=1473)
    ```

    Save and exit with `Ctrl+S`, then `Ctrl+X`.

6.  Open `scripts/pid_controller.py`:

    ```bash
    nano ~/catkin_ws/src/pidrone_pkg/scripts/pid_controller.py
    ```

    Go to **line 310** and update the takeoff altitude setpoint:

    ```python
    self.desired_z = 0.2  # was 0.05
    ```

    Save and exit with `Ctrl+S`, then `Ctrl+X`.

7.  Restart the nodes and fly. The drone should lift off and settle at roughly 20 cm. Some
    oscillation are normal and indicate the need for further PID controller tuning.