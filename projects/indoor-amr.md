flowchart TB
    subgraph Sensors ["Hardware & Embedded Layer"]
        ENC["Wheel Encoders"]
        IMU["IMU"]
        LIDAR["2D LiDAR"]
        MCU["Microcontroller<br/>(PID Motor Control & Serial Bridge)"]
        ENC --> MCU
    end

    subgraph Control ["ros2_control & State Estimation"]
        HWI["Custom ros2_control<br/>Hardware Interface"]
        DIFF["Diff-Drive Controller<br/>(Raw Odometry)"]
        EKF["robot_localization (EKF)<br/>& TF2 Tree"]
        MCU <== "Serial" ==> HWI
        HWI <--> DIFF
        DIFF --> EKF
        IMU --> EKF
    end

    subgraph Nav ["Mapping, Localization & Navigation (Nav2)"]
        SLAM["SLAM Toolbox<br/>(Occupancy Grid Mapping)"]
        AMCL["AMCL<br/>(Probabilistic Localization)"]
        PLAN["Nav2 Planners & Controllers<br/>(Global Path & Local Motion)"]
        BT["Behavior Trees<br/>(Mission Execution)"]
        LIDAR --> SLAM
        LIDAR --> AMCL
        EKF --> SLAM
        EKF --> AMCL
        AMCL --> PLAN
        BT --> PLAN
        PLAN -- "/cmd_vel" --> DIFF
    end
