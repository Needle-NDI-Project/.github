# Needle-NDI-Project

This project focuses on developing a robotic system for controlling a needle used in medical procedures such as prostate biopsies. The system integrates multiple subsystems, including state estimation, hardware interfacing, control algorithms, and path planning. Each subsystem is organized into its own ROS2 package, allowing for modular development and ease of maintenance.

The repositories included in this project are:

1. `ekf_system`
2. `ndi_sys`
3. `needle_controllers`
4. `needle_planning`
5. `zaber_system`

## Key Subsystems

### 1. EKF System (`ekf_system`)

Implements an Extended Kalman Filter (EKF) for state estimation by fusing data from various sensors, such as the needle position and tracker data. The EKF provides real-time estimations of the needle's pose, which are crucial for precise control and feedback.

### 2. NDI System (`ndi_sys`)

Manages communication with the NDI Aurora tracking system to retrieve real-time positional data of the needle and trackers. It publishes this data for use by other subsystems like the EKF system and visualization tools.

### 3. Needle Controllers (`needle_controllers`)

Implements various control algorithms to manipulate the needle based on desired trajectories and feedback from the EKF system. This includes feedback controllers (e.g., PID controllers) and methods like Broyden's method for Jacobian estimation.

### 4. Needle Planning (`needle_planning`)

Handles path planning for the needle using finite element models and machine learning techniques. It calculates optimal paths based on tissue stiffness models and provides services for path planning requests.

### 5. Zaber System (`zaber_system`)

Interfaces with Zaber motor hardware to control the physical movement of the needle along the x, y, and z axes. It translates high-level control commands into motor-specific instructions.

## ROS2 Package Layout

The project is organized into several ROS2 packages, each responsible for a specific functionality. This modular approach ensures scalability, ease of maintenance, and clear separation of concerns.

### Directory Structure

```plaintext
Needle-NDI-Project
├── ekf_system
├── ndi_sys
├── needle_controllers
├── needle_planning
└── zaber_system
```

## Interactions Between Packages

The subsystems interact through ROS2 topics and services to achieve coordinated control of the needle.

- **Data Flow**:
  - The **NDI System** publishes real-time positional data of the needle and trackers.
  - The **EKF System** subscribes to this data to estimate the needle's pose.
  - The **Needle Controllers** use the estimated pose and desired trajectories from the **Needle Planning** subsystem to compute control commands.
  - The **Zaber System** receives these commands and controls the physical movement of the needle.

- **Services**:
  - The **Needle Planning** package provides a service to calculate optimal paths based on current state and desired targets.

### Interaction Diagram

```plaintext
                                                         +---------------------+
                                                         |                     |
                                                         |  Needle Planning    |
                                                         |                     |
                                                         +----------+----------+
                                                                    |
                                                                    v
+------------------+         +-----------------+         +----------------------+
|                  |         |                 |         |                      |
|   NDI System     +-------> |   EKF System    +-------> | Needle Controllers   |
|                  |         |                 |         |                      |
+------------------+         +-----------------+         +-----------+----------+
                                                                    |
                                                                    v
                                                           +------------------+
                                                           |   Zaber System   |
                                                           |                  |
                                                           +------------------+
```

## Conclusion

This project is organized into modular ROS2 packages, each handling a specific aspect of the needle control system. This structure facilitates scalability, ease of development, and clear separation of concerns. By leveraging ROS2 for communication between subsystems, the project ensures real-time data sharing and coordination, essential for precise needle manipulation in medical procedures.

The integration of hardware interfaces (NDI Aurora and Zaber motors) with advanced algorithms (EKF, path planning, control algorithms) creates a robust framework for performing tasks like prostate biopsies with high precision and reliability.
