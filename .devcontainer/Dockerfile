FROM ros:foxy-ros-base-focal

# using bash
SHELL ["/bin/bash", "-c"]

# remove debconf errors (must run before any apt-get calls)

RUN echo "debconf debconf/frontend select Noninteractive" | debconf-set-selections

# install needed linux packages
# apt-get   https://wiki.ubuntuusers.de/apt/apt-get/

# -y yes to all displayed questions 
RUN apt-get update \
        && apt-get upgrade -y \
        && apt-get install -y apt-utils \
        software-properties-common \
        # Tools
        git \
        nodejs \ 
        openssh-client \ 
        # Driver lidar
        libudev-dev \
        libboost-all-dev \
        # CLI editors
        nano \
        vim \
        terminator  

# Set up ros workspace
WORKDIR /home
RUN git clone -b foxy https://github.com/Juliette1012/ros2_workspace.git

# Set up turtlebot3_ws
WORKDIR /home/ros2_workspace/src

# TurtleBot3 packages
RUN git clone -b foxy-devel https://github.com/ROBOTIS-GIT/DynamixelSDK.git

# Set up turtlebot3_ws
RUN mkdir /home/ros2_workspace/src/turtlebot3_ws 
# Set up ros2_workspace
WORKDIR /home/ros2_workspace/src/turtlebot3_ws
RUN git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
RUN git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3.git
RUN git clone -b ros2-devel https://github.com/ROBOTIS-GIT/ld08_driver.git

# Set up ros2_workspace
WORKDIR /home/ros2_workspace

# build the ROS2 workspace
RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
        && colcon build --symlink-install

# write source commands to .bashrc -> no need to source afterwards
RUN echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
RUN echo 'export TURTLEBOT3_MODEL=burger' >> ~/.bashrc
RUN echo 'export LDS_MODEL=LDS-02' >> ~/.bashrc
RUN echo 'export OPENCR_PORT=/dev/ttyACM0' >> ~/.bashrc
RUN echo 'export OPENCR_MODEL=burger' >> ~/.bashrc
RUN echo "source /home/ros2_workspace/install/setup.bash" >> ~/.bashrc