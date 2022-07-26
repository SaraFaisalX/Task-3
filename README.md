# **Robot arm**

Write those instruction on the terminal

`sudo apt-get install ros-noetic-catkin`

`mkdir -p ~/catkin_ws/src`

`cd ~/catkin_ws/`

`catkin_make`

`cd ~/catkin_ws/src`

`git clone https://github.com/smart-methods/arduino_robot_arm.git `

`cd ~/catkin_ws`

`rosdep install --from-paths src --ignore-src -r -y`

`sudo apt-get install ros-noetic-moveit`

`sudo apt-get install ros-noetic-joint-state-publisher ros-noetic-joint-state-publisher-gui`

`sudo apt-get install ros-noetic-gazebo-ros-control joint-state-publisher`

`sudo apt-get install ros-noetic-ros-controllers ros-noetic-ros-control`

`sudo nano ~/.bashrc`

at the end of the (bashrc) file add the follwing line
(`source /home/sara/catkin_ws/devel/setup.bash`)
then 
ctrl + o

`source ~/.bashrc`

`roslaunch robot_arm_pkg check_motors.launch`

![image](https://user-images.githubusercontent.com/108310176/180918170-8fe34c60-c7b4-462a-b3f4-6f432e183499.png)

# **Circuit diagram**
![image](https://user-images.githubusercontent.com/108310176/180919255-f17f10f4-c74a-4588-ab19-c02e12fde6f2.png)

# **Robot initial positions**
![image](https://user-images.githubusercontent.com/108310176/180919597-65d1e949-531c-4a9a-957c-660fd46baa04.png)

# **Configuring Arduino with ROS**
**Install Arduino IDE in Ubuntu**

write this instruction on the terminal

`sudo ./install.sh`

Install the arduino package and ros library

 http://wiki.ros.org/rosserial_arduino/Tutorials/Arduino%20IDE%20Setup

Make sure to change the port permission before uploading the Arduino code 

`sudo chmod 777 /dev/ttyUSB0`

![image](https://user-images.githubusercontent.com/108310176/180920705-504d37bd-b24f-4a3d-8735-a958f7b2a38e.png)

# **Usage**

**Controlling the robot arm by joint_state_publisher**

 `roslaunch robot_arm_pkg check_motors.launch`

**You can also connect with hardware by running:**

 `rosrun rosserial_python serial_node.py _port:=/dev/ttyUSB0 _baud:=115200`

## **Simulation**
**Run the following instructions to use gazebo**

` roslaunch robot_arm_pkg check_motors.launch`

 `roslaunch robot_arm_pkg check_motors_gazebo.launch`

 `rosrun robot_arm_pkg joint_states_to_gazebo.py`

`sudo chmod +x ~/catkin_ws/src/arduino_robot_arm/robot_arm_pkg/scripts/joint_states_to_gazebo.py`

**Controlling the robot arm by Moveit and kinematics**

 `roslaunch moveit_pkg demo.launch`

**You can also connect with hardware by running:**


 `rosrun rosserial_python serial_node.py _port:=/dev/ttyUSB0 _baud:=115200`

## **Simulation**
**Run the following instruction to use gazebo**

` roslaunch moveit_pkg demo_gazebo.launch`

# **Pick and place by using OpenCV**

## **Preparation**
**Download webcam extension for VirtualBox**

`https://scribles.net/using-webcam-in-virtualbox-guest-os-on-windows-host/`

# **Testing the camera and OpenCV**
**Run color_thresholding.py to test the camera**

**Before running, find the camera index normally it is video**

 `ls -l /dev | grep video`

**If it is not, update line 8 in color_thresholding.py**

`8 cap=cv2.VideoCapture(0)`

**Then run**

` python color_thresholding.py`

# **Using OpenCV with the robot arm in ROS**

# **In Real Robot**

**In a terminal run**

` roslaunch moveit_pkg demo.launch`

**this will run Rviz**

**connect with Arduino:**
**1- select the Arduino port to be used on Ubuntu system**

**2- change the permissions (it might be ttyACM)**

` ls -l /dev | grep ttyUSB`

 `sudo chmod -R 777 /dev/ttyUSB0`

**3-upload the code from Arduino IDE**

 `rosrun rosserial_python serial_node.py _port:=/dev/ttyACM0 _baud:=115200`

**In another terminal**
 `rosrun moveit_pkg get_pose_openCV.py`

**This will detect blue color and publish the x,y coordinates to /direction topic**

**(Note: check the camera index and update the script if needed)**

**Open another terminal**
` rosrun moveit_pkg move_group_node`

**This will subscribe to /direction topic and execute motion by using Moveit move group**

**The pick and place actions are performed from the Arduino sketch directly.**

# **In simulation (Gazebo)**
**In a terminal run**
 `roslaunch moveit_pkg demo_gazebo.launch`

**this will run Rviz and gazebo**

**In another terminal**
 `rosrun moveit_pkg get_pose_openCV.py`

**This will detect blue color and publish the x,y coordinates to /direction topic**


**Open another terminal**
 `rosrun moveit_pkg move_group_node`

**This will subscribe to /direction topic and execute motion by using Moveit move group**

**We can’t visualize the pick and place actions in gazebo**
