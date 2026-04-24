# Self-Pruning Neural Network Submission

## 1. Why an L1 penalty on the sigmoid gates encourages sparsity

In our architecture, the effective connection weight is determined by element-wise multiplication: $W \\times \\sigma(G)$. The gating value outputted by the sigmoid $(\\sigma)$ function is bounded strictly between 0 and 1. 

By adding an L1 penalty specifically on these sigmoid outputs—computed as $\\lambda \\sum \\sigma(G)$—to our total Cross-Entropy objective function, we actively penalize the network for keeping connections \"open\" (where $\\sigma(G) \\approx 1$). During backpropagation, in order to minimize this overall loss, the optimizer is constantly coerced to decay the internal Gate Scores $(G)$ toward large negative values. As $G \\rightarrow -\\infty$, the sigmoid activation $\\sigma(G)$ approaches exactly 0, effectively severing the connection in the forward pass. This encourages sparse connectivity by pushing unnecessary gates toward zero.

---

## 2. Results Summary

Below is the summary of the model trade-offs observed over multiple magnitude experiments using CIFAR-10. 

| Lambda ($\\lambda$)  | Test Accuracy | Sparsity Level (%) |
|---------------------|---------------|--------------------|
| **0.0001**          | **57.63%**    | **81.47%**         |
| 0.001               | 56.73%        | 99.89%             |
| 0.01                | 49.38%        | 99.99%             |
| 1.0                 | 10.50%        | 100.0%             |
| 10.0                | 10.00%        | 100.0%             |

*Note: The best performing model was obtained using $\\lambda = 0.0001$, as it successfully discarded $81.47\\%$ of its interconnects while retaining its competitive classification test accuracy.*

---

## 3. Distribution of Gate Values

Below is the distribution of the final gate values for the best performing model. As hypothesized for a successful model, there is **a massive spike at 0** denoting the pruned connections, and a distinct surviving cluster of values representing the core functional weights mapping the correct decisions.

![Gate Histogram](![Gate Histogram](https://github.com/Dhanu1110/tredence_intern_submission/raw/main/gate_histogram.png)

## 4. Conclusion

The experiment shows that gate-based regularization can significantly compress neural networks while maintaining useful accuracy. The best balance was achieved at λ = 0.0001.
