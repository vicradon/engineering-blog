---
title: Running ROS on two or more Computers
date: '2022-08-04'
tags: ['ROS', 'ROS-Noetic', 'Docker', 'Distributed Systems', 'Linux']
draft: false
summary: Robot Operating System (ROS) can run a distributed system of connected sensors, actuators, and controllers. This is possible throught the master-slave architecture. This tutorial will teach you how you can run ROS Noetic on two computers, and two Docker containers.
images: []
layout: PostSimple
canonicalUrl:
---

## Outline

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [How to Communicate between Two or More Machines using ROS](#how-to-communicate-between-two-or-more-machines-using-ros)
  - [Set up the ROS master](#set-up-the-ros-master)
  - [Run a ROS publisher](#run-a-ros-publisher)
  - [Start a listener node](#start-a-listener-node)
  - [Communicate between two Docker containers](#communicate-between-two-docker-containers)
- [Conclusion](#conclusion)

## Introduction

Components in a robotics/IoT system often need a way to communicate among themselves. Say you have an Arduino Uno board controlling a speaker and want that speaker to greet you whenever you step into your room. How do you go about building this? You might want to use face recognition such that when a camera recognizes your face, it sends a signal to the Arduino to power the speaker that greets you. How do you even go about this?

One effective solution to this problem is to use a single-board computer like a Raspberry Pi. The Raspberry Pi can connect with a camera and run a facial recognition script. But how will it communicate with your Arduino Uno? You can use ROS to ease that communication.

ROS (Robot Operating System) is a software middleware that allows two or more machines to communicate in a distributed system. ROS is not an operating system like Windows, macOS, or Linux. It is software that sits on top of an OS. This means ROS cannot run in a low-memory microcontroller like Arduino but can run in a single-board computer like a Raspberry Pi or RTOS.

This tutorial covers running ROS on an Ubuntu master node and a Docker slave node.

## Prerequisites

To follow along in this tutorial, you’ll need:

1. An Ubuntu 20.04 computer that will serve as the master node.
2. A different computer with Docker installed. This could be a Mac, Windows, or Linux computer.
3. An installation of ROS Noetic on the master node Ubuntu computer.
4. A local network that the master and slave will connect to. This could be a WiFi network, a phone Hotspot, or an Ethernet connection.

You can install ROS noetic on Ubuntu using [installation instructions on the ROS documentation](http://wiki.ros.org/noetic/Installation/Ubuntu). You can pull the noetic image into your Docker environment using the command below:

```python
docker run -it ros:noetic
```

## **How to Communicate between Two or More Machines using ROS**

You must plan your architecture to run ROS on two or more machines. ROS can power a distributed system with a master and one or more slaves (clients). So you could run a master instance on an Ubuntu machine while you run one slave on a Docker container.

![Master-Slave System in ROS](/static/images/master-slave-system.png)

### **Set up the ROS master**

To set up the ROS master, run the command below on your Ubuntu (master computer):

```bash
roscore
```

### **Run a ROS Publisher**

You can publish messages in ROS from a node or the `rostopic` CLI tool. To keep things simple, you will publish messages using `rostopic`. You need to export a ROS_IP environment variable before you can start publishing using `rostopic`. This variable is the IP address of the machine you are publishing from. You can run `hostname -I` in a terminal on your Ubuntu to get its network-assigned assigned IP address.

```bash
vicradon@vicradon:~$ hostname -I
192.168.0.124
```

Now that you have the IP address of the Ubuntu machine open a new terminal and export the IP address using the command below:

```bash
export ROS_IP=<YOUR_UBUNTU_IP>
```

In the same terminal where you exported the `ROS_IP`, run a publisher that will publish data to a `/hello` topic.

```bash
rostopic pub -r 10 /hello std_msgs/String "Hi, there"
```

The command above uses the `rostopic pub` command with a rate of 10 messages per second to send data of type [std_msgs/String](http://wiki.ros.org/std_msgs) to the `/hello` topic.

### Start a listener node

Now that you have set up the ROS publisher, you can run a listener that receives the published messages. You will be using Docker to set up this node. Run the command below to start a ROS Noetic docker container:

```bash
docker run -it ros:noetic
```

Ensure you connect the host machine of the docker container to the same network as the master node. Run the command below to connect your docker slave node to the master node. Replace the `<YOUR_MASTER_NODE_IP_ADDRESS>` with the IP address of your master node (Ubuntu machine).

```bash
export ROS_MASTER_URI=http://<YOUR_MASTER_NODE_IP_ADDRESS>:11311
```

You can then listen to the `/hello` topic using the command below:

```bash
rostopic echo /hello
```

This should produce a continuous stream of:

```bash
data: "Hi, there"
---
data: "Hi, there"
---
data: "Hi, there"
---
```

### Communicate between two Docker containers

You can communicate between two Docker containers in your ROS system. You must run a publisher on one container and a listener on another. Both containers will have to connect to the same master node to communicate. The modified architecture for this setup will look like the image below:

![Communicating between two Docker containers using ROS](/static/images/master-node-with-docker-containers.png)

#### Setting up the publisher docker container

You first need to stop the listener on the ROS Noetic container from the previous section, then get the container's IP address. Use the command below to get this IP address:

```bash
hostname -I
```

Finally, copy the returned IP and export it in an environment variable using the command below:

```bash
export ROS_IP=<YOUR_DOCKER_CONTAINER_IP>
```

You can then start a publisher that publishes to a `/learn` topic using the command below:

```bash
rostopic pub -r 10 /learn std_msgs/String "ROS Noetic"
```

#### Setting up the listener docker container

Open a new terminal and start a Docker container using the command below:

```bash
docker run -it ros:noetic
```

Now, export the `ROS_MASTER_URI` environment variable using the command below:

```bash
export ROS_MASTER_URI=http://<YOUR_MASTER_NODE_IP_ADDRESS>:11311
```

You can then run the listener on the `/learn` topic using the command below:

```bash
rostopic echo /learn
```

You should a continuous stream of “ROS Noetic”, like the snippet below:

```bash
data: "ROS Noetic"
---
data: "ROS Noetic"
---
data: "ROS Noetic"
---
data: "ROS Noetic"
```

## Conclusion

This tutorial taught you how to send data between two machines in a ROS network. You learned how to run a master node, publish to a topic, and listen to data from a topic. You also saw the possibility of running ROS on Docker containers. Finally, you learned how to communicate between two Docker containers in a ROS network.

If you’ll like to see more content on ROS, robotics, and microcontrollers, consider following me on [Twitter](https://twitter.com/vicradon) and [LinkedIn](https://linkedin.com/in/chukwujama-osinachi). Thanks for reading, Adios ✌🏾🧡.
