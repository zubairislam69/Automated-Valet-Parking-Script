# Autoware Automated Valet Parking Script

This script has been developed for use with Autoware. It allows for publishing messages that tell the ego vehicle to follow a looped route in the case that no free parking spots are found.

<br>

## Customizing Map Parameters

To run the script on a specific map, you need to update the initial pose and goal pose commands inside the script. This ensures proper functioning of the script with the chosen map. Follow the steps below to customize the map parameters.


<br>

## Generating a Publish Initial Pose Command

To automate the process of publishing the intitial pose of the ego vehicle, the publish command is required. Follow these steps:

#### 1. Ensure Autoware is running.
Before proceeding, ensure that Autoware is running. If not, start Autoware with the following command:

```
ros2 launch autoware_launch planning_simulator.launch.xml \
    map_path:=/path/to/your/map \
    vehicle_model:=sample_vehicle \
    sensor_model:=sample_sensor_kit
```

#### 2. Source Autoware and ROS2 Environment.
Open a new terminal and source the Autoware and ROS2 environment setup files. Modify the paths according to your installation directory if they differ.

```
source /path/to/your/autoware/install/setup.bash
source /opt/ros/humble/setup.bash
```

#### 3. Run the command inside the sourced terminal: 
```
ros2 topic echo /initialpose
```

#### 4. Set an initial pose on a lane inside the RViz window.

#### 5. Capture the coordinates from the echoed message.

### Example Echoed Message:
```
header:
  stamp:
    sec: 1708461407
    nanosec: 151011266
  frame_id: map
pose:
  pose:
    position:
      x: 3730.777099609375
      y: 73724.90625
      z: 0.0
    orientation:
      x: 0.0
      y: 0.0
      z: -0.9610824749843778
      w: 0.27626160840388697
  covariance:
  - 0.25
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.25
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.06853891909122467
```

#### 6. Use the obtained coordinates as a command line argument in the generate_initialpose_command.py script and execute the script:
```
python generate_initial_pose_command.py "{Include printed coordinates here}".
```
### Example Script Output

```
ros2 topic pub --once /initialpose geometry_msgs/msg/PoseWithCovarianceStamped '{header: {stamp: {sec: 1708461407, nanosec: 151011266}, frame_id: "map"}, pose: {pose: {position: {x: 3730.777099609375, y: 73724.90625, z: 0.0}, orientation: {x: 0.0, y: 0.0, z: -0.9610824749843778, w: 0.27626160840388697}}, covariance: [0.25, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.25, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.06853891909122467]}}'
```

<br>


## Generating a Publish Goal Pose Command

Similarly to the initial pose, the goal pose location coordinates are also required. Follow these steps:

#### 1. Run the command inside the sourced terminal: 
``` 
ros2 topic echo /planning/mission_planning/goal
```

#### 2. Select a goal pose on a lane inside the RViz window.

#### 3. Capture the coordinates from the echoed message.

### Example Echoed Message:
```
header:
  stamp:
    sec: 1708463402
    nanosec: 486667092
  frame_id: map
pose:
  position:
    x: 3733.503173828125
    y: 73758.703125
    z: 0.0
  orientation:
    x: 0.0
    y: 0.0
    z: -0.5108924948741428
    w: 0.859644611849149
```

#### 4. Use the obtained coordinates as a command line argument in the generate_goal_pose_command.py script and execute the script:
```
python generate_goal_pose_command.py "{Include printed coordinates here}".
```
### Example Script Output

```
ros2 topic pub /planning/mission_planning/goal geometry_msgs/msg/PoseStamped '{header: {stamp: {sec: 1708463402, nanosec: 486667092}, frame_id: 'map'}, pose: {position: {x: 3733.503173828125, y: 73758.703125, z: 0.0}, orientation: {x: 0.0, y: 0.0, z: -0.5108924948741428, w: 0.859644611849149}}}' --once
```


<br> 
*Note: README.md is a work in progress. More detailed documentation will be added soon.*

