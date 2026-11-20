
flowchart LR
    subgraph Planning ["Motion Planning & Kinematics (ROS2)"]
        M2["MoveIt 2<br/>(Motion Planning & Trajectory Gen)"]
        KDL["KDL & TF2<br/>(FK / IK / Differential Kinematics)"]
        M2 <--> KDL
    end

    subgraph Control ["Real-Time Control Layer"]
        JTC["Joint Trajectory Controller<br/>(ros2_control)"]
        HW["Custom Hardware Interface<br/>(SystemInterface C++)"]
        JTC <--> HW
    end

    subgraph Targets ["Execution Targets"]
        SIM["Gazebo / RViz2<br/>(Simulation Validation)"]
        MCU["Microcontroller & Drivers<br/>(Physical Actuators & Encoders)"]
    end

    M2 --> JTC
    HW -. "Sim Mode" .-> SIM
    HW == "Serial / CAN" ==> MCU
