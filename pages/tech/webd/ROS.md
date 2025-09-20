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


While there has been a significant shift towards hosting documentation within package repositories on platforms like GitHub and using tools like Sphinx for generation, the ROS Wiki still serves as a valuable resource, particularly for ROS 1.
Here's a breakdown:

• ROS 1 Documentation: Much of the documentation for ROS 1 packages continues to reside on the ROS Wiki (wiki.ros.org). While some package developers might also maintain documentation within their GitHub repositories, the Wiki remains a central hub for tutorials, package summaries, and general ROS 1 information.
• ROS 2 Documentation: For ROS 2, the approach has evolved. While initial development documentation was hosted on a GitHub wiki, it later moved to index.ros.org/doc/ros2. This content is generated from reStructuredText files maintained in the ros2/ros2_documentation repository on GitHub, allowing for a more structured review process. Package-specific documentation for ROS 2 is often found within the package's source repository on GitHub, typically in formats like reStructuredText or Markdown.
• Package-Level Documentation: A general trend in both ROS 1 and ROS 2 development is to include detailed documentation directly within the package's source repository (e.g., in a docs folder or README files). This makes the documentation directly accessible alongside the code and facilitates version control and contributions.
• ROS Index: The ROS Index (index.ros.org) provides a centralized way to discover and access documentation for both ROS 1 and ROS 2 packages, regardless of where the actual documentation files are hosted.

In summary, while GitHub plays a crucial role in hosting package source code and increasingly, package-level documentation, the ROS Wiki remains a significant resource for ROS 1, and dedicated documentation platforms like index.ros.org/doc/ros2 are used for ROS 2.
