# Intro 
This repo introduces the steps to setup ros-1 docker under ubuntu. \
The ROS image built by this method will support GUIs such as rviz.

## Target version the docker will build
Ubuntu: 18.04
ROS: ROS 1 Melodic

## Step 0, Dependancies
Make sure nvidia-docker2 package is installed

## Step 1, download 
download Dockerfile from this repo

## Step 2, docker build for Nvidia GPU
under the same folder with file downloaded at step 1, run the following command to build docker image
```docker build --rm --pull --build-arg from=nvidia/opengl:1.0-glvnd-runtime-ubuntu18.04 -t ros:nvidia .```

## run ROS image
Pre-setup
```
docker network create foo
xhost +local:docker
```

Terminal 1
```
nvidia-docker run \
              -it --rm \
              --net foo \
              --name master \
              ros:nvidia \
              roscore
```

Terminal 2
```
nvidia-docker run \
              -it --rm \
              --net foo \
              --name talker \
              -v /tmp/.X11-unix:/tmp/.X11-unix \
              --env DISPLAY=unix$DISPLAY \
              --env ROS_HOSTNAME=talker \
              --env ROS_MASTER_URI=http://master:11311 \
              -v /home/xiaojun/Desktop/SDA2/code/lexus_lidar_debug:/opt/ros/melodic/share/catkin_ws/src \
              ros:nvidia
```

if needed, can create more ros entries, Terminal-i
```
nvidia-docker exec -it talker bash
source /opt/ros/melodic/share/catkin_ws/devel/setup.bash
...
```


## Resources
Make sure the ros installation key is the latest (the old one is not valid anymore) inside Dockerfile
```
https://answers.ros.org/question/325039/apt-update-fails-cannot-install-pkgs-key-not-working/
```

Install Nvidia-Docker 2.0 on Ubuntu 18.04
```
https://medium.com/@linhlinhle997/how-to-install-docker-and-nvidia-docker-2-0-on-ubuntu-18-04-da3eac6ec494
```

