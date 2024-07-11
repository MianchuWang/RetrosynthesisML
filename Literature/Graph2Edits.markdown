# Retrosynthesis prediction using an end-to-end graph generative architecture for molecular graph editing

Zhong, W., Yang, Z. & Chen, C.YC. Retrosynthesis prediction using an end-to-end graph generative architecture for molecular graph editing. Nat Commun 14, 3009 (2023). 

DOI: https://doi.org/10.1038/s41467-023-38851-5

GitHub: https://github.com/Jamson-Zhong/Graph2Edits

---

# Contributions
1. **Graph2Edits**: an end-to-end architecture that generates arbitrary length graph edits in an auto-regressive way.
2. **D-MPNN**: encodes the atom/bond and LG to predict atom/bond edits and a termination, respectively.
3. **Edit label**: introduces chirality and cis-trans isomerism in predefined atom and bond edits.


## Data Preparation

This paper builds an edits vocabulary in the training set. These edits cover 99.9% of the reactions in the test set, including 6 bond edits, 152 atom edits, and a termination symbol. For example, 

* ('Change Bond', (2, 0)),
* ('Attaching LG', '*O'),
* ('Attaching LG', '*C(=O)CCCCBr').

They include 4 types of edits and a termination symbol:

* **Delete bond**
* **Change bond**
* **Change atom**, e.g., ('Change Atom', (0, 0)) the two numbers in brackets represent the number of hydrogen and the chiral type to be changed, respectively.
* **Attach leaving group (LG)**: e.g., ('Attaching LG', `*C(=O)c1ccccc1C(*)=O`) represents the functional group is added on the specified atom.
* **Terminate**

## Learning & Inference

The message passing neural network (MPNN), particularly the directed MPNN (D-MPNN),  operates on an undirected graph $\mathcal{G}=(\mathcal{V}, \mathcal{E})$ to build the atom representations of molecules. 

Graph2Edits learns an autoregressive model and predicts the next edit. The model is learned by cross-entropy loss over possible edits (bond edits, atom edits, termination).
