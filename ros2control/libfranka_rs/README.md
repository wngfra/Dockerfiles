<!--
 Copyright (c) 2022 wngfra
 
 This software is released under the MIT License.
 https://opensource.org/licenses/MIT
-->

* run
```bash
mkdir -p /workspace/src && \
cd /workspace && \
git clone https://github.com/ros2-rust/ros2_rust.git src/ros2_rust && \
vcs import src < src/ros2_rust/ros2_rust_${ROS_DISTRO}.repos
```
to get [`ros2_rust`](https://github.com/ros2-rust/ros2_rust) examples.