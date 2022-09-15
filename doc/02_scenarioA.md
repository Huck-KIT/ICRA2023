# Test Scenario A

## About this scenario

file:///home/huck/Downloads/scenario_edited.png![image](https://user-images.githubusercontent.com/56551323/190413958-3778b358-ed4e-479a-92b7-85f579e888ee.png)

Fig. 1: Scenario A

This is the scenario described in Sec. IV of our paper. Consider the HRC workstation in Fig. \ref{fig:scenario1} and the following collaborative task: the human worker starts in the centre area (A), retrieves a part from the table (B), places it in front of the robot (C), then walks back to the centre area (A), and activates the robot at the control panel (D). The robot performs a procedure on the part until the worker stops it through the control panel. The worker then retrieves the part and places it back on the table. As a safety measure, the area around the robot is monitored by a laser scanner (E, red area). A safety stop of the robot is triggered when the worker enters the detection area. To inspect the ongoing procedure from close distance without triggering a safety stop, the worker can override this safety function through the control panel. 

## EFA Modeling

The human model H is depicted in Fig. 2. Note that the names of actions and variables do not correspond with those in the paper, since we had to choose more compact names in the paper for sake of brevity. Nevertheless, the automata models are functionally equivalent (we give the coresponding variable and event names from the paper in brackets). 

![image](https://user-images.githubusercontent.com/56551323/190415835-18e659e6-ff51-4a68-8220-3007d6dfbc7e.png)

Fig. 2: Human Model H

