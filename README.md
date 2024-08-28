# pc_utm_to_mgrs_converter

A ROS 2 package for converting UTM point cloud maps to MGRS maps in Ubuntu 20.04 with ROS 2 Galactic.

## Environment Requirements

```bash
sudo apt update
sudo apt-get install libgeographic-dev
sudo geographiclib-get-geoids egm2008-1  # Already installed if Autoware environment is set up
```

## Common Error and Solution

If you encounter the following error:
```
Could not find a package configuration file provided by "GeographicLib"
with any of the following names:
GeographicLibConfig.cmake
geographiclib-config.cmake
```

Solution:
```bash
sudo ln -s /usr/share/cmake/geographiclib/FindGeographicLib.cmake /usr/share/cmake-3.16/Modules/
```
Note: Please check the cmake version in `/usr/share/` directory and modify the command accordingly.

## Installation

```bash
mkdir -p ~/pc_utm_to_mgrs_converter/src
cd ~/pc_utm_to_mgrs_converter/src
git clone https://github.com/silly-h/FP_pc_utm_to_mgrs_converter.git
cd ~/pc_utm_to_mgrs_converter
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

## Description

This package is designed for converting UTM point cloud maps to MGRS maps in Ubuntu 20.04 with ROS 2 Galactic environment.

## Usage

For detailed configuration, please refer to:
https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/integrating-autoware/creating-maps/converting-utm-to-mgrs-map/

To run the converter:
```bash
ros2 launch pc_utm_to_mgrs_converter pc_utm_to_mgrs_converter.launch.xml
```
