[![Build Status](https://travis-ci.org/amc-nu/HesaiLidar_Pandar64_ros.svg?branch=master)](https://travis-ci.org/amc-nu/HesaiLidar_Pandar64_ros)

# HesaiLidar_General_ROS

### Tasks
1. [DONE] Build Hesai docker 
2. Configure Network for docker to access the lidar
3. Test ros-bridge
```bash
https://github.com/ros2/ros1_bridge
```
5. Search about GPS - f9p - ros2 package
```bash
https://github.com/shashankyld/ros2-ublox-zedf9p
```
6. Check orbslam3_docker
7. Setup generic ros1 docker - ros2 docker bridge
8. push hesai_docker to gitlab and run it on the robot


### SOME PRE-REQUISITES
1. Connect Lidar to PC
2. Install net-tools
```bash
sudo apt isntall net-tools
```
3. Check current ip-addr
```bash
ifconfig
```
4. Usually eno1 is the LAN port
```bash
# Set ip addr for eno1 to match the lidar's ip range - check lidar config webpage - receiving ip settings
sudo ifconfig eno1 131.220.233.230
```
5. Check ip of lidar from wireshark
6. Verify the change in ip addr of eno1 by check ifconfig- This keeps on changing somehow
7. Makesure the destination IP in the lidar website settings should be the local ip on your machine
8. To launch the sensor
```bash
roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="PandarXT-32" frame_id:="PandarXT-32"
```
9. Alfredo IP
```bash
131.220.233.153
```

```bash
ssh alfredo@131.220.233.153
```
10. Running Docker
```bash
docker run -v /config/:/workspace/rosworkspace/src/HesaiLidar_General_ROS/config/  -it --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix ipbalfredo/hesai-node:noetic
```


## STEPS TO GET HESAI DOCKER RUNNING
```bash
make up
```
```bash
make enter
```
```bash
# Check ROS_MASTER_URI - expected
http://localhost:11311
```
```bash
# Ensure network is host for the docker
# Do this by observing same output for ifconfig
```
```bash
# Ensure destination port is the same as the one from ifconfig
```
```bash
cd /workspace/rosworkspace/src/HesaiLidar_General_ROS/launch/config
roslaunch hesai_lidar_alfredo.launch lidar_type:="PandarXT-32" frame_id:="PandarXT-32"
```
```bash
rviz
# Dont forget to set Fixed Frame as "PandarXT-32"
```








## About the project
HesaiLidar_General_ROS project includes the ROS Driver for：  
**PandarQT/Pandar64/Pandar40P/Pandar20A/Pandar20B/Pandar40M/PandarXT**  
LiDAR sensor manufactured by Hesai Technology.  

Developed based on [HesaiLidar_General_SDK](https://github.com/HesaiTechnology/HesaiLidar_General_SDK), After launched, the project will monitor UDP packets from Lidar, parse data and publish point clouds frames into ROS under topic: ```/pandar```. It can also be used as an official demo showing how to work with HesaiLidar_General_SDK.

## Environment and Dependencies
**System environment requirement: Linux + ROS**  

　Recommanded:  
　Ubuntu 16.04 - with ROS kinetic desktop-full installed or  
　Ubuntu 18.04 - with ROS melodic desktop-full installed or
  Ubuntu 20.04 - with ROS noetic desktop-full installed 
　Check resources on http://ros.org for installation guide 
 
**Library Dependencies: libpcap-dev + libyaml-cpp-dev**  
```
$sudo apt install libpcap-dev libyaml-cpp-dev
```

## Download and Build

**Install `catkin_tools`**
```
$ sudo apt-get update
$ sudo apt-get install python-catkin-tools
```
**Download code**  
```
$ mkdir -p rosworkspace/src
$ cd rosworkspace/src
$ git clone https://github.com/HesaiTechnology/HesaiLidar_General_ROS.git --recursive
```
**Build**
```
$ cd ..
$ catkin_make -DCMAKE_BUILD_TYPE=Release
```

## Configuration 
```
 $ gedit install/share/hesai_lidar/launch/hesai_lidar.launch
```
**Reciving data from connected Lidar: config lidar ip&port, leave the pcap_file empty**

|Parameter | Default Value|
|---------|---------------|
|server_ip |192.168.1.201|
|lidar_recv_port |2368|
|gps_recv_port  |10110|
|pcap_file ||　　
|multicast_ip ||

Data source will be from connected Lidar when "pcap_file" set to empty, when "multicast_ip" configured, driver will get data packets from multicast ip address. keep "multicast_ip" empty if you are not using multicast, thus driver will monitor the "liar_recv_port" to get data.
Make sure parameters above set to the same with Lidar setting

**Reciving data from pcap file: config pcap_file and correction file path**

|Parameter | Value|
|---------|---------------|
|pcap_file |pcap file path|
|lidar_correction_file |lidar correction file path|　

Data source will be from pcap file once "pcap_file" not empty 

**Reciving data from rosbag: config data_type and publish_type,leave the pcap_file empty**

|Parameter | Value|
|---------|---------------|
|pcap_file ||
|publish_type |points|　
|data_type |rosbag|

Data source will be from rosbag when "pcap_file" is set to empty and "data_type" is set to "rosbag"
Make sure the parameter "publish_type" is set to "points"
Make sure the parameter "namespace" in file hesai_lidar.launch is same with the namespace in rosbag

## Run as independent node

1. Make sure current path in the `rosworkspace` directory
```
$ source devel/setup.bash
```
```
for PandarQT
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="PandarQT" frame_id:="PandarQT"
for Pandar64
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="Pandar64" frame_id:="Pandar64"
for Pandar20A
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="Pandar20A" frame_id:="Pandar20A"
for Pandar20B
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="Pandar20B" frame_id:="Pandar20B"
for Pandar40P
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="Pandar40P" frame_id:="Pandar40P"
for Pandar40M
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="Pandar40M" frame_id:="Pandar40M"
for PandarXT-32
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="PandarXT-32" frame_id:="PandarXT-32"
for PandarXT-16
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="PandarXT-16" frame_id:="PandarXT-16"
for PandarXTM
$ roslaunch hesai_lidar hesai_lidar.launch lidar_type:="PandarXTM" frame_id:="PandarXTM"
```
2. The driver will publish PointCloud messages to the topic `/pandar`  
3. Open Rviz and add display by topic  
4. Change fixed frame to frame_id to view published point clouds  

## Run as nodelet

1. Make sure current path in the `rosworkspace` directory
```
$ source devel/setup.bash
```
```
for PandarQT
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="PandarQT" frame_id:="PandarQT"
for Pandar64
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="Pandar64" frame_id:="Pandar64"
for Pandar20A
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="Pandar20A" frame_id:="Pandar20A"
for Pandar20B
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="Pandar20B" frame_id:="Pandar20B"
for Pandar40P
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="Pandar40P" frame_id:="Pandar40P"
for Pandar40M
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="Pandar40M" frame_id:="Pandar40M"
for PandarXT-32
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="PandarXT-32" frame_id:="PandarXT-32"
for PandarXT-16
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="PandarXT-16" frame_id:="PandarXT-16"
for PandarXTM
$ roslaunch hesai_lidar cloud_nodelet.launch lidar_type:="PandarXTM" frame_id:="PandarXTM"
```
2. The driver will publish PointCloud messages to the topic `/pandar`  
3. Open Rviz and add display by topic  
4. Change fixed frame to frame_id to view published point clouds  

## Details of launch file parameters and utilities
|Parameter | Default Value|
|---------|---------------|
|pcap_file|Path of the pcap file, once not empty, driver will get data from pcap file instead of a connected Lidar|
|server_ip|The IP address of connected Lidar, will be used to get calibration file|
|lidar_recv_port|The destination port of Lidar, driver will monitor this port to get point clouds packets from Lidar|
|gps_port|The destination port for Lidar GPS packets, driver will monitor this port to get GPS packets from Lidar|
|start_angle|Driver will publish one frame point clouds data when azimuth angle step over start_angle, make sure set to within FOV|
|lidar_type|Lidar module type|
|frame_id|frame id of published messages|
|pcldata_type|0:mixed point clouds data type  1:structured point clouds data type|
|publish_type|default "points":publish point clouds "raw":publish raw UDP packets "both":publish point clouds and UDP packets|
|timestamp_type|default "": use timestamp from Lidar "realtime" use timestamp from the system  driver running on|
|data_type|default "":driver will get point clouds packets from Lidar or PCAP "rosbag":driver will subscribe topic /pandar_packets to get point clouds packets|
|namespace|namesapce of the launching node|
|lidar_correction_file|Path of calibration file, will be used when not able to get calibration file from a connected Liar|
|multicast_ip|The multicast IP address of connected Lidar, will be used to get udp packets from multicast ip address|
|coordinate_correction_flag|default "false":Disable coordinate correction "true":Enable coordinate correction|

