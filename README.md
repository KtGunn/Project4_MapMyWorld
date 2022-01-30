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