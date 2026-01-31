# Sensor Suite and State Estimation

In GPS-denied environments, a drone must rely on onboard sensors to estimate its position and orientation. Without GPS, traditional position
measurements are unavailable, and the navigation system must be designed to compute motion and location using only onboard measurements. The
combination of multiple sensors working together is known as the **sensor suite**, and the process of interpreting their data to estimate the drone’s
state is called **state estimation**.

## Sensor Suite

A typical sensor suite for GPS-denied navigation includes the following components:

- **Inertial Measurement Unit (IMU):**  
  Measures linear acceleration and angular velocity of the vehicle. IMUs provide high-frequency motion data but are subject to integration error
  (drift) over time.

- **Camera (Monocular or Stereo):**  
  Captures visual information from the environment. Vision sensors can be used for tracking features and computing relative motion through
  visual odometry.

- **LiDAR (optional):**  
  Uses laser pulses to measure distances to surrounding objects. LiDAR provides accurate depth information and can be used for 3D mapping and
  obstacle detection.

No single sensor is sufficient for accurate navigation. For example, IMUs drift over time, and cameras can fail in low-light or low-texture
areas. Combining multiple sensors minimizes the weaknesses of individual sensors.

## Sensor Suite Architecture

The sensor suite architecture defines how multiple onboard sensors are organized and how their data is processed to support autonomous
navigation in GPS-denied environments. Instead of relying on a single sensor, the system integrates complementary sensors to improve
robustness, accuracy, and reliability.

In the proposed architecture, the drone is equipped with an Inertial Measurement Unit (IMU), a vision sensor (camera), and optionally a LiDAR
sensor. Each sensor provides different information about the drone’s motion and surroundings.

The IMU continuously measures linear acceleration and angular velocity at a high update rate. This data is used to predict short-term motion but
suffers from accumulated drift over time. Vision sensors capture images of the environment and provide relative motion estimates by tracking
visual features across consecutive frames. LiDAR sensors, when used, offer accurate depth measurements and obstacle distance information.

All sensor data is fed into a central **state estimation module**, where sensor fusion techniques combine the strengths of individual sensors.
The estimated state—including position, velocity, and orientation—is then provided to higher-level navigation and control modules responsible
for path planning and safe maneuvering.

This modular architecture ensures that the failure or degradation of a single sensor does not result in total system failure, making it suitable
for challenging GPS-denied environments.

![Sensor Suite Architecture](../diagrams/sensor_suite_architecture.png)

*Figure 1: Sensor suite architecture for GPS-denied autonomous drone navigation.*

As shown in Figure 1, the onboard sensor suite consists of an IMU, a vision sensor, and an optional LiDAR unit. Each sensor provides
complementary information about the drone’s motion and environment. The sensor measurements are fused within the state estimation module
using techniques such as an Extended Kalman Filter (EKF) to generate a consistent estimate of the drone’s position, velocity, and orientation.
This estimated state is then used by the navigation and control module to perform path planning and execute safe maneuvers.


## State Estimation

State estimation is the process of using measurements from the sensor suite to estimate the current “state” of the drone. 
The state typically includes:
- Position
- Velocity
- Orientation (roll, pitch, yaw)

In a GPS-denied system, the drone must estimate these quantities using only onboard sensors.

### Sensor Fusion

A common approach to state estimation is **sensor fusion**, in which data from multiple sensors are fused to produce a more accurate estimate than
any single sensor alone. A widely used method for this purpose is the **Extended Kalman Filter (EKF)** which:
1. Uses IMU measurements to **predict** the drone’s motion at high
   frequency.
2. Uses observations from cameras or LiDAR to **correct** the predicted
   motion and reduce drift.

### Example: Visual-Inertial Odometry (VIO)

One example of sensor fusion is **Visual-Inertial Odometry (VIO)**. 
In VIO:
- The IMU predicts changes in position and orientation over time.
- Vision sensors track features in the environment and help correct
  accumulated IMU errors.
- The fusion algorithm (e.g., EKF) combines both to produce a
  consistent motion estimate.

This method enables the drone to estimate its state in real time without
GPS, making it suitable for navigation in indoor and GPS-denied spaces.
