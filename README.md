# ur_ik_plugin

UR inverse kinematics plugin for ROS MoveIt. Core ik solver code is from the official `ur_kinematics` package and the structure is generated by `ikfast`.

#### Motivation

The official `ur_kinematics` package only supports one ik solution in its MoveIt api.

The UR ik solver generated by `ikfast` is slow (over 1000us on average), and often returns no solution if the 6th axis of UR is vertical.

This plugin combines the above two solutions. We replaced the ik solver code of `ikfast` with functions from the official `ur_kinematics` package. Now this plugin can output all the 8 solutions of UR within 25us via MoveIt, which are important features in motion planning tasks.

#### Usage

1. Clone this repo to the `src` folder of your ROS workspace. Usually you should also have the `universal_robot` package in the same worksapce.

2. Run   `catkin_make` to build the project, Then `source devel/setup.bash` to source the project.
   
3. Set the kinematics_solver option in `config/kinematics.yaml` of your `ur*_moveit_config` package to the following lines to enable the plugin.

```
ur_ik_plugin/UR3KinematicsPlugin
ur_ik_plugin/UR5KinematicsPlugin
ur_ik_plugin/UR10KinematicsPlugin
```

example:

edit `ur3_moveit_config/config/kinematics.yaml`:

```yaml
manipulator:
  kinematics_solver: ur_ik_plugin/UR3KinematicsPlugin 
  kinematics_solver_attempts: 3
  kinematics_solver_search_resolution: 0.005
  kinematics_solver_timeout: 0.005
```
4. Enjoy! You can run `roslaunch ur3_moveit_config demo.launch`,  drag the end effector and try motion planning to test if it works correctly. 
