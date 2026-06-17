(environment_setup)=
# Environment setup


## Development container setup

To make it possible for you to develop your own code on the drone, you need to set up a docker workspace from the source code of the repository `pidrone_pkg`.

1.  Run `ssh duckie@[yourdrone].local` from your base station to open an ssh terminal into your drone. All the following commands will have to be input in this shell. The password is `quackquack`.

1. Setup the duckietown stack by executing the following command:

    ```
    docker-compose -f /data/autoboot/duckiedrone.yaml rm && dt-autoboot
    ```

1. Update the containers to the latest versions by executing the following commands one at a time:

    ```
    docker stop dtps ros1-tof driver-tof && docker rm dtps ros1-tof driver-tof
    ```

1. Clone the repository to your Duckiedrone's SD card and switch to the branch `dd24`:

    ```shell
    mkdir -p catkin_ws/src && cd catkin_ws/src
    git clone https://github.com/duckietown/pidrone_pkg -b dd24 && cd pidrone_pkg
    ```

    ```{note}
    The next step will take a long time because it has to download all the dependencies to build the image.  Make sure your Raspberry Pi is plugged into an external USB power supply.
    ```
    
1.  Finally build the Docker image needed to run the software (which is back on an older version of ROS, ROS Kinetic).

    ```
    rake build && rake create
    ```

1.  You also need to download the `raspicam_node` package to be installed later:

    ```shell
    cd ~/catkin_ws/src/
    git clone https://github.com/duckietown/raspicam_node -b kinetic-dd24
    git clone https://github.com/okalachev/vl53l1x_ros.git
    ```

1.  You can start the container and go inside it by running, from the `pidrone_pkg` directory:

    ```shell
    cd ~/catkin_ws/src/pidrone_pkg
    rake start
    ```

1.  Once in the container, you need to install the `pidrone_pkg` and `raspicam_node` ROS packages. To do this execute:

    ```shell
    cd ~/catkin_ws
    catkin_make && catkin_make install
    ```

1.  Now that all the packages are installed, to access the workspace you will use to control the drone, run:
    ```shell
    cd ~/catkin_ws/src/pidrone_pkg
    screen -c pi.screenrc
    ```

This will start a screen session with each of the ROS nodes needed to
run the drone and make it fly. 

```{note}
Anytime you'll want to interact with the drone software you will first need to start the container by running `rake start` from the `~/catkin_ws/src/pidrone_pkg` directory and then, inside the container, running `screen -c pi.screenrc`.
``` 
