# Definitions
- **curse of dimensionality**: it's the main problem that many machine learning problems suffers in which the feature space is described with a huge amount of features, introducing *redundancy* in terms of data and *irrelevant* or *less information* in terms of features
	- Working in spaces with lower dimensionality helps in *generalization* and *performances*.
	- With this problem we don't want a way to delete features, instead we want something that *combines* features while reducing the dimensionality
- Solution -> **dimensionality reduction**: execute a *mapping function* on an initial space $R^d$  and produce a space with lower dimensionality $R^k$, where $k<d$.
- There are plenty of techniques that implements dimensionality reduction:
	- PCA: unsupervised linear mapping with the objective to hold the information from one space to another (bounded to the second order)
	- LDA: supervised linear mapping in which the mapping selects the most relevant patterns on the training set
	- t-SNE: unsupervised non-linear mapping that reduces the space into a 2/3 dimensional space for visualizing multidimensional data.
	- ICA: linear transformation that consider statistically independent patterns (unbounded to the second order)
	- kernel PCA: non-linear PCA
	- LLE: non-linear transformation for pattern relations
- They are specifically designed for one type of problem, i.e. LDA and PCA solutions are very different from one to another:
	- PCA keep the information during the mapping, identifying the hyperplane that divide one class from another
	- LCA instead consider the hyperplane in which we can distinguish one class from another
![[Pasted image 20261110115847.png]]



# PCA
- Given a training set $TS=\{x_i\in R^d, i = 1,...n\}$, and $$\bar{x}=\frac{1}{n}\sum_{i=1}^n x_i \rightarrow \text{mean} \in R^d \\ \Sigma = \frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})(x_i-\bar{x})^t \rightarrow \text{covariance matrix} \in R^{d\times d}$$
- Then given a $k$ that is $k<d, k<n, k>0$, the space $k$-dimensional space $S_{\bar{x},\Phi_k}$ is defined by the mean vector and the **projection matrix** $\Phi_k \in R^{d\times k}$ in which columns are *eigenvectors* of $\Sigma$ and for the most large $k$ *eigenvalues*: $$ \Phi_k = [\phi_{i_1}, \phi_{i_2}, \phi_{i_3}, ..., \phi_{i_k}] \space \text{with} \space \lambda_{i_1} \geq \lambda_{i_2} \geq \lambda_{i_3}, ...\geq \lambda_{i_d}$$
- $\phi_{i_r}$ eigenvector of $\Sigma$ corresponds to the eigenvalue $\lambda_{i_r}$
![[Pasted image 20261110122210.png]]
- The first $k$ eigenvectors are called **principal components** and the first eigenvector represents the direction with the most variance in the training set
- the eigenvalues specifies how the solution is stretched
- But how to apply PCA? This can done in two steps:
	- **Projection**: once we found the PCA space, we need to find the projection of the pattern in the PCA space, that means applying the transposed projection matrix to difference of that vector with its mean.$$\rightarrow{PCA}: R^d\rightarrow R^k \\ \rightarrow PCA(x, S_{\bar{x}, \Phi_k})= \Phi_k^t(x-\bar{x})$$ ![[Pasted image 20261111145552.png]]
	- **Retro-projection**: also we need to change back the vector that now belongs to the PCA space to the original space by reversing the operation. One thing to be noted is that the orthogonal component of $x$ is lost when the projection matrix is applied in the projection phase. In fact we obtain the best approximation of $x$ with the retro projection operation, in other words $x'$.  $$\leftarrow{PCA}: R^k\rightarrow R^d \\ \leftarrow PCA(y, S_{\bar{x}, \Phi_k})= \Phi_k y + \bar{x}$$
	- Example: ![[Pasted image 20261111150235.png]]
	- The ellipse represents the pattern distribution in the training set
	- $\phi_1$ e $\phi_2$ are the eigenvectors of the covariance matrix
	- The eigenvalues $\lambda_1$ e $\lambda_2$ are the variances of the distribution along the axes $\phi_1$ and $\phi_2$.
	- **Eckart-Young-Mirsky (EYM) Theorem**: is a fundamental result in linear algebra and multivariate statistics stating that the best low-rank approximation of a matrix can be found using its singular value decomposition (SVD) and cut it down to the first $k$ terms to obtain the projection matrix. $$X_k = U_k\Sigma_kV_k^t$$
- But how to choose $k$? 
	- This could be done by choosing the variables that preserves the *most relevant information* by looking its **variance** or in other words its **eigenvalues** . There are some techniques that can achieve the optimal $k$ value:
		- **Cumulative variance**: Given a percentage value $t$ ($t=95\%$) we choose the minimum $k$ such that $$k = argmin_z\{\frac{\sum_{i=1}^z\lambda_i}{\sum_{i=1}^d\lambda_i}\geq t\}$$
		- **Scree plot**: plot the eigenvalues in decrescent order in respect of $i$. Typically it has an "elbow" in which starting from the $k$ component the eigenvalues are lower the curve flattens out. ![[Pasted image 20261111160003.png]]
		- There are also other methods like the Kraiser criteria for standardized values and model based methods that are used in statistics.
	- ![[Pasted image 20261111160343.png]]
