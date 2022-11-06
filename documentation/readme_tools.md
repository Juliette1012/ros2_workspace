# ROS2 tools and helpers information -> in development !


Useful CLI aliases to safe time and finger-exhausting (located in dev container -> ~/.bash_aliases):
```
# alias for: colcon build --symlink-install --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON --event-handlers console_direct+
cb

# alias for: colcon build --symlink-install --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON --event-handlers console_direct+ --packages-select
cbps

# alias for: colcon build --symlink-install --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON --event-handlers console_direct+ --packages-up-to
cbput

# alias for: source install/setup.bash
s
```


DDS tuning steps:
https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html

```
sudo sysctl net.ipv4.ipfrag_time=5

sudo sysctl net.ipv4.ipfrag_high_thresh=134217728
```
