# Technical Overview
Here we give an quick overview of the technical implementation of our work (corresponding to the method described in Sec. III of our paper).

## First Layer: Synthesis of Unsafe Behaviours (corresponding to Sec. III-A of our paper)

The human-robot system is modeled by extended finite automatons (EFAs) in the tool supremica. The supremica with models are found in the folder
experiments/models/supremica/Scenario* (file extension .WMOD). Supervisor synthesis is performed using supermica's built in synthesis algorithms.
The resulting supervisor automaton is saved as an XML file containing the states and transitions. We parse this XML file with a custom-written pyhton
script parse_automaton.py, which generates a CSV (supervisor_scenario_\*.xml) file containing all potentially hazardous behaviors up to a certain length ("action_sequences_supervisor_scenario_\*.csv")

For the two simulation-only approaches (MCTS and Random), we extract from the supremica models a set of all feasible behahiors of the human model. Note that here,
no supervisor synthesis is performed. The extracted data is only used to check whether a certain behavior sampled by the MCTS or random search is feasible or not (e.g., the human model cannot
put down a workpiece before picking it up, etc...). It is *not* used to draw any inferences about potential hazards.
To that end, we extract the human model automaton as an XML file ("human_model_scenario_\*.xml") and use parse_automaton.py to extract all feasible behaviors as a CSV file ("action_sequences_human_model_scenario_\*.csv").

## Second Layer: Simulation of Unsafe Behaviours (corresponding to Sec. III-B of our paper)

In this layer, we use the CoppeliaSim models (see experiments/models/coppeliasim/scenario_\*.ttt) to simulate the human-robot interactions in more detail. We use the behavior of the human as a simulation input to control a digital human model in simulation and check if the robot responds in a safe or unsafe manner to the behaviors of the human. To quantify the level of risk associated with a particular human behavior, the simulation computes a domain-specific risk metric (compare Eq. (1) in our paper).

We deploy three different simulation approaches:
- Two-layer: The two-layer approach takes the unsafe behaviours synthesized in the first layer (i.e., the sequences contained in action_sequences_supervisor_scenario_\*.csv) and simulates each sequences multiple times with different randomly sampled parameters (e.g., different walking speeds of the human model) until a certain budget of simulation runs is exhausted. This simulation is controlled by the script simulate_supervisor_sequences.py
- Random: The random approach samples behaviors randomly from the set of all feasible behaviours (i.e., from the sequences contained in action_sequences_human_model_scenario_\*.csv) and executes them in simulation until the bduget until a certain budget of simulation runs is exhausted. This simulation is controlled by the script simulate_supervisor_sequences.py
- MCTS: This approach uses a Monte Carlo Tree Search algorithm to sample behaviors event by event, using the transitions encoded in human_model_scenario_\*.xml to check whether a given event is feasible in the current state (note that this is only to avoid infeasible events, no inference about potential hazards are drawn from this). The MCTS algorithm receives a risk metric as a reward. By attempting to maximise the reward (and thus, the risk that is associated with an event sequence), MCTS finds behaviours leading to unsafe states.

Each simulation script saves the respective results in a .csv file containing the event sequences along the the parameters and the risk metric.


## Reproducing the Experiments
If you want to reproduce experiments, we recommend to go on by reading howto.md, where more details are explained.