## PCA Whitening 
- It's a pre-normalization technique that involves rescaling the eigenvectors in a way that the variance is *uniform* (or equal to one) for each direction.
- Formally, if $y = \Phi_k^T(x - \bar{x})$ is the PCA projection, then whitening is defined as:
$$y_{\text{white}} = \Lambda_k^{-1/2} y = \Lambda_k^{-1/2} \Phi_k^T(x - \bar{x})$$

- where $\Lambda_k^{-1/2} = \text{diag}(1/\lambda_1, \dots, 1/\lambda_k)$.
- This means that correlations among directions are removed by rotating the eigenvectors in a way are aligned to the Cartesian axes. ![[Pasted image 20261111161427.png]]
- Transform the hyper-ellipse into an **hyper-sphere**:![[Pasted image 20261111161603.png]]
# LDA
- Linear and supervised dimensional reduction in which the main objective is to maximize the separation between classes by using their labels and minimize the internal dispersion.
![[Pasted image 20261111161801.png]]
- This objective can be achieved by using these **scattering** **matrices**:
	- **Within-class $S_w$**: indicates how the vectors are scattered with respect to the center of the classes. $$S_w = \sum_{i=1}^s S_i, \quad S_i = \sum_{x_j | y_j=i} (x_j - \bar{x}_i)(x_j - \bar{x}_i)^t$$ where $\bar{x}_i$ is the mean of the $i$-th class. If $S_w$ is small, each class is compact.
	- **Between-class $S_b$**: indicates how the class centers are scattered relative to the overall center of the distribution $$S_b = \sum_{i=1}^s n_i(\bar{x}_i - \bar{x}_0)(\bar{x}_i - \bar{x}_0)^t, \quad \bar{x}_0 = \frac{1}{n} \sum_i n_i \bar{x}_i$$where $n_i$ is the number of patterns in class $i$ and $\bar{x}_0$ is the global mean. If $S_b$ is large, the classes are well-separated.
- *Optimization criteria* (most used): $$J_1=tr(S_w^{-1}S_b)=\sum_{i=1}^d \lambda_i$$ where $tr$ it's the *trace* (sum of eigenvalues).
- Analogy with PCA and the EYM theorem: choosing the first $k$ eigenvalues (where also $k<s$ where $s$ is the number of classes) we can define the LDA space.

# Dimensional Reduction techniques for visualization
- They are used for visualizing multidimensional data
- They are typically unsupervised and non-linear
## t-SNE (t-distributed Stochastic Neighbor Embedding)
- Van der Maaten, Hinton, 2008
- Main idea: *preserving **local distances***. For each pattern we compute the probability distribution based on distances from the neighbours on the main space and another one in the 2D space with *t-Student*. Then the algorithm optimize the 2D distances in a way that are more similar to the original space by using t-Student.
- The t-Student gives a good solution to the "crowding problem": points would tend to crowd toward the center and it allows patterns to be more well separated 
- In scikit-learn there is the `perplexity` parameter that allows to select how many neighbours we need to control (typically between 5 and 50, check [distill.pub/2016/misread-tsne](https://distill.pub/2016/misread-tsne/) for more information)
- There are also some limitations:
	- It doesn't preserve the global structure
	- Computationally expensive ($O(n^2)$)
- Examples:
	- MNIST; L.J.P. van der Maaten and G.E. Hinton. Visualizing High-Dimensional Data Using t- SNE. Journal of Machine Learning Research, 2008; https://lvdmaaten.github.io/tsne/
![[Pasted image 20261111171226.png]]
- With scikit-learn:
```python
from sklearn.manifold import TSNE

tsne = TSNE(n_components=2, perplexity=30, n_iter=1500, init='random', random_state=42, verbose=2)

X_embedded_hog = tsne.fit_transform(hog_features_x)

plt.scatter(X_embedded_hog[0:1000, 0], X_embedded_hog[0:1000, 1], c="r")
plt.scatter(X_embedded_hog[0:1000, 0], X_embedded_hog[0:1000, 1], c="g")
plt.show()
```
![[Pasted image 20261111171610.png]]
## UMAP (Uniform Manifold Approximation and Projection for Dimension Reduction)

- McInnes, Healy, 2018 -> McInnes, L, Healy, J, UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction, ArXiv e-prints 1802.03426, 2018
- Main idea: build up a neighbour graph of the original space and then optimize a 2D representation of that graph. It preserves local distances, maintaining how clusters are separated from one to another.
- Differently from t-SNE preserves better the global structure and inter-cluster distances independently from their original similarity.
- https://umap-learn.readthedocs.io/en/latest/
- Example: 
![[Pasted image 20261111172231.png]]