
# Data and Patterns

- Data are fundamental for machine learning, more specifically for domains that have multiple entries (multi-dimensional), in which samples in that domain are called *Data-point*.
- Synonyms for data-point are various, but the most used is **pattern**.
- Pattern Recognition is based on recognizing patterns in images, text and so on and it is a discipline based 

## Types of Patterns
 - **Numerical**: measurable values, typically continuous values that have ordering relations, representable forms
 - **Categorical**: values that represent a characteristic (yes/no value), ordinal values. With some *encoding* techniques, like one-hot encoding, we can translate from categorical to numerical patterns 
 - **Sequential**: sequential patterns that have relations in terms of space and time and have variable length. The relation between predecessors and successors are crucial.
 - There are others based on complex structure, like graph and trees
## Tabular Data
- Some information are organized in a tabular form, where each *row* is a **data-point** and each *column* is a **feature**:
	- features can be in numerical or categorical form and can be heterogeneous
	- some data point can be missing or corrupted, so some preprocessing is required to clean the data
	- a metric must be defined before the training phase, to evaluate the quality of the model and its performance

## Encoding
- **One-Hot encoding**: a feature with n possible values is represented with n features, each one with a binary value (0 or 1) and a pattern consist of that feature if it is 1, otherwise it is 0
- **Ordinal Encoding**: a feature with n possible values is represented with a single feature with a numerical value in the range \[0, n\] and a pattern consist of that feature if it is equal to the value of the feature, otherwise it is 0
- **Hybrid: Ordinal+One Hot**: rare feature are encoded with a category "other" and the remaining features are encoded with ordinal encoding
- Target Encoding, Bayesian Encoding


# Problems and Approaches
## Classification
- **Class**: a set of patterns that share a common property, for example a set of images that represent the same object. It has semantic properties and strictly depends on the domain of the problem.
- **Classification**: the task of assigning a class to a pattern, based on the features of the pattern and the classes that are known.
- Types of classification:
	- Binary classification: two classes, for example spam or not spam
	- Multi-class classification: more than two classes, for example a set of images that represent different objects
- Problems in which classification is involved:
	- Spam detection
	- Credit Card fraud detection
	- Face recognition
	- Pedestrian classification
	- Medical diagnosis
	- Stock Trading
[[Classification]]
## Regression
- **Regression**: the task of predicting a continuous value based on the features of the pattern and the known values. It is a supervised learning problem. The solution is a function that maps the features to the continuous value.
- Problems in which regression is involved:
    - House price prediction
    - Stock price prediction
    - Weather forecasting
    - Demand forecasting
[[Univariate Regression]]
## Clustering
- **Clustering**: the task of grouping patterns that share similar properties (*cluster*), based on the features of the patterns. It is an unsupervised learning problem. The solution is a set of clusters that group the patterns.
- Problems in which clustering is involved:
    - Customer segmentation
    - Image segmentation
    - Anomaly detection
    - Document clustering
[[Clustering]]
## Dimensional Reduction
- **Dimensional Reduction**: the task of reducing the number of features in a pattern, based on the features of the patterns. It is an unsupervised learning problem. The solution is a set of features that represent the patterns.
- Problems in which dimensional reduction is involved:
    - Data visualization
    - Feature selection
    - Noise reduction
    - Data compression
- $R^d\rightarrow R^k (k<d)$ 
[[Dimensional Reduction]]
## Representation Learning
- **Representation Learning**: the task of learning a representation of the patterns, based on the features of the patterns. The solution is a set of features that represent the patterns.
- **Hand-crafted feature**: a feature that is designed by a human, based on the domain knowledge. The solution is a set of features that represent the patterns. The field in which this kind of feature are created is called **Feature Engineering**.

## Vision Problems
- Classification
- Localization
- Detection
- Segmentation
## Discriminative and Generative Models
- Both focused on finding the **Decision** **Boundary**
- *Discriminative models* are focused on finding the boundary (called *decision boundary*) between classes, while *generative models* are focused on finding the *distribution* of the classes.

## Generative AI
- Generative AI is a type of artificial intelligence that can generate new content, such as images, text, or audio, based on the patterns it has learned from existing data. It uses **generative models** to create realistic and novel outputs that resemble the training data.
- They are based on *deep neural networks*
# Learning
- Supervised
- Unsupervised
- Semi-Supervised


