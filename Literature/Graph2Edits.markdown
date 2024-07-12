# Retrosynthesis prediction using an end-to-end graph generative architecture for molecular graph editing

Zhong, W., Yang, Z. & Chen, C.YC. Retrosynthesis prediction using an end-to-end graph generative architecture for molecular graph editing. Nat Commun 14, 3009 (2023). 

DOI: https://doi.org/10.1038/s41467-023-38851-5

GitHub: https://github.com/Jamson-Zhong/Graph2Edits (MIT License)

---

## Contributions
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

## Implementations

### Edit generation

Graph2Edits gives different priorities to graph edits: delete bond > change bond > change atom > attach LG.

> [!TIP]
> All following generations are based on the premise that the reaction product and reactants are *atom-mapped*.
>
> Example:
> 
> Reaction SMILES Without Atom-Mapping:
>
> `CC(Cl)Cl + BrBr >> CC(Br)Br + BrH + BrH`
> 
> Reaction SMILES With Atom-Mapping
> 
> `[CH3:1][CH2:2]([Cl:3])[Cl:4] + [Br:5][Br:6] >> [CH3:1][CH2:2]([Br:5])[Br:6] + [H:7][Br:3] + [H:8][Br:4]`
> 

1. Delete bond
```python
    for bond in prod_bonds:
        if bond not in react_bonds:
            # We know this is a delete bond edit.
```
2. Change bond. Note that a bond is a two-element list, e.g., [2, 0], [1, 0].
```python
    for bond in prod_bonds:
         if bond in react_bonds and prod_bonds[bond] != react_bonds[bonds]:
            # We know this is a changed bond.
```
3. New bond
```python
    for bond in react_bonds:
        if bond not in prod_bonds:
            a1, a2 = bond
            if a1 in p_amap_idx and a2 in p_amap_idx:
                # This branch does not mean that the matter is not conserved:
                # some groups which are divisible are mapped as one unit, such as [CH2:23] -> [CH:23].
                # We know this is a new bond.
```
