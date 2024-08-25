# ROS 2 Development Container with Nvidia

This repository contains the `Dockerile` and the `devcontainer.json` configuration for VS Code. By default it requires an Nvidia GPU to run. The GPU is useful for running simulators like Gazebo from the docker container and achieve good FPS. Furthermore, using this container we can run Neural Networks using the GPU in ROS2 humble.

The container comes with the following ROS Humble packages already installed:
- ignition-fortress (for Gazebo)
- ros-humble-clearpath-simulator
- ros-humble-clearpath-nav2-demos

## Sources and other docs

- https://docs.ros.org/en/iron/How-To-Guides/Setup-ROS-2-with-VSCode-and-Docker-Container.html
- https://catalog.ngc.nvidia.com/orgs/nvidia/teams/isaac/containers/ros
- https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common

## Getting Started

These instructions will cover usage information and for the docker container 

### Prerequisities

Make sure that you have at least **15 GB** free. The final container will have ~5 GB but building it along with the other dependencies will take more space.

If running on windows, you will need to install [Windwos Subsystem for Linux (WSL)]("https://learn.microsoft.com/en-us/windows/wsl/install"), which you can will install and setup following the next section.

#### WINDOWS only:

Make sure your Nvidia drivers are up to date and you have VSCode installed.

Install WSL with:
- open a cmd or PowerShell **as administrator**
- run `wsl --install`
- **restart windows**

To open the WSL run `wsl` in a cmd or PowerShell. In the resulting windows, run `code .`

Make sure the Nvidia drivers are working with `nvidia-smi`

If everything is Ok, continue with the following sections. All the commands from now on must be run *inside* the WSL. 

### Setup

In order to run this container you'll need docker installed.

* [Linux](https://docs.docker.com/linux/started/)


#### Nvidia Drivers and the Nvidia Container Toolkit

- Make sure that your drivers are installed (proprietary, not nouveau) and up to date. Follow [these instructions for Ubuntu](https://ubuntu.com/server/docs/nvidia-drivers-installation). You can skip this for Windows/WSL.
- The Container Toolkit can be installed under Ubuntu from [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html). Follow the instructions from:
    - Installation
        - Prerequisites
        - Installing with Apt
    - Configuration
        - Prerequisites
        - Configuring Docker

You can also disable Nvidia if your PC does not have an Nvidia GPU or you encounter problems with it. To disable it:
- open the `.devcontainer/devcontainer.json` file 
- find the `"runArgs"` section
- use the `//` symbols to comment the last 2 lines:
```bash
"--gpus=all",
"--runtime=nvidia"
```


### Usage

Firstly, you need to open the root of the repository in VS Code as a workspace with the command:
```bash
code /path/to/the/repo/root
```

This repo contains a hidden folder `.devcontainer`. To see it, run 
```bash
ls -a
```

This hidden folder contains 2 files that we will need to configure: `Dockerfile` and `devcontainer.json`.

#### Configuration:

Find your username in the linux host, running `echo $USER` in the terminal and copy it. 

**`Dockerfile`**:
- find the line `USERNAME=CHANGE_ME`
- change `USERNAME_REPLACE_ME` with the linux username you copied

**`devcontainer.json`**:
- open the file in VS Code and search the file with `Ctrl + F`
- search for `USERNAME_REPLACE_ME` and replace all occurences with the linux username you copied 


#### Building the container:

The building procedure will take a lot of time (15 - 30 minutes). It needs to be run only the first time you want to use the docker and every time you modify either the `Dockerfile` or the `devcontainer.json` from the hidden directory.

To build the container:

- open the VS Code command palette using `Ctrl + Shift + P`
- search for `Dev Containers: Rebuild and Open in Container`

Wait for the building process to finish. After it is done, you can open a new terminal from the `+` sign in the right hand side of the bottom toolbar (if the toolbar is not open, use `Ctrl + ~`).


#### Open the container after it was built:

You can safely close the window and shut down the PC. The work you did *should* be saved inside the container and you can re-open it without having to rebuild. 

To re-open the container:

- open the VS Code command palette using `Ctrl + Shift + P`
- search for `Dev Containers: Reopen in Container`

The container should start quickly (under 1 minute).

#### Making changes permanent:

After you decide that you need another (ros) package permanently, you can configure the build process to install it automatically. For that, you need to modify the `.devcontainer/Dockerfile` file.

Example: if you want to install the `htop` utility in the container and make sure it stays installed:

- open `.devcontainer/Dockerfile`
- find the comment lines 
```Dockerfile
# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************
```
- add the next command after the comment
```Dockerfile
RUN apt install -y htop
```

**Notes**
- do not use sudo in the Dockerfile
- note the use of `-y`. It tells apt to always select Yes in [Y/n] prompts. Without this option, the container will fail to build.

#### Volumes

The root folder of the repository will be mounted inside the docker container. As such, all the files in the repository will be available inside the docker container and all the files you create in the docker container **inside** the `/home/YOUR_USERNAME/ws` will remain in the repository.

## Test the container:

- verify that the GPU is available using `nvidia-smi`. You should see info about your GPU.
- verify that you can install repositories from apt: `sudo apt install nvtop`. Test the installation with `nvtop`.
- verify that ros is working: `. /opt/ros/humble/setup.bash && ros2 run rviz2 rviz2`. Rviz should open.

## Authors

* **Victor Domsa** - *Initial work* - [Github](https://github.com/DomsaVictor)
