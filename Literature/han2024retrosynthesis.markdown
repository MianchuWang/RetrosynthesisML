# Retrosynthesis prediction with an iterative string editing model

Han, Y., Xu, X., Hsieh, CY. et al. Retrosynthesis prediction with an iterative string editing model. Nat Commun 15, 6404 (2024).

DOI: https://doi.org/10.1038/s41467-024-50617-1

GitHub: https://github.com/yuqianghan/editretro

---

## Markov Decision Process on Retrosynthesis

Notation $<S, A, E, F, s^0>$

- $s = (s_1, s_2, \dots, s_L) \in S$ is a sequence of tokens, where each token $s_i$ is drawn from a predefined vocabulary $V$.
- $A$ is the set of editing actions.
- $F$ is the reward function, negative value of the distance $D$ between the generated output and the ground-truth sequence.

**An agent interacts with an environment that receives the agent's editing actions and returns the modified sequence.**

Policy $\pi: S \rightarrow P(A)$, that maps the current generation over a probability distribution over $A$.

**The policy receives an input sequence $s$ and selects an editing action $a \in A$ to refine it.**
