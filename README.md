# 7Dwarfs

Following instructions have been tested on ROS Version **melodic** installed as `ros-desktop-full-install` on Ubuntu 18.04.4(bionic) on the laptop + raspberry pi where the raspberry pi was Raspberry Pi 4 Model B.
## Setup

### Install packages:

#### Standard ROS Packages
* move-base
* map-server

#### CHARISMA packages 

**On raspberry pi:**
* https://github.com/charisma-lab/neato_controller (7dwarves branch)
* https://github.com/charisma-lab/neato_planner (master branch)
* https://github.com/KIT-MRT/neato_robot  (hydro-devel branch) (Used specifically for the Neato D5 Connected model -- our lab has the Neato D80/85 models)

**On laptop:**
* https://github.com/charisma-lab/neato_localization (master branch)

#### How to install?
* `git clone` them in `catkin_ws/src`
* From `catkin_ws` directory , run `catkin_make` followed by `catkin_make install`
    
### Print and place aruco markers:

Using [https://chev.me/arucogen/](https://chev.me/arucogen/) print Aruco Markers, 1, 5 and `obstacle` of size 4x4 and size at least 200. 
Place the aruco markers - `1` on the robot, `5` on the hat (the object which will go on person) and `obstacle` to mark one corner of the area which the robot will be operating in.

### Setting up the environment
On the laptop, as well as the raspberry pi make sure you have the following bash variables set-up:

* NEATO_NAME
* ROS_MASTER_URI
* NEATO_PORT

Also, make sure that the laptop's hostname and chairbot's hostname are added to each others `/etc/hosts` file.
Basically, on the chairbot raspberry pi, you should have in the `/etc/hosts` an entry like:

```
#ip-address hostname-of-laptop 
192.168.1.71 ubuntu-Lenovo-YOGA-720-13IKB
```

## Running the system:

You will need multiple terminals.Make sure not to confuse the laptop's terminals with the ones where you are running a SSH session with the raspberry pi


0. Edit `catkin_ws/src/neato_localization/scripts/tracking_aruco_markers.py` to change camera number, on the laptop.
1. In first terminal, run `roslaunch neato_localization neato_localization.launch`. This is to start localization package. Using the overhead camera it will start the localizer and publish poses and orientation of all the markers it sees, on the laptop.
1. In first terminal, run `roslaunch neato_controller bringup.launch`. This is to start neato robot's low level driver, on the raspberry pi.
1. In second terminal, run `rosrun neato_localization gen_behavior_trajectory.py`. This is to generate path for a particular emotion, you'll see a prompt for entering the emotion condition, on the raspberry pi. 
1. In second terminal, run `roslaunch neato_planner pure_pursuit.launch`. The path generated from gen_trajectory_behavior, will now be fetched by this node and then implemented by the robot, on the raspberry pi.
1. In a third terminal, run `rviz` This is just for visualization. Once rviz starts...*instructions to be added*

### 

## Troubleshooting: 
* Make sure the laptop and the raspberry pi  are connected to the same network router.
* Make sure that you can run `rostopic echo <topicname>` FROM the raspberry pi as well as the laptop and it gives you output. Try `/neato01/pose` as the topic once neato_localization is succesfully running.
* If the steps on laptop aren't working: make sure the camera is plugged into the laptop, check to make sure that the aruco markers on the ground are not blocked, and make sure that neato01 and neato05 (hat) are in the frame so the markers are being picked up by the camera
* If the step on the raspberry pi are not working: make sure the raspberry pi is turned on, plugged into the neato, and has a powersource / make sure the neato is charged

