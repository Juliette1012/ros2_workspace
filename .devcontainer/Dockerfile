##################################################################
# Ubuntu base image 
##################################################################
# foxy & galactic = ubuntu:20.04
# humble = ubuntu:22.04
FROM ubuntu:20.04 as base

# using bash as shell
SHELL ["/bin/bash", "-c"]

# specify to accept the default answer for all questions
ENV DEBIAN_FRONTEND=noninteractive

# install base packages
RUN apt-get update \
    && apt-get install -y software-properties-common \
                          bash-completion \
                          build-essential \
                          curl \
                          wget \
                          lsb-release \
                          gnupg \
                          gnupg2 \
                          # Python3
                          python3 \
                          python3-pip \
                          # code libraries e.g. clang compiler
                          llvm \
                          gdb \
                          cppcheck \
                          # https://clangd.llvm.org/installation
                          # clangd-14 \
                          # CLI editors
                          nano \
                          vim \
                          terminator \
                          # tools
                          git \
                          cmake \
                          # CPU / memory usage tools
                          htop \
                          # https://github.com/KDAB/hotspot
                          hotspot \
                          # https://github.com/KDE/heaptrack
                          heaptrack \
                          heaptrack-gui \
    # storage area for state information for each package resource
    # will be regenerated after a new 'apt-get update' call
    && rm -rf /var/lib/apt/lists/*

# install system information CLI tool - btop++
# https://github.com/aristocratos/btop
RUN wget -qO btop.tbz https://github.com/aristocratos/btop/releases/latest/download/btop-x86_64-linux-musl.tbz \
    && tar xf btop.tbz -C /usr/local bin/btop

# set environment variable for ROS2 version
ENV ROS_DISTRO=foxy

# ROS2 base image from dockerhub: https://hub.docker.com/_/ros
# additional commands based on Dockerfile from Allison Thackston
# https://github.com/athackst/dockerfiles/blob/main/ros2/humble.Dockerfile
ENV LANG en_US.UTF-8
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null \
    && apt-get update \
    && apt-get install -y ros-${ROS_DISTRO}-ros-base \
    && rm -rf /var/lib/apt/lists/*

# set ROS2-side-packages Python version
# foxy & galactic = python3.8
# humble = python3.10
ENV ROS_PYTHON_SITE_PACKAGES_VERSION=python3.8
# set environment variables for the ROS2 installation
ENV AMENT_PREFIX_PATH=/opt/ros/${ROS_DISTRO}
ENV COLCON_PREFIX_PATH=/opt/ros/${ROS_DISTRO}
ENV LD_LIBRARY_PATH=/opt/ros/${ROS_DISTRO}/lib
ENV PATH=/opt/ros/${ROS_DISTRO}/bin:$PATH
ENV PYTHONPATH=/opt/ros/${ROS_DISTRO}/lib/${ROS_PYTHON_SITE_PACKAGES_VERSION}/site-packages
ENV ROS_PYTHON_VERSION=3
ENV ROS_VERSION=2
# let ament use slower cppcheck versions to be able to use testing
ENV AMENT_CPPCHECK_ALLOW_SLOW_VERSIONS=true

# create non-root user 'ros'
ARG USER_NAME=ros
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

RUN groupadd --gid ${USER_GID} ${USER_NAME} \
    && useradd -s /bin/bash --uid ${USER_UID} --gid ${USER_GID} -m ${USER_NAME} \
    # add sudo support for the non-root user
    && apt-get update \
    && apt-get install -y sudo \
    && echo ${USER_NAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USER_NAME}\
    && chmod 0440 /etc/sudoers.d/${USER_NAME} \
    # cleanup
    && rm -rf /var/lib/apt/lists/*

# Set up auto-source of workspace for user 'ros'
RUN echo "source /usr/share/bash-completion/completions/git" >> /home/${USER_NAME}/.bashrc \
    && echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /home/${USER_NAME}/.bashrc

# Create alias file for ROS2 related CLI simplification commands
# https://colcon.readthedocs.io/en/main/user/how-to.html
RUN echo "alias cb='colcon build --symlink-install --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON --event-handlers console_direct+'" >> /home/${USER_NAME}/.bash_aliases \
    && echo "alias cbps='colcon build --symlink-install --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON --event-handlers console_direct+ --packages-select'" >> /home/${USER_NAME}/.bash_aliases \
    && echo "alias cbput='colcon build --symlink-install --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON --event-handlers console_direct+ --packages-up-to'" >> /home/${USER_NAME}/.bash_aliases \
    && echo "alias s='source install/setup.bash'" >> /home/${USER_NAME}/.bash_aliases

# switch to interactive user mode
ENV DEBIAN_FRONTEND=dialog

##################################################################
# OpenCR Setup
##################################################################

# dpkg --add-architecture armhf apt update \ && apt install libc6:armhf \ export OPENCR_PORT=/dev/ttyACM0 \
#    && export OPENCR_MODEL=burger \ && rm - rf ./opencr_update.tar.bz2 \

#ENV OPENCR_PORT=/dev/ttyACM0
#ENV OPENCR_MODEL=burger
#RUN  wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2 \
#    && tar -xjf ./opencr_update.tar.bz2

#RUN cd ~/opencr_update \
#RUN ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr

##################################################################
# Full ROS2 image 
##################################################################
FROM base AS full

# specify to accept the default answer for all questions
ENV DEBIAN_FRONTEND=noninteractive

# install additional packages
RUN apt-get update \
    && apt-get install -y python3-colcon-common-extensions \
                          python3-rosdep \
                          python3-setuptools \
                          python3-vcstool \
                          # ROS2 testing packages
                          python3-autopep8 \
                          python3-flake8* \
                          python3-pytest* \
                          ros-${ROS_DISTRO}-ament* \
                          ros-${ROS_DISTRO}-launch* \
                          # full ROS2 installation
                          ros-${ROS_DISTRO}-desktop \
                          # ROS data plotting GUI:
                          # https://github.com/facontidavide/PlotJuggler
                          ros-${ROS_DISTRO}-plotjuggler-ros \
    && rm -rf /var/lib/apt/lists/* \
    && rosdep init || echo "########## rosdep already initialized ##########"

# command completion for colcon CLI
RUN echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> /home/${USER_NAME}/.bashrc

# ROS visual CLI tool
# https://github.com/eduidl/rtui
RUN pip3 install --user git+https://github.com/eduidl/rtui.git

# switch to interactive user mode
ENV DEBIAN_FRONTEND=dialog


##################################################################
# Gazebo / Ignition simulation
##################################################################
FROM full AS gazebo

# specify to accept the default answer for all questions
ENV DEBIAN_FRONTEND=noninteractive

# install ROS2 Gazebo and Ignition packages
RUN apt-get update \
    && apt-get install -y ros-${ROS_DISTRO}-gazebo* \
                          # https://gazebosim.org/docs/fortress/install_ubuntu
                          ignition-fortress \
                          # https://docs.ros.org/en/humble/Tutorials/Advanced/Simulators/Ignition.html
                          ros-${ROS_DISTRO}-ros-ign-bridge \
    && rm -rf /var/lib/apt/lists/*

# fix for Gazebo to work with VMware 3D acceleration (on NVIDIA graphics card)
# https://robocademy.com/2020/05/02/solved-opengl-issues-with-gazebo-and-vmware/
RUN echo "export SVGA_VGPU10=0" >> /home/${USER_NAME}/.bashrc

# switch to interactive user mode
ENV DEBIAN_FRONTEND=dialog


##################################################################
# Navigation2 and Behavior Tree Editor
##################################################################
FROM gazebo AS nav2

# specify to accept the default answer for all questions
ENV DEBIAN_FRONTEND=noninteractive
# specify a placeholder variable for the Groot workspace directory
ENV GROOT_WS=/home/${USER_NAME}/groot_ws

# install Behavior Tree Editor - Groot
RUN apt-get update \
    && mkdir -p ${GROOT_WS}/src \
    && cd ${GROOT_WS}/src \
    # Behavior Tree GUI editor: https://github.com/BehaviorTree/Groot
    && git clone https://github.com/BehaviorTree/Groot.git \
    && cd .. \
    && rosdep update \
    && rosdep install -y -r --from-paths src \
                            --ignore-src \
                            --rosdistro=${ROS_DISTRO} \
    && colcon build --symlink-install \
                    --event-handlers console_direct+ \
                    --cmake-args -DCMAKE_BUILD_TYPE=Release

RUN echo "source ${GROOT_WS}/install/setup.bash" >> /home/${USER_NAME}/.bashrc

# install Navigation2 packages
# https://navigation.ros.org/build_instructions/index.html
RUN apt-get update \
    && apt-get install -y ros-${ROS_DISTRO}-navigation2 \
                          ros-${ROS_DISTRO}-nav2-bringup \
                          ros-${ROS_DISTRO}-turtlebot3* \
                          # additional Nav2 plugins
                          # https://navigation.ros.org/configuration/index.html
                          #ros-${ROS_DISTRO}-nav2-constrained-smoother \
    && rm -rf /var/lib/apt/lists/*

# switch to interactive user mode
ENV DEBIAN_FRONTEND=dialog


##################################################################
# DDS vendor implementation
##################################################################
FROM nav2 AS dds_setup

# specify to accept the default answer for all questions
ENV DEBIAN_FRONTEND=noninteractive

# additional DDS implementation:
# https://docs.ros.org/en/humble/Installation/DDS-Implementations.html
RUN apt-get update \
    && apt-get install -y ros-${ROS_DISTRO}-rmw-cyclonedds-cpp \
    && rm -rf /var/lib/apt/lists/*

# improve Wifi performance by reducing IP fragment buffering time (default: 30s)
# and the memory size used to reassemble IP fragments (default: 262144 Bytes)
# https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html
# 5 = 5s
# 134217728 = 128 MB
RUN sysctl net.ipv4.ipfrag_time=5 \
    && sysctl net.ipv4.ipfrag_high_thresh=134217728

# set the used DDS implementation:
# rmw_fastrtps_cpp
# rmw_cyclonedds_cpp
# rmw_connextdds
ENV RMW_IMPLEMENTATION=rmw_fastrtps_cpp

# unique ID to separate ROS2 networks from each other
# selectable between 0 and 101 (default: 0)
ENV ROS_DOMAIN_ID=0

# switch to interactive user mode
ENV DEBIAN_FRONTEND=dialog