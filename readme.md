
# `nuscenes2bag`

Convert `nuscenes` dataset to `rosbag` package

![](rviz/1.png)

## 1 Download and Compile

~~~python
# 1. Create a ROS workspace
mkdir -p nus2bag_ws/src

# 2. Clone the source code
cd nus2bag_ws/src
git clone https://github.com/linClubs/nuscenes2rosbag.git

# 3. Install package dependencies
cd ..
rosdep install -r -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO

# 4. Build
catkin_make
~~~


## 2 Converting the 'mini' dataset:

1. Download the mini dataset

[Download Link](https://link.csdn.net/?target=https%3A%2F%2Fwww.nuscenes.org%2Fnuscenes%23download)

Registration and login are required to download.

Download the **`Map expansion-v1.3`** and **`Full dataset(v1.0)-mini`** versions.

After downloading, extract the files and copy the three files from `nuScenes-map-expansion-v1.3` to the `v1.0-mini/map` directory. Finally, rename the new `v1.0-mini` folder to `nuscenes` to obtain the following directory structure for the dataset:

~~~python
nuscenes/
    ├── maps
        ├── 36092f0b03a857c6a3403e25b4b7aab3.png
        ├── 37819e65e09e5547b8a3ceaefba56bb2.png
        ├── 53992ee3023e5494b90c316c183be829.png
        ├── 93406b464a165eaba6d9de76ca09f5da.png
        ├── basemap
        ├── expansion
        └── prediction
    ├── samples
    ├── sweeps
    └── v1.0-mini
~~~


2. Convert the `mini` dataset scenes to rosbag

~~~python
rosrun nuscenes2bag nuscenes2bag --scene_number 0061 --dataroot data/nuscenes/ --out nuscenes_bags/
~~~

+ --scene_number    Scene number (0061 or 0103)
+ --dataroot        Data path
+ --out             Rosbag save path
+ --version         Optional parameter: `v1.0-mini` (default) or `v1.0-trainval`
+ --jobs            Number of scenes to process simultaneously (default: 4)

+ All scenes in `mini` dataset
~~~python
103.bag   1094.bag  553.bag  655.bag  796.bag
1077.bag  1100.bag  61.bag   757.bag  916.bag
~~~

4. Convert all `mini` dataset scenes

~~~python
rosrun nuscenes2bag nuscenes2bag --dataroot /home/lin/code/maptr2/MapTR/data/nuscenes/ --out nuscenes_bags/ --jobs 4
~~~

5. Visualization
~~~python
# 1. Play the dataset
rosbag play 103.bag -l

# 2. Open the visualization window
roslaunch nuscenes2bag view.launch
~~~

![](rviz/2.png)