# ICRA2023

## About this repository
This repository accompanies our paper "Hazard Analysis of Collaborative Automation Systems: A Two-layer Approach based on Supervisory Control and Simulation" submitted to the IEEE ICRA 2023 conference.

## How to get started

We recommend that you start by reading the [overview](doc/00_overview.md). After that, if you are just only interested in a more detailed description of our testing scenarios and you do not want to install any software, we recommend that you read the descriptions of [scenario A](doc/02_scenarioA.md) and [scenario B](doc/03_scenarioB.md).

If you want to reproduce our experiments our [instructions](doc/01_howto.md) will give you a step-by-step guide. You can also opt to reproduce subsets of the experiments e.g. only the supervisor synthesis, or only the simulations).

To recreate our experiments you need the following sofware:
- [Supremica](http://supremica.org/) for formal modelling and synthesis
- [CoppeliaSim](https://www.coppeliarobotics.com/ (version 4.2.0) for simulations
Both software tools are freely available for academic purposes, with version for both Ubuntu and Windows. We used Ubuntu 18.04.

However, please note that your results may differ slightly as some aspects of our experiments contain non-detmerinism.

## Contents
The repository is structured as follows:
- doc
  - [overview.md](doc/00_overview.md) // general overview of the technical aspects of our work
  - [scenarioA.md](doc/02_scenarioA.md) // description of testing scenario A
  - [scenarioB.md](doc/03_scenarioB.md) // description of testing scenario B
  - [howto.md](doc/01_howto.md) // instructions on how to reproduce our experiments
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
