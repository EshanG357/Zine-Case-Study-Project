# Introduction

Autonomous aerial vehicles are increasingly being used in environments where Global Positioning System (GPS) signals are unavailable, unreliable, or deliberately
denied. Such environments include indoor spaces, underground tunnels, disaster-hit areas, dense urban structures, and extraterrestrial surfaces.

The ISRO Robotics Challenge – URSC 2025 (IRoC-U 2025) focuses on designing an autonomous drone capable of navigating through a GPS-denied environments while
performing mission-specific tasks. This case study analyzes the key technical challenges associated with such type of navigation and explores feasible solutions using onboard sensor selection,
state estimation, environment perception, and autonomous planning.

The objective of this study is not to implement a physical drone system, but to conceptually design and analyze a robust navigation framework that can operate
independently of external positioning Software, Hardware and Infrastructure.

---

# Problem Statement

In conventional drone navigation systems, GPS plays a critical role in localization and global path planning. However, in GPS-denied environments, the absence of reliable
global position data leads to accumulated localization errors, sensor drift, and increased risk of collision.

The problem addressed in this case study is to determine how an autonomous drone can:
- Estimate its own position and orientation without GPS
- Perceive and map an unknown environment in real time
- Plan safe and efficient paths toward mission objectives
- Operate reliably under sensor noise and environmental uncertainty

This study proposes a sensor-driven navigation approach combining inertial sensing, visual perception, and mapping techniques to achieve autonomous operation in GPS-denied scenarios.
