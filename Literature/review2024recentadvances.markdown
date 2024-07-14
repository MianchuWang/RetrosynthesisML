# Recent advances in deep learning for retrosynthesis

Zhong Z, Song J, Feng Z, Liu T, Jia L, Yao S, et al. Recent advances in deep learning for retrosynthesis. WIREs Comput Mol Sci. 2024; 14(1):e1694.

DOI: https://doi.org/10.1002/wcms.1694

---

## Molecule Representation

### 1. One-dimensional molecule representation

* **Fingerprints**: encodes physicochemical or structural properties but irreversible.

* **SMILES**: canonicalization algorithm ensures one-to-one mapping between SMILES and molecules.

### 2. Two-dimensional molecule representation

An adjacency matrix represents a graph, where atoms are represented by nodes and bonds by edges.

## One-step Retrosynthesis

### 1. Selection-based methods

They formulate retrosynthesis as a selection problem by predefining reactant or reaction template candidates.

* **Reactant selection**: select reactants from the molecule candidates by calculating the matching score between the product and the possible reactant.
  - Excellent performance but with impractical assumption on reactant candidates. 
* Template selection
* Semi-template selection
* Template-free selection
