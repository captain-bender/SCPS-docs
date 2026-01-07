## Create workspace
```
mkdir -p ~/turtlesim_ws/src
cd ~/turtlesim_ws
colcon build
```


## Create ros package
```
cd turtlesim_ws/
cd src/
ros2 pkg create --build-type ament_python turtlesim_controller
```

## Write the P-controller script 
for the p-controller and save it inside the turtlesim_controller folder which is inside the turtlesim_controller package
e.g. call it turtle_triangle_controller.py

After you save the controller script, in turtlesim_controller/setup.py, add an entry point
```
entry_points={
    'console_scripts': [
        'turtle_triangle_controller = my_turtle_pkg.turtle_triangle_controller:main',
    ],
},
```

## Compile it and source it
```
colcon build
source install/setup.bash
```

## Execute
Start the turtlesim simulation in one terminal
```
ros2 run turtlesim turtlesim_node
```

and then in the existing terminal that you have after the successfull compilation the turtle_triangle_controller.py script.
```
ros2 run turtlesim_controller turtle_triangle_controller
```

## Write the virtual sensors script 
for the tutrle and save it inside the turtlesim_controller folder which is inside the turtlesim_controller package
e.g. call it turtle_virtual_sensors.py

After you save the controller script, in turtlesim_controller/setup.py, add an entry point
```
entry_points={
    'console_scripts': [
        'turtle_triangle_controller = my_turtle_pkg.turtle_triangle_controller:main',
        'turtle_virtual_sensors = turtlesim_controller.turtle_virtual_sensors:main',
    ],
},
```

## Compile it and source it:
```
colcon build
source install/setup.bash
```

## Execute
Start the turtlesim simulation in one terminal
```
ros2 run turtlesim turtlesim_node
```

Start the keybord teleoperation in the second terminal:
```
ros2 run turtlesim turtle_teleop_key
```

in the existing terminal that you have after the successfull compilation the turtle_virtual_sensors.py script.
```
ros2 run turtlesim_controller turtle_virtual_sensors
```

To view the list of topics:
```
ros2 topic list
```

or a specific part of the virtual sensor, for example the position:
```
ros2 topic echo /virtual/odom --field pose.pose.position
```
