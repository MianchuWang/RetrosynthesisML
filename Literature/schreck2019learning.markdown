# Learning Retrosynthetic Planning through Simulated Experience

Schreck, J. S., Coley, C. W., & Bishop, K. J. M. (2019). Learning Retrosynthetic Planning through Simulated Experience. ACS Central Science, 5(6), 970–981.

DOI: https://doi.org/10.1021/acscentsci.9b00055

GitHub: https://github.com/jsschreck/retroRL (GNU GPL, no longer maintained)

---

## The Retrosynthesis Game

**Spaces**: The chemist starts from the *target molecule* and identifies a set of *reaction candidates*.
Once a reaction is selected, its reactants become new targets.

**Rewards**: The sum of the cost of the reaction (=1) and the buyable substrates (=0).

**Playing**: By repeating the game many times starting from many target molecules, it is possible to estimate the value for each target and its precursors.

## Methods

1. Training/testing sets include 95774/23945 molecules from the Reaxys dataset.
2. An interesting molecule clustering algorithm - Taylor-Butina (TB) algorithm.
3. Reaction templates include substructure matching and bond rewiring, implemented by RDKit and RDChiral.
4. Template prioritizer takes as input a representation of the target molecule and generates a probability distribution over the set of templates based on their likelihood of success.

## Conclusions

(Near) optimal policies trained on the synthesis of ~100000 diverse molecules generalise well to the synthesis of unfamiliar molecules.
