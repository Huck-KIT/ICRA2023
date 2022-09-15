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
    
We recommend that you start by reading overview.md. Note that this repository allows you to completeley reproduce our experiments (however, your results may differ slightly as some aspects of our experiments contain non-detmerinism). If you want to do reproduce the experiments, howto.md will give you a step-by-step intstruction. You can also opt to reproduce subsets of the experiments (e.g. only the supervisor synthesis, or only the simulations).

Our experiments were conducted on Ubuntu 18.04 using the following tools:
- Supremica IDE 2.7.1 for formal modelling and synthesis
- CoppeliaSim 4.2.0 for simulations

If you are just interested in a more detailed description of our workflow and testing scenarios and don't want to isntall any software, you can read overivew.md, scenarioA.md and scenarioB.md, which will provide detailed 