## Learning phase
- **Batch**
- **Incremental**
- **Natural**

[[Learning process and Approaches]]
## Reinforcement Learning
- It is focus on learning a **behaviour**, based on the feedback received from the environment. The solution is a policy that maps the state of the environment to an action.
- Models assumes the role of **agents** and they interact with the **environment**, changing their **state** and receiving **rewards** for what they do.
![[Pasted image 20260928191206.png]]
- Widely used in robotics
- Approaches:
	- **Q learning**
	- Markov Decision Process
	- **Deep Reinforcement Learning** (Google DeepMind)

# Objective Function and Loss Function
- **Objective Function**: It's any function you're trying to maximize or minimize. It doesn't have to involve error or predictions at all - it just defines what "better" means for your problem.
	- Optimal solution: $\theta* = argmax_\theta f(\text{Train}, M(\theta))$
- **Loss function**: measures the error for a single training example - how far off your model's prediction is from the actual value. Mean squared error for one data point, for instance, is a loss function.
	- Consist of resolving the reversed problem $\theta* = argmin_\theta f(\text{Train}, M(\theta))$
- **Cost function**: aggregates the loss across the entire dataset, usually by averaging it. So the cost function is what you're actually minimizing during training - it summarizes model performance across all examples, not just one.
- Optimization for resolving the loss function:
	- Direct: resolving partial derivative of $f$ (*gradient*), equal to zero and resolving equation system
	- Greedy: using heuristic methods that modifies parameters.

[[Objective and Loss Function]]

# Manipulating the dataset

## Training, Validation and Test set
- **Training set**: the dataset used to train the model. It is used to learn the parameters of the model.
- **Validation set**: the dataset used to evaluate the model during training. It is used to tune the hyperparameters of the model and to prevent overfitting.
- **Test set**: the dataset used to evaluate the model after training. It is used to assess the generalization performance of the model.
- In benchmarks this distinction is predefined for comparing results.

[[Training, Validation and Test set]]

## Hyperparameter
- **Hyperparameter**: a parameter that is set before the training phase and it is not learned from the data. It is used to control the learning process and the complexity of the model.
- First, we need to define the hyperparameters of the model, then we can use them to train the model and evaluate its performance. 
- For each hyperparameter configuration the model is trained and evaluated (with the validation set), and the best configuration is selected based on the performance metrics.
- More specifically:
```
For H in Hyperparameter values
	Θ∗ = 𝑎𝑟𝑔𝑚𝑖𝑛Θ 𝑓 𝑇𝑟𝑎𝑖𝑛, 𝑀(𝐻, Θ)
	Evaluate 𝑀(𝑉𝑎𝑙𝑖𝑑, 𝐻, Θ∗)
Select best hyperparameters (𝐻∗)
```
[[Hyperparameters of ML models]]

## Disjoint Set
- If data are sufficiently large, the dataset can be split into three disjoint set
- The split process depends on the awaited error: big train set allows a better learning, but small validation and test set can lead to a poor evaluation of the model. The split process is usually done randomly(**shuffling**), but it can be done in a *precised* and stratified way, to ensure that the distribution of the classes is similar in all the sets.
## K-Fold Cross Validation
- The dataset is divided into K subsets of equal size (**folds**).
- The model is trained K times where each time a different fold is used as the validation set and the remaining K-1 folds are used as the training set. This is done for each *hyperparameter combination*
- The final result is the average of the accuracy of the K results.
- More specifically it returns the *mean score* ($\mu = \frac{1}{k}\sum_{i=1}^k S_i$), that represents the expected performance of the model on unseen data, and the *standard deviation* ($\sigma = \sqrt{\frac{1}{k}\sum_{i=1}^k (S_i - \mu)^2}$), that measures the model's stability across different data subsets:
	- A low $\sigma$ indicates that the model's performance is consistent across different folds, suggesting that it generalizes well to unseen data.
	- A high $\sigma$ indicates that the model's performance varies significantly across different folds, suggesting that it may be sensitive to the specific data used for training and validation, and may not generalize well to unseen data.
