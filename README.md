# dnnls_final_Project
Improving Grounding and Contrastive Alignment in Multimodal Sequence Prediction

## Project Overview

This project improves a pretrained multimodal grounded sequence prediction model that takes a sequence of image-text pairs from a story and predicts the next image-text element.

The main goal was to improve two components of the baseline model:

1. **Grounding**
2. **Alignment / contrastive loss**

The experiments were carried out on the StoryReasoning dataset.

## Task

The model receives a sequence of story frames and their matching text descriptions.  
It must predict the next multimodal story element.

In simple terms, the model tries to understand:

- what is happening in the current images
- what the text says about those images
- how the story is developing over time
- what should come next

---

## Chosen Improvements

### 1. Grounding
Grounding helps the model connect the correct words to the correct image regions.

For example, if the text refers to a person, object, or scene detail, grounding helps the model link that text to the relevant visual evidence.

### 2. Alignment / Contrastive Loss
Contrastive alignment is intended to bring correct image-text pairs closer together in representation space and push incorrect pairs further apart.

The idea is to improve the shared multimodal embedding space and make image-text matching more meaningful.


## Experimental Setup

Four controlled experiments were run under the same general model setting.

### Experiment 1: Baseline
- Grounding: OFF
- Contrastive loss: OFF

### Experiment 2: Grounding-only
- Grounding: ON
- Contrastive loss: OFF

### Experiment 3: Contrastive-only
- Grounding: OFF
- Contrastive loss: ON

### Experiment 4: Grounding + Contrastive
- Grounding: ON
- Contrastive loss: ON

All final comparisons reported here are based on **10-epoch experiments**.


## Final Results

| Experiment | Final Total Loss | Ground Loss | Contrast Loss | Interpretation |
|---|---:|---:|---:|---|
| Baseline | 0.602102 | 0.000000 | 0.000000 | Reference model |
| Grounding-only | 0.592998 | 0.366814 | 0.000000 | Best validated improvement |
| Contrastive-only | 0.588823 | 0.000000 | 0.000000 | Lowest loss but inconclusive |
| Grounding + Contrastive | 0.643672 | 0.000000* | 0.000000* | Worse than baseline |

## Main Findings

### Grounding-only
This produced the strongest validated improvement.

It reduced the final total loss from:

- **0.602102** in the baseline
to
- **0.592998**

This suggests that grounding improved the model’s ability to connect visual evidence with the story text.

### Contrastive-only
This achieved the lowest numerical final loss:

- **0.588823**

However, the logged contrastive loss stayed at zero throughout training, so this result is treated as **promising but inconclusive**. The contrastive branch likely needs more debugging or tuning before strong claims can be made.

### Grounding + Contrastive
This configuration performed worse than the baseline:

- **0.643672**

This suggests that the combined objective was unstable under the current settings and likely needs better loss balancing or training strategy.

---

## Qualitative Observations

The text prediction branch generally worked better than the direct image generation branch.

In early runs, the predicted image often appeared as a grey blurry block, which showed that the visual decoder was weaker than the text generation side.

To improve interpretability, a retrieval-based predicted image visualisation was used in later qualitative analysis. This made it easier to inspect what kind of next scene the model was pointing toward.
