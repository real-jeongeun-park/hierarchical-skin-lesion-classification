# Efficient Hierarchical Skin Lesion Classification via Multi-Task Learning

This repository contains the implementation associated with the paper **"Efficient Hierarchical Skin Lesion Classification via Multi-Task Learning"**, presented at the International Conference on Next-generation Convergence Technology (ICNCT 2026).

**Authors:** Jeongeun Park¹†, Hyeonseo Lim²†, Daesung Kang¹²*
¹ School of AI Convergence, Sungshin Women's University, Seoul, Republic of Korea
² School of Bio-Health Convergence, Sungshin Women's University, Seoul, Republic of Korea
*Corresponding author: danniskang@gmail.com

## Overview

The **DERM12345** dataset provides 12,345 dermatoscopic images annotated with a three-tier hierarchical taxonomy:

- **40 subclasses**
- **15 main classes**
- **5 super classes**

Conventional approaches train a separate model for each hierarchy level, which increases computational cost, storage requirements, and fails to exploit the semantic relationships among hierarchical labels.

This project introduces a **multi-task learning framework** that attaches three independent classification heads to a single shared backbone, enabling simultaneous prediction of subclass, main class, and super class labels from a single model.

## Method

- **Backbone:** ResNet-34, pre-trained on ImageNet, used as a shared feature extractor.
- **Heads:** Three independent fully connected classification heads for:
  - Subclass prediction (40-way)
  - Main class prediction (15-way)
  - Super class prediction (5-way)
- **Preprocessing:** Images resized to 224×224 and normalized using standard ImageNet statistics.
- **Training:** 50 epochs with the AdamW optimizer. Three learning rates were tested (1×10⁻³, 1×10⁻⁴, 2×10⁻⁴), and the model with the lowest validation loss was selected for final evaluation.
- **Loss function:**

  ```
  L_total = L_subclass + L_main_class + L_super_class
  ```

  This jointly supervises all three hierarchy levels during training.

### Data Split

- Official DERM12345 train/test split: 80% / 20%
- 20% of the training set held out for validation

## Results

The multi-task model was compared against single-task baselines trained independently for each hierarchy level.

| Level | Model | Acc | AUPRC | Sen | Pre | F1 |
|---|---|---|---|---|---|---|
| Subclass | Single-task | 58.31 | 0.595 | 0.583 | 0.567 | 0.556 |
| Subclass | **Multi-task** | **60.76** | **0.616** | **0.608** | **0.593** | **0.581** |
| Main class | Single-task | 63.06 | 0.656 | 0.631 | 0.620 | 0.622 |
| Main class | **Multi-task** | **64.35** | **0.659** | **0.643** | **0.639** | **0.635** |
| Super class | Single-task | 90.91 | **0.947** | 0.909 | 0.907 | 0.907 |
| Super class | **Multi-task** | **91.31** | **0.947** | **0.913** | **0.911** | **0.913** |

**Key findings:**
- The multi-task model matched or surpassed single-task performance across all three hierarchy levels.
- The largest gains appeared at the **subclass level**, the most challenging due to fine-grained morphological differences among lesion types.
- Using a single shared model reduces storage requirements by approximately **two-thirds** and shortens overall training time compared to training three separate models.

## Conclusion

This work demonstrates that hierarchical multi-task learning on a single ResNet-34 backbone can achieve competitive or superior classification performance across subclass, main class, and super class levels, while significantly reducing computational and storage overhead. This makes the approach a promising direction for real-world dermatology AI systems, particularly in resource-constrained clinical environments where training and storing multiple models is impractical.

## Dataset

- **DERM12345**: A large, multisource dermatoscopic skin lesion dataset with 40 subclasses.
  Yilmaz A, Yasar SP, Gencoglan G, Temelkuran B. *Scientific Data*, 2024, 11(1), 1302.

## References

1. Yilmaz A, Yasar SP, Gencoglan G, Temelkuran B. "Derm12345: A large, multisource dermatoscopic skin lesion dataset with 40 subclasses." *Scientific Data*, 2024, 11(1), 1302.
2. He K, Zhang X, Ren S, Sun J. "Deep residual learning for image recognition." In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition*, 2016, pp. 770-778.

## Citation

If you use this work, please cite:

```
Park J, Lim H, Kang D. "Efficient Hierarchical Skin Lesion Classification via Multi-Task Learning."
International Conference on Next-generation Convergence Technology (ICNCT 2026).
```
