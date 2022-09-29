# VSCode template for ROS2 workspace
Based on the template from Allison Thackston: https://github.com/athackst/vscode_ros2_workspace/tree/humble


## Workspace structure
The ROS2 workspace directory should always look like the following:

```
workspace
|-- .devcontainer
    |-- devcontainer.json       # properties of the develop container
    |-- Dockerfile              # dockerfile which sets up the container workspace
|-- .vscode
    |-- c_cpp_properties.json   # C++ properties e.g. include paths and compiler
    |-- launch.json             # launch actions for the environment
    |-- settings.json           # VScode specific properties
    |-- tasks.json              # task definitions which can be executed
|-- build                       # build directory of all packages
|-- install                     # installation directory of all packages
|-- log                         # log files of all ROS2 run processes
|-- src                         # contains all ROS2 packages
    |-- package_1
    |-- package_2
    |-- package_n
|-- .clang-format               # code formatting definitions
|-- .clang-tidy                 # static code analysis properties and naming definitions
|-- .cmake-format.yaml          # CMake formatting definitions
|-- .gitignore                  # directories/files which will be ignored by git
|-- .gitmodules                 # definitions of git submodules in the src directory
|-- LICENSE                     # license information and example implementation
|-- README.md                   # documentation description file for the repository
```

## ROS2 package handling

⚡ Packages should always look as described in: https://code.siemens.com/simatic-systems-support/agv/package_template/-/tree/devel

To add an existing package git repository to the workspace open a terminal within the workspace directory and type in:

```
# -f to force cloning -> needed because directory src/* is in .gitignore list
git submodule add -f <URL> <directory> 

# example: ROS2 template git package
git submodule add -f git@code.siemens.com:simatic-systems-support/agv/package_template.git
                     src/package_template

# init and update the specified submodules
git submodule init
git submodule update --remote
```


## Further README links

General topics:  
[git](documentation/readme_git.md)  
[docker](documentation/readme_docker.md)  
[docker-compose](documentation/readme_docker_compose.md)  
 
ROS2 in general:  
[useful tools and helpers](documentation/readme_tools.md)  
[colcon](documentation/readme_colcon.md)  
[launch files](documentation/readme_launch_file.md)

ROS2 packages:  
[CMakeLists.txt](documentation/readme_cmake.md)  
[package.xml](documentation/readme_package_xml.md)  
[C++ header](documentation/readme_cpp_header.md)  
[C++ source](documentation/readme_cpp_source.md)  
[Python](documentation/readme_python.md)  
[custom messages](documentation/readme_custom_messages.md)  


## Prerequisites for VSCode setup

Following programs need to be installed first:

* [docker](https://docs.docker.com/desktop/linux/install/ubuntu/)
* [vscode](https://code.visualstudio.com/)
* [vscode remote containers plugin](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## Useful VSCode extensions
* [Docker from Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
* [Python from Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
* [CMake Tools from Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools)
* [clangd from LLVM](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd)
* [Git Graph from mhutchie](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)
* [ROS2 from nonanonno](https://marketplace.visualstudio.com/items?itemName=nonanonno.vscode-ros2)


## First usage
After cloning this template open VSCode inside the repository directory with the terminal command:

```
vscode .
```

Or open vscode manually and press:

```
CTRL + K
CTRL + O
```
... and select the repository workspace folder.


## Open workspace in a container
After vscode has opened there should pop up a window on the bottom right:
![image](documentation/pictures/reopen_in_container.png?raw=true)


## Execute tasks
To execute a task select "Terminal / Run Task..." or press:
```
CTRL + ALT + T
```
... a popup window shows all available tasks which can be selected e.g. to create a new ROS2 package:
![image](documentation/pictures/execute_task.png?raw=true)


## Clang-Format

The .clang-format file helps to specify formatting rules in the code: 
https://clang.llvm.org/docs/ClangFormatStyleOptions.html  
It is part of clangd, which is included in the devcontainer setup.  
The automatic formatting of the active file can be triggered with the following command:

```
CTRL + SHIFT + i
```

## Clang-Tidy

The .clang-tidy file specifies the static code analysis rules for the code: 
https://clang.llvm.org/extra/clang-tidy/checks/readability-identifier-naming.html  
It declares the naming conventions for e.g. variable or method naming and is also part of clangd.  
Once the document is opened or saved all analysis errors are displayed like the following:
![image](documentation/pictures/clang_tidy_popup.png?raw=true)


## Useful tools

### terminator

Program for visualising multiple terminal windows next to each other: https://github.com/gnome-terminator/terminator

```
terminator
```

### hotspot

Performance analysis tool to help track down slow code:
https://github.com/KDAB/hotspot

```
hotspot
```

### heaptrack

Find and trace heap allocations in your code:
https://github.com/KDE/heaptrack

```
heaptrack
```

### btop++

Show all system information e.g. CPU & memory usage in a terminal:
https://github.com/aristocratos/btop

```
btop
```

### PlotJuggler

GUI tool to visualize ROS data:
https://github.com/facontidavide/PlotJuggler

```
ros2 run plotjuggler plotjuggler
```

### rtui

Terminal tool to visualize ROS data e.g. topics, nodes, services:
https://github.com/eduidl/rtui

```
rtui topics
rtui nodes
rtui services
rtui actions
```

### Groot

GUI tool to edit and monitor behavior trees: 
https://github.com/BehaviorTree/Groot  
-> uses the BehaviorTree.CPP library: https://github.com/BehaviorTree/BehaviorTree.CPP

```
ros2 run groot Groot
```

## Additional information

Slides from Davide Faconti regarding C++, behavior trees ...  
https://slides.com/davidefaconti  

C++ best practices from Jason Turner  
https://github.com/cpp-best-practices/cppbestpractices  

ROS2 documentation  
https://docs.ros.org/en/humble/index.html

ROS Enhancement Proposals (REPs)  
https://ros.org/reps/rep-0000.html

ROS2 API and message types (no humble API available yet)  
https://docs.ros2.org/galactic/api

ROS2 Navigation2 documentation  
https://navigation.ros.org/index.html

ROS related topics, announcements, discussions  
https://discourse.ros.org/latest


### Update the template with your code

1. Specify the repositories you want to include in your workspace in `src/ros2.repos` or delete `src/ros2.repos` and develop directly within the workspace.
2. If you are using a `ros2.repos` file, import the contents `Terminal->Run Task..->import from workspace file`
2. Install dependencies `Terminal->Run Task..->install dependencies`
3. (optional) Adjust scripts to your liking.  These scripts are used both within tasks and CI.
   1. `setup.sh` The setup commands for your code.  Default to import workspace and install dependencies.
   2. `build.sh` The build commands for your code.  Default to `--merge-install` and `--symlink-install`
   3. `test.sh` The test commands for your code.
4. Develop!
