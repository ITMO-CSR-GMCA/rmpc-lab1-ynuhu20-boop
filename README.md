[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/L0glmnMn)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23627353&assignment_repo_type=AssignmentRepo)
# RMPC-lab1

First laboratory work for course Robot Motion Planning and Control.

Assignment

1. For the selected robot option, load the manipulator model from the toolbox **different then Puma560** and output the Denavit-Hartenberg parameters.
2. Specify the masses of the links, the positions of their centers of mass, the tensors of inertia, the moments of inertia of the drives, the coefficients of viscous friction of the drives, the coefficients of Coulomb friction of the drives, the gear ratios of the reducers and the constraints on the generalized coordinates. It is recommended to refer to the data sheets of the manipulators used.
3. Specify and display arbitrary initial and final configurations of the robot.
4. Plan a trajectory between the specified configurations.
5. Solve the inverse dynamics problem using the Newton-Euler method for the following scenarios: <br>
+ non-zero velocities and accelerations $\dot{q} \neq 0, \ddot{q} \neq 0$;
+ non-zero velocities and negligible accelerations $\dot{q} \neq 0, \ddot{q} \approx 0$ - quasi-statics;
+ zero velocities and accelerations $\dot{q} = 0, \ddot{q} = 0$ - maintaining a given position;
6. Determine the numerical values ​​of the elements of the matrices $M(q), C(q, \dot{q}), G(q)$ at each calculated moment of time.
7. Plot graphs of the moments of the manipulator links along the trajectory for each scenario (see point #5).
8. Create a report in .ipynb format, with detailed comments.
9. All steps and conclusions are formulated based on the results of the work in README.md
  

LAB_1 REPORT SOLUTION
Lab 1 - Dynamic Analysis of KUKA KR5
Report Overview
This laboratory work focuses on the dynamic modelling and control of an industrial robotic manipulator. Instead of the standard Puma560, I selected the KUKA KR5 robot. The goal of this task is to understand how physical properties like mass, friction, and inertia affect the movement and required joint torques of a 6-axis robot during various motion scenarios.
1. Robot Configuration
The KUKA KR5 was initialized using its Standard Denavit-Hartenberg (DH) parameters.
	Joints: 6 Revolute joints (RRRRRR).
	Constraints: Physical joint limits were manually set to ensure the robot operates within its mechanical safety range (generalized coordinates).
2. Physical and Dynamic Parameters
To simulate a real-world environment, I assigned specific dynamic properties to each of the six links:
	Mass & Center of Mass: Each link was given a realistic mass (ranging from 1 kg to 6 kg) and a specific CoM vector.
	Inertia Tensors: Assigned values for Lxx, Lyy, Lzz to account for the distribution of mass.
	Actuator Dynamics: Added motor inertia (Jm), viscous friction (B), and Coulomb friction (Tc) to calculate the energy loss during motion.
	Gear Ratios: Set to 100 for all joints to represent the reducers used in industrial gearboxes.
3. Motion Planning
I defined two arbitrary configurations:
	Initial State (q_start): The "Zero" position where the robot stands vertically.
	Final State (q_end): A complex reaching pose.
A smooth trajectory was generated between these points using a quintic polynomial approach (jtraj), providing the position (q), velocity (q_dot), and acceleration (q_ddot) data for 50-time steps over a 2-second duration.
4. Inverse Dynamics Analysis
Using the Recursive Newton-Euler (RNE) method, I solved the inverse dynamics problem for three distinct scenarios:
	Full Dynamics (q_dot ≠ 0, q_ddot ≠ 0): Calculating the torque required for actual motion.
	Quasi-statics (q_dot ≠ 0, q_ddot ≈ 0): Calculating torque for very slow motion where acceleration forces are ignored.
	Static Maintenance (q_dot = 0, q_ddot = 0): Calculating the torque needed just to hold the robot against gravity.
5. Numerical Results & Visualization
I extracted the numerical matrices for Inertia (M), Coriolis (C), and Gravity (G) at the midpoint of the motion. Finally, I plotted the joint moments for all six links.
Joint Torque Plots Description:
Six subplots were generated (2 × 3 layout), each representing one joint (Link 1 to Link 6).
For each joint, three torque profiles were plotted:
	τ (solid line): Full dynamic torque (includes inertia, Coriolis, gravity, and friction effects)
	τ₀ (dashed line): Quasi-static torque (velocity present, acceleration ignored)
	τₛ (dotted line): Static torque (only gravity effect)
Axes:
	X-axis: Time (seconds)
	Y-axis: Torque (N·m)
Key Conclusions:
	Inertia Impact: The "Full Dynamics" scenario consistently showed higher torque peaks than the others, proving that acceleration (q_ddot) significantly impacts the energy required to move the robot.
	Gravity Dominance: In joints 2 and 3, the "Static" torque was high, showing that gravity is the primary force the robot must overcome to maintain its pose.
	Friction Effects: By comparing Quasi-static and Static scenarios, the influence of viscous and Coulomb friction becomes evident as the torque changes slightly even without acceleration.
