```{seo}
:description: Learn about the ROS 2 architecture in the Duckiedrone DD24-B, including nodes, topics, messages, publishers, and subscribers.
:keywords: Duckiedrone ROS 2, ROS 2 topics, ROS 2 nodes, ROS 2 messages, ROS 2 publishers, ROS 2 subscribers, DD24 software, robot operating system
```

(dd24-ros-basics)=
# ROS 2 (Robot Operating System)

## Overview

ROS 2 is middleware for exchanging typed data between *nodes*. A node is a named participant in the ROS graph; one process can host one or more nodes. Nodes communicate through *topics*, each of which has a *message* type that defines the data exchanged between publishers and subscribers.

## General Components

### ROS 2 discovery

ROS 2 does not use the ROS 1 master or `roscore`. On the Duckiedrone, ROS 2 components run in Docker containers. The `zenoh-router` container runs `rmw_zenohd`; the current ROS 2 containers use `ROS_DOMAIN_ID=42` and communicate through this router. See [](duckiedrone-containers) for the current container reference.

### ROS 2 nodes

ROS 2 nodes may publish to and subscribe from topics. The processes that host Duckiedrone nodes run inside containers rather than GNU Screen windows. For example, `ros2-camera` republishes the camera feed as a ROS 2 topic, while `ros2-mavros` connects the flight controller to ROS 2.

## Messages

ROS 2 messages are structured data types. A message can contain primitive fields, such as numbers and strings, as well as nested message types. To exchange data on a topic, publishers and subscribers must use the same message type and compatible quality-of-service (QoS) settings.

## Topics

Topics are named, typed channels over which nodes exchange messages. A topic can have multiple publishers and subscribers. The Duckiedrone's ROS 2 bridge containers expose data such as camera and Time-of-Flight readings to ROS 2 consumers.

## Publishers

Publishers send a specific message type to a topic. Sensor bridges publish readings, and control nodes can publish command information for the rest of the flight stack.

## Subscribers

Subscribers receive messages from a topic. A subscriber specifies the topic, message type, and QoS settings, then reacts when a compatible publisher sends a new message.
