# K-Fold Cross Validation
- The dataset is divided into K subsets of equal size (**folds**).
- The model is trained K times where each time a different fold is used as the validation set and the remaining K-1 folds are used as the training set. This is done for each *hyperparameter combination*
- The final result is the average of the accuracy of the K results.
- More specifically it returns the *mean score* ($\mu = \frac{1}{k}\sum_{i=1}^k S_i$), that represents the expected performance of the model on unseen data, and the *standard deviation* ($\sigma = \sqrt{\frac{1}{k}\sum_{i=1}^k (S_i - \mu)^2}$), that measures the model's stability across different data subsets:
	- A low $\sigma$ indicates that the model's performance is consistent across different folds, suggesting that it generalizes well to unseen data.
	- A high $\sigma$ indicates that the model's performance varies significantly across different folds, suggesting that it may be sensitive to the specific data used for training and validation, and may not generalize well to unseen data.
- Once optimal hyperparameters are selected, the model is trained with the entire training set and evaluated on the test set to assess its final performance.
- **Leave-one-out**: a special case of K-Fold Cross Validation where K is equal to the number of data points in the dataset. Each data point is used once as a validation set while the remaining data points form the training set. This method can be computationally expensive for large datasets but provides an unbiased estimate of model performance.

# Cherry Picking



# Nested K-Fold Cross Validation



# Stratified K-Fold Cross Validation