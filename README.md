# General
This is a base setup for using ExoMy in Isaac Gym v3, without implemention of reward functions, etc.

# Useful commands
[Commands](docs/Commands.md)

# Install isaac-rover

<!---<details><summary>Docker (click to expand)</summary>--->

### Requirements

- Ubuntu 18.04, or 20.04.
- Python 3.6, 3.7, or 3.8
- Minimum recommended NVIDIA driver version: 470.74 (470 or above required)

  
### Dependencies
  ```bash
# Docker
curl https://get.docker.com | sh \
  && sudo systemctl --now enable docker
  
# Setting up NVIDIA Container Toolkit
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
# Install the nvidia-docker2 package (and dependencies) after updating the package listing:
sudo apt-get update
sudo apt-get install -y nvidia-docker2
# Restart the Docker daemon to complete the installation after setting the default runtime:
sudo systemctl restart docker

```
  
### 1. Download Isaac Gym
  
  1. Download Isaac Gym from https://developer.nvidia.com/isaac-gym
  2. Unzip Isaac Gym
  3. nano isaacgym/docker/run.sh
  4. Remove all text and paste
```
#!/bin/bash
set -e
set -u

if [ $# -eq 0 ]
then
    echo "running docker without display"
    docker run -it --network=host --gpus=all --name=isaacgym_container isaacgym /bin/bash
else
    export DISPLAY=$DISPLAY
	echo "setting display to $DISPLAY"
	xhost +
	docker run -it -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY --network=host --gpus=all --name=isaacgym_container isaacgym /bin/bash
	xhost -
fi
```
5. nano isaacgym/docker/Dockerfile
6. Insert the follwing code at the bottom of the file and save.

```
RUN git clone https://github.com/ExoMyRL/isaac_rover.git /home/gymuser/isaac_rover
RUN pip3 install -e /home/gymuser/isaac_rover/.
RUN git clone https://github.com/Toni-SM/skrl.git
RUN pip3 install -e skrl/.
WORKDIR /home/gymuser/isaac_rover

```
7. sudo groupadd docker
8. sudo gpasswd -a $USER docker
9. restart PC
10. bash docker/build.sh
11. bash docker/run.sh DISPLAYPORT
12. Enter container from different terminals --- sudo docker exec -it isaacgym_container bash 
  
13. cd isaac_rover 
14. pip install -e .

<!---</details>--->


### Installing Pytorch on Xavier from source
https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-10-now-available/72048

### Installing ROS on Xavier from source
https://www.miguelalonsojr.com/blog/robotics/ros/python3/2019/08/20/ros-melodic-python-3-build.html
