# An overview of available RAS modules
This section provides a comprehensive description of the various maritime robotics experiment tools at your disposal. It serves as a starting point for collaborators to familiarize themselves with the available resources.

The benefit of using the modules developed by RAS engineers is that these elements have usually been developed up to a reasonable state of reliability, which becomes extra desirable when there are numerous single-points-of-failure. Leveraging these well-tested modules allows you to focus more on your goal. Utilizing proven components you can devote more time and effort to pushing your own project goals, while contributing by maturing the software through making small improvements, bug reports or feedback. 

If you intend to develop a control architecture utilizing any of these modules, we highly recommend sparring with the engineers of RAS when you want to start designing a control architecture.

## Utilities

| Name                | Description                                                                    | Source code |
| ------------------- | ------------------------------------------------------------------------------ | ----------------- |
| Joystick controller | Allows joystick control of actuators by a human operator                       |->  |
| ras_tf_lib          | Various python functions packed in a ros library that is used in other modules |link  |
| Nausbot             | Turtlebot but as a ship: lightweight real time ship simulator on a ros node    |link  |

## Control modules
| Name                | Description                                                                    | Link              |
| ---------------- | --------------------- | ------------------------------ |
| Control effort allocator 3dof  | Allocates a desired control effort of a delfia with simple constraints.   |link  |
| DP + vessel assembly + formation_adaptive_control  | Vessel dynamic positioning, assembling and adjusting their 1)control effort generation and 2) allocation.  | |
| Heading controller| Controls heading of a vessel (PID based) |link  |
| Waypoint following manager | Manages waypoints of a track that a vessel needs to follow. Has a click point user interface to adjust path. Calculates reference heading of a ship and detects if ship needs to snap to a next waypoint. |link  |
| USV_geopos_heading_differentiator | Estimates ship body fixed velocities through differentiation and filtering |link  |
| Surge velocity controller | Aims to let ship surge follow a reference (PID based) |link  |
| Surge-yaw TitoNeri thrust allocator | Allocates desired control effort (Fx, Mzz) over thrusters |link  |
| USV_formation_control_1/formation_configuration_broadcaster | simple periodic fixed predefined formation state broadcaster |link  |
| USV_formation_control_1/formation_state_estimator | Estimates state of a formation given ship positions and known desired formation shape |link  |
| USV_formation_control_1/trajectory_planner | Takes a predefined path. Smoothes. Adds a time/velocity profile. Moves a formation origin along profile in real time. |link  |
| USV_formation_control_1/vessel_reference_generator | Generates reference heading and velocity to let a ship follow formation reference at set distance |link  |
| Mechatronics  course Simulink Tito Neri Dynamic Positioning | Dynamic positioning system for Tito Neri running on Matlab Simulink. | link|

# Backend systems
| Name                | Description                                                                    | Link              |
| ---------------- | --------------------- | ------------------------------ |
| Ros optitrack bridge | Takes optitrack body pose and streams it on ros |  |
| Web-Diagnostics | Python server and Vue.js webapp code that form the web diagnostics panel. |  |
| Pico-Logger | Voltage/current logging utility |  |
| Server-Docker-Stack | Stack that runs VPN server services |  |
| boat-daemon | Sends vessel diagnostics to Web-Diagnostics |  |
| reach-bridge | Connects the emlid reach to stream vessel position on ROS |  |
| TN_arduino_low_level | Controls actuators to follow input reference and returns telemetry |  |
| ROS-Arduino-Bridge | passes Ros vessel actuator commands to microcontroller. Returns telemetry & Heading. returns system diagnostics |  |
