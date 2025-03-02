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
- Human Interpretation: Exactly what it sounds like, and the hardest problem to fix. This could happen due to *confirmation bias* or *availability bias*. In other words a [ID10T](https://community.spiceworks.com/t/what-are-your-favorite-ways-to-refer-to-user-errors/696040) error.

## The 5 Main Algorithm Techniques

### Symbolic Reasoning

This uses symbolic reasoning to find a solution to a problem. **Deduction** *vs* **Induction** (inverse deduction).

- Deduction: Expands the realm of human knowledge The engineering portion.
- Induction: Raises the level of human knowledge. The science portion.

By working together, *Induction* opens new fields of exploration and *Deduction* explores those fields to see if the problem gets solved.

- *Deduction*: "If a tree is green and green trees are alive, the tree must be alive"
- *Induction*: "Tree is green and the tree is also alive; therefore, green trees are alive"

Therefore *Induction* provides the answer to what knowledge is missing.

### Neural Networks

This algorithm tries to mimic the brain's neurons as algorithm and each neuron solves a small part of the problem. Similar to how the brain works.

A neural network can provide a method of correction for errant data. The most common method of correction is **backpropagation**. This is what you will hear every person who has read a 5 min article on AI talk about. Throwing out words like **weights** --how much a particular input figures into the result-- *and* **biases** --which features are selected-- and how they need to be adjusted in pred to remove errors from the network. Once weights and biases are adjusted for the current neuron line, it gets passed to the next line.

### Evolutionary Algorithms

This strategy closely resembles survival of the fittest. We use a *fitness function* to gauge the viability of each function. If it doesn't make the cut, it is out. The winner of each level of evolution gets to build the next level of functions until the problem is solved. *Seems Recursive? It is*

### Bayesian Inference

The use of many statistical methods to solve problems. This approach selects the best statistical method in terms of succeeding. Think of the spam filter. Saying the word `security`, `free`, `social` and `winner`. Are not words that let you know that a message contains spam, but image getting the following email:

```txt
Subject: Winner!!
Body: You have won a $200 Amazon Gift Card. Send your Social Security Number to claim your Reward!
```

These algorithms don't return a true or a false but return a statistical probability of the problem.

### Analogy Systems

The use of kernel machines to recognize patterns in data. This uses similarity to determine the best solution of a problem. This is prevalent in Recommendation Algorithms. Have you ever seen the `Customers also Bought` on Amazon?
