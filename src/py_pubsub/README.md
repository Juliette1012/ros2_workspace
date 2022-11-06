# ROS2 package template with examples

This repository provides all information about ROS2 packages and implementation details  
as well as example implementations which can be used as references for lookup.  
Use this template to create your own ROS2 package.


## Package structure
The ROS2 package directory should always look like the following:
```
package_name
|-- include
    |-- package_name            # name of the specified package
        |-- header_1.hpp        # header for rclcpp::Node based class 1
        |-- header_2.hpp        # header for rclcpp::Node based class 2
        |-- header_n.hpp        # header for rclcpp::Node based class n
|-- launch
    |-- launch_file.launch.py   # launch file for ROS2 node (with parameters)
|-- src
    |-- main.cpp                # entrypoint of the package -> main() function
    |-- source_1.cpp            # implementations of the class 1
    |-- source_2.cpp            # implementations of the class 2
    |-- source_n.cpp            # implementations of the class n
|-- .gitlab-ci.yml              # CI/CD defined steps
|-- CMakeLists.txt              # defines all compile commands and the build process
|-- Dockerfile                  # commands to setup a docker image
|-- LICENSE                     # license information -> NEEDS TO BE CLARIFIED BEFORE PUBLISHED!
|-- README.md                   # documentation description file for the repository
|-- package.xml                 # project name definition and package dependencies
```


## Further README links

General topics:  
[git](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_git.md)  
[docker](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_docker.md)  
[docker-compose](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_docker_compose.md)  
 
ROS2 in general:  
[useful tools](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_tools.md)  
[colcon](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_colcon.md)  
[launch files](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_launch_file.md)

ROS2 packages:  
[CMakeLists.txt](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_cmake.md)  
[package.xml](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_package_xml.md)  
[C++ header](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_cpp_header.md)  
[C++ source](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_cpp_source.md)  
[Python](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_python.md)  
[custom messages](https://code.siemens.com/simatic-systems-support/agv/vscode_ros2_workspace/-/blob/galactic/documentation/readme_custom_messages.md)  


## Official ROS2 example packages

[C++](https://github.com/ros2/examples/tree/galactic/rclcpp)  
[Python](https://github.com/ros2/examples/tree/galactic/rclpy)  


## Published topics

This tables should display all topics / services / actions, which your package provides.

| Topic                  | Message type                 | Description                            |
| ---------------------- | ---------------------------- | -------------------------------------- |
| /example/topic         | std_msgs/msg/Empty           | Description for this topic             |


## Subscribed topics

| Topic                  | Message type                 | Description                            |
| ---------------------- | ---------------------------- | -------------------------------------- |
| /example/topic         | std_msgs/msg/Empty           | Description for this topic             |


## Service server topics

| Topic                  | Service type                 | Description                            |
| ---------------------- | ---------------------------- | -------------------------------------- |
| /example/service       | std_srvs/srv/SetBool         | Description for this service           |
