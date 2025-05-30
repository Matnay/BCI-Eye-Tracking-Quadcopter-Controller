# BCI-Eye-Tracking-Quadcopter-Controller
Quadcopter Control by fusing EEG data along with eye tracking data using the Extended Kalman Filter

Eviacam was used for gazetracking ,
You can download the software using the steps given below-

```
sudo apt-get update
sudo apt-get install eviacam
```
All algorithms can be simulated on the px4 SITL simulator using Gazebo.
https://github.com/PX4/sitl_gazebo

![Image EXAMPLE RESULT1](https://github.com/Matnay/Quad_BCI_EYE_TRACK_FUSION_EKF/blob/master/Pictures/Screenshot%20from%202019-06-30%2019-06-19.png)
For setting up your workspace and execution of files;

STEP 1:
installation of mavros

Install all dependencies

```
sudo apt install -y \
	ninja-build \
	exiftool \
	python-argparse \
	python-empy \
	python-toml \
	python-numpy \
	python-yaml \
	python-dev \
	python-pip \
	ninja-build \
	protobuf-compiler \
	libeigen3-dev \
	genromfs

pip install \
	pandas \
	jinja2 \
	pyserial \
	cerberus \
	pyulog \
	numpy \
	toml \
	pyquaternion
```
![Image EXAMPLE RESULT2](https://github.com/Matnay/Quad_BCI_EYE_TRACK_FUSION_EKF/blob/master/Pictures/Screenshot%20from%202020-03-13%2023-10-28.png)
Create a new workspace:	
```	
mkdir -p ~/catkin_ws/src
```
and run the following commands:
```
cd ~/catkin_ws
catkin init && wstool init src

rosinstall_generator --rosdistro kinetic mavlink | tee /tmp/mavros.rosinstall
rosinstall_generator --upstream mavros | tee -a /tmp/mavros.rosinstall

wstool merge -t src /tmp/mavros.rosinstall
wstool update -t src -j4
rosdep install --from-paths src --ignore-src -y

sudo ./src/mavros/mavros/scripts/install_geographiclib_datasets.sh
sudo apt install ros-kinetic-catkin python-catkin-tools

cd ~/catkin_ws/src
git clone https://github.com/PX4/Firmware.git
cd Firmware
git checkout v1.8.0
make posix_sitl_default gazebo

catkin build
```

* Open a new terminal and type:
```
sudo gedit ~/.bashrc
```
* Add the following lines to the file that opens:
```
source ~/catkin_ws/devel/setup.bash
source ~/catkin_ws/src/Firmware/Tools/setup_gazebo.bash ~/catkin_ws/src/Firmware/ ~/catkin_ws/src/Firmware/build/posix_sitl_default
export ROS_PACKAGE_PATH=ROS_PACKAGE_PATH:~/catkin_ws/src/Firmware
export ROS_PACKAGE_PATH=ROS_PACKAGE_PATH:~/catkin_ws/src/Firmware/Tools/sitl_gazebo
```
* Create a new workspace and copy the codes into the 'src' directory
 The package can directly be placed in the /src of your package
 Run the scripts by
```
cd <workspace_name>
source devel/setup.bash
rosrun <package_name> <script_name.py>
```
![Image EXAMPLE RESULT3](https://github.com/Matnay/Quad_BCI_EYE_TRACK_FUSION_EKF/blob/master/Pictures/Screenshot%20from%202020-05-01%2015-56-23.png)

FUTURE WORK
* Implement own DL agorithm for robust classification
