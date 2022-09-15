# Test Scenario B

## About this scenario
In this scenario, we consider a HRC assembly workflow where the human worker transitions to a shelf, grabs some parts, returns to the robot, reaches into the workpiece housing, and then presses a button to activate the robot. The robot then starts to put a gearwheel into the housing. Meanwhile, the human reaches into the workpiece cover and then mounts the cover onto the housing. Note that this workflow is inspired by a [real-world HRC application](https://youtu.be/02TzqIvWiso).

## EFA modeling
We model the human worker, the robot, and the workpiece by EFAs. Fig. shows the human model, where we model the events "human_transition" to represent the transitioning between the shelf and the workstation, "human_reach_parts" (representing the retrieval of parts from the shelf), "human_press_button" (activates the robot by setting the shard variable robot_activated=1), "human_reach_in_housing", "human_reach_in_cover", and "human_mount_cover". We further introduce the variables "parts_taken" to track whether the human has retrieved parts from the shelf (which is a precondition for further assembly actions), "cover_mounted" to track whether the human has already mounted the cover on the workpiece (note that the human cannot reach into housing or cover after the cover is moutned, as shown by the guard statement "cover_mounted==0". Further , we use the variable "collab_workspace_occupied_human" to track whether the human is currently reaching into the collaborative workspace (which is the case for all reaching motions). Note that the action "retract_hand" sets the variable "collab_workspace_occupied_human" back to zero.

![Fig. 1](https://user-images.githubusercontent.com/56551323/190402050-ed328d50-de6b-4b0d-911b-42db1c50542b.png)

Fig. 2 shows the robot model. The robot is initially in idle mode (S0). After the human has activated it (denoted by the guard robot_activated==1), the robot moves to the housing, thereby entering the collaborative workspace (see action collab_workspace_occupied_robot=1), and inserting the gearwheel (Note: the guard gearwheel_inserted==0 and the action gearwheel inserted=1 ensure that the gearwheel can only be inserted once). Afterwards, the robot moves bach, clearing the collaborative workspace and going back into idle mode (as denoted by the actions collab_workspace:occupied_robot=0 and robot_activated=0).   

![Fig. 2](https://user-images.githubusercontent.com/56551323/190404921-c7454263-8c09-400a-9f3a-48f2971094fd.png)






## Simulation

To further vary the range of simulation conditions and expose additionl hazards, we randomized some parameters of human model, namely its position at the table (laterally, +/- 20cm), its hand velocity (+/- 20%), and the upper body angle when reaching for the workpieces (+/- 25%). As mentioned above, the goal is to find combinations of action sequences and motion parameters that result in a critical collision. 

_Note: As you may notice from the sometimes awkward human motions, we use a simplified human model in this case. This example is not about achieving an extremely detailed simulation of human motion, it is about the general idea of searching for hazardous behaviors!_
