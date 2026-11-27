# What model receives
- **Supervised**: Each input $x$ is paired with an output label $y$ (ground truth, in other terms the true value). The model is trained to find the function $f:X\rightarrow Y$ in a way that minimises the error between the predicted output ($f(x)$) and the ground truth($y$). 
	- **Ground truth** is information that is known to be real or true, provided by direct observation and measurement (i.e. empirical evidence) as opposed to information provided by inference.
	- Used in [[Classification]] and [[Univariate Regression]] problems
- **Unsupervised**: The model is given a set of inputs $X$ without any corresponding output labels. The model is trained to find patterns, structures, or relationships within the data. 
    - Used in [[Clustering]] and [[Dimensional Reduction]] problems
- **Semi-Supervised**: The model is given a set of inputs $X$ where some inputs have corresponding output labels and some do not. The model is trained to leverage the labeled data to learn patterns and structures in the unlabeled data. 
    - Used in [[Classification]] and [[Clustering]] problems
- **Self-supervised**: another approach that involves generating a fully labeled dataset from a fully unlabeled one. Once the dataset is labeled, a supervised model can be used. 
	- But more often than not, a self-supervised model trained isn't the main goal
	- It's possible to tweak the model for another purpose. E.g.: if the model is trained for repairing images, then it can be tuned for predicting what kind of object is in each photo.
	- *Transfer learning*: transferring knowledge from one task to another, especially in neural networks
- **Reinforcement learning**: It's very the others, due to the fact that involves an *agent* that *interacts* with an *environment* with *actions* and receives *rewards* or *penalities* in return if the *policy* is correctly applied. Main goal for the agent is to receive the most reward over time. 
- This distinction concerns how the model is trained and the type of feedback it receives during the learning process. It does not necessarily reflect the complexity or difficulty of the problem being solved, as both supervised and unsupervised learning can be applied to a wide range of tasks with varying levels of complexity.

## Learning phase
- **Batch** (*Offline learning*): The model is trained on the entire dataset at once. It processes all the data in one go, and updates its parameters after seeing the entire dataset. This approach is suitable for static datasets where all the data is available upfront.
	- The downside is that it can be computationally expensive and time-consuming for large datasets, and it may not adapt well to new data that arrives after the initial training.
- **Incremental** (*Online Learning*): The model is trained on one data point at a time or in small batches. It updates its parameters incrementally as new data arrives. This approach is suitable for dynamic datasets where data is continuously generated, and the model needs to adapt to new information.
	- The downside is that it may be more sensitive to noise and outliers in the data, and it may require more careful tuning of learning rates and other hyperparameters to ensure convergence. This may lead to *catastrophic forgetting*, the phenomenon where the model forgets previously learned information when it is trained on new data.
- **Natural**: The model is trained in a way that mimics how humans learn, by interacting with the *environment* and receiving *feedback*. This approach is suitable for tasks that involve complex decision-making and learning from experience, such as reinforcement learning. This is used in [[Reinforcement Learning]] problems.
- Maintenance logic