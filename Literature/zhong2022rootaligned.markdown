# Root-aligned SMILES: a tight representation for chemical reaction prediction

Zhong, Z., Song, J., Feng, Z., Liu, T., Jia, L., Yao, S., … Song, M. (2022). Root-aligned SMILES: a tight representation for chemical reaction prediction. Chem. Sci., 13, 9023–9034.

DOI: https://doi.org/10.1039/d2sc02763a

GitHub: https://github.com/otori-bird/retrosynthesis

---

## Summary

This paper proposes the root-aligned SMILES (R-SMILES), 
which specified a tightly aligned one-to-one mapping between the product and
the reactant SMILES for synthesis prediction and retrosynthesis.

## Example

The explanation is a little different from the original paper.

1. We have a reaction
   
   Reactants: `Cl[C:1]([CH:2]=[CH2:3])=[O:4]` `[OH:5][CH2:6][C:7]([Cl:8])([Cl:9])[Cl:10]`
   
   Product: `[C:1]([CH:2]=[CH2:3])(=[O:4])[O:5][CH2:6][C:7]([Cl:8])([Cl:9])[Cl:10]`

   The structure can be visualised in [[Link]](https://www.antvaset.com/smiles-to-structure).

2. Randomly select a root (head) atom from the product. Here we use `[Cl:8]`.
  
3. Rewrite the product with the root atom `[Cl:8]`:
   
   `[Cl:8][C:7]([Cl:9])([Cl:10])[C:6][O:5][C:1](=[O:4])[C:2]=[C:3]`

   Note that, in the molecule, the first atom appears in the first reactant is `[C:1]` and
   the first atom appears in the second reactant is `[Cl:8]`.
   
4. Rewrite the reactants with the root atom `[C:1]` and `[Cl:8]`, respectively.

   Reactant 1: `C(=O)(Cl)C=C`

   Reactant 2: `ClC(Cl)(Cl)CO`

5. We have the R-SMILES representation for both reactants and the product:

   Reactants: `ClC(Cl)(Cl)CO` `C(=O)(Cl)C=C`
   
   Product: `ClC(Cl)(Cl)COC(=O)C=C`
   
