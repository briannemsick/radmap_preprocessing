The following tools do the preprocessing of the RadMAP system for cartographer: converting velodyne_msgs/VelodyneScan --> sensor_msgs/PointCloud2, merging {sensor_msgs/PointCloud2,sensor_msgs/Imu,sensor_msgs/NavSatFix} bags, and aligning 2 velodynes. 

The bagfile names are assumed to be {port,starboard,imu}.bag with the appropriate topic names {/novatel_fix,/novatel_imu,/velodyne_packets_port, /velodyne_packets_starboard}.

Requirements:
https://github.com/ros-drivers/velodyne

# Merging the bags
1. Place port.bag, starboard.bag, imu.bag into [folder]
2. cd [folder]
3. roslaunch radmap_preprocess lasers.launch 
4. rosrun  rosrun radmap_preprocess lasers 
5. python [path]/src/bag_merge_radmap.py

# Vizualize aligned scans
1. roslaunch radmap_preprocess calibrated_tf.launch
2. rosbag play [bagfile]
3. rviz

# Run cartographer
1. roslaunch cartographer_ros demo_radmap.launch bag_filename:=[bagfile path]
2. rosservice call /finish_trajectory [name]
3. cd ~/.ros -> [name].ply, [name]_xray_{X,Y,Z}.png

# Coordinate frame calibration
1. velodyne_calib.m
2. fill in calibrated_tf.launch
3. roslaunch radmap_preprocess calibrated_tf.launch
4. rosrun tf tf_echo imu_link velodyne_port --> radmap.lau -velodyne_port_joint - origin
5. rosrun tf tf_echo imu_link velodyne_starboard --> radmap.lau - velodyne_starboard_joint - origin
