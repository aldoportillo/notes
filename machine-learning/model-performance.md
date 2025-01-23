# Model Performance in Supervised Learning

## Introduction

Understanding and evaluating model performance is critical in supervised learning. This involves various metrics and methods to ensure that the model not only fits the training data well but also generalizes effectively to new data.

## Key Metrics

### Accuracy

- Measures the proportion of true results (both true positives and true negatives) among the total number of cases examined.
- Formula:
  $$
  Accuracy = \frac{TP + TN}{Total\ Examples}
  $$

### Confusion Matrix

- A table layout that visualizes the performance of an algorithm.
- Each row represents the instances in an actual class while each column represents the instances in a predicted class.

  ![Confusion Matrix](https://imgs.search.brave.com/6BSx2SSzJeK7lfz1h9Is3x3z1YBPNigXtXQLMGyckMY/rs:fit:860:0:0/g:ce/aHR0cHM6Ly9hc3Nl/dHMtZ2xvYmFsLndl/YnNpdGUtZmlsZXMu/Y29tLzVkN2I3N2Iw/NjNhOTA2NmQ4M2Ux/MjA5Yy82M2I0MTNk/MmNkYzEzMzQ0NmFh/MjNmYzVfNjM2Yjkx/ODJjZmFlZjIxMTVl/MDI4OTIxX0hFUk9f/MV9Db25mdXNpb24u/cG5n)

### Sensitivity (Recall or True Positive Rate)

- Measures the proportion of actual positives correctly identified.
- Formula:
  $$
  Sensitivity = \frac{TP}{TP + FN}
  $$

### Specificity (True Negative Rate)

- Measures the proportion of actual negatives correctly identified.
- Formula:
  $$
  Specificity = \frac{TN}{TN + FP}
  $$

### Precision

- Measures the proportion of positive identifications that were actually correct.
- Formula:
  $$
  Precision = \frac{TP}{TP + FP}
  $$

### F1 Score

- Harmonic mean of precision and sensitivity.
- Useful in scenarios where the balance between precision and recall is essential.
- Formula:
  $$
  F1\ Score = 2 \times \frac{Precision \times Sensitivity}{Precision + Sensitivity}
  $$

## Model Validation Techniques

### Holdout Validation

- Involves splitting the data into:
  - Training Set: For training the model.
  - Validation Set: For tuning model parameters.
  - Test Set: For testing the model's final performance.
- ROC Curve (Receiver Operating Characteristic curve) plots sensitivity vs 1 - specificity, aiding in visualizing the trade-off.

  ![ROC Curve](https://imgs.search.brave.com/wpmcguDEG0HMYpe_5Y9FwPNTv6vqG0lsRlSYDIcICQI/rs:fit:860:0:0/g:ce/aHR0cHM6Ly91cGxv/YWQud2lraW1lZGlh/Lm9yZy93aWtpcGVk/aWEvY29tbW9ucy83/LzdhL1JPQ19jdXJ2/ZV9leGFtcGxlX2hp/Z2hsaWdodGluZ19z/dWItYXJlYV93aXRo/X2xvd19zZW5zaXRp/dml0eV9hbmRfbG93/X3NwZWNpZmljaXR5/LnBuZw)

- AUC (Area Under the ROC Curve) measures the entire two-dimensional area underneath the ROC curve, providing an aggregate measure of performance.

## Model Development Process

1. Define the Problem.
2. Formulate Hypotheses.
3. Implement Simple Heuristic Models.
4. Measure Impact and Evaluate.
5. Develop More Complex Techniques.
6. Measure Impact and Iterate.
7. Fine-Tune the Model.
8. Compare and Replace Existing Techniques, if necessary.

## Generalization

- A model's ability to perform well not only on the test set but also on new, unseen examples.
- Ensuring good generalization is key to avoid overfitting and ensure the model's practical applicability.
