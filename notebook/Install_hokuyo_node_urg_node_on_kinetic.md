## How to install hokuyo_node / urg_node on kinectic
* make directory.
```
$ md ~/catkin_ws/src
$ cd ~/catkin_ws/src
```
* clone repos.
```
$ git clone https://github.com/ros-perception/laser_proc.git
$ git clone https://github.com/ros-drivers/urg_c.git
$ git clone https://github.com/ros-drivers/driver_common.git
$ git clone https://github.com/ros-drivers/urg_node.git
```
* run hokuyo_node.
```
$ roslaunch urg_node urg_lidar.launch
```
*references*
- https://github.com/ros-perception/laser_proc
- https://github.com/ros-drivers/urg_c
- https://github.com/ros-drivers/driver_common
- https://github.com/ros-drivers/urg_node
- http://answers.ros.org/question/243232/how-to-install-hokuyo_nodeurg_node-on-kinetic/