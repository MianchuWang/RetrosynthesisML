# Efficient retrosynthetic planning with MCTS exploration enhanced A* search

Zhao, D., Tu, S. & Xu, L. Efficient retrosynthetic planning with MCTS exploration enhanced A* search. Commun Chem 7, 52 (2024).

PDF: https://doi.org/10.1038/s42004-024-01133-2

GitHub: https://github.com/CMACH508/MEEA (No License)

---

## Background

MCTS and A* search have significantly contributed to computer-aided retrosynthesis.

* **A\* search** is guaranteed to find the optimal solution, but the efficiency of A* is compromised due to insufficient exploration.
* **MCTS** has exploration, but compulsive exploration decreases the efficiency.

> [!NOTE]
> Given enough resources, A* should be able to complete the task?

MEEA* incorporates the exploration capacity of MCTS into A* search.

## Markov Decision Process for Retrosynthesis Planning

* State space $\mathcal{S}= \\{ m_0, m_1, \dots \\}$ is a set of molecules. The initial state is the target molecule. The molecules are represented by SMILES.
* Action space $\mathcal{A}$ is potential chemical reactions (the action space is vast since a molecule can be synthesized by various reactions).
* Transition $\mathcal{T}$ produces the reactants.
* Cost $\mathcal{C}$ is defined as $-\log p_r(a \mid m)$, which is a predictive model to estimate the probability of selecting reaction $a$ for molecule $m$.
This implies that the reaction type should be included in the training data.

## MEEA* Search

> [!CAUTION]
> The paragraph break in the MEEA* search section is inadequate. 

Following the paper, MEEA* includes three data structures:

* **OPEN** for generated but unexplored nodes,
* **CLOSE** for explored nodes，
* **Candidate** for nodes from exploration.

But I don't find the place where OPEN or CLOSE is used.

For the algorithm, MEEA* iteratively executes the three steps:

* **Simulation**: conduct $K_{MCTS}$ iterations to collect *exploratory* candidates.

* **Selection**: select the candidate node with the lowest $f$.

* **Expansion**: use single-step expansion model to provide $50$ successor states.

> [!TIP]
> The primary difference between MEEA* and MCTS is that MEEA* buffers more than 1 MCTS traversed leaf nodes and expands the node with the lowest $f$, not expands the leaf node immediately after reaching it.
>
> The primary difference between MEEA* and A* is that MEEA* expands the node in the candidate set with the lowest $f$, rather than expands the node with the lowest $f$.

| $K_{MCTS}$ | MEEA* equals to |
|------------|---------|
| =1 | MCTS |
| Relatively small | MCTS with increased exploration |
| Extremely large | A* |

