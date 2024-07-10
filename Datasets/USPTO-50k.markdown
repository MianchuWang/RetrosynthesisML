# USPTO-50k 

Mianchu's Example: [[ipynb](../Code/Examples/USPTO_50k_example.ipynb)]

Helpful Website: https://tdcommons.ai/generation_tasks/retrosyn/

---

**Data Description**

USPTO (United States Patent and Trademark Office) 50k consists of 50,036 extracted atom-mapped reactions with 10 reaction types.

**Data Preparation**

1. **Data split**: 40k, 5k, 5k for training, validation, and testing, respectively [[Graph2Edits](../Literature/Graph2Edits.markdown)].
2. **Information leak**: canonicalise the product SMILES and re-assign the mapping numbers to the reactant atoms [[Graph2Edits](../Literature/Graph2Edits.markdown)].