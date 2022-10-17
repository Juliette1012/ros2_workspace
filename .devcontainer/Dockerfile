FROM ros:foxy-ros-base-focal

# using bash
SHELL ["/bin/bash", "-c"]

# remove debconf errors (must run before any apt-get calls)

RUN echo "debconf debconf/frontend select Noninteractive" | debconf-set-selections

# install needed linux packages
# apt-get   https://wiki.ubuntuusers.de/apt/apt-get/

# -y        yes to all displayed questions 
RUN apt-get update \
        && apt-get upgrade -y \
        && apt-get install -y apt-utils \
        software-properties-common \
        git \
        vim \
        nano \
        nodejs

# set up robot workspace
RUN mkdir -p /home/ros_ws
WORKDIR /home/ros_ws
RUN git clone git@github.com:Juliette1012/ros2_workspace.git \
        git checkout foxy 

# build the ROS2 workspace
RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
        && colcon build --symlink-install
# write source commands to .bashrc -> no need to source afterwards
RUN echo "source /home/ros_ws/install/setup.bash" >> ~/.bashrc

CMD ros2 run py_pubsub talker
