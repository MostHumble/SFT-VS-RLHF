# SFT vs. DPO: A Visual Introduction to LLM Fine-Tuning

This repository contains a hands-on Jupyter notebook that visually demonstrates the fundamental differences between Supervised Fine-Tuning (SFT) and Direct Preference Optimization (DPO). Using a simple "toy problem," this experiment provides a clear and intuitive understanding of when to use each technique.

![model_comparison_subplots (5) (1)](https://github.com/user-attachments/assets/2e5a2247-d90d-4553-8130-77fdd8c2abc6)


## The Experiment: The RGB Tile Game

The core of this project is a simple generative task with a clear set of rules and a specific goal:

1.  **The Grammar (Local Rules):** A model must learn to generate sequences of colored tiles (R, G, B) following a strict grammar:
    * **R** must be followed by **G**.
    * **G** must be followed by **B**.
    * **B** can be followed by **R** or **G**.

2.  **The Goal (Global Objective):** The model's objective is to maximize a "preference score," defined by the frequency of the `B -> G` transition.

This setup creates a perfect testbed: can the model learn the basic rules while also learning to optimize for a global preference?

## Key Insight: Imitation vs. Optimization

This experiment clearly illustrates the core difference between SFT and DPO:

* **Supervised Fine-Tuning (SFT)** learns by **imitation**. By training on a dataset of the "best" examples, the SFT model becomes better at following the rules and producing a diverse range of high-quality outputs, but it doesn't learn to actively optimize for the goal.
* **Direct Preference Optimization (DPO)** learns by **optimization**. By training on pairs of "better" and "worse" examples, the DPO model learns an implicit reward function. This pushes it to discover and exploit the most effective strategies for maximizing the preference score, even at the cost of creativity and, sometimes, rule adherence.

## How to Run This Project


### Execution
Simply open and run the cells in the notebook in a Jupyter environment. The notebook handles data generation, pre-training, SFT, DPO, and final evaluation.

* A GPU is recommended for faster training times (altough the datasets are somewhat already small as)

## Results Summary

The final comparison between the BASE, SFT, and DPO models reveals three key findings:

| Model          | Avg. Preference Score | Avg. Rule Adherence | Output Entropy (Variety) |
| -------------- | --------------------- | ------------------- | ------------------------|
| BASE           | 0.2434                | 86.66%              | **7.64 bits** |
| SFT (Imitator) | 0.2966                | **92.62%** | **7.64 bits** |
| DPO (Explorer) | **0.4551** | 88.44%              | 6.66 bits               |

1.  **DPO is a vastly superior optimizer**, achieving a much higher preference score.
2.  There is a clear **creativity-optimization trade-off**, where DPO sacrifices output variety (lower entropy) to focus on the goal.
3.  Alignment has a **"cost,"** as the DPO model's rule adherence is slightly lower than the SFT model's, showing its aggressive optimization can lead to minor regressions in other capabilities.

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.
