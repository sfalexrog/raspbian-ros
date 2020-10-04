FROM sfalexrog/raspios:buster-2020-08-24

# install packages
RUN apt-get update && apt-get install -q -y \
    dirmngr \
    gnupg2 \
    && rm -rf /var/lib/apt/lists/*

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 \
    && curl http://deb.coex.tech/aptly_repo_signing.key 2> /dev/null | apt-key add -

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu buster main" > /etc/apt/sources.list.d/ros1-latest.list \
    && echo "deb http://deb.coex.tech/opencv4 buster main" > /etc/apt/sources.list.d/opencv4.list \
    && echo "deb http://deb.coex.tech/ros buster main" > /etc/apt/sources.list.d/coex-ros.list

# FIXME: Add COEX rosdep definitions
#COPY ./assets/melodic-rosdep-clever.yaml /etc/ros/rosdep/

# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    python3-rosdep \
    python3-rosinstall \
    python3-vcstools \
    && rm -rf /var/lib/apt/lists/*

# setup environment variables
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# setup rosdep
# FIXME: Re-add "yaml file:///etc/ros/rosdep/noetic-rosdep-clover.yaml" to /etc/rosdep/sources.list.d
RUN rosdep init \
    && rosdep update

# install base ROS system
ENV ROS_DISTRO noetic
RUN apt-get update && apt-get install -y \
    ros-noetic-ros-core \
    && rm -rf /var/lib/apt/lists/*

COPY ./assets/ros_entrypoint.sh /

ENTRYPOINT [ "/ros_entrypoint.sh" ]
CMD [ "/bin/bash" ]
