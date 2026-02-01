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

This method enables the drone to estimate its state in real time without GPS, making it suitable for navigation in indoor and GPS-denied spaces.

## State Estimation Pipeline

The state estimation pipeline defines the sequence of operations used to estimate the drone’s position, velocity, and orientation using onboard
sensor data. In GPS-denied environments, this pipeline operates continuously to compensate for sensor noise and drift.

### Sensor Data Acquisition
The pipeline begins with data acquisition from onboard sensors. 
IMU measurements provide high-frequency acceleration and angular velocity data, while vision sensors and LiDAR provide environmental observations
such as visual features and depth information.

### Prediction Step
In the prediction step, IMU data is integrated using a motion model to predict the drone’s state at the next time step. Although this prediction
is fast and responsive, it accumulates error over time due to sensor noise and bias.

### Measurement Update
Vision or LiDAR measurements are used to correct the predicted state. 
These measurements provide relative motion or distance information that helps reduce accumulated drift.

### Sensor Fusion
An Extended Kalman Filter (EKF) fuses the predicted state with sensor measurements to produce an optimal state estimate. This fusion process
balances the fast response of inertial sensing with the accuracy of external observations.

### Output State
The final estimated state—including position, velocity, and orientation—is passed to the navigation and control module for path
planning and stable flight control.

## Sensor Limitations, Noise, and Drift Accumulation

All onboard sensors used in GPS-denied navigation are affected by noise, bias, and environmental limitations. Understanding these limitations is
critical for designing a reliable state estimation system.

### IMU Limitations and Drift
Inertial Measurement Units (IMUs) provide high-frequency acceleration and angular velocity measurements. 
However, IMU sensors are affected by noise and bias, which accumulate over time when integrated. 
Since position estimation requires double integration of acceleration, even small measurement errors can result 
in significant drift in position and orientation estimates.

### Vision Sensor Limitations
Vision sensors are commonly used to correct inertial drift by tracking visual features in the environment. However, their performance depends
on environmental conditions such as lighting, texture availability, and camera motion. Poor lighting, motion blur, or featureless surfaces can
temporarily degrade visual measurements.

### LiDAR Limitations
LiDAR sensors provide accurate depth and distance measurements and are less sensitive to lighting conditions. Despite their accuracy, LiDAR
systems increase payload weight and computational requirements and may struggle with reflective or transparent surfaces.

### Noise and Drift Accumulation
Sensor noise introduces random fluctuations in measurements, while drift refers to systematic error accumulation over time. 
In GPS-denied environments, drift is particularly problematic because there is no external reference to correct long-term errors. 
Sensor fusion techniques mitigate these effects by combining fast inertial predictions with corrective observations from vision or depth sensors.

### Summary of Sensor Characteristics and Limitations

| Sensor | Information Provided | Strengths | Limitations | Typical Errors |
|------|----------------------|----------|-------------|----------------|
| IMU | Acceleration, angular velocity | High update rate, independent of environment | Error accumulates over time | Noise, bias, drift |
| Camera | Visual features, relative motion | Rich environmental information | Sensitive to lighting and texture | Motion blur, feature loss |
| LiDAR | Depth and distance measurements | Accurate range sensing, lighting independent | High cost and computational load | Measurement noise, reflection errors |

The complementary strengths and weaknesses of these sensors motivate the use of sensor fusion techniques to achieve reliable state estimation
in GPS-denied environments.

## Validation Strategy

In GPS-denied navigation systems, reliable validation of the state estimation pipeline is essential due to the absence of external
positioning references. In this case study, validation is performed using a combination of simulation-based evaluation and sensor degradation
analysis to assess accuracy, stability, and robustness.

### Simulation-Based Validation
A simulation-based approach is used to evaluate the performance of the state estimation pipeline under controlled conditions. The simulated
environment provides access to ground-truth position and orientation data, which allows the estimated state to be compared against known
reference trajectories. Virtual IMU, camera, and LiDAR sensors are configured with realistic noise and bias characteristics to reflect
real-world operating conditions.

### Evaluation Metrics
The performance of the estimation pipeline is evaluated using metrics such as position error, orientation error, and drift accumulation over
time. These metrics help assess the accuracy and stability of the estimated state.

### Comparative Analysis
Different sensor configurations are compared to evaluate the benefit of sensor fusion. An IMU-only configuration exhibits rapid drift, while the
addition of vision-based measurements significantly reduces error. The integration of LiDAR further improves robustness and stability in complex
environments.

### Sensor Degradation Analysis
To evaluate robustness, the state estimation pipeline is further analyzed under sensor degradation scenarios. These scenarios include increased
IMU noise, reduced camera frame rates, and temporary loss of visual measurements. The objective is to observe how the system behaves when
sensor data quality deteriorates.

A robust sensor fusion-based estimation system is expected to degrade gracefully rather than fail completely. The continued operation of the
pipeline under degraded sensor conditions demonstrates the effectiveness of multi-sensor fusion in handling real-world uncertainties and sensor
imperfections.

Together, these validation strategies demonstrate that the proposed state estimation pipeline is both accurate and resilient, making it
suitable for autonomous navigation in GPS-denied environments.

### Real-World Alignment

The proposed state estimation and sensor fusion approach is consistent with navigation strategies used in real-world autonomous systems
operating in GPS-denied environments. A notable example is the *Ingenuity Mars Helicopter*, which relies on inertial sensing and
vision-based navigation to estimate its state in the absence of GPS. Similar visual-inertial fusion techniques are also employed in indoor
inspection and search-and-rescue drones, where external positioning infrastructure is unavailable or unreliable.

With a reliable state estimation pipeline established, the next stage focuses on how the drone perceives and maps its surrounding environment
to support safe navigation.

