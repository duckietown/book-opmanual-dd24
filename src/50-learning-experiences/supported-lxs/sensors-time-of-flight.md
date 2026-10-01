(lx-dd-sensor-tof)=
# LX: Sensors - Time-of-Flight (ToF)

```{seo}
:description: Learn about time-of-flight sensors with hands-on activities with physical and virtual Duckiedrones.
:keywords: Duckietown, Duckiedrone, DD24-B, LX, learning experience, time-of-flight, ToF, VL53L1X, range sensor, ROS2
```

```{needget}
- Computer setup `dts`: [](dd24-initial-setup)
- (recommended) A successful Duckiematrix installation: [Duckiematrix first steps](https://docs.duckietown.com/ente/duckietown-manual/50-duckiematrix/getting-started/duckiematrix-first-steps.html)
- (optional) A "flight ready" Duckiedrone: [](flying_your_drone)
---
- Running the Sensors - ToF learning experience
```

This Learning Experience introduces time-of-flight (ToF) sensors, and in particular the VL53L1X model equipped on the Duckiedrone (DD24-B). You will learn how a ToF sensor turns reflected light into a distance and when it cannot, how the Duckiedrone's ToF readings flow from the sensor to a ROS 2 topic, and how to write a ROS 2 node that reads sensor_msgs/Range messages from all five ToF sensors.

```{figure} ../../_images/lxs/tof/time-of-flight.svg
:alt: Light leaving a time-of-flight sensor, reflecting off a target and returning
:width: 60%
:name: duckiedrone-lx-sensor-tof
:align: center

The principle of time-of-flight ranging: light leaves the sensor, reflects off the target, and returns, so the time it takes covers the distance twice ([source](https://en.wikipedia.org/wiki/Time-of-flight_camera#/media/File:20200501_Time_of_flight.svg)).
```

```{admonition} Intended Learning Outcomes
:class: tip

After completing this learning experience, learners will be able to:
- Explain how a time-of-flight sensor recovers a distance, and predict how its maximum range changes with the surface it is pointed at, the ambient light, and its timing budget.
- Trace a range reading from the sensor chip to a ROS 2 topic, naming what each step changes and what sets its rate.
- Read a `sensor_msgs/Range` message from a Duckiedrone and tell a usable distance apart from a reading to discard.
- Write, deploy, and run a ROS 2 node on a Duckiedrone that subscribes to one ToF topic, then to all five.
```

```{admonition} Available repositories
:class: seealso

- [ToF LX - Learning Experience](https://github.com/duckietown/lx-dd-sensors-tof)
- [ToF LX - Recipe](https://github.com/duckietown/lx-dd-sensors-tof-recipe)
- [ToF LX - Solution](https://github.com/duckietown/lx-dd-sensors-tof-solution)

Access to the solution repository is reserved to Duckietown instructors. Reach out to [info@duckietown.com](mailto:info@duckietown.com) or [upgrade your plan](https://hub.duckietown.com/plans/?plan=institutional) through the Duckietown Hub. 
```

{{ dt_workspace_matrix_lx_warning.format(dt_workspace_note_prefix) }}

## About these learning activities

```{note}
This learning experience runs on a virtual Duckiedrone in [the Duckiematrix](https://docs.duckietown.com/ente/duckietown-manual/50-duckiematrix/getting-started/duckiematrix-first-steps.html) and on a [physical Duckiedrone](https://get.duckietown.com/products/autonomous-raspberrypi-quadcopter-duckiedrone-dd24).
```

## Notebooks

Each notebook includes hands-on learning activities and a checkpoint to self-assess understanding. Work through the notebooks in order, since each one builds on the previous ones.

| Notebook | Topics |
| --- | --- |
| 1 | What a time-of-flight sensor measures, how the VL53L1X measures it, and when it cannot |
| 2 | The path from the VL53L1X to `/<robot>/<sensor>_tof_driver_node/range`, and where its settings live |
| 3 | Writing, deploying, and running a ROS 2 node that reads a ToF topic |
| 4 | Starting the other four sensors and reading all five with the same node |

(lx-forking-dd-sensor-tof)=
## Forking the Repository

The recommended way to use the repository of an LX is to make a fork, and then clone that fork. Forking can be done through the GitHub web interface, and creates a personal copy that can still be synchronized with the upstream Duckietown code.

Cloning the repository directly also works, at the cost of not being able to push personal changes.

