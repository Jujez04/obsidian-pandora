- In regression problem we have *numerical values* as the main targets to be achieved
- If with classification there is a label $y_i$ that has to be predicted, with regression that label is a *numerical and continue value in* $R$ and the main goal of the model is to learn a **function** $f(x):R \rightarrow y$  
- In regression $x$ is called **independent** variable and $y$ is called **dependent** variable.
- If $x$ is a *vector*, the learning function is a *regression hyperplane*.
# Linear 

## Simple linear regression
- Given a training set TS containing $n$ patterns $(x_1, y_1), \dots, (x_n, y_n)$, and the following relationship between the independent and dependent variables:
$$y_i = \alpha + \beta \cdot x_i + \varepsilon_i$$
- where:
	- $\varepsilon_i$ is the (unknown) measurement error for pattern $x_i$
	- $\alpha, \beta$ are parameters to be determined
- **Least Square**:The solution of this problem is
$$
f(x)=y=\alpha+\beta\cdot x
$$
- That minimize the **Mean Square Error**(MSE):$$
\alpha^*,\beta^*=argmin_{\alpha,\beta}\sum_{i=1}^{n}(f(x)-y_i)^2=\sum_{i=1}^n\varepsilon_i^2
$$
- This problem can be solved by using **partial derivatives** with respect to the parameters $\alpha$ and $\beta$ and setting them to zero, obtaining the following formulas:
$$\beta^*=\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^{n}(x_i-\bar{x})^2}$$
$$\alpha^*=\bar{y}-\beta^*\cdot\bar{x}$$

- Where $\bar{x}$ and $\bar{y}$ are the mean values of the independent and dependent variables, respectively.
## Multiple linear regression
- For $x\in R^d$ where $d$ are the number of features, the regression function is a hyperplane:
$$f(x)=y=\alpha+\sum_{j=1}^{d}\beta_j\cdot x_j$$
- In matrix form:
- $$f(X)=y=X\cdot\beta$$
- Where $X$ is the matrix of independent variables, $\beta$ is the vector of $d + 1$ elements of coefficients, and $y$ is the vector of dependent variables.
$$X=
\begin{bmatrix}
x_{11} & x_{12} & \dots & x_{1d} 1\\
x_{21} & x_{22} & \dots & x_{2d} 1\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \dots & x_{nd} 1
\end{bmatrix}
$$
$$
y=\begin{bmatrix}
y_1 \\
y_2 \\
\vdots \\
y_n
\end{bmatrix}

\beta=\begin{bmatrix}
\beta_1 \\
\beta_2 \\
\vdots \\
\beta_{d+1}
\end{bmatrix}
$$
- The objective function to be minimized (MSE) can be expressed in matrix form as:
$$
MSE= ||y-X\beta||^2
$$
- The *normal equation* can be derived by taking the derivative of the MSE with respect to $\beta$ and setting it to zero:
$$
\frac{\partial MSE}{\partial \beta} = -2X^T(y - X\beta) = 0
$$
- Resolving:
$$
X^Ty = X^TX\beta \implies \beta = (X^TX)^{-1}X^Ty
$$
- Note that $X$ must be invertible:
	- $n=d+1 \implies X$ is square and invertible -> *interpolation*
	- $n>d+1 \implies X$ is rectangular and not invertible -> *approximation*
	- $X^TX$ is invertible if $X$ has full column rank, which means that the columns of $X$ are linearly independent. This condition is satisfied when there are no perfect multicollinearities among the independent variables.
	- $X^TX$ is often *bad conditioned*, then we can use *SVD decomposition* (Singular Value Decomposition) to compute the pseudo-inverse of $X$:$$
X= U\Sigma V^T 
$$
	- Where $U$ is an orthogonal matrix of size $n \times n$, $\Sigma$ is a diagonal matrix of singular values of size $n \times d + 1$, and $V^T$ is an orthogonal matrix of size $(d+1) \times (d+1)$. 
	- It can be demonstrated that: $$
\beta^*= V\Sigma^+U^Ty
$$
	- Where $\Sigma^+$ is the *pseudo-inverse* of $\Sigma$, obtained by taking the reciprocal of the non-zero singular values and transposing the resulting matrix.
## Regression vs Geometrical fitting
![[Pasted image 20261026170315.png]]
- Linear regression (least squares) minimizes the “vertical” distances from the hyperplane, rather than the Euclidean distances (which, as we shall see, can be easily obtained via PCA).
- This is a consequence of the fact that the independent and dependent variables do not play a symmetrical role (the independent variable is considered exact, while the dependent variable is subject to error).
- Note: swapping the two variables changes the result.
- In the case of geometric point clouds, the coordinates play the same role, and PCA fitting is preferable.

