# Test Scenario B

## About this scenario
![Fig. 1](https://user-images.githubusercontent.com/56551323/190410401-a6365186-1b3a-4371-a55a-c68cdc2d82d6.png)

Fig. 1: Scenario B

In this scenario, we consider a HRC assembly workflow depicte in Fig. 1. The workflow is as follows: the worker retrieves parts from a shelf (A), inserts them into a housing and activates the robot with a button (D) which then inserts a gearwheel (E) into the housing. Meanwhile, the worker inserts a part into the cover (C) and finally mounts the cover onto the housing. Potential hazards consist in the hand being crushed between gearwheel and housing, and the head colliding with the robot's elbow joint. (further details on on GitHub and in the accompanying video). The robot then starts to put a gearwheel into the housing. Meanwhile, the human reaches into the workpiece cover and then mounts the cover onto the housing. This workflow is inspired by a [real-world HRC application](https://youtu.be/02TzqIvWiso).

## EFA modelling
We model the human worker, the robot, and the workpiece by EFAs.

### Human
Fig. 2 shows the human model H, where we model the events "human_transition" to represent the transitioning between the shelf and the workstation, "human_reach_parts" (representing the retrieval of parts from the shelf), "human_press_button" (activates the robot by setting the shard variable robot_activated=1), "human_reach_in_housing", "human_reach_in_cover", and "human_mount_cover". We further introduce the variables "parts_taken" to track whether the human has retrieved parts from the shelf (which is a precondition for further assembly actions), "cover_mounted" to track whether the human has already mounted the cover on the workpiece (note that the human cannot reach into housing or cover after the cover is moutned, as shown by the guard statement "cover_mounted==0". Further , we use the variable "collab_workspace_occupied_human" to track whether the human is currently reaching into the collaborative workspace (which is the case for all reaching motions). Note that the action "retract_hand" sets the variable "collab_workspace_occupied_human" back to zero.

![Fig. 1](https://user-images.githubusercontent.com/56551323/190402050-ed328d50-de6b-4b0d-911b-42db1c50542b.png)

Fig. 2: Human model H

### Robot
Fig. 3 shows the robot model R. The robot is initially in idle mode (S0). After the human has activated it (denoted by the guard robot_activated==1), the robot moves to the housing, thereby entering the collaborative workspace (see action collab_workspace_occupied_robot=1), and inserting the gearwheel (Note: the guard gearwheel_inserted==0 and the action gearwheel inserted=1 ensure that the gearwheel can only be inserted once). Afterwards, the robot moves bach, clearing the collaborative workspace and going back into idle mode (as denoted by the actions collab_workspace:occupied_robot=0 and robot_activated=0).   

![Fig. 2](https://user-images.githubusercontent.com/56551323/190404921-c7454263-8c09-400a-9f3a-48f2971094fd.png)

Fig. 3: Robot model R

### Workpiece
Note that in contrast to Scenario A, where we only modelled human and robot, scenario B also includes a model of the workpiece, since in this case, a more complex assembly procedure is considered where the workpiece can be in different states of the assembly procedure, and the feasible worker actions depend on the current assembly state of the workpiece. The workpiece model is shown in Fig. 4. Note that the workpiece model has no "own" events, but only shared actions with human and robot, as the workpiece changes state, but only through being maninupated my human or worker, but not autonously. In S0, the workpiece is in its initial assembly state. When the gearwheel is inserted, the workpiece changes its state to S1, and when the cover is mounted, to S2. Note that the assemly is unidirectional, i.e., we do not consider any disassembly steps. This is a simplifying assumption to keep modeling complexity at manageable. Also note that the feasible actions of the worker depend on the worpiece state. For instance, the event "human_reach_in_cover" is not feasible in S2 where the cover is already mounted. Also, the event "human_reach_in_housing" is not feasible when a gearwheel is already inserted (S1) or when the cover is mounted (S2).

![Fig. 3](https://user-images.githubusercontent.com/56551323/190406251-efb3772a-3541-4c4b-8c5b-49ccc5857115.png)

Fig. 4: Workpiece model W

The complete model of the collaborative system is given by the synchronous composition H||R||W and has 
161 states and 367 transitions. It is not depicted here due to its size.

### Safety Specification
The safety specification SP is shown in Fig. 5. It is very concise, simply consisting of an unmarked state (safe, S0) and a marked state (unsafe, S1). An unsafe state is defined as any state where both human and robot occupy the collaborative workspace at the same time (denoted by the guard collab_workspace_occupied_human == 1 & collab_workspace_occupied_robot == 1).

![image](https://user-images.githubusercontent.com/56551323/190409162-87739a43-7595-4f75-8827-670ecb3ed191.png)

Fig. 5: Safety Specification SP

Performing a supervisor synthesis for the plant H||R||W w.r.t. the specification SP results in a supervisor with 117 states and 264 transitions, which is not depicted here due to its size.  However, users may inspect this supervisor directly in supremica (see the [instructions](doc/01_howto.md)). From this supervisor, we have extracted 83 potentially hazardous sequences (up to a maximum length of 10 human actions).

## Simulation
The simulation model is depicted in Fig. 1. The human actions from EFA model H are used as simulation inputs to control the digital human model in the simulator. To further vary the range of simulation conditions and expose more hazards, we randomized some parameters of human model, namely its position at the table (laterally, +/- 20cm), its hand velocity (+/- 20%), and the upper body angle when reaching for the workpieces (+/- 25%). As mentioned above, the goal is to find combinations of action sequences and motion parameters that result in a critical collision. 

_Note: As you may notice from the sometimes awkward human motions, we use a simplified human model in this case. This example is not about achieving an extremely detailed simulation of human motion, it is about the general idea of searching for hazardous behaviors!_


## Hazards
Hazards in this scenario consist of the worker changing the order of actions, resulting in his hand being trapped between robot and housing or colliding with the robot while it is moving, as well as the worker bending to far forward over the workpiece while the worker is activated, resulting in a collision between robot and the worker's head. Note that each of these hazards may manifest itself in multiple action sequences, explaning the large number of sequences found in the analysis. 
