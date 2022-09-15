# How to Reproduce the Experiments

## First Layer: Synthesis of Unsafe Behaviours
In the first layer, we perform supervisor synthesis and extract the behaviors used as inputs for the second layer. These steps may be a bit cumbersome if you are not familiar with the use of Suprmica. However, our repository already contains all the required outputs that are generated below, you you can skip this layer and jump directly to layer 2.

1. Use supremica to open the .WMOD file our the respective scenario that you want to reproduce. You will finde the automata and variables in a pane on the left.
By double-clicking on the automata (green symbols) and safety spec (Red symbols) you can inspect them. When comparing the automata from sceneario A with Fig. 3 in the paper,
please note that the names of variables and transitions may differ from those used in the
paper, since we had to shorten the names for a more compact reprsentation in the paper. Functionally, however, the models are equivalent.

2. For the two-layer approach, you need to synthesize and extract all hazardous behaviors of the human model. To do this:
- Click on "analyzer", then mark all sub-models in the analyzer pane by clicking on them keeping the shift-key pressed. Then, right-click, select "synthesize", keep the default settings ("standard options") as they are and click on "OK". The synthesized supervisor will appear.
Next, right click on the supervisor and select "hide events". Select all events except the "collision" event and those events that begin with "human_\*"
(hiding the events does not change the functionality of the supervisor, but ensures that in the second layer, our extracted event sequences will only contain those events that we use as a simulation input. Compare the discussion on "proactive" and "reactive" events in Sec. III-B of the paper. Then right-click on the resulting supervisor and select export -> xml -> OK. Save the resulting xml file under models/supremica/XML/supervisor_scenario_\*.xml
  
- Open the script parse_automaton.py, choose the following settings, and then run the script:
    - max_sequence_length sequence = The maximum length of sequences to be extracted (e.g. 10)
    - terminate_on_event = True //we want to terminate only if we have a specific event, in this case a collision
    - filepath_source = "models/supremica/XML/supervisor_scenario_\*.xml" (insert "A" or "B" for \*)
    - filepath_dest = "models/supremica/CSV/action_sequences_supervisor_scenario_\*.csv"
   This should generate a file models/supremica/CSV/action_sequences_supervisor_scenario_\*.csv containing all hauardous event sequences up to your chosen length.
    
3. For MCTS and Random Sampling, you need to extract all feasible behaviors (not considering whether they are hazardous or not) of the human.
  - In the analyzer tab, select the human and all variables that appear in the human model (in scenario A: collaborative_workspace_occupied, part_status, and laser_scanner_occupied), then right click and select "synchronize" --> "OK". Then, hide events and save the supervisor automaton as an xml file (models/supremica/XML/human_model_scenario_\*.xml), analogous to step 2. 
  - Open the script parse_automaton.py, choose the following settings, and then run the script:
    - max_sequence_length sequence = The maximum length of sequences to be extracted (same as above)
    - terminate_on_event = False //here we want to get all possible behaviors, regardless whether they result in a collision or not
    - filepath_source = "models/supremica/XML/human_model_scenario_\*.xml" (insert "A" or "B" for \*)
    - filepath_dest = "models/supremica/CSV/action_sequences_human_model_scenario_\*.csv"
   This should generate a file models/supremica/CSV/action_sequences_human_model_scenario_\*.csv containing all hazardous event sequences up to your chosen length.

## Second Layer: Simulation of Unsafe Behaviours
1. Open your desired simulation scenario (models/coppeliasim/scenario_\*.ttt) in CoppeliaSim (we used version 4.2.0). Click on "Add-ons" --> "B0 remote Api server" and wait a few seconds.
2. Then, open your desired simulation script to perform one of the three approaches:
  - Two-layer approach: Open "simulate_sueprvisor_sequences.py", select the following settings, and then run it:
    - scenario =  1 // 1 for scenario A, 2 for scenario B
    - max_sequence_length = desired length of the simulation sequence in terms of the number of actions that are executed (should correspond with step 2)
    - use_random_parameters = True
    - log_results = True
    - trigger_actions_manually = False // activate this if you want to see the exectuion step by step
    - random.seed(1) // don't forget to vary the random seed if you want to conduct multiple test runs.
    The results are written to "results/results_supervisor_scenario_\*.csv"
  - MCTS approach: Open "simulate_mcts.py". Select your scenario in line 184 (1=scenario A, 2=scenario B), select your settings in line 184 to 186, and run the script. Results are written to "results/results_mcts_scenario_\*.xml"
  - Random approach: Open "simulate_random_sequences.py", select your settings lin line 9-15, and run the script. Results are written to "results/results_random_scenario_\*.csv"
