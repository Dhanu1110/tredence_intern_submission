# Self-Pruning Neural Network Submission

## 1. Why an L1 penalty on the sigmoid gates encourages sparsity

In our architecture, the effective connection weight is determined by element-wise multiplication: $W \times \sigma(G)$. The gating value outputted by the sigmoid $(\sigma)$ function is bounded [...]

By adding an L1 penalty specifically on these sigmoid outputs—computed as $\lambda \sum \sigma(G)$—to our total Cross-Entropy objective function, we actively penalize the network for keeping[...]

---

## 2. Results Summary

Below is the summary of the model trade-offs observed over multiple magnitude experiments using CIFAR-10. 

| Lambda ($\lambda$)  | Test Accuracy | Sparsity Level (%) |
|---------------------|---------------|--------------------|
| **0.0001**          | **57.63%**    | **81.47%**         |
| 0.001               | 56.73%        | 99.89%             |
| 0.01                | 49.38%        | 99.99%             |
| 1.0                 | 10.50%        | 100.0%             |
| 10.0                | 10.00%        | 100.0%             |

*Note: The best performing model was obtained using $\lambda = 0.0001$, as it successfully discarded $81.47\%$ of its interconnects while retaining its competitive classification test accuracy.*

---

## 3. Distribution of Gate Values

Below is the distribution of the final gate values for the best performing model. As hypothesized for a successful model, there is **a massive spike at 0** denoting the pruned connections, and a d[...]

![Gate Histogram](https://github.com/Dhanu1110/tredence_intern_submission/raw/main/gate_histogram.png)

## 4. Conclusion

The experiment shows that gate-based regularization can significantly compress neural networks while maintaining useful accuracy. The best balance was achieved at λ = 0.0001.