- Once optimal hyperparameters are selected, the model is trained with the entire training set and evaluated on the test set to assess its final performance.
- **Leave-one-out**: a special case of K-Fold Cross Validation where K is equal to the number of data points in the dataset. Each data point is used once as a validation set while the remaining data points form the training set. This method can be computationally expensive for large datasets but provides an unbiased estimate of model performance.
![[Pasted image 20260928205210.png]]

[[K-Fold Cross Validation]]


## Cherry Picking
- **Cherry picking** (formally known as the **fallacy of incomplete evidence** or **suppressing evidence**) is the practice of selecting only the data, facts, or examples that support a preferred position while ignoring or omitting contradictory evidence.
- **Selective Reporting:** Reporting only the best metric run (e.g., highlighting a 95% accuracy score obtained on an unrepresentative test split while hiding poor precision, recall, or cross-validation variance).
- **Data Dredging (p-hacking):** Testing dozens of feature combinations or subsets until an arbitrary statistical significance threshold ($p < 0.05$) is achieved by chance, then presenting that single result as generalizable.
- **Leakage / Biased Subsetting:** Evaluating a model only on clean, curated test subsets where edge cases and noisy samples have been pruned.
- Never use the test set and only it. Cherry picking can lead to overfitting and poor generalization, as the model may perform well on the selected subset but fail on other data. 
- If the test set is limited in terms of size or diversity, the model may not be able to learn the underlying patterns in the data and may not generalize well to new data. This will lead to good perfomance but the model will fail in real world scenarios. 
## Hyperparameter Selection
- Grid Search
- Random Search
- Others:
	- Early Stopping
	- Learning curves
	- Pipelines

[[Hyperparameter Tuning]] #TODO
## Auto-ML
- Auto-ML system automates the process of choosing the right model for the defined problem.
- Also it automatically applies the right techniques for processing features and data.
```
For Mi in Models
	For H in Hyperparameter values for Mi
		Θ∗ = 𝑎𝑟𝑔𝑚𝑖𝑛Θ 𝑓(𝑇𝑟𝑎𝑖𝑛, 𝑀𝑖 (𝐻, Θ))
		Evaluate 𝑀𝑖(𝑉𝑎𝑙𝑖𝑑, 𝐻, Θ∗)
Select best model and hyperparameters (𝑀∗, 𝐻∗)
```
- Auto-ML system are executed in cloud, but there are some versions that can be executed locally, like Auto-Sklearn and Auto-Keras.
![[Pasted image 20260929093439.png]]

[[Auto-ML and Neural Architecture Search]]
# Metrics
- Metrics allow to evaluate the performance of a model and to compare different models.
- There are some metrics that could be embedded in cost and loss functions
## Classification Metrics

$$Accuracy = \frac{correct\_classified\_patterns}{classified\_pattern}$$
$$Error = 100\% - accuracy$$
- If the dataset is unbalanced, accuracy is not a good metric to evaluate the performance of a model, because it can be misleading. In this case, other metrics should be used, like precision, recall and F1-score.
## Regression Metrics
- **RMSE** (Root Mean Squared Error): measures the average magnitude of the errors between predicted and actual values. It is sensitive to outliers and gives higher weight to larger errors.
$$RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}$$

- **MAE** (Mean Absolute Error): measures the average magnitude of the errors between predicted and actual values. It is less sensitive to outliers than RMSE.
- $$MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat{y}_i|$$
## Confusion Matrix
- Confusion Matrix is a table that is used to evaluate the performance of a classification model. It shows the number of true positives, true negatives, false positives and false negatives.
- The diagonal elements represent the number of correct predictions, while the off-diagonal elements represent the number of incorrect predictions.
![[Pasted image 20260929100922.png]]

- It can be normalized and each element summed in a row to 1, to represent the probability of each class given the true class.
- In a binary classificator there are:
	- True Positive (TP): the number of positive patterns that are correctly classified as positive. 
    - True Negative (TN): the number of negative patterns that are correctly classified as negative.
    - False Positive (FP): the number of negative patterns that are incorrectly classified as positive.
    - False Negative (FN): the number of positive patterns that are incorrectly classified as negative.
- We can formalize the concept of accuracy:
$$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$

