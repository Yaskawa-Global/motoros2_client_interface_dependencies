# motoros2_client_interface_dependencies

[![Github Issues](https://img.shields.io/github/issues/yaskawa-global/motoros2_client_interface_dependencies.svg)](http://github.com/yaskawa-global/motoros2_client_interface_dependencies/issues)

[![license - apache 2.0](https://img.shields.io/:license-Apache%202.0-yellowgreen.svg)](https://opensource.org/licenses/Apache-2.0)

[![support level: vendor](https://img.shields.io/badge/support%20level-vendor-brightgreen.svg)](http://rosindustrial.org/news/2016/10/7/better-supporting-a-growing-ros-industrial-software-platform)

![Version: 0.1.1](https://img.shields.io/badge/version-0.1.1-informational.svg)

## Overview

This package exists to facilitate installation of the ROS 2 packages containing message, service and action definitions used by client applications to communicate with a [MotoROS2](https://github.com/yaskawa-global/motoros2) server running on a Yaskawa Motoman industrial robot controller.

This package is an empty package and does not provide any interface definitions itself, nor does it host any code.
Its sole task is to help `rosdep` install the MotoROS2 client dependencies on a (companion) PC.

## Supported MotoROS2 versions

Version `0.1.1` of this package is compatible with version `0.2.0` of MotoROS2.

## Supported ROS 2 versions

This package supports ROS 2 Foxy, Galactic, Humble, and Jazzy.
It is expected to also build successfully on other versions of ROS 2, but has only been tested on the previously mentioned releases.

Please also take the compatibility of MotoROS2 with various ROS 2 releases into account.
This information can be found in the [General Requirements](https://github.com/Yaskawa-Global/motoros2/blob/main/README.md#general-requirements) section of the main readme of MotoROS2 itself.

## Build and installation

If necessary, create a Colcon / ROS 2 workspace and place a copy of the repository in the `src` space of that workspace (use `git clone` or download a `.zip` or `.tar.gz` from Github).

### Linux

The following instructions show an *example* workflow which would add `motoros2_client_interface_dependencies` to an existing Colcon workspace at `$HOME/colcon_ws`, install the dependencies and finally build the workspace for ROS 2 Jazzy.

This procedure assumes that base ROS 2 packages and development tools have been installed and sourced from the command line.
Please refer to [Installation / Ubuntu (Debian packages)](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debians.html) for details on how to install ROS 2 Jazzy on Ubuntu.
If you are using a different distribution of Linux, or a different version of ROS 2, make sure to refer to the appropriate documentation for that combination.

Update commands where necessary if a different ROS 2 version should be used or the Colcon workspace is in a different location.

If you run into errors (such as `Command [...] not found`), please ensure the `ros-dev-tools` package has been properly installed.

As follows:

```bash
# change to the root of the Colcon workspace
cd $HOME/colcon_ws

# retrieve the latest development version of this package
# Note: if you wish to use a version other than the latest, you will need to
# specify a tag/commit other than "master" in the following command.
git clone \
  -b master \
  https://github.com/yaskawa-global/motoros2_client_interface_dependencies.git \
  src/motoros2_client_interface_dependencies

# these need to be checked out from their repositories as they haven't
# been released yet
vcs import \
  --input src/motoros2_client_interface_dependencies/source_deps.repos \
  src/

# check build dependencies. First update databases
sudo apt update
source /opt/ros/jazzy/setup.bash
rosdep update --rosdistro=$ROS_DISTRO

# now install all dependencies.
# Note: this may install additional packages, depending on the software
# already installed on the machine.
# Note2: this should not print any warnings or errors
rosdep install \
  --from-paths $HOME/colcon_ws/src/ \
  --ignore-src \
  --rosdistro $ROS_DISTRO

# build the package (including any dependencies). To build the entire
# workspace, simply run 'colcon build'
colcon build \
  --packages-up-to motoros2_client_interface_dependencies \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

If there were no warnings or errors, the workspace should now be activated using:

```bash
source $HOME/colcon_ws/install/local_setup.bash
```

At this point all messaging-related dependencies of MotoROS2 should be installed and can be used by client applications.

### Windows

TODO.

## Troubleshooting

Please find troubleshooting and other information in the [motoros2](https://github.com/yaskawa-global/motoros2) repository.
