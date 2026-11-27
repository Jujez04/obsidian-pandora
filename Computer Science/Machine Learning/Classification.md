# Bayes Classification
- If all probabilities are known, the Bayes classifier is optimal.
- For each $x \in V$ and for each $w_i \in W$, it is called $\rho(x|w_i)$ as *conditional probability density*
- Prior probability: $P(w_i)$
- Absolute probability: $P(x)$
- Then:
$$P(x) = \sum_{i=1}^s\rho(x|w_i)P(w_i)$$
- Where $\sum_{i=1}^s P(w_i) = 1$
- For the Bayes theorem: 
$$P(w_i|x) = \frac{P(x|w_i)P(w_i)}{\rho(x)}$$
- For each patter $x$, we can assign it to the class $w_i$ that maximizes the posterior probability $P(w_i|x)$.
- Prior probabilities: $P(w_i)$
- Conditional probabilities: $P(x|w_i)$
- The Bayes classifier assign to $x$ to the class $b$ the maximum posterior probability:
$$b = argmax_{w_i \in W} \{P(w_i|x)\}$$
- Then it means to maximize conditional probability and minimize the classification error
- Suppose that we a binary classifier and we have to minimize the classification error:
$$P(error) = \int_{R_1}\rho(x|w_2)P(w_2)dx + \int_{R_2}\rho(x|w_1) P(w_1)dx$$
![[Pasted image 20260929123537.png]]
- The area under the intersection of the two gives the probability of error. The Bayes classifier minimizes the probability of error. 
- $x^*$ is the point where we identify two region in which the classifier choose class $w_1$ ($\mathcal{R}_1 = (-\infty, x^*]$) or $w_2$ ($\mathcal{R}_2 = (x^*, +\infty)$)
- The *reducible error* is the area in which we choose $w_1$ then $w_2$ despite $w_2$ has higher probability, due to the fact it has an higher error in $x^*$.
- The *Bayes error* is when the error cannot be reduced and comes from the overlap between two distributions.
- Know conditional probabilities is impossibile in practice. There are two solutions that are adopted:
	- **Parametric approach**: making hypothesis on distributions (ex. multinormal distribution) and finding parameters of the training set
	- **Non parametric approach**: evaluate distributions from the training set (ex. Parzen Window)
- The parametric approach is used when the dataset isn't large enough or where the probabilities can be evaluated through reasoning(*hope*)

