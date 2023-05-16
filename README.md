# motoros2_client_interface_dependencies

[![Github Issues](https://img.shields.io/github/issues/yaskawa-global/motoros2_client_interface_dependencies.svg)](http://github.com/yaskawa-global/motoros2_client_interface_dependencies/issues)

[![license - apache 2.0](https://img.shields.io/:license-Apache%202.0-yellowgreen.svg)](https://opensource.org/licenses/Apache-2.0)

[![support level: vendor](https://img.shields.io/badge/support%20level-vendor-brightgreen.svg)](http://rosindustrial.org/news/2016/10/7/better-supporting-a-growing-ros-industrial-software-platform)

## Overview

This package exists to facilitate installation of the ROS 2 packages containing message, service and action definitions used by client applications to communicate with a [MotoROS2](https://github.com/yaskawa-global/motoros2) server running on a Yaskawa Motoman industrial robot controller.

This package is an empty package and does not provide any interface definitions itself, nor does it host any code.
Its sole task is to help `rosdep` install the MotoROS2 client dependencies on a (companion) PC.

## Build and installation

If necessary, create a Colcon / ROS 2 workspace and place a copy of the repository in the `src` space of that workspace (use `git clone` or download a `.zip` or `.tar.gz` from Github).

### Linux

The following instructions show an *example* workflow which would add `motoros2_client_interface_dependencies` to an existing Colcon workspace at `$HOME/colcon_ws`, install the dependencies and finally build the workspace for ROS 2 Humble.

Update commands where necessary if a different ROS 2 version should be used or the Colcon workspace is in a different location.

As follows:

```bash
# change to the root of the Colcon workspace
cd $HOME/colcon_ws

# retrieve the latest development version of this package
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
source /opt/ros/humble/setup.bash
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
  --mixin release \
  --packages-up-to motoros2_client_interface_dependencies
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
