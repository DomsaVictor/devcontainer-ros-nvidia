FROM ros:humble
# FROM nvcr.io/nvidia/isaac/ros:x86_64-ros2_humble_45d368cdbbe4a484643464d0d492c764
ARG USERNAME=CHANGE_ME
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES all

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME 
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
RUN apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
RUN apt-get update && apt-get upgrade -y --allow-downgrades
# Install packages from apt
RUN apt-get install -y python3-pip
RUN apt-get install -y ignition-fortress
RUN apt install -y ros-humble-clearpath-desktop
# these 2 might already be installed, but for now we leave them here.
RUN apt-get install -y ros-humble-clearpath-simulator
RUN apt-get -y install ros-humble-clearpath-nav2-demos
RUN mkdir -p /home/$USERNAME/ws
ENV SHELL /bin/bash

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME
CMD ["/bin/bash"]