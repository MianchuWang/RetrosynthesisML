# USPTO-50k 

Schneider, N., Stiefl, N., & Landrum, G. A. (2016). What’s What: The (Nearly) Definitive Guide to Reaction Role Assignment. Journal of Chemical Information and Modeling, 56(12), 2336–2346.

DOI: https://doi.org/10.1021/acs.jcim.6b00564

Mianchu's Example: [[ipynb](../Code/Examples/USPTO_50k_example.ipynb)]

Helpful Website: https://tdcommons.ai/generation_tasks/retrosyn/


---

**Data Description**

USPTO (United States Patent and Trademark Office) 50k consists of 50,036 extracted atom-mapped reactions with 10 reaction types.

**Data Preparation**

1. **Data split**: 40k, 5k, 5k for training, validation, and testing, respectively [[Graph2Edits](../Literature/Graph2Edits.markdown)].
2. **Information leak**: The product atom with atom-mapping 1 is part of the edit in 75% of the cases, 
so related research should canonicalise the product SMILES and *remap* the existing dataset 
[[Graph2Edits](../Literature/Graph2Edits.markdown), [GraphRetro](../Literature/GraphRetro.markdown)].