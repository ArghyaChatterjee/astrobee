# Astrobee ROS Guest Science Demo

<div align="center">
  <a href="https://www.youtube.com/watch?v=HtXU6vCQlZI">
    <img src="media/astrobee.mp4" alt="Astrobee Navigation" width=600">
  </a>
</div>

This package contains two simple examples to interface custom ROS nodes for autonomous control: one using Python - `nodes/python_ros_node_template.py` - and one using C++ - `nodes/cpp_ros_node_template.py`. Both nodes have associated launch files under the `launch/` directory.

## Requirements
This work has been tested on Ubuntu 18.04, ROS Melodic and Python 3.8.

## Installation
To install this package, make sure that you follow the [Astrobee Installation Guide](https://nasa.github.io/astrobee/html/md_INSTALL.html) for building the code natively. 

Assuming your catkin workspace is set at `~/catkin_ws`, then
```
cd ~/catkin_ws
git clone https://github.com/ArghyaChatterjee/astrobee_ros.git src/astrobee_ros
catkin build
source devel/setup.bash
```
If the package `python-catkin-tools` is not installed, replace `catkin build` with `catkin_make`.

## Run an Example
Once the installation is done, a simple example can be run with either the `cpp_template_interface.launch` or `python_template_interface.launch`. To this end, make sure that you have the most up-to-date version of the Astrobee Simulator. 

1. Start the simulator with
```
roslaunch astrobee_ros_demo astrobee_sim.launch
```
2. Launch the template interface, for instance
```
roslaunch astrobee_ros_demo python_template_interface.launch
```
3. Make sure that Honey is not in a faulty state by overriding its state with
```
rostopic pub /honey/mgt/sys_monitor/state ff_msgs/FaultState '{state: 0}'
```
4. At this point, the template node should show "Sleeping..." as a ROS Info message. This means that the node is waiting ot be started. To start the node, call the starting service with
```
rosservice call /honey/start "data: true"
```

At this point, the template node output should show "Elapsed time: 0" as a ROS Info message, since there is no control input being generated.

## Interface your own control algorithm
This package serves as a template for those looking to use the Astrobee for autonomous operations. Your controllers can be included directly on the templated code, by modifying the variables [self.u_traj](https://github.com/ArghyaChatterjee/astrobee_ros/blob/main/nodes/python_ros_node_template.py#L298) in Python or [control_input_](https://github.com/ArghyaChatterjee/astrobee_ros/blob/main/nodes/cpp_ros_node_template.cpp#L238) in C++. This can be accomplished either by 
1. Subscribing to another node that runs your control method that modifies these variables directly, or 
2. Modifying the template to include your control method in the class itself.
