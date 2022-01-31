# Project4_MapMyWorld

## Introduction
This is Project #4 of Udacity's Robotics Software Nanodegree program (RSND). The project's objective is to learn the RTAB-Map (Real-Time Appearance-Base Mapping) SLAM (simultaenous localization and mapping) package of ROS.

RTAB is an advanced Graph-Based SLAM technique. RTAB-Map uses computervision and sensor fusion techniques to map an environment and localize a robot. Input to the method are odometry and sensor data, RGB-D images and lidar scans. Output is a map and the robot's trajectory. RTAB-Map obtains loop-closure using a bag-of-words approach to match new images with prior saved images.

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
- re-mapping of rtopics
- launch files

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

# Results
Part of this project is to investigate how the environment affect performance of rtab-map. The robot was teleoperated clockwise two times in the bare environment shown above.

![world_rviz](</images/bare_graph_view.png>)

The above image shows that a remarkably good map has been created with only two loops. This is surprising since the environment is 'bare,' that is it only a walled in space.  The bottom line shows that nineteen (19) loop closures were obtained.

![world_rviz](</images/bare_features.png>)

The above image shows that because the environment was mostly bare there were sections of the robot's trajectory where no feature points were detected and stored. It also shows that the combination of the patterned wallls together with the edges of the interior wall created features the rtab-map method could latch onto. The image demnstrates this; the left image #id 40 registerd no features. It is not until the robot approached the edge of the left wall that feature points are registered in image #id 48. Inbetween, no feature points were registered. Below we see that finally image $id 50 generated a loop closure.

![world_rviz](</images/bare_ConVue_closure_50.png>)

The constrained image view above shows where a loop closure, one of nineteen, was obtained. Below we see the two images saved from the two loops where the loop closure match was obtained.

![world_rviz](</images/bare_features_lpclose_50.png>)

