# Recent advances in deep learning for retrosynthesis

Zhong Z, Song J, Feng Z, Liu T, Jia L, Yao S, et al. Recent advances in deep learning for retrosynthesis. WIREs Comput Mol Sci. 2024; 14(1):e1694.

DOI: https://doi.org/10.1002/wcms.1694

---

## Molecule Representation

### 1. One-dimensional molecule representation

* **Fingerprints**: encodes physicochemical or structural properties but is irreversible.

* **SMILES**: canonicalization algorithm ensures one-to-one mapping between SMILES and molecules.

### 2. Two-dimensional molecule representation

An adjacency matrix represents a graph, where atoms are represented by nodes and bonds by edges.

## One-step Retrosynthesis

### 1. Selection-based methods

They formulate retrosynthesis as a selection problem by predefining reactant or reaction template candidates.

* **Reactant selection**: select reactants from the molecule candidates by calculating the matching score between the product and the possible reactant.
  - Excellent performance but with an impractical assumption on reactant candidates.

* **(Reaction) template selection**:
  - This paper mentions an interesting data augmentation method from Fortunato et al. (2020).

### 2. Generation-based methods

They generate the target reactants directly in representation of string sequences or molecular graphs.

* **Semi-template selection**
  - Step one (P2S): identify the reaction center to get intermediate molecules called synthons,
  - Step two (S2R): Complete synthons to reactants.

* **Template-free selection**: retrosynthesis as a sequence generation problem.

## Multi-step Retrosynthesis

This method usually needs a search tree or a directed acyclic graph starting from the target molecule and ending with the building blocks.