## Price Estimation Problem
- California Housing Dataset
- Independent values:  \[MedInc, HouseAge, AveRooms, AveBedrms, Population, AveOccup, Latitude, Longitude]
- Dependent value: \[MedHouseVal] where $1 =100K\$$
```python
X_train, X_test, y_train, y_test = train_test_split(housing.data, housing.target,test_size = 0.2)

lin_reg = LinearRegression()
lin_reg.fit(X_train, y_train)
y_train_predicted = lin_reg.predict(X_train)
rmse = np.sqrt(mean_squared_error(y_train, y_train_predicted))
print('Train RMSE: ', rmse)
y_test_predicted = lin_reg.predict(X_test)
rmse = np.sqrt(mean_squared_error(y_test, y_test_predicted))
print('Test RMSE: ', rmse)

X_train_b = np.c_[np.ones((X_train.shape[0],1)), X_train]
X_test_b = np.c_[np.ones((X_test.shape[0],1)), X_test]
beta_star = np.linalg.inv(X_train_b.T.dot(X_train_b)).dot(
X_train_b.T).dot(y_train)
y_train_predicted = X_train_b.dot(beta_star)
```
- Train RMSE:  0.7233
- Test RMSE:  0.7274 ($72 K\$$  error)
## Prediction accuracy
- **RMSE (Root Mean Square Error)**: 
	- $RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}$
	- Used to measure the average magnitude of the errors in a set of predictions, without considering their direction. It’s the square root of the average of squared differences between prediction and actual observation.
- **MAE (Mean Absolute Error)**:
	- $MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat{y}_i|$
	- Used to measure the average magnitude of the errors in a set of predictions, without considering their direction.  
	- Consider that square errors penalize larger errors more than smaller ones, while absolute errors treat all errors equally.
- **MAPE (Mean Absolute Percentage Error)**:
	- $MAPE = \frac{1}{n}\sum_{i=1}^{n}\left|\frac{y_i - \hat{y}_i}{y_i}\right|$
	- Used to measure the accuracy of a forecasting method in predicting values. It expresses the error as a percentage of the actual values, making it easier to interpret and compare across different datasets or models.
- **R2 (R Squared or Determination Coefficient)**:
	- $R^2 = 1 - \frac{\sum_{i=1}^{n}(y_i - \hat{y}_i)^2}{\sum_{i=1}^{n}(y_i - \bar{y})^2}$
	- Used to measure the proportion of the variance in the dependent variable that is predictable from the independent variable(s). It provides an indication of how well the model fits the data, with values closer to 1 indicating a better fit.
	- It is not valid for linear regression with a single variable, as it can be negative if the model is worse than simply predicting the mean of the dependent variable.
## Non Linear Independent Variables
- *Regression function must be linear in the parameters, but not necessarily in the independent variables.*
- Due to the fact that the relationship between the independent and dependent variables may not be linear, we can use **non-linear transformations** of the independent variables to capture more complex relationships. Some common transformations include:
    - Polynomial features: $x^2, x^3, \dots$
    - Logarithmic features: $\log(x)$
    - Exponential features: $e^x$
- Ex: chickens in intensive training treatment. We have to measure the weight of the chicken after 10 weeks but the model with quadratic regression doesn't satisfy the accuracy. Due to the fact that the density is linked with the weight of the chicken by its volume we can introduce a non linear term that capture this information:$$y=\beta_1x^{\frac{3}{2}}+ \beta_2x+ \beta_3$$
 
# Non-Linear

## Known dependency and Optimization
- If the dependency between the independent and dependent variables is known, we can use **non-linear regression** to fit the data. This involves specifying a non-linear function that describes the relationship between the variables and estimating the parameters of that function using optimization techniques.
- #opt For example in biology, more specifically in enzyme-mediated reaction kinetics, the relationship between the substrate concentration and the reaction rate can be described by the **Michaelis-Menten equation**:
$$v = \frac{V_{max} [S]}{K_m + [S]}$$
- #opt Where $v$ is the reaction rate, $[S]$ is the substrate concentration, $V_{max}$ is the maximum reaction rate, and $K_m$ is the Michaelis constant. In this case, we can use non-linear regression to estimate the parameters $V_{max}$ and $K_m$ from experimental data.
- If the dependency is known we can use *iterative techniques* that optimize the parameters of the non-linear function to minimize the error between the predicted and observed values:
	- Gradient Descent
	- Gauss-Netwon
 
## Non-Linear Regressors (Unknown Dependency)
- These represents a class of machine learning algorithms that can model complex relationships between independent and dependent variables without requiring a predefined functional form. 
- They are particularly useful when the underlying relationship is unknown or difficult to specify. 
- Some common non-linear regressors include:
	- Support Vector Regressor (SVR)
	- Random Forest Regressor
    - Gradient Boosting Regression Trees
- Main advanteges:
	- They can *capture complex, non-linear relationships* in the data and *control it*.
	- More *flexibility* with more degrees of freedom
	- They can use more *alternative loss*