## Parametric approach
### Normal distribution
$$p(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$
- Where $\mu$ is the mean and $\sigma$ is the standard deviation, and $\sigma^2$ is the variance.
### Multinormal distribution
- We call:
	- $\text{x}_i$ as the i-th pattern
	- $x_i^j$ as the j-th component of the i-th pattern
- The density of the multinormal distribution is:
$$p(x) = \frac{1}{(2\pi)^{d/2}|\Sigma|^{1/2}}e^{-\frac{1}{2}(\text{x}-\mu)^T\Sigma^{-1}(\text{x}-\mu)}$$
- Where $d$ is the number of components, $\Sigma$ is the covariance matrix ($d \times d$) and $\mu$ is the mean vector
- The covariance matrix is always positive semidefinite and symmetric
- $\sigma^{ii}$ is the variance of the i-th component and $\sigma^{ij}$ is the covariance between the i-th and j-th component.
	- $\sigma^{ij} = 0$, then the two components are independent
	- $\sigma^{ij} > 0$, then the two components are positively correlated
	- $\sigma^{ij} < 0$, then the two components are negatively correlated
- Ex ($d=2$) :
![[Pasted image 20260929143035.png]]
- $\mu = [\mu^1, \mu^2]$ is the mean vector and controls the center of the distribution and each ellipse corresponds to a locus of points with constant density
- $\Sigma = \begin{bmatrix}\sigma^{11} & \sigma^{12} \\ \sigma^{21} & \sigma^{22}\end{bmatrix}$ is the covariance matrix, where the diagonal values control the ellipse shape and the off-diagonal values control the ellipse orientation.
### Mahalanobis Distance
- The Mahalanobis distance is a measure of the distance between a point and a distribution. It is defined as:
$$r = \sqrt{(x-\mu)^T\Sigma^{-1}(x-\mu)}$$
- The Mahalanobis distance is used to identify outliers in a dataset. A point is considered an outlier if its Mahalanobis distance is greater than a certain threshold (anomaly detection problems are based on it).
- Also it replaces the euclidean distance in the case of correlated variables, because it takes into account the correlation between variables.
![[Pasted image 20261005131910.png]]
#more on slides
- For example: in anomaly detection we can use Mahalanobis distance with gauss fitting(evaluation of $\mu$ and $\Sigma$) to evaluate if a data exceeds a threshold and then label it as an anomaly.
- Maximum likelihood vector: $\mu = \frac{1}{n} \sum_{i=1}^{n}x_i$
- Covariance: $\Sigma = \frac{1}{n}\sum_{i=1}^{n}(x_i-\mu)(x_i-\mu)^t$ o $\Sigma = \frac{1}{n}XX^t$
- Ex:
![[Pasted image 20261005132739.png]]
### Bayes Classifier with Multinormal Distribution
- With the multinormal distribution, the Bayes classifier can be expressed as:
$$b = argmax_{w_i \in W} \{P(w_i|x)\} = argmax_{w_i \in W} \{P(x|w_i)P(w_i)\}$$
- Where $P(x|w_i)$ is the multinormal distribution and $P(w_i)$ is the prior probability of class $w_i$.
- The main problem is to find the **decision boundary**, the border between two classes, that is the locus of points where the posterior probabilities are equal:
$$P(w_1|x) = P(w_2|x)$$
- The resulting decision boundary can be linear or non-linear depending on the covariance matrices of the classes:
    - If $\Sigma_1 = \Sigma_2$, then the decision boundary is linear -> hyperplane
    - ![[Pasted image 20261005133621.png]]
    - If $\Sigma_1 \neq \Sigma_2$, then the decision boundary is non-linear -> hyperquadric
	- ![[Pasted image 20261005133636.png]]
- #more on slides
### Classification Confidence
- For the fact that the Bayes classifier produce in output a probability, it is possible to evaluate the confidence of the classification. The confidence is defined as:
$$C = P(w_i|x)$$
- Where $w_i$ is the class assigned to $x$. The confidence can be used to evaluate the reliability of the classification. A low confidence value indicates that the classification is uncertain and a high confidence value indicates that the classification is reliable.
- This can be used for:
	- Reject option in open-set problems: if the confidence is lower than a threshold, the classifier can reject the classification and ask for human intervention.
	- Build a multi-classifier
### Parametric Bayes Reliability
- For the fact that we have to suppose what kind of distribution we have, the parametric approach is practically unreliable. 
- There are techniques that can help in evaluating the results of the distributions produced:
	- formal way: statistical tests (ex. Malkovich - Afifi test) to evaluate if the distribution is correct
	- empirical way: data clouds generated by 3d analysis tools or with histograms.
- Prior probabilities can be deduced from the dataset or they can be set to be equal for all classes.
- Then mean vector and maximum likelihood are evaluated when the model size is *fixed*.
- Main pros are associated to the fact that works well with few parameters and data with low variance. 
- Cons regards instead on the assumption of a functional form of the distribution of $P(x|w_i)$. It can't be all the times a Gaussian distribution due to the fact there are densities that could be bimodal or skewed.
- With more samples, μ and Σ are estimated more and more precisely, but they converge to the **best Gaussian** for that data, not to the true density. The gap between “best Gaussian” and “true shape” is a **systematic error (bias)** and doesn’t shrink with N.
## Non parametric Approach
- There are no assumptions on the data distribution and densities are evaluated directly on the training set
- So the main problem also applies the Bayes rule ($b = argmax_{w_i\in W} \space p(x|w_i) P(w_i)$) and with parametric approach we pick as an assumption that the distribution is Gaussian curve.
- In this case we pick no shape for the fact that the training set is directly the model
- But this can be more complex when the dimensionality grows (**curse of dimensionality**)
### Density Estimation
- The probability that a pattern $x$ belongs to $\mathcal{R}$ is:
$$P_1 = \int_{\mathcal{R}}p(x')dx'$$
- Given $n$ independent patterns and $k$ patterns in $\mathcal{R}$, the probability of $k$ patterns in $\mathcal{R}$ can be evaluated as a binomial distribution:
$$P_k = \binom{n}{k}P_1^k(1-P_1)^{n-k}$$
- The expected value of $k$ is:
$$E[k] = nP_1$$
- Assuming that the region of $\mathcal{R}$ (volume $V$) is small enough that the density is constant, we can approximate:
$$P_1 \approx p(x)V$$
- The probability density function can be evaluated as:
$$p(x) = \lim_{V\to 0}\frac{P_1}{V} = \frac{k}{nV}$$

### Parzer Window
- The region $\mathcal{R}$ is a hypercube with volume $V = h^d$ and it can be defined as the function $\phi$
$$\phi(u) = 
\begin{cases}
1 \space \text{ for  } |u_j| \leq \frac{1}{2}, \space j = 1,...d\\
0 \space \text{ otherwise}
\end{cases}
$$
- Then the number of patterns in $\mathcal{R}$ is:
$$k = \sum_{i=1}^{n}\phi\left(\frac{x-x_i}{h}\right)$$
- The probability density function can be evaluated as:
$$p(x) = \frac{1}{nh^d}\sum_{i=1}^{n}\phi\left(\frac{x-x_i}{h}\right)$$
- The size of the window $V_n$, where $n$ is the last pattern inside the window, can affect the impact on the result:
	- If $V_n$ is too small, the density is underestimated and the result is noisy and unstable
	- If $V_n$ is too large, the density is overestimated and the result is smooth and biased
- For convergence the window size must be evaluated taking into account the number of patterns:
$$V_n = \frac{V_1}{\sqrt{n}}$$
- Where $V_1$ is the initial volume of the window and an hyperparameter of the classifier.
### Soft Kernel Parzen Window
- There are different types of kernel functions that can be used to evaluate the density function and approximate the hypercube.
- In practice this is the most used due to the fact that each pattern contributes to the density function with a weight that depends on the distance from the pattern $x$.
- This resolves the *hard kernel problem*: the hard kernel is a binary function that gives the same weight to all patterns inside the window and zero to all patterns outside the window. This can lead to discontinuities in the density function and can be sensitive to the window size.
- With soft kernel we can have smoothed decision boundaries that model the data better.
![[Pasted image 20261010114457.png]]
- On the left: kernel hypercube
![[Pasted image 20261010114544.png]]
- On the right: kernel gaussian/normal
![[Pasted image 20261010114557.png]]
# Nearest Neighbor Classifier
- First of all in these problems we have to define a *distance function* (e.g euclidean distance)
- Nearest Neighbor Classifier classifies a pattern $x$ with the element $x'$ of the same class as the element more close in the training set:
$$dist(x,x') = min_{x_i\in TS}\{dist(x,x_i\}$$
- The main problem is to maximize the posterior probability, in fact if $x$ is close to $x'$ then $P(w_i|x) \approx P(w_i|x')$.
- There is theorem that states that the error probability $P$ of the nearest neighbor classifier is bounded by:
$$P^* \leq P \leq 2P^*$$
- Where $P^*$ is the Bayes error probability. This means that the nearest neighbor classifier is a good approximation of the Bayes classifier at the optimal case.
![[Pasted image 20261010135606.png]]

## Voronoi Taxellation
![[Pasted image 20261010135815.png]]
- The **Voronoi Taxellation** is a partition of the space into regions based on the distance to a specific set of points. Each region corresponds to a point and contains all the points that are closer to that point than to any other point.
- This is the rule of the nearest neighbor classifier: each pattern is classified based on the class of the nearest neighbor in the training set.
- Due to taxellation, the nearest neighbor classifier is quite unreliable due to the fact it can produce *outliers* that are misclassified.
## k-NN 
- The k-Nearest Neighbor (k-NN) rule determines the k elements closest to the pattern 𝐱 to be classified (k is a hyperparameter); each pattern among the k neighbors votes for the class to which it belongs; the pattern 𝐱 is assigned to the class that received the highest number of votes.
- It's done with odd number of neighbors to avoid ties.
- For $TSsize \rightarrow \infty$, the 1-NN is the best and converges to the Bayes classifier. For finite size, the best value of k is evaluated with cross-validation.
- #more on slides
![[Pasted image 20261010140919.png]]
- It's quite simple to obtain the confidence in which the pattern is classified. The confidence can be evaluated as:
$$C = \frac{v_i}{k}$$
- Where $v_i$ is the number of votes for the class $w_i$ and $k$ is the number of neighbors.
- The computational model can be expensive in terms of cost, due to the fact that for each pattern to classify we have to evaluate the distance with all the patterns in the training set.
- There are different techniques that avoid these problems:
	- editing/condensing the training set
	- using data structures like KD-tree or Ball-tree to reduce the number of distance evaluations
## Editing/Condensing and Class Prototypes
- The main idea is to reduce the size of the training set by removing patterns that are not useful for classification. This can be done by:
    - **Editing**: removing patterns from the Training Set that are misclassified by the k-NN rule.
    - **Condensing**: removing patterns that are redundant and store only the ones that define the decision boundary
- This can lead to evaluate less distances and increase reliability by using *prototypes*, which are patterns that represent a class and are used to classify new patterns.
- Vector Quantization or Clustering techniques can be used to find more prototypes per class.
## Distance Metrics 
- The distance metric is a function that defines the distance between two patterns. The most common distance metrics are:
    - Euclidean distance: $d(x,y) = \sqrt{\sum_{i=1}^{n}(x_i-y_i)^2}$
    - Manhattan distance: $d(x,y) = \sum_{i=1}^{n}|x_i-y_i|$
    - Minkowski distance: $d(x,y) = (\sum_{i=1}^{n}|x_i-y_i|^p)^{1/p}$
    - Mahalanobis distance: $d(x,y) = \sqrt{(x-y)^T\Sigma^{-1}(x-y)}$
    - Cosine distance: $d(x,y) = 1 - \frac{x \cdot y}{||x|| ||y||}$
### Normalization
- To avoid large scale patterns, it is possible to normalize the patterns before evaluating the distance. This can be done by:
    - Min-Max normalization: $x' = \frac{x-x_{min}}{x_{max}-x_{min}}$
    - Z-score normalization: $x' = \frac{x-\mu}{\sigma}$
- These techniques are used to scale the patterns to a common range or to have zero mean and unit variance for each feature *independently*.
### Whitening Transform
- The Whitening Transform acts to transform the patterns to have zero mean and unit variance for each feature *considering the correlation between features*. This is done by evaluating the covariance matrix of the training set and then applying the transformation:
$$x' = \Sigma^{-1/2}(x-\mu)$$
- The alternative is to use the Mahalanobis distance, which is a distance metric that takes into account the correlation between features. It is defined as:
$$d(x,y) = \sqrt{(x-y)^T\Sigma^{-1}(x-y)}$$
## Metric Learning #evolve
- LDA, PCA, t-SNE, UMAP, etc. are techniques that can be used to learn a distance metric from the data. These techniques can be used to reduce the dimensionality of the data and to find a distance metric that is more suitable for the classification task.
- Cosine Similarity

[[Metric Learning]]
# SVM
- Statistical Learning Theory - Vapnik, 1965
- It's based on finding *classification boundaries*
- There are several techniques that can be used to find the classification boundaries:
    - Linear with linearly separable data
    - Linear with non linearly separable data
    - Non Linear
    - Multiclass
![[Pasted image 20261012132307.png]]
- Main problem: finding the classification boundary that has the maximum *margin*.
- **Margin**: distance between the classification boundary and the closest pattern of each class. The patterns that are closest to the classification boundary are called *support vectors*. This concept is linked with the *generalization* of the classifier. The larger the margin, the better the generalization.
- This problem is a *convex optimization* problem that can be solved with *Lagrange multipliers* and *KKT conditions* and admit one global minimum solution.
## Linear
- Given two classes linearly separable and a training set with $n$ patterns ($x_i, y_i$) where $x_i \in \mathbb{R}^d$ and $y_i \in \{-1, 1\}$, the classification boundary(or the *hyperplane*) can be defined with the parameters ($w, b$):
$$D(x) = w x + b$$
- Where:
	- $w$ is the weight vector that defines the orientation of the hyperplane
    - $b$ is the bias that defines the position of the hyperplane
    - $\frac{b}{||w||}$: distance from the origin
- $x$ can be defined as: $x = x_p + r \frac{w}{||w||}$
- The distance of a vector $x$ from the hyperplane can be defined as: $r = \frac{D(x)}{||w||}$
- The hyperplanes that separate with the minimum distance $\frac{1}{||w||}$ are defined as:
$$
D(x)=
\begin{cases}
wx_i + b \geq +1  \space \text{if} \space y_i=1\\
wx_i + b \leq -1 \space \text{if} \space y_i=-1\\
\end{cases}$$
- Or: 
$$y_i(wx_i + b) \geq +1 \space \text{for}\space i = 1...n$$
![[Pasted image 20261012133803.png]]
- #recall Euclidean Distance: $d = ||x-x'||$
![[Pasted image 20261012160835.png]]
- $q = xx'$ and $t = x'x''$ are orthogonal and belongs to the hyperplane so $w^Tt+b = 0$ and $w^Tq + b = 0$
- With some evaluations we can say: 
$$d = \frac{w^Tx+b}{||w||}$$
- The main problem can be formalized in this way: Suppose that $s_1$ and $s_2$ are support vectors $$\frac{w^Ts_1+b}{||w||} + \frac{w^Ts_2+b}{||w||} = \frac{1}{||w||} + \frac{1}{||w||} = \frac{2}{||w||} = \tau$$
$$max \frac{2}{||w||} = min \frac{||w||}{2} = min \frac{w^Tw}{2}$$
$$\min_{\mathbf{w}, b} \frac{\mathbf{w}^\top \mathbf{w}}{2} \quad \text{s.t.} \quad y_i(\mathbf{w}^\top \mathbf{x}_i + b) \geq 1 \quad \forall i$$
- This is a quadratic optimization problem with linear constraints and can be solved with Lagrange multipliers:
$$L(w, b, \alpha) = \frac{w^Tw}{2} - \sum_{i=1}^{n}\alpha_i[y_i(w^Tx_i+b)-1]$$
- Where $\alpha_i \geq 0$ are the Lagrange multipliers.
- So this combines a first part where $w$ and $b$ should be minimized and the second part where $\alpha_i$ should be maximized. 
- Karush-Kuhn-Tucker:
$$\alpha_i \geq 0, \qquad y_i(\mathbf{w}^\top \mathbf{x}_i + b) - 1 \geq 0, \qquad \alpha_i \left[ y_i(\mathbf{w}^\top \mathbf{x}_i + b) - 1 \right] = 0$$
- The last condition (complementarity condition) means that either $\alpha_i = 0$ or $y_i(\mathbf{w}^\top \mathbf{x}_i + b) - 1 = 0$. This means that if $\alpha_i > 0$, then the corresponding pattern $x_i$ is a support vector and lies on the margin. Otherwise patterns that are far from the margin will have less impact, and their $\alpha_i$ will be zero.
- $\alpha_i$ will have optimal value, while other $\alpha_i$ will be zero. The patterns with $\alpha_i > 0$ are the **support vectors**, or more specifically are the vectors that lays on the decision boundary, satisfying this condition: $|w^Ts +b |=1$ 
- Applying the **Karush-Kuhn-Tucker conditions**, the problem can be formalized in dual form (called **Wolfe Dual**):
$$\max_{\alpha} \sum_{i=1}^{n}\alpha_i - \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jx_i^Tx_j$$
$$\text{s.t.} \quad \sum_{i=1}^{n}\alpha_iy_i = 0 \quad \text{and} \quad \alpha_i \geq 0$$
- This is an application of Hard duality theorem.
- For more details: 
	- S. Gunn, Support Vector Machines for Classification and Regression
	- C. Burges, A Tutorial on Support Vector Machines for Pattern Recognition
	- http://web.mit.edu/6.034/wwwbob/svm-notes-long-08.pdf
- The optimal hyperplane can be formalized as: 
$$w^* = \sum_{i=1}^{n}\alpha_i^*y_ix_i \text{ and } b^*=y_s-\sum_{i=1}^n \alpha_i^*y_i(x_i\cdot x_s)$$
- Where ($x_s, y_s$) is a support vector.
- As regards the distance from the hyperplane, we can say that:
$$D(x) = w^* \cdot x + b^* = \sum_{i=1}^{n} \alpha_i^* y_i (x \cdot x_i) + b^*$$
- The sign of this function gives the class of the pattern $x$.
- Characteristics:
	- The solution is based on a subset with few elements that defines the support vectors.
	- The number of support vectors defines the complexity of the classifier. The more support vectors, the more complex the classifier.
	- With scalar product, the dimensionality is irrelevant in terms of evaluations
	- The computational complexity is $O(n^2)$ for training (due to dual form) and $O(n)$ for classification.
- Soft margin version with slack variables: $y_i(w\cdot x_i + b) \geq 1 - \xi_i$ where $\xi_i \geq 0$ is the slack variable that allows some patterns to be misclassified, introducing deviations. For separable patterns $\xi_i = 0$, otherwise $\xi_i > 0$
- In this case the optimal hyperplane should maximize the margin and at the same time minimize the number of misclassified patterns. This can be formalized as:
$$\min_{w, b, \xi} \frac{||w||^2}{2} + C \sum_{i=1}^{n} \xi_i$$
$$y_i(w\cdot x_i + b) \geq 1 - \xi_i$$
- The *C* parameter is a hyperparameter that controls the trade-off between maximizing the margin and minimizing the number of misclassified patterns. A large value of *C* will lead to a smaller margin and fewer misclassified patterns, while a small value of *C* will lead to a larger margin and more misclassified patterns.
- With the dual form:
$$Q(\alpha) =  \sum_{i=1}^{n}\alpha_i - \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jx_i^Tx_j$$
$$\text{s.t.} \quad \sum_{i=1}^{n}\alpha_iy_i = 0 \quad \text{and} \quad 0 \leq \alpha_i \leq C$$
## Non Linear
- When there is a complex decision boundary, the linear SVM is not enough to separate the classes. In this case, we can use a non-linear SVM that uses a kernel function to map the data into a higher dimensional space where the classes are linearly separable.
- We define a non linear *mapping function* $\phi: \mathbb{R}^d \rightarrow \mathbb{R}^{d'}$ that maps the data into a higher dimensional space. The kernel function is defined as:
$$K(x_i, x_j) = \phi(x_i) \cdot \phi(x_j)$$
- The kernel function allows us to compute the dot product in the higher dimensional space without explicitly computing the mapping function $\phi$. This is called the *kernel trick*.
- Due to the fact that with the dual form of the problem we don't have to manipulate the dimensionality of the data, we can use the kernel trick to compute the dot product in the higher dimensional space without explicitly computing the mapping function $\phi$. This allows us to use SVMs for non-linear classification problems without having to worry about the curse of dimensionality.
- #recall Gram Matrix: With a set of vectors $x_1, x_2, ..., x_n$, the Gram matrix is defined as the matrix $n \times n$ that belongs to an euclidean or hilbert space with all possible scalar products between the vectors.
- **Mercer Theorem**: $K$ is a kernel function if and only if the *Gram matrix* defined on $K$ is positive semidefinite. This means that for any set of patterns $x_1, x_2, ..., x_n$, the matrix $K$ defined as:
$$K = \begin{bmatrix}
K(x_1, x_1) & K(x_1, x_2) & ... & K(x_1, x_n) \\
K(x_2, x_1) & K(x_2, x_2) & ... & K(x_2, x_n) \\
... & ... & ... & ... \\
K(x_n, x_1) & K(x_n, x_2) & ... & K(x_n, x_n) \\
\end{bmatrix}$$
- Is positive semidefinite. This means that for any vector $v \in \mathbb{R}^n$, we have $v^T K v \geq 0$.
- The most common kernel functions are:
    - Linear kernel: $K(x_i, x_j) = x_i \cdot x_j$
    - Polynomial kernel: $K(x_i, x_j) = (x_i \cdot x_j + c)^q$
    - Radial basis function (RBF) kernel: $K(x_i, x_j) = e^{(-\frac {||x_i - x_j||^2)}{2\sigma^2})}$ where $\sigma$ is a hyperparameter that controls the width of the Gaussian function.
    - Sigmoid kernel: $K(x_i, x_j) = tanh(\alpha x_i \cdot x_j + c)$ where $\alpha$ and $c$ are hyperparameters that control the slope and the intercept of the sigmoid function.
- The choice of the kernel function and its parameters can have a significant impact on the performance of the SVM. 
- The RBF kernel is the most commonly used kernel function due to its ability to handle non-linear decision boundaries and its flexibility in modeling complex data distributions.
- With the kernel trick, the decision boundary function can be defined as:
$$D(x) = \sum_{i=1}^n \alpha_i^*y_iK(x,x_i) + b^*$$
![[Pasted image 20261012172907.png]]
## Multiclass

- The **Multiclass** extension of SVMs can be done in two main ways:
    - One-against-One: For each pair of classes, a binary SVM is trained. For $k$ classes, this results in $k(k-1)/2$ classifiers. The final classification is done by voting among the classifiers.
    - One-against-All : For each class, a binary SVM is trained to separate that class from all other classes. For $k$ classes, this results in $k$ classifiers. The final classification is done by selecting the class with the highest decision function value. For each class $w_k$, an SVM is used to determine the separating surface between the patterns of $w_k$ (labeled +1) on one side and the patterns of all remaining classes $w_h$ ($h \neq k$, labeled -1) on the other, yielding the function $D_k(\mathbf{x})$, which indicates the distance of $\mathbf{x}$ from the decision surface in the direction of $w_k$. The higher the value of $D_k(\mathbf{x})$, the more confident we are that $\mathbf{x}$ belongs to $w_k$. At the end of training, pattern 𝐱 is assigned to class 𝑘∗ for which the distance from the decision surface is maximum:$𝑘^∗ = 𝑎𝑟𝑔𝑚𝑎𝑥_𝑘 \{D𝑘(𝐱)\}$. Note: It is necessary to perform 𝑠 SVM training
## Implementation
- SVM training requires non-trivial numerical algorithms capable of solving a quadratic programming problem. Some implementations are available online:
	- LIBSVM - http://www.csie.ntu.edu.tw/~cjlin/libsvm
		- Note: Kernels (RBF, etc.) are parameterized differently from the standard convention (see LibSvm's Readme.txt and [1]). Specifically, the gamma (𝛾) parameter is used to adjust the complexity of the decision surface. Increasing γ allows the surface to take on more complex shapes.
		- N.B. With the RBF kernel, γ operates inversely to 𝜎.
		- γ is also included in the polynomial kernel (alongside the polynomial degree and Coef0).
	- For multiclass classification, it internally uses the One-Against-One approach [2] (accurate but inefficient for many classes).
	- Wrapped by Scikit-Learn → sklearn.svm.SVC
	[1] C.W. Hsu, C.C. Chang, and C.J. Lin, A Practical Guide to Support Vector Classification, available on the LIBSVM website
	[2] C.C. Chang and C.J. Lin. LIBSVM: a library for support vector machines. ACM Transactions on Intelligent Systems and Technology, 2:27:1--27:27, 2011, available on the LIBSVM website
	- LIBLINEAR - https://www.csie.ntu.edu.tw/~cjlin/liblinear/ 
		- Same authors as LIBSVM; recommended for linear cases involving high dimensionality and a large number of patterns.
		- Wrapped by Scikit-Learn → sklearn.svm.LinearSVC
	- SVM-light - http://svmlight.joachims.org
## When use SVM?
- When data are omogeneous and the number of patterns is not too high (otherwise the training time can be very long).
- For $> 5000$ features we can use linear SVMs due to the fact that patterns are sparse and the linear SVM is able to find a good decision boundary.
- For lower dimensionalities ($< 20$) we can use non-linear SVMs and we have to regolate the C parameter.
- Parameters like C must be regulated with cross-validation or grid-search on the training set.
- With multiclass cases:
	- One-against-One is better for small number of classes (less than 10)
    - One-against-All is better for large number of classes (more than 10)
# Multi-class Classification
- A multi-class classifier is a model that sorts input data into one of three or more mutually exclusive categories.
- They could work either in *parallel* or *sequentially*. 
- *Decisions* done by classifier are *merged* at a certain level of classification.
- Combining multiple classifier is called **Ensemble Learing** and it is effective when the classifiers are *diverse* and *independent*.
- **Independency** can be obtained by:
	- Using different features
	- Using different algorithms for feature extraction
	- Using different classification algorithms
	- Training on different subsets of the training set (also called **bagging**)
	- Considering and combining **errors** done by predecessors (also called **boosting**)
- **Combination** can be either done at decision level or at confidence level.

## Decision Merge

- Each classifier gives a decision on the class of the pattern to classify. The final decision can be obtained by combining the decisions of the classifier in certain ways.
	- **Majority vote rule**: each classifier gives a vote for the class it predicts. The class with the most votes is the final decision. More formally:
$$\text{If } \{C_1, C_2,...\} \text{ is a set of classifiers } nc, \text{then\\ } \theta_{ij} = \begin{cases}
1 \space \text{if } C_i \text{ predicts class } w_j\\
0 \space \text{otherwise}
\end{cases} \text{\\ }
t = argmax_{j=1,...,nc} \sum_{i=1}^{nc} \theta_{ij}$$
	- **Borda count**: each classifier gives a ranking of the classes it predicts. The class with the highest total score is the final decision. In addition it considers also the second, third, etc. choice of the classifiers and not only the first choice.
![[Pasted image 20261023153200.png]]
- Also we can consider one-against-one and one-against-all approaches for multi-class classification. In the first case, we have to train $k(k-1)/2$ classifiers, where $k$ is the number of classes. In the second case, we have to train $k$ classifiers. The final decision can be obtained by combining the decisions of the classifiers in certain ways.
## Confidence Merge
- Each classifier $C_j, j=1...nc$ gives a *confidence* value for each classes $conf_j = [conf_{j1},conf_{j2},conf_{j3},...conf_{ji}]$ where the $i$-th component represents if the pattern belongs to the $i$-th class. The final decision can be obtained by combining the confidence values of the classifiers in certain ways.
    - **Sum rule**: the final confidence value for each class is the sum of the confidence values of the classifiers for that class. The class with the highest final confidence value is the final decision. More formally:
$$
t = argmax_{j=1,...,nc} \sum_{i=1}^{nc} conf_{ij}
$$
    - **Product rule**: the final confidence value for each class is the product of the confidence values of the classifiers for that class. The class with the highest final confidence value is the final decision. More formally:
$$
t = argmax_{j=1,...,nc} \prod_{i=1}^{nc} conf_{ij}
$$
	- Minimum rule: the final confidence value for each class is the minimum of the confidence values of the classifiers for that class. The class with the highest final confidence value is the final decision. More formally:
$$
t = argmax_{j=1,...,nc} \min_{i=1,...,nc} conf_{ij}
$$
	- Maximum rule: the final confidence value for each class is the maximum of the confidence values of the classifiers for that class. The class with the highest final confidence value is the final decision. More formally:
$$
t = argmax_{j=1,...,nc} \max_{i=1,...,nc} conf_{ij}
$$
- In practice, the sum rule is the most commonly used method for combining confidence values.
- **Weighted sum rule**: $avgsum = \sum_{j = 1}^{nc} g_j \cdot conf_j$ and $t = argmax_{i=1...s}\{avgsum_i\}$ where $g_j$ is the weight (*ability degree*) associated to the $j$-th classifier. The weights can be defined by classifier's performance. 
- Confidence can also represents **similarity**, so pay attention to the fact that the higher the confidence, the more similar the pattern is to the class. 

## Decision Trees
- For categorized data, decision trees are a good choice. They are simple to understand and interpret, and they can handle both numerical and categorical data. Decision trees can be used for both classification and regression tasks.
- ID3, C4.5, CART  (default in Random Forest).

### Pruning
- Pruning is a technique used to reduce the size of a decision tree by removing nodes that do not provide significant predictive power. This helps to prevent overfitting and improve the generalization of the model. There are two main types of pruning:
    - **Pre-pruning**: Stop growing the tree before it reaches its maximum depth or before all leaves are pure. This can be done by setting parameters such as `max_depth`, `min_samples_split`, or `min_samples_leaf`.
    - **Post-pruning**: Grow the full tree and then remove nodes that do not contribute significantly to the model's performance. This can be done using techniques such as cost-complexity pruning, which balances the complexity of the tree with its accuracy on a validation set.


### Random Forest
- Leo Breiman, 2001, Ensemble Method
- A single tree may have high variance and low bias, which means that it can overfit the training data. Random Forests reduce variance by averaging multiple trees, each trained on a different random subset of the data and/or features.
- **Bagging**: 
	-  Each tree is trained on a bootstrap sample of the training set, a random subset of size $n$ drawn **with replacement**.
	- On average, each bootstrap sample leaves out 37% of the original data (*out-of-bag samples*), which can be used to estimate the error without the need for a separate validation set.
	- Without this measure (also known as *feature bagging*), many trees would, with high probability, select the same variables (the most discriminating ones).
- **Feature Randomness**: 
    - At each node of the tree, a random subset of features is selected (typically $\sqrt{p}$ for classification and $p/3$ for regression).
    - The best split is chosen among the selected features.
- Increasing number of trees over a certain amount(>10000) does not improve the performance, but it increases the training time and memory usage.
- **Out-of-Bag**: a technique that evaluates model performance without needing a separate validation dataset or cross-validation. For each pattern in the training set, predictions are made using only the trees that did not include that pattern in their bootstrap sample. The final prediction is obtained by majority voting (for classification) or averaging (for regression) among these trees. The OOB error is then calculated by comparing the OOB predictions with the true labels of the patterns.
## Gradient Boosting

#TODO 

### AdaBoost
- Freund, Schapire, 1995
- Binary and multi-class classifier
- Let:
	- $𝐱_1, 𝑦_1 … 𝐱_𝑛, 𝑦_𝑛$ be the patterns in the training set (TS) and their respective labels, where $𝑦_𝑖 ∈ \{+1, −1\}$
	- $𝑤_𝑖^𝑡$ be the weight of pattern $𝐱_𝑖$ at iteration $𝑡$
	- $ℎ^𝑡(𝐱) ∈ \{+1, −1\}$ be the classifier introduced at iteration $𝑡$, chosen from the set of classifiers $ℎ_𝑗^𝑡(𝐱)$ available at iteration $𝑡$
	- $𝛼^𝑡$ be the weight of the classifier $ℎ^𝑡(𝐱)$ within the multi-classifier ensemble
1. **Initialize sample weights:** $$w_i^{(1)} = \frac{1}{n}, \quad \text{for } i = 1, \dots, n$$
2. **For $t = 1$ to $T$:**
	1. **Normalize the weights** so they sum to 1: $$w_i^{(t)} \leftarrow \frac{w_i^{(t)}}{\sum_{j=1}^{n} w_j^{(t)}}, \quad \text{for } i = 1, \dots, n$$ 
	2. **Evaluate candidate classifiers:** For each available candidate weak classifier $h_j^{(t)}(\mathbf{x}) \in \mathcal{H}$, compute the weighted error: $$\epsilon_j = \sum_{i=1}^{n} w_i^{(t)} \cdot \mathbb{I}\left(h_j^{(t)}(\mathbf{x}_i) \neq y_i\right)$$ where the indicator function is defined as: $$\mathbb{I}(\text{cond}) = \begin{cases} 1 & \text{if cond is true} \\ 0 & \text{otherwise} \end{cases}$$ 
	3. **Select the best weak classifier:** Choose $h_t(\mathbf{x}) = \arg\min_{h_j^{(t)}} \epsilon_j$, with minimum error $\epsilon_t = \min_j \epsilon_j$. d. **Compute the classifier weight (importance):** $$\alpha_t = \frac{1}{2} \ln\left(\frac{1 - \epsilon_t}{\epsilon_t}\right)$$
		- If the error $\epsilon^t = 0.5$: The classifier is equivalent to a coin flip (random behavior). Consequently, $\frac{1 - 0.5}{0.5} = 1$ and $\ln(1) = 0$, so $\alpha^t = 0$. Its vote has no influence whatsoever on the final decision. 
		- If the error $\epsilon^t < 0.5$ (a good classifier): The ratio $\frac{1-\epsilon^t}{\epsilon^t}$ is greater than 1, making $\alpha^t > 0$. As the error approaches 0, the logarithm increases significantly, and the weight assigned to that classifier becomes very large.
		- If the error $\epsilon^t > 0.5$ (worse than random chance): $\alpha^t$ becomes negative. A negative weight instructs the model to systematically flip the prediction provided by that weak classifier ("do the opposite of what it says"). ![[Pasted image 20261023173314.png]]
	4. **Update sample weights for the next iteration:** $$w_i^{(t+1)} = w_i^{(t)} \cdot \exp\left(-\alpha_t \cdot y_i \cdot h_t(\mathbf{x}_i)\right), \quad \text{for } i = 1, \dots, n$$
3. **Output final ensemble classifier:** $$H(\mathbf{x}) = \text{sign}\left(\sum_{t=1}^{T} \alpha_t h_t(\mathbf{x})\right)$$ ![[Pasted image 20261023174200.png]]
	- Recalling that the true labels $y_i$ and the predictions $h^t(x_i)$ take values ​​of either $+1$ or $-1$: 
		- The product $y_i h^t(x_i) = +1$ if the classifier has correctly identified the class (both are $+1$ or both are $-1$). 
		- The product $y_i h^t(x_i) = -1$ if the classifier has made an error. 
	- Assuming $\alpha^t > 0$ (the typical case for a good classifier): 
		- For a misclassified pattern, $y_i h^t(x_i) = -1$, which implies $z = -\alpha^t(-1) = +\alpha^t > 0$. As shown in the graph of $e^z$ on the right, for values ​​ $z > 0$, $e^z > 1$. Consequently, the weight $w_i^{t+1}$ increases. This forces the classifiers in subsequent iterations to focus more on this critical sample.
		- For a correctly classified pattern, $y_i h^t(x_i) = +1$, which implies $z = -\alpha^t(1) = -\alpha^t < 0$. In this case, $e^z < 1$, and therefore the weight $w_i^{t+1}$ decreases (the sample is already handled and becomes a lower priority).
- **Viola & Jones Face Detector**: in 2001 AdaBoost was used to develop the first real-time system for face-recognition:
	- Within a scanning window, approximately 160,000 oriented rectangular filters, known as *Haar features*, are calculated; these consist of light and dark areas.
$$
h_j(x) =
\begin{cases}
1 \space \sum(\text{pixels in white area})-\sum(\text{pixels in black area})>\theta_j
\\
-1 \space \text{otherwise}
\end{cases}
$$
	- In a real-world image, most of the examined sub-windows do not contain faces. Evaluating all the selected weak classifiers for every position would be too slow. Viola and Jones adopt a *cascade structure*:
		- Multiple strong classifiers are used, arranged in a series.
		- If one of the classifiers (at any level in the sequence) classifies the pattern as "non-face," it is immediately discarded.
		- Strong classifiers must be selected and tuned to minimize false negatives rather than the total classification error.
		- The classifiers at the beginning of the sequence are simple and fast (the first one uses only two weak classifiers and discards approximately 50% of the non-face patterns)
### XGBoost
#TODO



# Usage

- Use forest when there are many features and many samples