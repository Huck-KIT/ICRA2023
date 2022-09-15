# Technical Overivew
Here we give an overview of the technical implementation of our work (corresponding to the method described in Sec. III of our paper)

## First Layer: Synthesis of Unsafe Behaviours (corresponding to Sec. III-A of our paper)

The human-robot system is modeled by extended finite automatons (EFAs) in the tool supremica. The supremica with models are found in the folder
experiments/models/supremica/Scenario* (file extension .WMOD). Supervisor synthesis is performed using supermica's built in synthesis algorithms.
The resulting supervisor automaton is saved as an XML file containing the states and transitions. We parse this XML file with a custom-written pyhton
script parse_automaton.py, which generates a CSV (supervisor_scenario_*.xml) file containing all potentially hazardous behaviors up to a certain length ("action_sequences_supervisor_scenario_*.csv")

For the two simulation-only approaches (MCTS and Random), we extract from the supremica models a set of all feasible behahiors of the human model. Note that here,
no supervisor synthesis is performed. The extracted data is only used to check whether a certain behavior sampled by the MCTS or random search is feasible or not (e.g., the human model cannot
put down a workpiece before picking it up, etc...). It is *not* used to draw any inferences about potential hazards.
To that end, we extract the human model automaton as an XML file ("human_model_scenario_*.xml") and use parse_automaton.py to extract all feasible behaviors as a CSV file ("action_sequences_human_model_scenario_*.csv").

## Second Layer: Simulation of Unsafe Behaviours
