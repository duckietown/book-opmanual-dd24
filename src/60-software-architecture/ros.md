```{seo}
:description: Learn about the ROS 2 architecture in the Duckiedrone DD24-B, including nodes, topics, messages, publishers, and subscribers.
:keywords: Duckiedrone ROS 2, ROS 2 topics, ROS 2 nodes, ROS 2 messages, ROS 2 publishers, ROS 2 subscribers, DD24 software, robot operating system
```

(dd24-ros-basics)=
# ROS 2 (Robot Operating System)

## Overview
ROS 2 is middleware that lets processes, known as *nodes*, communicate through *topics*. Each topic has a *message* type that defines the data exchanged between publishers and subscribers.

## General Components

### ROS 2 discovery
ROS 2 does not use the ROS 1 master or `roscore`. On the Duckiedrone, ROS 2 components run in Docker containers, and the `zenoh-router` container lets them discover and communicate with each other. See [](duckiedrone-containers) for the current container reference.

### ROS 2 nodes
ROS 2 nodes are processes that publish to and subscribe from topics. On the Duckiedrone, nodes run inside containers rather than GNU Screen windows. For example, `ros2-camera` republishes the camera feed as a ROS 2 topic, while `ros2-mavros` connects the flight controller to ROS 2.

## Messages
ROS messages are structured data types. A message can contain primitive fields, such as numbers and strings, as well as nested message types. Publishers and subscribers on a topic must use compatible message types.

## Topics
Topics are named channels over which nodes exchange messages. A publisher writes messages to a topic, and one or more subscribers receive them. The Duckiedrone's ROS 2 bridge containers expose data such as camera and Time-of-Flight readings to ROS 2 consumers.

## Publishers
Publishers send a specific message type to a topic. They make sensor readings and control information available to the rest of the flight stack.

## Subscribers
Subscribers receive messages from a topic. A subscriber specifies the topic and compatible message type, then reacts whenever a new message arrives.
