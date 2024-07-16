# Self-improved retrosynthetic planning

Kim, J., Ahn, S., Lee, H. &amp; Shin, J.. (2021). Self-Improved Retrosynthetic Planning. Proceedings of the 38th International Conference on Machine Learning.

PDF: https://proceedings.mlr.press/v139/kim21b.html

GitHub: https://github.com/junsu-kim97/self_improved_retro

---

## Summary

**Overview**: the authors propose an end-to-end framework for directly training the DNNs towards generating reaction pathways with the desired properties.

**Idea**: self-improving trains the model to imitate successful trajectories found by itself.

**Contributions**: 
1. Self-improving procedure - maximise the success rate of search algorithms,
2. Likelihood-based criterion - generate realistic reaction pathways,
3. Augmentation - improve the generalisation ability,

## Method

### Models

1. Backward reaction model: $p_b(m | \mathcal{R}; \theta_b)$.
2. Reference backward reaction model: $p_b(m | \mathcal{R}; \bar{\theta}_b)$.
3. Forward reaction model: $p_f(m | \mathcal{R}; \theta_f)$.

### Notes

1. Molecule representation: Morgan fingerprint (Rogers & Hahn, 2010).

2. Planning method: RETRO* (Chen et al., 2020).

3. Reference backward reaction model $p_b(m | \mathcal{R}; \bar{\theta}_b)$: whether a reaction resembles reactions in the real world.

4. Synthesis route cost: $-\sum_{(m, \mathcal{R}) \in \tau} \log p_b(\mathcal{R} | m; \bar{\theta}_b)$.