## Support Vector Regressor
- The **Support Vector Regressor (SVR)** is an extension of the Support Vector Machine (SVM) algorithm for regression tasks. It aims to find a function that approximates the relationship between independent and dependent variables while maintaining a certain level of accuracy. 
- The main idea behind SVR is to find a hyperplane in which we want *the largest possible number of patterns from the training set to fall within the margin band.*
![[Pasted image 20261026173615.png]]
- The approximation can be linear or non-linear (e.g., RBF).
- The hyperparameter C and the regularizers of the kernel used (in the non-linear case) allow for the control of overfitting and help keep the regressor sufficiently smoothed.
## Random Forest Regressor
- Uses CART trees for regression.
- Works exactly like Random Forest in [[Classification]] (it is an ensemble method and so on)
### CART
![[Pasted image 20261026174230.png]]
- The **CART** (*Classification and Regression Trees*) algorithm constructs the tree using a **greedy top-down** strategy: at each node, it seeks the split that most effectively reduces the error on the data, without backtracking to re-evaluate previous choices.
- The splitting process is performed (in a greedy manner) by considering a single feature $k$ and a threshold $t_k$.

$$\mathcal{L}(j, t) = \frac{|S_L|}{|S|} \cdot \text{MSE}(S_L) + \frac{|S_R|}{|S|} \cdot \text{MSE}(S_R)$$
- where $S_L = \{i \in S : x_{ij} \leq t\}$ and $S_R = \{i \in S : x_{ij} > t\}$ are the left and right subsets.
- It continues recursively in this manner until a stopping condition is met (e.g., maximum depth, minimum number of samples, etc.).
- One of the most valuable features of trees is **interpretability**: it is possible to see exactly which rules the model has learned, unlike models such as neural networks or SVMs, which are essentially black boxes.
- **Feature importance** measures the extent to which each feature contributed to reducing the overall MSE during the construction of the tree. For each feature $j$:

$$\text{importance}(j) = \sum_{\text{nodes } t \text{ that use } j} \frac{|S_t|}{n} \cdot \Delta\text{MSE}(t)$$

- where $\Delta\text{MSE}(t)$ is the reduction in MSE achieved by the split at node $t$. The values ​​are normalized so that they sum to 1.
## Gradient Boosting Regressor Trees
- Gradient Boosting is an ensemble technique that combines the predictions of multiple weak learners (typically decision trees) to create a strong predictive model. It builds the model in a stage-wise fashion, where each new tree is trained to correct the errors made by the previous trees. The key idea is to minimize a loss function by iteratively adding trees that improve the model's performance and minimize the **residual value** of the loss function.
- Ex with MSE:
![[Pasted image 20261026175018.png]]
- In classification problems there is a *mapping function* that maps the predicted values to class labels, while in regression problems there is no such mapping function. 
- The predicted values are continuous numerical values that can take any value within a certain range, depending on the input features and the learned model.
### XGBoost

#TODO
## Quantile regression
- Quantile regression is a type of regression analysis that estimates the conditional quantiles of the response variable, rather than the mean. It provides a more comprehensive view of the relationship between the independent and dependent variables, allowing for the modeling of different parts of the distribution. This is particularly useful when the data exhibits heteroscedasticity or when we are interested in understanding the impact of predictors on different quantiles of the response variable.
- The quantile regression model can be expressed as:
$$Q_y(\tau | x) = \alpha(\tau) + \beta(\tau) \cdot x$$
- where $Q_y(\tau | x)$ is the $\tau$-th conditional quantile of the response variable $y$ given the independent variable $x$, and $\alpha(\tau)$ and $\beta(\tau)$ are the parameters to be estimated for the $\tau$-th quantile. The parameters are estimated by minimizing the **quantile loss function** (MAE loss), which is defined as:
$$
L_\tau(y, \hat{y}) = \sum_{i=1}^{n} \rho_\tau(y_i - \hat{y}_i)
$$
- where $\rho_\tau(u)$ is the **check function**:
$$
\rho_\tau(u) = \begin{cases}
\tau u & \text{if } u \geq 0 \\
(\tau - 1) u & \text{if } u < 0
\end{cases}
$$
- With 50% quantile regression, we are estimating the median of the response variable, while with other quantiles (e.g., 25%, 75%), we can analyze the lower and upper tails of the distribution. 
- Quantile regression is particularly useful in situations where the data has outliers or when the conditional distribution of the response variable is skewed, as it provides a more robust estimation of the relationship between variables compared to traditional mean regression methods.

# Which regressor model?
- With few patterns and few features:
	- Simple Linear Regression
	- Multiple Linear Regression
- With few patterns and many features:
	- SVR
- With many patterns and few features:
    - Random Forest Regressor
    - Gradient Boosting Regressor (optionally with Neural Network)