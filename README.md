# 7Dwarfs

## Setup

### Install packages:

**On raspberry pi:**
* https://github.com/charisma-lab/neato_controller 
* https://github.com/KIT-MRT/neato_robot

**On laptop:**
* https://github.com/charisma-lab/neato_localization 

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

## Running the system:

### On the laptop

You will need three terminals.

1. In first terminal, run `roslaunch neato_localization neato_localization`. This is to start localization package. Using the overhead camera it will start the localizer and publish poses and orientation of all the markers it sees.
1. In second terminal, run `rosrun neato_localization gen_behavior_trajectory.py`. This is to generate path for a particular emotion, you'll see a prompt for entering the emotion condition. 
3. In a third terminal, run `rviz`. This is just for visualization. Once rviz starts...*instructions to be added*

### On the raspberry pi

You will need two terminals again. Make sure not to confuse the laptop's terminals with the ones where you are running a SSH session with the raspberry pi

1. In first terminal, run `roslaunch neato_controller bringup.launch`. This is to start neato robot's low level driver.
2. In second terminal, run `roslaunch neato_planner pure_pursuit.launch`. The path generated from gen_trajectory_behavior, will now be fetched by this node and then implemented by the robot.


## Troubleshooting: 
* Make sure the laptop and the raspberry pi  are connected to the same network router.
* If the steps on laptop aren't working: make sure the camera is plugged into the laptop, check to make sure that the aruco markers on the ground are not blocked, and make sure that neato01 and neato05 (hat) are in the frame so the markers are being picked up by the camera
* If the step on the raspberry pi are not working: make sure the raspberry pi is turned on, plugged into the neato, and has a powersource / make sure the neato is charged

