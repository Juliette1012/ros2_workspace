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
# Create turtlebot3_ws
RUN mkdir -p /home/ros_ws/src

# Set up turtlebot3_ws
WORKDIR /home/ros_ws/src

# TurtleBot3 packages
RUN git clone -b foxy-devel https://github.com/ROBOTIS-GIT/DynamixelSDK.git

# Set up turtlebot3_ws
RUN mkdir turtlebot3_ws 
WORKDIR /home/ros_ws/src/turtlebot3_ws 
RUN git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
RUN git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3.git
RUN git clone -b ros2-devel https://github.com/ROBOTIS-GIT/ld08_driver.git


# Our ROS2 package
WORKDIR /home/ros_ws 
RUN git clone -b foxy https://github.com/Juliette1012/ros2_workspace.git

# Set up ros_ws
WORKDIR /home/ros_ws

# build the ROS2 workspace
RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
        && colcon build --symlink-install

# write source commands to .bashrc -> no need to source afterwards
RUN echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
RUN echo 'export TURTLEBOT3_MODEL=burger' >> ~/.bashrc
RUN echo 'export LDS_MODEL=LDS-02' >> ~/.bashrc
RUN echo 'export OPENCR_PORT=/dev/ttyACM0' >> ~/.bashrc
RUN echo 'export OPENCR_MODEL=burger' >> ~/.bashrc
RUN echo "source /home/ros_ws/install/setup.bash" >> ~/.bashrc