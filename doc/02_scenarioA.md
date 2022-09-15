# Test Scenario A

## About this scenario

[image](https://user-images.githubusercontent.com/56551323/190413958-3778b358-ed4e-479a-92b7-85f579e888ee.png)

Fig. 1: Scenario A

This is the scenario described in Sec. IV of our paper. Consider the HRC workstation in Fig. \ref{fig:scenario1} and the following collaborative task: the human worker starts in the centre area (A), retrieves a part from the table (B), places it in front of the robot (C), then walks back to the centre area (A), and activates the robot at the control panel (D). The robot performs a procedure on the part until the worker stops it through the control panel. The worker then retrieves the part and places it back on the table. As a safety measure, the area around the robot is monitored by a laser scanner (E, red area). A safety stop of the robot is triggered when the worker enters the detection area. To inspect the ongoing procedure from close distance without triggering a safety stop, the worker can override this safety function through the control panel. 

## EFA Modeling
Note that the names of actions and variables in our implementation do not correspond with those in Fig. 2 of the paper, since we had to choose more compact names in the paper for sake of brevity. Nevertheless, the automata models are functionally equivalent (we give the coresponding variable and event names from the paper in brackets). 

### Human
The human model H is depicted in Fig. 2. We model the human behavior with the following events: human_transition_1 (t_1) denotes a transition between area A and B and human_transition_2  (t_2) between A and C, as well as picking up/putting down the part at the storage table with human_pick_up_part_storage (u_S)/human_put_down_part_storage (d_S) and at the robot station with human_pick_up_part_robot (u_R)/human_put_down_part_robot (d_R), respectively. Also, the human can press several buttons to stop or activate the robot, which are modelled as events human_press_button_stop (b0), human_press_button_1 (b_1, start in normal mode), and human_press_button_2 (b_2,start in safety override mode). We introduce the variable part_status (P) to track the part (0: at storage, 1: in worker's hands, 2: at robot station) and the variables collaborative_workspace_occupied (W) lasr_scanner_occupied (S) to track if the human currently occupies the shared human-robot workspace or the laser scanner zone, respectively (0: not occupied, 1: occupied). Observe that some events require guard statements to be fulfilled before the transition is enabled. (e.g., to human_put_down_part_storage (d_S), the guard is part_status==1 (P=1), because the worker must first be in possession of the part to put it down).

![image](https://user-images.githubusercontent.com/56551323/190415835-18e659e6-ff51-4a68-8220-3007d6dfbc7e.png)
Fig. 2: Human Model H

### Robot

For the robot model R (see Fig. 3), a state is introduced for each operation mode, namely S0: idle (q_R0), S1: working (q_R1), and s2: working in safety override mode (q_R2). The events human_press_button_stop (b_0), human_press_button_1 (b_1) and human_press_button_2 (b_2) cause the robot to change its operation mode (note that the button-events are shared events appearing both in H and R, as the human also influences the robot state by performing these events). Additionally, the robot has an event robot_safety_stop (safety stop) which is enabled if the robot is running in normal operation mode and the laser scanner is occupied, as denoted by the guard laser_scanner_occupied==1 (S=1). Note that the safety stop is not available in safety override mode (i.e., in S2 (q_R2)). The variable robot_running (R) tracks if the robot is currently idle (0) or active (1).
Note that in our models, the EFAs do not include any information about the duration of events. For instance, in R, it is not determined whether the safety stop is executed immediately as the human enters the detection zone, or if the robot requires some stopping time. It is only stated that occupancy of the lser sacnner zone is a guard (i.e., a precondition) for the safety stop. This is deliberate: by leaving the question of timing open, we force the synthesis algorithm to consider all possible interleavings of events when searching for unsafe sequences. Whether these sequences are indeed possible under the actual timing behaviour of the system is determined in simulation, the second analysis step (compare our discussion in Sec. III-B of the paper).

![image](https://user-images.githubusercontent.com/56551323/190419807-01d94754-dcce-409e-a113-62ccd54bec85.png)

Fig. 3: Robot Model R

### Safety Specification
The safety specification SP simply has two locations S0 (q_SP0) (safe) and S1 (q_SP1) (unsafe). The unsafe location represents a collision. Observe that the unsafe location is marked, but only reachable through event collision (c), which has the guard statement (robot_running==1 and collaborative_workspace_occupied==1). In other words, we consider a state a potential collision state if the human is in the collaborative workspace and the robot is running at the same time.

Performing a supervisor synthesis of the system H||R with respect to the Specification SP yields a supervisor with 90 states and 182 transitions. 

