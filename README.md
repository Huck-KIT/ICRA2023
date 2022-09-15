# ICRA2023

This repository accomanies our paper "Hazard Analysis of Collaborative Automation Systems: A Two-layer Approach based on Supervisory Control and Simulation" submitted to the IEEE ICRA 2023 conference.

The repository is structured as follows:
- doc
  - overview.md // general overview of the technical aspects of our work
  - scenarioA.md // description of testing scenario A
  - scenarioB.md // description of testing scenario B
  - howto.md // step-by step guide on how to reproduce our experiments
- experiments
  - models
    - supremica // formal models (EFAs, supervisor synthesis)
    - coppeliasim // simulation models
  - results
    - Scenario_A // logs from all simulation in scenario A, containing event sequences and risk metrics
    - Scenario_B // logs from all simulation in scenario B, containing event sequences and risk metrics
    - evaluation.xlsx // a spreadsheet summarizing the results
  - simulate_supervisor_sequences.py // two-level approach
  - simulate_mcts.py // MCTS approach 
  - simulate_random_sequences.py // Random approach
  - parse_automaton.py // A script to extract event sequences from synthesized automata
    
