# Pulse Robot REST API Python

This folder contains `Python` wrapper for the [pulse robot](https://rozum.com/robotic-arm/) REST api. 
Tested on Python 3. Compatibility with Python 2 is not guaranteed but this client uses 
[swagger-codegen](https://github.com/swagger-api/swagger-codegen) so it may work for you.

### Installation
Currently, installation via pip is not supported. So, copy the `rozum` module into your project root manually.

### Getting started
```python
from pprint import pprint
import rozum as rr
# create an instance of the API wrapper class
host = "127.0.0.1:8080"  # replace with valid robot address 
robot = rr.RestRobot(host)
# create motion targets
pose = rr.Pose([0, 0, 0, 0, 0, 0])
position = rr.Position(rr.Point(-0.33, -0.33, 0.43), rr.Rotation(0, -1.1659, 0))
SPEED = 100
try:
    # execute desired actions
    robot.set_pose(pose, SPEED)
    robot.await_motion()
    robot.set_position(position, SPEED)
    robot.await_motion()
    # get some information
    status_motors = robot.status_motors()
    pprint(status_motors)
except rr.RestApiException as e:
    print("Exception when calling RestRobot %s\n" % e)
```

## Robot Functionality
Method | HTTP request | Description
------------- | ------------- | -------------
[**change_base**](README.md#change_base) | **POST** /base | Setting a new zero point position
[**change_tool**](README.md#change_tool) | **POST** /tool | Setting tool properties
[**close_gripper**](README.md#close_gripper) | **PUT** /gripper/close | Asking the arm to close the gripper
[**freeze**](README.md#freeze) | **PUT** /freeze | Asking the arm to go to the freeze state
[**get_base**](README.md#get_base) | **GET** /base | Getting the actual position of the arm base
[**get_digital_output**](README.md#get_digital_output) | **GET** /signal | Get level of digital output signal
[**get_pose**](README.md#get_pose) | **GET** /pose | Getting the actual arm pose
[**get_position**](README.md#get_position) | **GET** /position | Getting the actual arm position
[**get_tool**](README.md#get_tool) | **GET** /tool | Getting actual tool properties
[**identifier**](README.md#identifier) | **GET** /robot/id | Getting the arm ID
[**open_gripper**](README.md#open_gripper) | **PUT** /gripper/open | Asking the arm to open the gripper
[**relax**](README.md#relax) | **PUT** /relax | Asking the arm to relax
[**run_poses**](README.md#run_poses) | **PUT** /poses/run | Asking the arm to move to a pose
[**run_positions**](README.md#run_positions) | **PUT** /positions/run | Asking the arm to move to a position
[**set_digital_output_high**](README.md#set_digital_output_high) | **PUT** /signal/high | Set high level of digital output signal
[**set_digital_output_low**](README.md#set_digital_output_low) | **PUT** /signal/low | Set low level of digital output signal
[**set_pose**](README.md#set_pose) | **PUT** /pose | Setting a new arm pose
[**set_position**](README.md#set_position) | **PUT** /position | Setting a new arm position
[**status_motion**](README.md#status_motion) | **GET** /status/motion | Getting the actual motion status
[**status_motors**](README.md#status_motors) | **GET** /status/motors | Getting the actual status of servo motors

### **change_base**
> Position robot.change_base(position)

Setting a new zero point position

The function enables setting a new zero point position of the robotic arm as required for the current user environment (e.g., considering the surrounding equipment). The new zero point position is described as a set of x, y, and z coordinates, as well as _roll_, _pitch_, and _yaw_ rotation angles. The coordinates define the desired offset (in meters) from the physical centerpoint of the arm base (original zero point) along the x, y, and z axes accordingly. _Roll_ stands for the rotation angle around the x axis; _pitch_ - the rotation angle around the y axis; _yaw_ - the rotation angle around the z axis. All rotation angles are in radians and relative to the physical centerpoint of the arm base.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **position** | **Position** | Request Body | 

#### Return type
**Position**

### **change_tool**
> Tool robt.change_tool(tool)

Setting tool properties

The function enables setting new TCP to account for the properties of an attached or changed work tool. The tool properties define the following: - _name_ - any random name of the work tool defined by the user (e.g., \"gripper\") - _position_ - a set of x, y, and z coordinates and rotation angles - _roll_, _pitch_, and _yaw_. The coordinates define the actual distance (in meters) from the arm's zero point to the new TCP along the x, y, and z axes accordingly. _Roll_ stands for the rotation angle of the new TCP around the x axis; _pitch_ - the rotation angle around the y axis; _yaw_ - the rotation angle of the new TCP around the z axis. All rotation angles are in radians. - _radius_ - radius of the work tool (in meters) measured from its physical center point.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **body** | **Tool** | Request Body | 

#### Return type
**Tool**

### **close_gripper**
> robot.close_gripper(timeout=timeout)

Asking the arm to close the gripper

The function commands the robot to close the gripper. It has no request body, but the user can optionally set how long (in milliseconds) the arm should remain idle, waiting for the gripper to close. The default manufacturer-preset value is 500 ms. ### Note: Setting the parameter, it is recommended to use values above 0 ms.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **timeout** | **float**| Time in milliseconds to wait for gripper closing | [optional] 

#### Return type
None

### **freeze**
> robot.freeze()

Asking the arm to go to the freeze state

The function sets the arm in the \"freeze\" state. The arm stops moving, retaining its last position. ### Note: In the state, it is not advisable to move the arm by hand as this can cause damage to its components.

#### Parameters
This endpoint does not need any parameter.

#### Return type
None

### **get_base**
> Position robot.get_base()

Getting the actual position of the arm base

The function returns the actual position of the arm's zero point in the user environment. The actual zero point position is described as a set of x, y, and z coordinates, as well as _roll_, _pitch_, and _yaw_ rotation angles. The coordinates define the offset (in meters) from the physical centerpoint of the arm base (original zero point) to the actual zero point position along the x, y, and z axes accordingly. _Roll_ stands for the rotation angle around the x axis; _pitch_ - the rotation angle around the y axis; _yaw_ - the rotation angle around the z axis. All rotation angles are in radians and relative to the physical centerpoint of the arm base.

#### Parameters
This endpoint does not need any parameter.

#### Return type
**Position**

### **get_digital_output**
> Signal robot.get_digital_output(port)

Get level of digital output signal

The function returns the actual signal level on the digital output specified in the Port parameter. A digital output is a physical port on the back panel of the control box that controls the robotic arm.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **port** | **int**| The parameter indicates the number of the digital output on the back of the control box where you send the request to get the actual signal level. Since the control box has two digital outputs, the parameter value can be either 1 (corresponds to Relay output 1 on the control box) or 2 (corresponds to Relay output 2 on the control box). | 

#### Return type
**Signal**

### **get_pose**
> Pose robot.get_pose()

Getting the actual arm pose

The function returns the actual pose of the robotic arm. The pose is described as a set of output flange angles (in degrees) of all the six servos in the arm joints.

#### Parameters
This endpoint does not need any parameter.

#### Return type
**Pose**

### **get_position**
> Position robot.get_position()

Getting the actual arm position

The function returns the actual position of the PULSE robotic arm, which is described as a set of x, y, and z coordinates, as well as _roll_, _pitch_, and _yaw_ rotation angles. The coordinates define the actual distance (in meters) from the zero point of the robotic arm to the tool center point (TCP) along the x, y, and z axes accordingly. _Roll_ stands for the TCP rotation angle around the x axis; _pitch_ - the TCP rotation angle around the y axis; _yaw_ - the TCP rotation angle around the z axis. All rotation angles are in radians and relative to the zero point.

#### Parameters
This endpoint does not need any parameter.

#### Return type
**Position**

### **get_tool**
> Tool robot.get_tool()

Getting actual tool properties

The function returns the actual TCP position that accounts for the offset from the original TCP due to attaching/changing the work tool. The actual TCP position is described as a set of the following properties: - _name_ - any random name of the work tool defined by the user (e.g., \"gripper\") - _position_ - x, y, and z coordinates, as well as _roll_, _pitch_, and _yaw_ rotation angles. The coordinates define the distance (in meters) from the arm's zero point to the actual TCP along the x, y, and z axes accordingly. _Roll_ stands for the actual TCP rotation angle around the x axis; _pitch_ - the actual TCP rotation angle around the y axis; _yaw_ - the actual TCP rotation angle around the z axis. All rotation angles are in radians. - _radius_ - radius of the work tool (in meters) measured from its center point.

#### Parameters
This endpoint does not need any parameter.

#### Return type
**Tool**

### **identifier**
> str robot.identifier()

Getting the arm ID

The function returns the unique identifier (ID) of the robotic arm. The ID is an alphanumeric designation that consists of individual servo motor identifications.

#### Parameters
This endpoint does not need any parameter.

#### Return type
**str**

### **open_gripper**
> robot.open_gripper(timeout=timeout)

Asking the arm to open the gripper

The function commands the robot to open the gripper. It has no request body, but the user can optionally set how long (in milliseconds) the arm should remain idle, waiting for the gripper to open. The default manufacturer-preset value is 500 ms. ### Note: Setting the parameter, it is recommended to use values above 0 ms.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **timeout** | **float**| Time in milliseconds to wait for gripper opening | [optional] 

#### Return type
None

### **relax**
> robot.relax()

Asking the arm to relax

The function sets the arm in the \"relaxed\" state. The arm stops moving without retaining its last position. In this state, the user can move the robotic arm by hand (e.g., to verify/test a motion trajectory).

#### Parameters
This endpoint does not need any parameter.

#### Return type
None

### **run_poses**
> str robot.run_poses(poses, speed, type=type)

Asking the arm to move to a pose

The function allows for setting a trajectory of one or more waypoints to move the robotic arm smoothly from one pose to another. In the trajectory, each waypoint is a set of output flange angles (in degrees) of the six servos in the arm joints. Two motion types supported: linear and joint. Default: joint ### Note: Similarly, you can move the arm from one pose to another through one or more waypoints using the PUT/pose function. When the arm is executing a trajectory of PUT/pose waypoints, it stops for a short moment at each preset waypoint. With the PUT/poses/run function, however, the arm moves smoothly though all waypoints without stopping, which reduces the overall time of going from one pose to another.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **poses** | **list[Pose]** | Request Body | 
 **speed** | **float**| Speed of Robot | 

#### Return type
**str**

### **run_positions**
> str robot.run_positions(positions, speed, type=type)

Asking the arm to move to a position

The function allows for setting a trajectory of one or more waypoints to move the robotic arm smoothly from one position to another. Two motion types supported: linear and joint. Default: joint In the trajectory, each waypoint is described as a set of x, y, and z coordinates, as well as _roll_, _pitch_, and _yaw_ rotation angles. The coordinates define the distance (in meters) from the zero point to the desired TCP along the x, y, and z axes accordingly. _Roll_ stands for the desired TCP rotation angle around the x axis; _pitch_ - the desired TCP rotation angle around the y axis; _yaw_ - the desired TCP rotation angle around the z axis. All rotation angles are in radians.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **positions** | **list[Position]** | Request Body | 
 **speed** | **float** | Speed of Robot | 

#### Return type
**str**

### **set_digital_output_high**
> str robot.set_digital_output_high(port)

Set high level of digital output signal

The function sets the digital output specified in the Port parameter to the HIGH signal level. A digital output is a physical port on the back panel of the control box that controls the robotic arm.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **port** | **int**| The parameter indicates the number of the digital output on the back of the control box that you want to set to the HIGH signal level. Since the control box has two digital outputs, the parameter value can be either 1 (corresponds to Relay output 1 on the control box) or 2 (corresponds to Relay output 2 on the control box). | 

#### Return type
**str**

### **set_digital_output_low**
> str set_digital_output_low(port)

Set low level of digital output signal

The function sets the digital output specified in the Port parameter to the LOW signal level. A digital output is a physical port on the back panel of the control box that controls the robotic arm.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **port** | **int**| The parameter indicates the number of the digital output on the back of the control box that you want to set to the LOW signal level. Since the control box has two digital outputs, the parameter value can be either 1 (corresponds to Relay output 1 on the control box) or 2 (corresponds to Relay output 2 on the control box). | 

#### Return type
**str**

### **set_pose**
> str robot.set_pose(pose, speed, type=type)

Setting a new arm pose

The function commands the arm to move to a new pose. A pose is described as a set of output flange angles (in degrees) of the six servos integrated into the robot joints.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **pose** | **Pose** | Request Body | 
 **speed** | **float**| Speed of Robot | 

#### Return type
**str**

### **set_position**
> str robot.set_position(position, speed)

Setting a new arm position

The function commands the arm to move to a new position. The position is described as a set of x, y, and z coordinates, as well as _roll_, _pitch_, and _yaw_ rotation angles. The coordinates define the desired distance (in meters) from the zero point to the TCP along the x, y, and z axes accordingly. _Roll_ stands for the desired TCP rotation angle around the x axis; _pitch_ - the desired TCP rotation angle around the y axis; _yaw_ - the desired TCP rotation angle around the z axis. All rotation angles are in radians.

#### Parameters

Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **position** | **Position** | Request Body | 
 **speed** | **float** | Speed of Robot | 

#### Return type
**str**

### **status_motion**
> MotionStatus robot.status_motion()

Getting the actual motion status

The function returns the actual state of the robotic arm - whether it is running (in motion), or idle (not in motion), or in the zero gravity mode.

### Parameters
This endpoint does not need any parameter.

### Return type
**MotionStatus**

### **status_motors**
> list[MotorStatus] robot.status_motors()

Getting the actual status of servo motors

The function returns the actual states of the six servo motors integrated into the joints of the robotic arm. The states are described as arrays of values for the following properties: - _Angle_ - the actual angular position (in degrees) of the servo's output flange - _Rotor velocity_ - the actual rotor velocity (in RPM) - _RMS current_ - the actual input current (in Amperes) - _Phase current_ - the actual magnitude of alternating current (in Amperes) - _Supply voltage_ - the actual supply voltage (in Volts) - _Stator temperature_  - the actual temperature (in degrees C) as measured on the stator winding - _Servo temperature_ - the actual temperature (in degrees C) as measured on the MCU PCB - _Velocity setpoint_ - the user-preset rotor velocity (in RPM) - _Velocity output_ - the motor control current (in Amperes) based on the preset velocity - _Velocity feedback_ - the actual rotor velocity (in RPM) - _Velocity error_ - the difference between the preset and the actual rotor velocities (in RPM) - _Position setpoint_ - the user-preset position of the servo flange in degrees - _Position output_ - rotor velocity (in RPM) based on the position setpoint - _Position feedback_ - the actual position of the servo flange (in degrees) based on the encoder feedback - _Position error_ - the difference (in degrees) between the preset and the actual positions of the servo flange  Each property in an array has six values - one for each of the six servos in the arm joints.

#### Parameters
This endpoint does not need any parameter.

#### Return type
[**list[MotorStatus]**]

