# Introduction

## Types of Learning

### Deep Learning

Optimizing Neural Networks to perform unsupervised and supervised learning.

### Supervised Learning

When the input data is labeled and has a specific result. The training is done to create a Model that best fits the data.

| Labeled Input | Result Output | Real World Use |
| --- | --- | --- |
| Images | Boxes labeled with object's name | Image Detection |
| English Text | Russian Text | ML Translation |
| Image and Sensor Data | Steering, Breaking and Accelerating | Autonomous Driving |

#### Types of Supervised Learning

- [Linear Regression](./introduction.md)
- [Logistic Regression](./introduction.md)
- [Support Vector Machines](./introduction.md)
- [Naive Bayes](./introduction.md)
- [K-Nearest Neighbors](./introduction.md)

### Unsupervised Learning

When the input data is **not** labeled and the results are **not** known. The analysis of structures in the data produces the model.

#### Types of Unsupervised Learning

- [Clustering](./introduction.md)
- [Anomaly Detection](./introduction.md)
- [Neural Networks](./introduction.md)

### Self-Supervised Learning

Gives you the benefits of supervised learning without the tediousness of labeling data.

### Reinforcement Learning

Similar to self-supervised except that it includes a feedback loop for the results.

## The ML Process (TVT)

1. Training:  a model using a specific algo against training data that is representative of the problem's domain.

2. Validating: the model with testing data. Similar to the training data. The testing data must be representative of the problem's domain.

3. Testing: your model with real world data. Once your model is trained and validated.

## What happens when a Machine Learns?

Throughout the TVT process the output is a function. Not your typical `const func = () =>` or `def func:`, but a mathematical function `f(x)=` that is a multi-dimensional polynomial. Now you might think this polynomial should have the best fit to produce the best results; however, it can be **overfitted** and that is not great because we need **generalizations** to provide useful results. If the model is **underfitted**, it will not provide any useful results. Deciding how much to train a model and what algo to use is all up to *you*.

### Limits of Bias

Since the computer has no bias (*yet*), we must avoid introducing bias in several steps of the process.

- Data: If a particular value appears twice as much in the data as it does in the real world, our solution is tainted.
- Algorithm: If our chosen algorithm isn't the best choice,it will cause the model to fit the data incorrectly.
- Training: Too much or too little results in a overfitted or underfitted solution.
- Human Interpretation: Exactly what it sounds like, and the hardest problem to fix. My favorite error code for this is: [ID10T](https://community.spiceworks.com/t/what-are-your-favorite-ways-to-refer-to-user-errors/696040).