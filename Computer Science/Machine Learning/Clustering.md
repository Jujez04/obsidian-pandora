# Problem Definition and Algorithms
- The clustering problem consists of grouping a set of objects in such a way that objects in the same group (called a **cluster**) are more similar (in some sense or another) to each other than to those in other groups (clusters).
- It's a NP hard problem, due to the fact that the number of possible partitions of a set grows exponentially with the size of the set: given $n$ patterns and $s$ clusters, the number of possible solutions is given by the Stirling number of the second kind $S(n,s)$, which can be computed recursively as follows:
$$S(n,s) = s * S(n-1,s) + S(n-1,s-1) \approx \frac{s^{n}}{s!}$$
- If $s$ in not given then the number of possible solutions is given by the Bell number $B(n)$, which can be computed recursively as follows:
$$B(n) = \sum_{k=0}^{n-1} \binom{n-1}{k} B(k)$$
## Criteria for Clustering
- Criteria are used in this case to determine an optimal solution with an algorithmical approach. The most common criteria are:
	- **Minimization of centroid distance**: minimize the quadratic distance between the centroid of a cluster and the points in that cluster. This is good criteria for cluster with a *radial symmetry*, in which the points are distributed around the centroid. .Given $C_i$ the $i$-th cluster, $n_i$ the number of patterns that are contained and $\bar{x_i}$ is the centroid: $$J_e=\sum_{i=1}^s\sum_{x\in C_i}||x-\bar{x_i}||^2 \space \text{where } \bar{x_i}=\frac{1}{n_i}\sum_{x\in C_i}x$$
	- **Minimization of intra-cluster distance**: this is a cohesion property that aims to minimize the distance between the points in a cluster.
		- Main function $J_e = \sum_{i=1}^s n_i \cdot \bar{s}_i$ where $\bar{s}_i$ is the distance between the points in the cluster $C_i$ and $n_i$ is the number of patterns in that cluster.
			- If patterns are far from each other then $J_e$ is high and vice versa, if they are close to each other then $J_e$ is low (and it is a compact cluster).
		- $s_i$ can be defined in many ways due to the fact it is the distance. The main idea is this: $s_i = \frac{1}{n}\sum_{x \in C_i}f_s(x,C_i)$, where $f_s (x,C_i)$ is the distance between the pattern $x$ and the cluster $C_i$. The most common definitions are:
            - **Average distance**(1): $f_s(x,C_i) = \frac{1}{n_i}\sum_{x'\in C_i}||x-x'||^2$ 
            - **Minimum distance** (2): $f_s(x,C_i) = \min_{x'\in C_i \wedge x' \neq x}||x-x'||^2$ -> useful if the cluster is stretched, in fact it is enough that one point is close to another to consider the cluster compact.
		- Ex: for $s=2$ the (1) promotes the aggregation done by dashed circles and (2) promotes the aggregation with blue and red points.  ![[Pasted image 20261102171650.png]]



## Algorithms
- Main algorithms for clustering are:
	- **Hierarchical Clustering**: *bottom-up* operations are done to create a hierarchy based on distances and the structure results in a **dendrogram**.
	- **Centroid-Based Clustering**: heuristic approaches are used to find the centroids of clusters and assign patterns to them in a way that their distances among each other is minimized.
	- **Density-Based Clustering**: clusters are defined as areas of high density of patterns.
- Another distinction:
	- **Hard clustering (exclusive)**: each pattern belongs to one and only one cluster.
    - **Soft clustering (fuzzy)**: each pattern belongs to one or more clusters with a certain probability.
- The assignment can be made exclusive by choosing, for each pattern, the cluster for which the degree of membership is highest.
# Hierarchical Clustering
- This algorithms are generally *bottom-up* (agglomerative), but there are also *top-down* (divisive) approaches.
- How bottom up works:
	- At the beginning each pattern is a cluster.
	- The two closest clusters are merged into one cluster.
	- Then iteratively at each *level* there is a new merge, that could be between:
		- Two clusters
		- Two patterns
		- A pattern and a cluster
- This works with a *similarity function* that measures the relation between two patterns in terms of distance.
- At each level there is threshold that defines the maximum distance between two clusters to be merged.
- Main differences in terms of implementation depends on how the similarity is evaluated:
	- *Single Link*: minimum distance between two clusters.
	- *Average Link*: average distance between two clusters.
	- *Complete Link*: maximum distance between two clusters.
![[Pasted image 20261102173540.png]]
# Centroid-Based Clustering
## K-means
- The K-means algorithm is a centroid-based clustering algorithm that aims to partition a set of $n$ patterns into $s$ clusters by minimizing the distance between the patterns and their respective cluster centroids.
- It requires in input the number of clusters and an initial set of centroids, which can be chosen randomly or by using some heuristic methods.
- The optimization is *iterative* and *local*, so it converges to a local minimum of the objective function.
- The solution can be an *hyper-sphere* if the distance is Euclidean, but it can be a *hyper-ellipsoid* if the distance is Manhattan.
- Each centroid is moved to the arithmetic **mean** (center of mass) of all the points belonging to its cluster.
- Base algorithm:
```
Initialize n,s
Choose s patterns as initial centroids
do {
	assign for each pattern the closest cluster with the minimum distance to the respective centroid
	evaluate the new centroids for each cluster
} while(centroids change and iteration < max)
```
![[Pasted image 20261102183523.png]]
- It doesn't work well with clusters of different sizes and densities, and it is sensitive to outliers.
- *Clustering Validation* for determining the initial solution and the number of classes, that can be chosen by using some heuristics or by using some validation methods, such as the *Elbow Method* or the *Silhouette Method*.
- Validation techniques are based a posteriori evaluation of the clustering results, and they are used to compare different clustering solutions and choose the best one.
	- Problem: Considering the criterion of "minimizing distances from the centroids" and running K-means with different values ​​of $s$, it is much easier to obtain lower values ​​of $J_e$ when $s$ is large. What is the value of $J_e$ when $s = n$ (i.e., each cluster contains only a single pattern)?
	- Possible solution: assign a *penalty* to the objective function that increases with the number of clusters, so that the objective function is minimized when the number of clusters is optimal.
- Main validation techniques:
	- **Elbow Method**: 
        - Plot the objective function $J_e$ as a function of the number of clusters $s$.
        - The optimal number of clusters is the one that corresponds to the "elbow" of the curve, where the decrease in $J_e$ becomes less pronounced.S
        - ![[Pasted image 20261102184358.png]]
	- **Silhouette Method**: 
        - For each pattern $x$, the silhouette coefficient $s(x)$ is defined as follows:
        $$s(x) = \frac{b(x) - a(x)}{\max(a(x), b(x))}$$
        where $a(x)$ is the average distance between $x$ and all other patterns in the same cluster, and $b(x)$ is the minimum average distance between $x$ and all other clusters.
        - The silhouette coefficient ranges from -1 to 1, where a value close to 1 indicates that the pattern is well clustered, a value close to 0 indicates that the pattern is on the border of two clusters, and a value close to -1 indicates that the pattern is misclassified.
        - The optimal number of clusters is the one that maximizes the average silhouette coefficient over all patterns.
        - Computationally expensive for large datasets
## Fuzzy K-means
- The Fuzzy K-means algorithm is a soft clustering algorithm that allows patterns to belong to multiple clusters with a certain *degree of membership*, or more specifically a probability.
- The optimization criteria is to minimize the weighted distance between the patterns and the cluster centroids, where the weights are given by the degree of membership of each pattern to each cluster. $$J_{fuz}=\sum_{i=1}^s\sum_{j=1}^{n}[P(C_i|x_j,\theta)]^m\cdot||x_j-\theta_i||^2$$
- Where $P(C_i|x_j,\theta)$ is the degree of membership of pattern $x_j$ to cluster $C_i$, $\theta=\{\theta_1, ..., \theta_s\}$ is the set of centroids, where $\theta_i$ is the centroid of $C_i$, and $m$ is a parameter that controls the fuzziness of the clustering (usually set to 2).
- Centroids are evaluated as the *weighted mean* of the patterns in the cluster, where the weights are given by the degree of membership of each pattern to the cluster. $$\theta_i=\frac{\sum_{j=1}^{n}P(C_i|x_j,\theta)\cdot x_j}{\sum_{j=1}^{n}P(C_i|x_j,\theta)}$$
- The degree of membership is evaluated as the inverse of the distance between the pattern and the centroid, normalized over all clusters. $$P(C_i \mid x_j, \Theta) = \frac{1}{\sum_{k=1}^s \left( \frac{\|x_j - \theta_i\|}{\|x_j - \theta_k\|} \right)^{\frac{2}{m - 1}}}$$
- Where $m>1$ is the fuzziness parameter that controls the degree of membership. When $m=1$, the algorithm reduces to the K-means algorithm, where each pattern belongs to one and only one cluster.
```
Initialize n,s
Randomly intitialize 𝑃(𝐶𝑖|𝐱𝑗, Θ) for each i=1,...s and j=1,...n
do {
	evaluate centroids 𝜃𝑖 for each cluster 𝐶𝑖
	evaluate 𝑃(𝐶𝑖|𝐱𝑗, Θ) for each i=1,...s and j=1,...n
} while(Jfuz significantly change and iteration < max)
```
- It is more robust to noise and outliers than K-means, but it depends more intensively on the number of clusters.
## Expectation - Maximization (EM) with Gaussian Mixture
- The EM algorithm is a soft clustering algorithm that assumes that the data is generated from a mixture of Gaussian distributions, where each cluster corresponds to a Gaussian component and the final result is a *multi-modal* distribution of patterns:
![[Pasted image 20261102191234.png]]
- The fundamental assumption is that the patterns $\mathcal{X} = \{x_1, \dots, x_n\}$ have been generated by a mixture of $s$ multinormal (Gaussian) distributions:
   $$p(x \mid \Theta) = \sum_{i=1}^s \alpha_i \cdot p_i(x \mid \theta_i)$$
- where: $\alpha_i$ is the weighting coefficient (or prior probability) of the $i$-th component, with $\sum_{i=1}^s \alpha_i = 1$ and $\alpha_i \ge 0$; $\theta_i = \{\mu_i, \Sigma_i\}$ comprises the mean vector $\mu_i$ and the covariance matrix $\Sigma_i$ of the $i$-th Gaussian; and $\Theta = \{\alpha_1, \dots, \alpha_s, \theta_1, \dots, \theta_s\}$ encompasses all the unknown parameters of the model.
- Main goal: to determine, based on the patterns in the training set, the parameters of the individual distributions that generated them. -> **Maximizing the likelihood** of the observed data given the model parameters. -> In other words it is an inversion problem: given the patterns, we want to find the parameters of the distributions that generated them. -> $L(\Theta|\mathcal{X}) = p(\mathcal{X}|\Theta)$
- The main target function in reality is the log of the likelihood function, which is easier to maximize: $$\log p(\mathcal{X} \mid \Theta) = \sum_{j=1}^n \log \left( \sum_{i=1}^s \alpha_i \cdot p_i(x_j \mid \theta_i) \right)$$
- Unfortunately, this maximization is very difficult due to the summation inside the logarithm. To eliminate the summation, we would need to know, for each pattern $\mathbf{x}_j$ in the training set, which component $p_i$ of the mixture it was generated from.
- To overcome this obstacle, EM introduces a latent (unobserved) variable $y_j \in \{1, \dots, s\}$ for each pattern $x_j$, which indicates which component actually generated the data. The complete dataset thus becomes $\mathcal{Z} = \{[x_j, y_j]\}_{j=1}^n$, for which the calculation of the log-likelihood would be straightforward.
- **Complete-log-likelihood**:
$$\log p(\Theta \mid \mathcal{Z} ) = \log L(\Theta|\mathcal{X},\mathcal{y}) = p(\mathcal{X},\mathcal{y}|\Theta) = \sum_{j=1}^n \log \left( \alpha_{y_j} \cdot p_{y_j}(x_j \mid \theta_{y_j}) \right)$$
- The EM algorithm consists of two main steps that are iteratively applied until convergence:
1. **Expectation Step (E-step)**: Given the current estimates of the parameters $\Theta^g$, compute the *expected value* of the complete-log-likelihood with the distribution of the aleatory value $y$ that is $f(y|\mathcal{X},\Theta^g)$: $$Q(\Theta \mid \Theta^g)=E(log(p(\mathcal{X},y|\Theta))|\mathcal{X}, \Theta^g) = \int_{y\in\Psi}log(p(\mathcal{X},y|\Theta)\cdot f(y|\mathcal{X},\Theta^g)dy $$ $$Q(\Theta\vert{}\Theta^g) = \sum_{y \in \Psi} \left( \sum_{j=1..n} \log (\alpha_{y_j} \cdot p_{y_j}(x_j\vert{}\theta_{y_j})) \cdot \prod_{j=1..n} P(y_j\vert{}x_j, \Theta^g) \right)$$ Applying the Bayes theorem, we can compute the posterior probabilities of each pattern belonging to each Gaussian component, given the current estimates of the parameters. These probabilities are often referred to as the *responsibilities* of each component for each pattern and are denoted as:   $$P(C_k \mid x_j, \Theta^g) = P(y_j=k \mid x_j, \Theta^g) = \frac{\alpha_k^g \cdot p_k(x_j \mid \theta_k^g)}{\sum_{i=1}^s \alpha_i^g \cdot p_i(x_j \mid \theta_i^g)}$$
	For a generic vector $y$ of observations we can compute: $$P(y \mid \mathcal{X}, \Theta^g) = \prod_{j=1..n} P(y_j \mid x_j, \Theta^g) $$
2. **Maximization Step (M-step)**: Using the probabilities calculated in the E-step, the parameters are updated by maximizing the expected value of the complete log-likelihood: $$\Theta^{g+1} = \operatorname{argmax}_\Theta Q(\Theta\vert{}\Theta^g) \implies \frac{\partial Q(\Theta\vert{}\Theta^g)}{\partial \Theta} = 0$$
   - Jensen's Inequality: Each time we increase or maximize $Q(\Theta\vert{}\Theta^g)$, we also improve (or keep unchanged) the true log-likelihood of the observed data, $\log p(\mathcal{X}\vert{}\Theta)$.
   - Update the mixing coefficients of the clusters: $$\alpha_k^{g+1} = \frac{1}{n} \sum_{j=1}^n P(C_k \mid x_j, \Theta^g)$$
   - Update the means: $$\mu_k^{g+1} = \frac{\sum_{j=1}^n P(C_k \mid x_j, \Theta^g) \cdot x_j}{\sum_{j=1}^n P(C_k \mid x_j, \Theta^g)}$$
   - Update the covariance matrices: $$\Sigma_k^{g+1} = \frac{\sum_{j=1}^n P(C_k \mid x_j, \Theta^g)(x_j - \mu_k^{g+1})(x_j - \mu_k^{g+1})^T}{\sum_{j=1}^n P(C_k \mid x_j, \Theta^g)}$$
   - Repeat the E-step and M-step until convergence, which is typically determined by checking if the change in the log-likelihood between iterations is below a predefined threshold or if the maximum number of iterations has been reached.
```
Initialize n,s
Intialize g = 0, alpha[k] and theta[k]={mu[k], sigma[k]} for each k=1,...s
do {
	Expectation Step: compute P(C_k|x_j, Theta^g) for each k=1,...s and j=1,...n
    Maximization Step: update alpha[k] and theta[k]={mu[k], sigma[k]} for each k=1,...s
} while(alpha[k] and theta[k] is stable and iteration < max)
```
- Covariance matrix can be initialized as diagonal matrix, due to the fact they must symmetric ad positive definite.
- $P(C_k \mid x_j, \Theta^g)$ can be quite small and cause numerical problems, so initialize with multipliers and then normalize them.
![[Pasted image 20261102223823.png]]

# Application example: Speaker recognition
- The EM algorithm can be used for speaker recognition, where the goal is to identify a speaker based on their voice characteristics.
- GMM (Gaussian Mixture Model) was used to model the distribution of feature vectors extracted from audio recordings of different speakers.
- Used features: MFCC (Mel Frequency Cepstral Coefficients), which capture the spectral characteristics of the speech signal.
	- First of all we analize the spectrogram of the audio signal: ![[Pasted image 20261102231404.png]]
	- MFCCs are derived from the spectrogram by switching to a logarithmic scale (on the y-axis) and applying an additional transformation to decorrelate the frequencies.
	- Typically, for each time window (which we call a "frame"), 20 primary coefficients are used; to these, another 20 values ​​for the first derivative and 20 for the second derivative are added, which indicate how a given frequency varies over time.
- **Why clustering?**: in this kind of problem we don't know a priori who are the speakers, we mainly know which traces they have recorded but we don't know about the true identity. For this reason it should be used a clustering algorithm for recognizing instead of a classification algorithm. In this case in particular the EM algorithm works well on identifying because it works with probabilities.
- For each $i$-th speaker we can concatenate each frame to obtain a unique $mfcc_i$ representation
- Then for each speaker we can train a GMM that fits $mfcc_i$ by making a reasonable assumption: we assume that each frame of the $i$-th speaker are generated from a gaussian mixture
- Thus, each speaker $i$ has a distribution $p(x|GMM_i)$. A speaker's GMM describes how their MFCC frames—that is, the characteristics of their vocal tract—are distributed.
- Given a new trace $X$:
	- We extract some frame MFCC $f_r$
	- Then for each model GMM of the $i$-th speaker we evaluate the avarage log-likelihood: $$\frac{1}{t}\sum_{r=1}^t \log(p(f_r|GMM_i))$$
	- Then we assign the audio record with the maximum likelihood: $$k=argmax_i\frac{1}{t}\sum_{r=1}^t \log(p(f_r|GMM_i))$$
- To make this comparison, a separate model is required for each speaker. If one were to train a single GMM with $s$ components across all speakers, the components would not necessarily correspond to the speakers: clustering is unsupervised and does not guarantee that each component represents a specific person.
- For more details about how to extract MFCCs check https://librosa.org/doc/latest/index.html