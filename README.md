# ROS Navigation stack local planner benchmark with Turtlebot3 Model

### Requirements: 
- ROS Noetic Desktop Full
- Git
- Ubuntu 20.04
- [Modified Navigation package](https://github.com/rureverek/navigation/tree/potential_field) for potential field planner

# Build and installation

If the requirements are met, jump to [Install package](#install-package).

### Install ROS:
```sh
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo apt update
sudo apt install ros-noetic-desktop-full
```

#### Source setup.bash

```sh
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
#### Init rosdep

```sh
sudo apt install python3-rosdep
sudo rosdep init
rosdep update
```
#### Init catkin
```sh
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
```
### Install git
`sudo apt install git`

## Install package

### Clone repo
```sh
cd ~/catkin_ws/src
git clone https://github.com/rureverek/turtlebot3_local_planner_benchmark.git
```
### Source workspace and Install dependencies
```sh
cd ~/catkin_ws
echo "source $(pwd)/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
rosdep install turtlebot3_local_planner_sim
```
### Build package
```sh
cd ~/catkin_ws
catkin_make
```
### Configure environment

in ~/.bashrc
```sh
echo "export TURTLEBOT3_MODEL=waffle_pi" >> ~/.bashrc
echo "export GAZEBO_PLUGIN_PATH=~/catkin_ws/src/turtlebot3_local_planner_benchmark/plugins" >> ~/.bashrc
source ~/.bashrc
```

# Run the simulation

**DWA Planner: `roslaunch turtlebot3_local_planner_sim run_simulation_dwa.launch scenario:=<name-of-scenario>`**

Standalone: 
```yaml
DWAPlannerROS:
  potential_planner: False
```
Potential Field planner:
```yaml
DWAPlannerROS:
  potential_planner: True
```
(param/dwa_local_planner_params_waffle_pi.yaml)

**TEB Planner: `roslaunch turtlebot3_local_planner_sim run_simulation_teb.launch scenario:=<name-of-scenario>`**

Scenarios currently supported: 

- `Static`
- `Dynamic_simple`
- `Dynamic_complex`
- `Maze`
- `Narrow`
- `House`
- `Track`
- `Big_house`
- `Big_house_part_unknown`

Note: 
- Letter case matters when specifying scenario name in the run command

Dwa init             |  Dwa just before collision
:-------------------------:|:-------------------------:
![halo](/img/rviz/dwa_dynamic_simple00.png)  |  ![](/img/rviz/dwa_dynamic_simple01.png)
Teb init             |  Teb avoiding dynamic obstacle
![](/img/rviz/teb_dynamic_simple00.png)  |  ![](/img/rviz/teb_dynamic_simple_01.png)