## DET Curve and ROC curve
- DET curve (Detection Error Tradeoff curve) is a graphical representation of the tradeoff between the false positive rate (FPR) and the false negative rate (FNR) at various threshold settings. It is used to evaluate the performance of a binary classifier.
- ROC curve (Receiver Operating Characteristic curve) is a graphical representation of the tradeoff between the true positive rate (TPR) and the false positive rate (FPR) at various threshold settings. It is used to evaluate the performance of a binary classifier.
![[Pasted image 20260929103412.png]]
![[Pasted image 20260929103538.png]]
- AUC (Area Under the Curve) is a metric that measures the area under the ROC curve. It ranges from 0 to 1, where 1 indicates a perfect classifier and 0.5 indicates a random classifier. It's measured as the integral of the ROC curve:
- ![[Pasted image 20260929103723.png]]

## Precision and Recall
- **Precision** is the ratio of true positives to the sum of true positives and false positives. It measures the accuracy of the positive predictions made by the model.
$$Precision = \frac{TP}{TP + FP}$$
- **Recall** (also known as sensitivity or true positive rate) is the ratio of true positives to the sum of true positives and false negatives. It measures the ability of the model to identify all relevant instances in the dataset.
$$Recall = \frac{TP}{TP + FN}$$
- **F1-score** is the harmonic mean of precision and recall. It is a single metric that balances both precision and recall, providing a more comprehensive evaluation of the model's performance.
$$F1-score = 2 * \frac{Precision * Recall}{Precision + Recall}$$
- **Average Precision (AP)** is a metric that summarizes the precision-recall curve as the weighted mean of precisions achieved at each threshold, with the increase in recall from the previous threshold used as the weight:
- $$AP = \sum_n (R_n - R_{n-1}) * P_n$$
	- Where $P_n$ and $R_n$ are the precision and recall at the $n_{th}$ threshold, respectively. The AP metric provides a single value that reflects the model's performance across all thresholds, making it particularly useful for evaluating models in imbalanced datasets or when the cost of false positives and false negatives varies.
- **mAP (mean Average Precision)** is the mean of the AP scores for each class in a multi-class classification problem. It provides an overall measure of the model's performance across all classes, taking into account both precision and recall for each class.

[[Evaluation Metrics]]
# Main Problems

## Closed set and Open set
- Closed set problems assume that all possible classes are known during training. The model is trained to classify patterns into a fixed set of classes, and it is expected that all test patterns belong to one of these classes.
- Open set problems allow for the possibility of encountering new, unseen classes during testing. They require the model to not only classify known classes but also to recognize when a pattern does not belong to any known class and should be treated as an unknown or novel class.
## Anomaly Detection
- Anomaly detection is the task of identifying patterns in data that do not conform to expected behavior. 
- These patterns are often referred to as outliers, novelties, or exceptions.
- Anomaly detection is crucial in various applications, including fraud detection, network security, and fault detection in industrial systems.

# Main Achievements

## Convergence
- Convergence in machine learning refers to the process by which a learning algorithm approaches a stable solution or optimal set of parameters as it iteratively updates its model based on the training data. 
- Convergence is typically assessed by monitoring the loss function or objective function over time, and it indicates that the model has learned the underlying patterns in the data and is no longer making significant improvements with further training.
- Convergence must be achieved before evaluating the model on the test set, as it ensures that the model has sufficiently learned from the training data and is ready for generalization to unseen data. 
- Convergence depends on loss function and accuracy:
	- loss must decrease and reach a minimum value (not necessarily zero)
	- accuracy must increase and reach a maximum value (not necessarily one)

## Generalization and Overfitting
- Generalization is the ability of a machine learning model to perform well on unseen data, beyond the training dataset. It indicates that the model has learned the underlying patterns in the data rather than memorizing specific examples.
- Overfitting occurs when a model learns the training data too well, capturing noise and specific patterns that do not generalize to new data. This results in high accuracy on the training set but poor performance on the validation or test set.
- To prevent overfitting, techniques such as regularization, cross-validation, and pruning can be employed. Regularization methods like L1 and L2 add penalties to the model's complexity, encouraging simpler models that generalize better. They improve control on *degree of freedom* of the model, reducing the risk of overfitting.
- Also exists the opposite problem, called **underfitting**, in which the model is too simple to capture the underlying patterns in the data, resulting in poor performance on both the training and test sets.

# Tools
- Scikit-learn
- OpenCV
- Weka
- R
- Tensorflow
- PyTorch