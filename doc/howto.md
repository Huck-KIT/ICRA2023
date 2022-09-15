# How to Reproduce the Experiments

## First Layer: Synthesis of Unsafe Behaviours
1. Use supremica to open the .WMOD file our the respective scenario that you want to reproduce. You will finde the automata and variables in a pane on the left.
By double-clicking on the automata (green symbols) and safety spec (Red symbols) you can inspect them. When comparing the automata from sceneario A with Fig. 3 in the paper,
please note that the names of variables and transitions may differ from those used in the
paper, since we had to shorten the names for a more compact reprsentation in the paper. Functionally, however, the models are equivalent.
2. To synthesize the supervisor, click on "analyzer", then mark all sub-models in the analyzer pane by clicking on them keeping the shift-key pressed. Then,
right-click, select "synthesize", keep the default settings ("standard options") as they are and click on "OK". The synthesized supervisor will appear.
Next, right click on the supervisor and select "hide events". Select all events except the "collision" event and those events that begin with "human_\*"
(hiding the events does not change the functionality of the supervisor, but ensures that in the second layer, our extracted event sequences will only contain those events
that we use as a simulation input. Compare the discussion on "proactive" and "reactive" events in Sec. III-B of the paper).
