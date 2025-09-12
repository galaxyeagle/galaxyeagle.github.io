---
layout : page
title: ROS
date : 2025-09-12
---

ROS (Robot Operating System) isn’t an OS — it’s a middleware (framework) for robotics.

Think of it as Linux for robots: it lets you connect sensors, algorithms, and actuators into a single system.

Key concepts:

-  Nodes = small programs (e.g., IMU driver, SLAM algorithm, motor controller).
-  Topics = channels for sending messages (IMU publishes acceleration → EKF node subscribes).
-  Services / Actions = request–reply or long-running tasks.
-  Packages = collections of nodes, launch files, configs.

Since I am on on Ubuntu 22.04, I will install ROS2 Humble (LTS, supported until 2027).

Most ROS packages are hosted at [this](github.com/ros2) and [this](github.com/ros-planning) github organisations. You’ll use this for contributions later.
