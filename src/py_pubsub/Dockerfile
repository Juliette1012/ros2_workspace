# base docker image
# TODO(Manuel Kreutz): use workspace docker image from gitlab -> with user 'ros'
FROM ros:foxy-ros-base

# the used shell
SHELL ["/bin/bash", "-c"]

# specify to accept the default answer for all questions
ENV DEBIAN_FRONTEND=noninteractive

# install additional software packages
RUN apt-get update \
    && apt-get install -y nano \
                          vim \
                          # install FastDDS (ROS2 Galactic default: CycloneDDS)
                          ros-${ROS_DISTRO}-rmw-fastrtps-cpp \
    # storage area for state information for each package resource
    # will be regenerated after a new 'apt-get update' call
    && rm -rf /var/lib/apt/lists/*

# set the used DDS implementation:
# rmw_fastrtps_cpp
# rmw_cyclonedds_cpp
# rmw_connextdds
ENV RMW_IMPLEMENTATION=rmw_fastrtps_cpp

# define environment variable for the used ROS2 workspace
ENV ROS_WS=/root/ros_ws

# create package workspace directory
RUN mkdir -p ${ROS_WS}/src

# copy all package content to container package directory
COPY . \
     ${ROS_WS}/src

# set workspace as the actual used directory
WORKDIR ${ROS_WS}

# source ROS2 installation in .bashrc file
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

# install package dependencies
# these are defined in the package.xml file
RUN apt-get update \
    && rosdep update \
    && rosdep install -y -r --from-paths src \
                            --ignore-src \
                            --rosdistro=${ROS_DISTRO}

# build ROS2 package
# --symlink-install -> link non source files e.g. launch files to install folder
# --event-handlers console_direct+  -> show build output in console
# --cmake-args -DCMAKE_BUILD_TYPE=Release   -> build type of the executable
RUN source /opt/ros/${ROS_DISTRO}/setup.bash \
    && colcon build --symlink-install \
                    --event-handlers console_direct+ \
                    --cmake-args -DCMAKE_BUILD_TYPE=Release

# source ROS2 package workspace in .bashrc file
RUN echo "source ${ROS_WS}/install/setup.bash" >> ~/.bashrc

# switch to interactive user mode
ENV DEBIAN_FRONTEND=dialog

# docker container entrypoint commands
# if not otherwise specified via docker-compose
# those commands will be executed on every container startup
CMD source /opt/ros/${ROS_DISTRO}/setup.bash \
    && source ${ROS_WS}/install/setup.bash \
    && ros2 launch example_package example_launch_file.launch.py
