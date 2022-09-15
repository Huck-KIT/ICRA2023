# Test Scenario B

## About this scenario
In this scenario, we consider a HRC assembly workflow where the human worker transitions to a shelf, grabs some parts, returns to the robot, reaches into the workpiece housing, and then presses a button to activate the robot. The robot then starts to put a gearwheel into the housing. Meanwhile, the human reaches into the workpiece cover and then mounts the cover onto the housing. Note that this workflow is inspired by a [real-world HRC application](https://youtu.be/02TzqIvWiso).

## EFA modeling
We model the human worker, the robot, and the workpiece by EFAs. Fig. shows the human model, where we model the events "human_transition" to represent the transitioning between the shelf and the workstation, "human_reach_parts" (representing the retrieval of parts from the shelf), "human_press_button" (activates the robot), "human_reach_in_housing", "human_reach_in_cover", and "human_mount_cover". We further introduce the variables "parts_taken" to track whether the human has retrieved parts from the shelf (which is a precondition for further assembly actions), "cover_mounted" to track whether the human has already mounted the cover on the workpiece (note that the human cannot reach into housing or cover after the cover is moutned, as shown by the guard statement "cover_mounted==0". Further , we use the variable "collab_workspace_occupied_human" to track whether the human is currently reaching into the collaborative workspace (which is the case for all reaching motions). Note that the action "retract_hand" sets the variable "collab_workspace_occupied_human" back to zero.

![Fig. 1](https://user-images.githubusercontent.com/56551323/190402050-ed328d50-de6b-4b0d-911b-42db1c50542b.png)









To simulate the effects of human error, the human model can change the order of worksteps (within some boundaries, since the resulting sequences still must be feasible, see section 'Generate Action Sequences' below). To caputure the variability of human motion, the human model can also vary its position at the table (laterally, +/- 20cm), its hand velocity (+/- 20%), and the upper body angle when reaching for the workpieces (+/- 25%). As mentioned above, the goal is to find combinations of action sequences and motion parameters that result in a critical collision. 

_Note: As you may notice from the sometimes awkward human motions, we use a simplified human model in this case. This example is not about achieving an extremely detailed simulation of human motion, it is about the general idea of searching for hazardous behaviors!_
