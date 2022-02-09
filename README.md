# Project4_MapMyWorld

## Introduction

This is Project #4 of Udacity's Robotics Software Nanodegree program (RSND). The project's objective is to learn the RTAB-Map (Real-Time Appearance-Base Mapping) SLAM (simultaenous localization and mapping) package of ROS.

RTAB is an advanced Graph-Based SLAM technique. RTAB-Map uses computervision and sensor fusion techniques to map an environment and localize a robot. Input to the method are odometry and sensor data, RGB-D images and lidar scans. Output is a map and the robot's trajectory. RTAB-Map obtains loop-closure using a bag-of-words approach to match new images with prior saved images.

![world_rviz](</images/mapping_run.gif>)

## Specific Objectives

A few criteria are to be met to complete this project:

- develop a ROS package to interface with the ROS RTAB-Map package;
- add a RGB-D camera to a robot created in previous project;
- ensure the simulation environment has features suitable for RTAB-Map; 
- create launch files to bring up all packages, publish and re-map topics as needed;
- use the ROS teleop package to move the robot during mapping operation;
- demonstrate successul mapping.


# Preparation

This section presents how the project was structured and prepared to meet required objectives. Topics include:

- the simulation environment created in Gazebo;
- augmentation of the robot with an RGB-D camera;
- messages and topics

## Simulation environment

A simulation environment was specifically created for RTAB-Map. RTAB-Map requires a feature rich environment. This implies a good variety in patterns and structure of visual imagery. The environment shown below uses a varied pattern on walls and internal walls which provide edges and break up the camera's view.

![world_rviz](</images/basic_environment.png>)

## Robot

A simple robot was created using two powered wheels. This robot has been used in prior projects. The modification was to add a link and joint to represent an RGB-D or depth camera. An RGB camera already existed on the front of the robot and the RGB and Depth cameras are co-located. The difference between the two is that the depth camera uses a different view frame from the rgb camera. In the depth frame sensing is in the z-axis direction, i.e. forward, y-axis points down and x-axis points right. This frame is shown in the image below. RGB uses a regular x-axis forward convention for sensing.

![world_rviz](</images/small_bot_in_gazebo.png>)

![world_rviz](</images/small_bot_links_rviz.png>)

## Messages and topics

The screenshot belows shows the robot in an empty world facing a cylindrical object. Another screenshot shows corresponding the rgb image, lidar scan and depth cloud displayed in RViz. Finally, a screenshot of rqt_graph shows the connection of topics between modules.

![world_rviz](</images/rob_cyl_gazebo.png>)

![world_rviz](</images/cyl_image_scan_rviz.png>)

![world_rviz](</images/rqt_graph_scan_depth.png>)

rqt_graph shows that the robot in gazebo is publishing on /scan, /camera/depth and /camera/rgb. These are the default topics. In retrospect and if this was part of a larger project, my_robot should publish in its own namespace and remap for rtab-map, but this works.

# Install & run

Here are instructions for downloading and installing the project and lauching it. The application was developed in Linux Ubuntu version 16.04 with ROS distribution Kinetic. The application depends on a few packages that must be present: navigation, move_base and rtabmap_ros. These can be installed using,

> sudo apt install ros-kinetic-<name_of_required_package>

The repository should be cloned

> git clone https://github.com/KtGunn/Project4_MapMyWorld.git

The user must navigate to the catkin_ws sub-directory. In case the two directories 'devel' and 'setup' are present, they should be removed, followed by the compilation command, catkin_make

> rm -rf devel setup && catkin_make

To bring up the application the following commands are issued, each in its own console,

> roslaunch my_robot <a_launch_file>

As explained in the following section there are numerous nodes that must be up and running. 'world.laun

## Launch files

Several launch files were prepared. Three separate files are normally invoked when running this application: Gazebo+Rviz brings up the simulation environment and data visualizer, teleoperator to move the robot my keyboard commands, rtabmap for mapping or map source for localization.

- **world.launch**

> This launch file is normally brought up first and it start Gazebo and RViz. It will bring up the full environmen (called 'busy_world') but the original world catn be commented out and the full world commented to bring up the bare envirionment.

- **my_robot_gazebo.launch**

> This file is called by world.launch and brings the robot up in Gazebo along with nodes to publish the robot's state.

- **mapping.launch**

> This file brings up rtabmap which performs the mapping.

- **localization.move_base.launch**

> This file is used for localization (optional) after mapping has been done. It also brings up move_base, which does the actual navigating of the robot. In this mode, rtabmap's main purpose is to supply the map to move_base.

Another important node is needed i.e. teleop_twist_keyboard. This node is brought up using rosrun.

# Results

Part of this project is to investigate how the environment affects performance of rtab-map. Two scenarios of using the same environment were investigated. First the bare-bones environment was mapped, second the same environment with numerous objects installed was mapped.

## Minimal environment

The robot was teleoperated clockwise two times in the bare environment (see 'Gazebo' image above). Results are presented in the 'Graph view' image below.

![world_rviz](</images/bare_graph_view.png>)

The 'Graph view' image shows that a remarkably good map was created with only two loops. This is surprising since the environment is 'bare,' that is it only a walled in space.  The bottom line shows that nineteen (19) loop closures were obtained.

![world_rviz](</images/bare_features.png>)

Because the environment was mostly bare there were sections of the robot's trajectory where no feature points were detected and stored (see image above). Also, the combination of the patterned wallls together with the edges of the interior wall created features the rtab-map method could latch onto. The image demonstrates this; the left image #id 40 registerd no features. It is not until the robot approached the edge of the left wall that feature points are registered in image #id 48. Inbetween, no feature points were registered. Below we see that finally image #id 50 generated a loop closure.

![world_rviz](</images/bare_ConVue_closure_50.png>)

The constrained image view above shows where a loop closure, one of nineteen, was obtained. Below we see the two images saved from the two loops where the loop closure match was obtained.

![world_rviz](</images/bare_features_lpclose_50.png>)

Finally, we have the 3D map constructed from the mapping operation. We notice the holes in the map where more features are needed to fill them in. Alternatively, a longer mapping run can create a fuller map.

![world_rviz](</images/bare_3dmap.png>)

## Occupied environment

Numerous objects were installed in the simulation environment as shown below. The idea is to give the rtabmap algorithm a feature rich environment to sense and thus obtain an accurate map quickly.

![world_rviz](</images/complete_2sides_gazebo_endofmapping.png>)

A mapping run of two teleoperated figure-eight patterns was done.

![world_rviz](</images/gravue_bosides_2fig8_40LS.png>)

The image above shows the map that results and that forty (40) loop closures were found. The resulting 3d map is below.

![world_rviz](</images/3d_bosides_2fig8.png>)

To obtain a full reconstruction of the space, more than the two mapping loops are needed. It turns out this space is challenging to map. The alternating patterns of brick, tiles and wood panels in straight vertical sections provides many opportunities for erroneous loop closures. The patterns are feature rich but the features repeat at various locations in the space. Of course rtabmap-databaseViewer can be used to reject incorrect loop closures and insert others that the algorithm missed.

# Localization

A minmal test of localization with the generated rtab map was performed. The move_base package was added to the system in a localization launch file that also set the parameters of rtab-map to operate in localization mode. This means that rtab-map supplies a map to the move_base package. Below is a view of rqt_graph with the needed nodes and packages.

![world_rviz](</images/rqt_graph_localization.png>)

Finally, below is a short video clip of the robot moving to a 2d navigation goal.

![world_rviz](</images/localization.gif>)
