# Roboclaw
Roboclaw is an extensible series of [Roboclaw][roboclaw] nodes for [ROS][ros]

## Features

- The base node "roboclaw_node" supports up to 8 Roboclaw controllers using packet serial mode
- Drive systems and odometry are decoupled from the base Roboclaw node
- A differential drive node is supported out of the box
- Written in roscpp for effecient memory usage and performance

## Requirements
- ROS Kinetic/Lunar/Melodic

## Nodes

### roboclaw_node

#### Parameters

| Param | Type  | Description  |
| :------------- |:-------------| :-----|
| serial_port | string | Path to the serial port to use |
| baudrate | int | Baudrate of the serial port |
| roboclaws | int | Number of Roboclaw controllers in packet serial mode |

#### Topics
| Action | Topic | Type |
| :------------- |:-------------| :-----|
| publish | motor_enc_steps | roboclaw/RoboclawEncoderSteps |
| subscribe | motor_cmd_vel | roboclaw/RoboclawMotorVelocity |

#### Notes

- When using one Roboclaw controller, configure it in packet serial mode with address 0x80. This will be motor index 0 in RoboclawEncoderSteps and RoboclawMotorVelocity.
- When using more than one Roboclaw controller, configure them in packet serial mode with 0x80 being motor index 0, 0x81 being motor index 1, and so on.

### diffdrive_node

#### Parameters

| Param | Type  | Description  |
| :------------- |:-------------| :-----|
| steps_per_meter | string | Number of encoder steps per meter |
| base_width | int | Diameter of the robots base from the center of each wheel |
| swap_motors | bool | Swap motor1 with motor2 |
| invert_motor_1 | bool | Invert drive and odometry for motor1 |
| invert_motor_2 | bool | Invert drive and odometry for motor2 |
| max_linear_speed | double | Linear speed at which the command will clip if it tries to go higher in m/s |
| max_linear_acceleration | double | Maximum linear acceleration in m/s^2 |
| max_angular_speed | double | Angular speed at which the command will clip if it tries to go higher in rad/s |
| tf_prefix | string | String added at the beginning of the frame id of the published TFs |


#### Topics
| Action | Topic | Type |
| :------------- |:-------------| :-----|
| publish | odom | Odometry |
| subscribe | cmd_vel | Twist |
| publish | cmd_vel_filtered | Twist |

#### Notes

- The *cmd_vel_filtered* topic is the actual command used to compute the motors command and it's published to give feedback to the user. The only filter used at the moment is to saturate the command whenever it tries to go over the maximum speed limits.

## Planned

- Support for the [NASA JPL Open Source Rover][jpl]'s drive system
- Exposing more of the Roboclaw's functionality via messages / services



[roboclaw]: http://www.basicmicro.com
[ros]: http://www.ros.org
[jpl]: https://opensourcerover.jpl.nasa.gov