# ORB_SLAM3_ROS2
This repository is a modified from([ORB_SLAM3_ROS2](https://github.com/zang09/ORB_SLAM3_ROS2)).

---

## Prerequisites
- Environment
  - Ubuntu 22.04
  - ROS2 Humble
  - OpenCV 4.5.4

- Build ORB_SLAM3
  - Go to this [ORB-SLAM3-STEREO-FIXED](https://github.com/cadkimbj/ORB-SLAM3-STEREO-FIXED) and follow build instruction.

- Install related ROS2 packages
```
$ sudo apt install ros-$ROS_DISTRO-vision-opencv && sudo apt install ros-$ROS_DISTRO-message-filters
```

- Update ROS2 packages
```
$ sudo apt update
$ sudo apt upgrade
```

## How to build
1. Clone repository to your ROS workspace
```
$ mkdir -p colcon_ws/src
$ cd ~/colcon_ws/src
$ git clone https://github.com/cadkimbj/ORB_SLAM3_ROS2.git orbslam3_ros2
```

2. Change this [line](https://github.com/cadkimbj/ORB_SLAM3_ROS2/blob/d5e6d4977aced7961f50073b7af665e7ac5ec5e2/CMakeLists.txt#L5) to your own `python site-packages` local path

3. Change this [line](https://github.com/cadkimbj/ORB_SLAM3_ROS2/blob/d55e7a65c6c6fd5ebc7429fd1b99dbe8850da8f2/CMakeModules/FindORB_SLAM3.cmake#L8) to your own `ORB_SLAM3` local path

4. build!
```
$ cd ~/colcon_ws
$ colcon build --symlink-install --packages-select orbslam3
```

5. Unzip ORBvoc.txt.tar.gz in `colcon_ws/src/orbslam3_ros2/vocabulary`

## Troubleshootings(Optional)
1. If you cannot find `sophus/se3.hpp`:  
Go to your `ORB_SLAM3_ROOT_DIR` and install sophus library.
```
$ cd ~/{ORB_SLAM3_ROOT_DIR}/Thirdparty/Sophus/build
$ sudo make install
```
2. Please compile with `OpenCV 4.5.4` version.

## How to use
1. Source the workspace  
```
$ source ~/colcon_ws/install/local_setup.bash
```

2. Run orbslam mode, which you want.  
This repository only support `MONO, STEREO, RGBD, STEREO-INERTIAL` mode now.  
You can find vocabulary file and config file in here. (e.g. `orbslam3_ros2/vocabulary/ORBvoc.txt`, `orbslam3_ros2/config/monocular/TUM1.yaml` for monocular SLAM).
  - `MONO` mode  
```
$ ros2 run orbslam3 mono PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE --ros-args -r camera:=/scene_camera/image
```
  - `STEREO` mode  
```
$ ros2 run orbslam3 stereo PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE BOOL_RECTIFY
```
  - `RGBD` mode  
```
$ ros2 run orbslam3 rgbd PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE --ros-args -r /camera/rgb:=/scene_camera/image -r /camera/depth:=/depth_camera/image
```
  - `STEREO-INERTIAL` mode  
```
$ ros2 run orbslam3 stereo-inertial PATH_TO_VOCABULARY PATH_TO_YAML_CONFIG_FILE BOOL_RECTIFY [BOOL_EQUALIZE]
```