1. **Create a fork**: navigate to [the `lx-dd-sensors-tof` repository](https://github.com/duckietown/lx-dd-sensors-tof).

    Find and press the "Fork" button on the top right:

    ```{figure} ../../_images/lxs/tof/lx-dd-sensors-tof-forking.png
    :alt: how to fork a Duckietown LX repository
    :width: 90%
    :name: dd-lx-forking-sensor-tof
    :align: center

    Fork the LX to be able to make local changes while still being able to receive updates.
    ```

    This creates a new repository at `<your_github_username>/lx-dd-sensors-tof`.

2. **Clone the fork**: clone the fork on the computer, replacing the GitHub username in the command below, and navigate to the new folder:

        git clone git@github.com:<your_github_username>/lx-dd-sensors-tof
        cd lx-dd-sensors-tof

3. **Configure upstream repo**: configure the Duckietown version of this repository as the upstream repository to synchronize with the fork.

    List the current remote repository for the fork,

        git remote -v

    Specify a new remote upstream repository,

        git remote add upstream https://github.com/duckietown/lx-dd-sensors-tof

    Confirm that the new upstream repository was added to the list,

        git remote -v

    Work can now be pushed to the personal repository using the standard GitHub workflow, and the beginning of every exercise prompts a pull from the upstream repository, updating the exercises to the latest version.

(lx-system-update-dd-sensor-tof)=
## Keeping the System Up To Date

- 💻 These instructions are for `ente` learning experiences. Ensure that the Duckietown Shell is set to an `ente` profile (and not a `daffy` one). The current profile is shown by:

    ```bash
    dts profile list
    ```

    To switch to an `ente` profile, follow [](dd24-initial-setup).

- 💻 Pull from the upstream remote to synchronize the fork with the upstream repo:

    ```bash
    git pull upstream ente
    ```

- 💻 Make sure the Duckietown Shell is updated to the latest version:

    ```bash
    pipx upgrade duckietown-shell
    ```

- 💻 Update the shell commands:

    ```bash
    dts update
    ```

- 💻 Update the computer and the Duckiedrone: follow [](dd24-software-update).

(lx-ssl-setup-dd-sensor-tof)=
## SSL Certificate Setup

```{note}
This procedure is only needed the first time an LX is run on a new computer.
```

Duckietown uses SSL certificates and TLS encryption to guarantee the highest standard of safety and privacy. Set up a local SSL certificate needed to run the LX editor inside the browser:

```bash
sudo apt install libnss3-tools
dts setup mkcert
```

```{note}
If a [Duckietown Workspace](https://docs.duckietown.com/ente/duckietown-manual/10-setup/00-computer/setup-duckietown-workspace.html) is in use, install `mkcert` on the host system by following the workspace setup instructions instead of running these commands inside the dev container.
```

(lx-code-editor-dd-sensor-tof)=
## Launching the Code Editor

```{important}
All `dts code` commands must be executed inside the root directory of the learning experience.
```

From inside the directory of the learning experience, open the code editor by running:

```bash
dts code editor
```

Wait for a URL to appear on the terminal, then click on it or copy-paste it in the address bar of the browser to access the code editor. The first thing shown in the code editor is a version of these instructions. The indications shown in the code editor take precedence over this page.

(lx-navigating-notebooks-dd-sensor-tof)=
## Walkthrough of Notebooks

Inside the code editor, use the navigator sidebar on the left-hand side to navigate to the `notebooks` directory and open the first notebook.

Follow the instructions on the notebook and work through them in sequence.

In many cases the last notebook instructs the learner to write some code inside the learning experience directory. That code needs to be **built** before **testing** it.

(lx-matrix-testing-dd-sensor-tof)=
### Testing with the Duckiematrix

Testing code in the Duckiematrix requires a virtual Duckiedrone attached to an ongoing session.

(lx-create-vdrone-dd-sensor-tof)=
#### 1. Creating and starting a virtual Duckiedrone

If this has not been done already (e.g., for a different LX), create a virtual Duckiedrone with the command:

```bash
dts duckiebot virtual create --type duckiedrone --configuration DD24 [VDRONE]
```

where `[VDRONE]` is the hostname. It can be any name, subject to the [same naming constraints of physical Duckiedrones](dd24-hostname-constraints).

When the command runs, DTS prompts for a password for the virtual Duckiedrone's `duckie` account, and asks to confirm it. The password must contain at least eight characters and cannot contain colons or line breaks. The typed characters are not displayed.

Then start the virtual robot with the command:

```bash
dts duckiebot virtual start [VDRONE]
```

The virtual Duckiedrone appears with status `Booting` and finally `Ready` in the output of `dts fleet discover`:

```
         | Hardware |    Type     | Model |  Status  |    Address
-------- | -------- | ----------- | ----- | -------- | --------------
[VDRONE] |  virtual | duckiedrone | DD24  |  Ready   | [VDRONE].local
```

```{note}
The Duckiedrone software stack runs on ROS 2. When running ROS 2 commands against a virtual or physical Duckiedrone from the computer, the `ROS_DOMAIN_ID` must match the one used by the robot.
```

(lx-start-matrix-dd-sensor-tof)=
#### 2. Starting the Duckiematrix with the virtual Duckiedrone

Once the virtual robot is ready, start the Duckiedrone simulation environment:

```bash
dts matrix run --standalone --map sandbox_drone --embedded
```

where `--standalone` starts both the Duckiematrix Engine and the Renderer in the same environment, and `--embedded` loads `sandbox_drone`, the Duckiedrone map shipped with the Duckiematrix.

```{note}
This command must be run from a local terminal and not from inside a Duckietown Workspace.
```

The Unity-based Duckiematrix simulator starts up in the `sandbox_drone` map, which contains a single Duckiedrone. For more details about using the Duckiematrix see the [Duckiematrix manual](https://docs.duckietown.com/ente/duckietown-manual/50-duckiematrix/introduction-to-the-duckiematrix-virtual-environment.html).

```{figure} ../../_images/lxs/duckiematrix-drone-sandbox.png
:alt: the Duckiedrone in the Duckiematrix sandbox map
:width: 80%
:name: dd-lx-sensor-tof-matrix-sandbox
:align: center

The Duckiedrone in the Duckiematrix sandbox map used by this learning experience.
```

To activate the Duckiematrix window, click anywhere on it and press <kbd>ENTER</kbd>. Press <kbd>ESC</kbd> at any time to disengage the mouse.

While the window is active, the point of view is moved with the <kbd>W</kbd>, <kbd>A</kbd>, <kbd>S</kbd>, and <kbd>D</kbd> keys, and the viewing angle with the mouse. All available keyboard commands are summarized in the "Settings" tab at the bottom left of the Duckiematrix window.

(lx-attach-vdrone-dd-sensor-tof)=
#### 3. Attaching the virtual Duckiedrone to the Duckiematrix

The virtual Duckiedrone and the Duckiematrix run as separate entities, and are connected with:

```bash
dts matrix attach [VDRONE] map_0/vehicle_0
```

where `[VDRONE]` is the name of the virtual Duckiedrone, and `map_0/vehicle_0` is the name of the Duckiedrone entity in the `sandbox_drone` map. Once attached, the sensor data produced by the Duckiematrix reaches the robot, and the commands produced by the robot move the Duckiedrone in the simulation.

(lx-stop-list-vdrone-dd-sensor-tof)=
#### Other useful virtual Duckiedrone commands

To disconnect the virtual Duckiedrone from the Duckiematrix without stopping it:

```bash
dts matrix detach [VDRONE]
```

Once the work session is over, stop the virtual robot:

```bash
dts duckiebot virtual stop [VDRONE]
```

The status of all virtual robots can be checked at any time with:

```bash
dts duckiebot virtual list
```

(lx-code-build-dd-sensor-tof)=
### Building the Code

From inside the learning experience root directory, build the code with:

```bash
dts code build -R [ROBOT_NAME]
```

where `[ROBOT_NAME]` is the name of either a physical or a virtual Duckiedrone.

This command packages the LX code and its dependencies into the image that will run on that Duckiedrone. The image is prepared for the selected robot, but the build command does not deploy it.

(lx-code-test-dd-sensor-tof)=
### Deploying the code on a (physical or virtual) Duckiedrone

To test the code on the Duckiedrone:

```bash
dts code workbench -R [ROBOT_NAME]
```

This command deploys the image built in the previous step to the specified Duckiedrone and starts the workbench, so that the code can be tested on the robot.

While the workbench is running, an interactive shell can be attached to its container from a second terminal:

```bash
dts code workbench -R [ROBOT_NAME] --shell
```

The shell can be used to inspect the running environment and execute commands inside the container.

After every change to the learning experience code, stop the workbench, then run `dts code build -R [ROBOT_NAME]` and `dts code workbench -R [ROBOT_NAME]` again to ship the new changes.

(lx-troubleshooting-dd-sensor-tof)=
## Troubleshooting

For more detailed instructions, refer to [](dd24-lx-general-procedure). For help troubleshooting common problems, refer to [its troubleshooting section](dd24-lx-troubleshooting).
