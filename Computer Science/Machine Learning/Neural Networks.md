# Introduction

## Biological Neurons 
- They belong to the nervous system
- Main components:
	- Dendrites: branching extensions that allow to receive signals (input)
	- Cell body or Soma: contains the nucleus and most cell's complex components. It generates electrical signals (action potentials or simply *signals*)
	- Axon: a long fiber extension that allows to reach other neurons
	- Synaptic terminals (synapse): fibers that propagate electrical pulses 
- When a neuron receives a sufficient amount of neurotransmitters within a few milliseconds, it fires its own signal and propagates it to the nearby neurons. More in particular:
	- There are some receptors, called *ion channels*, that are located on the postsynaptic neuron's membrane
	- The entrance of ions inside these channels produce a potential difference
	- If the potential difference go over a certain threshold, receptors will produce a *spike*, a pulse that is forwarded to the soma.
	- The neuron produce another electrical pulse called *action potential* along its axon that triggers the release of neurotransmitters from the synapses on the axon.
- There are also some neurotransmitters that don't allow the signal to be propagated.
- **Hebbian rule or Reweighting**: the alteration of synapse's transmission efficiency, linked to processes of memory and learning 

![[Pasted image 20261112093037.png]]
- Human cortex:
	- continuous laminar layer 2–4 mm thick
	- forming numerous convolutions to increase its surface area.
- Multiple layers in a biological neural network:
![[Pasted image 20261112093053.png]]
![[Pasted image 20261114115224.png]]
- **Connectome**: a map of connections between neurons in the brain (Connectome, How the Brain’s Wiring Make Us Who We Are; Sebastian Seung, 2012)
## Artificial Neurons
- First model by McCulloch and Pits, 1943, with binary input and output; it could do logic computation
![[Pasted image 20261114154951.png]]
- **Perceptron**
![[Pasted image 20261114155120.png]]
![[Pasted image 20261114154718.png]]
- It's composed of:
	- **Inputs** ($in_1, in_2, ... in_d$)
	- **Weights** ($w_{1i}, w_{2i}, ... w_{di}$, where $i$ is the $i^{th}$ layer)
	- **Bias** ($w_{0i}$): base value -> 1, 
	- **Potential** ($net_i$): weighted sum of inputs and bias
	- **Activation** **function** ($f(net_i)$): it transforms the potential into an output value ($out_i$)
- For ANN , the activation function is usually a **non-linear function** that is also *differentiable*, so that it can be used in *gradient descent optimization*.
- There are some non-linear activation functions:
    - **Sigmoid**: $$f(net_i) = \frac{1}{1 + e^{-net_i}}$$ ![[Pasted image 20261114160200.png]]
    - **Tanh**: $$f(net_i) = \frac{e^{net_i} - e^{-net_i}}{e^{net_i} + e^{-net_i}}$$![[Pasted image 20261114160217.png]]
    - **ReLU**: $$f(net_i) = max(0, net_i)$$
    - **Leaky ReLU**: $$f(net_i) = max(0.01 * net_i, net_i)$$
    - **Softmax**: $$f(net_i) = \frac{e^{net_i}}{\sum_{j=1}^{K} e^{net_j}}$$ where $K$ is the number of classes

## Neural Network Types
- Neural Networks are actually organized in **layers**:
	- Input layer that receives input
	- Hidden layers that process the input and extract features
	- Output layer that produces the output
- Each layer is composed of multiple neurons that are connected to the neurons in the previous and next layers.
### FeedForward
![[Pasted image 20261114172338.png]]
- Connections are unidirectional, from input to output
- No backward or recurrent connections
### Recurrent
![[Pasted image 20261114180532.png]]
- Backward connections are allowed, so that the output of a neuron can be used as input for itself or for other neurons in the same layer.
- *Unfolding in time*: unrolling the recurrent network into a feedforward network with one layer for each time step. This helps algorithms like Backpropagation to compute the gradients of the weights.
![[Pasted image 20261114182234.png]]

# Multi-layer Perceptron
- A Multi-layer Perceptron (MLP) is a feedforward neural network that is composed of at least *three layers*, with one or more hidden layers.
- **Universal Approximation Theorem**: A feedforward neural network with a single hidden layer containing a finite number of neurons can approximate any continuous function on compact subsets of $\mathbb{R}^n$, under mild assumptions on the activation function.
## Forward Propagation
- The propagation of information towards the output layer
![[Pasted image 20261114184139.png]]
- 3 layer NN ($d:n_H:s$) with $d$ input neurons, $n_H$ hidden neurons and $s$ output neurons
	- Total weights: $W = (d + 1) * n_H + (n_H + 1) * s$
	- The $k$-th output value can be computed as: $$z_k = f(net_k) = f(\sum_{j=1}^{n_H} w_{jk} \cdot y_j + w_{0k})$$
![[Pasted image 20261114190201.png]]![[Pasted image 20261114190220.png]]
## Backpropagation
- Rumelhart, Hinton and Williams, 1986
- **Error Backpropagation**:
	- **Loss function** is evaluated between the predicted output and the actual output with a forward process
	- Then there is a *backpropagation* phase in which the weights are updated based on the error
		- $L(\hat{y}(w),y ) = \frac{1}{2}\sum_{i=1}^{s}(y_i - \hat{y_i})^2=\frac{1}{2}||y-\hat{y}(w)||_2^2$   -> then the problem becomes finding optimal weights that minimize the loss function. We can use also other type of loss function, like a Binary Cross Entropy(for binary classification) or a Categorical Cross Entropy(for multi-category classification). For regression we can use MSE, MAPE, MAE
		- Then some heuristics could be applied for regularization or stability, using for example penalties
		- **Chain rule** formula is applied to the Loss function$$\frac{\partial L}{\partial w_{jk}} = \frac{\partial L}{\partial \hat{y}_k} \cdot \frac{\partial \hat{y}_k}{\partial net_k} \cdot \frac{\partial net_k}{\partial w_{jk}} = \frac{\partial L}{\partial \hat{y}_k} \cdot f'(net_k) \cdot y_j$$
		- Finally all weights are updated such that: $$w_{jk} \leftarrow w_{jk} - \eta \cdot (\hat{y}_k - y_k) \cdot f'(net_k) \cdot y_j$$ and for the bias: $$w_{0k} \leftarrow w_{0k} - \eta \cdot (\hat{y}_k - y_k) \cdot f'(net_k) \cdot 1$$
		- Delta shorthand: $\delta_k = \frac{\partial L}{\partial net_k} = (\hat{y}_k - y_k)f'(net_k)$, so the update function becomes: $$w_{jk} \leftarrow w_{jk} - \eta \, \delta_k \, y_j$$
		- ![[Pasted image 20261115100316.png]]
![[Pasted image 20261115101045.png]]![[Pasted image 20261115101100.png]]![[Pasted image 20261115101141.png]]
- To test some MLPs: http://playground.tensorflow.org/
- Math example with a classification and a 3-layer architecture
	- The desired output $t$ is: $$t =[-1,-1,-1,..., 1,...,-1,-1]$$where 1 is the correct class label and -1 is the other class label
	- With tanh as the activation function, the NN output is $z=[z_1, z_2, z_3,...]$ with patterns $x=[x_1, x_2, x_3,...]$ and weights $w_{ij}$
	- The loss function is: $$J(w,x) = \frac{1}{2}\sum_{i=1}^{s}(t_i - z_i)^2$$
	- The mean error on the training set can then be computed as: $$E(w) = \frac{1}{N}\sum_{n=1}^{N}J(w,x_n)$$
	- We want to calculate how the error $J$ changes as the individual weight $w_{jk}$ (which connects hidden neuron $j$ to output neuron $k$) varies.
![[Pasted image 20261115102637.png]]
1) In the sum $\sum_{c=1}^{s} (t_c - z_c)^2$, the only term that depends on the weight $w_{jk}$ is the one corresponding to output neuron $k$ (i.e., for $c = k$). All other terms ($c \neq k$) have a derivative of zero:
$$\frac{\partial J}{\partial w_{jk}} = \frac{\partial}{\partial w_{jk}} \left( \frac{1}{2} \sum_{c=1}^{s} (t_c - z_c)^2 \right) = \frac{1}{2} \cdot 2 (t_k - z_k) \cdot \frac{\partial (-z_k)}{\partial w_{jk}} = (t_k - z_k) \frac{\partial (-z_k)}{\partial w_{jk}}$$
2) Knowing that the output of neuron $k$ is $z_k = f(net_k)$, we apply the chain rule using the internal potential $net_k$:$$(t_k - z_k) \frac{\partial (-f(net_k))}{\partial w_{jk}} = -(t_k - z_k) \cdot \frac{\partial f(net_k)}{\partial net_k} \cdot \frac{\partial net_k}{\partial w_{jk}}$$
3) The internal potential $net_k$ is a linear combination of the outputs of the hidden neurons and the weights connecting them to the output neuron $k$: $net_k = \sum_{j=1}^{n_H} w_{jk} y_j + w_{0k}$. When differentiating with respect to $w_{jk}$, only the term with $r = j$ remains: $$\frac{\partial net_k}{\partial w_{jk}} = \frac{\partial}{\partial w_{jk}} \left( \sum_{r=1}^{n_H} w_{rk} \cdot y_r + w_{0k} \right) = y_j$$Then the derivative of the loss function with respect to the weight $w_{jk}$ is: $$\frac{\partial J}{\partial w_{jk}} = -(t_k - z_k) \cdot f'(net_k) \cdot y_j$$
4) We define the error signal of output neuron $k$ as:$$\delta_k = (t_k - z_k) \cdot f'(net_k) \quad \text{(Eq. 2)} $$The gradient then becomes:$$\frac{\partial J}{\partial w_{jk}} = -\delta_k \cdot y_j \quad \text{(Eq. 3)}$$Since in gradient descent we move in the direction opposite to the gradient ($w_{jk} \leftarrow w_ {jk} - \eta \frac{\partial J}{\partial w_{jk}}$), the two negative signs cancel each other out, leading to the update rule:$$w_{jk} = w_{jk} + \eta \cdot \delta_k \cdot y_j \quad \text{(Eq. 4)}$$(Note: For the bias $w_{0k}$, the same formula applies by setting $y_0 = 1$).
- Now let's calculate the derivative of the error $J$ with respect to a weight $w_{ij}$ in the first layer (which connects the input $x_i$ to the hidden neuron $j$). Unlike before, changing $w_{ij}$ affects the output $y_j$, which in turn affects all output neurons $c = 1 \dots s$.
1) Derivative of the sum over all outputs: $$\frac{\partial J}{\partial w_{ij}} = \frac{\partial}{\partial w_{ij}} \left( \frac{1}{2} \sum_{c=1}^{s} (t_c - z_c)^2 \right) = \sum_{c=1}^{s} (t_c - z_c) \cdot \frac{\partial (-z_c)}{\partial w_{ij}}$$
2) Applying the chain rule using the internal potential $net_c$ of output neuron $c$: $$-\sum_{c=1}^{s} (t_c - z_c) \frac{\partial z_c}{\partial net_c} \cdot \frac{\partial net_c}{\partial w_{ij}} = -\sum_{c=1}^{s} (t_c - z_c) \cdot f'(net_c) \cdot \frac{\partial net_c}{\partial w_{ij}}$$
3) The internal potential $net_c$ is a linear combination of the outputs of the hidden neurons: $net_c = \sum_{j=1}^{n_H} w_{jc} \cdot y_j + w_{0c}$. Of all the hidden neurons $r$, the only one affected by the weight $w_{ij}$ is the neuron $r = j$ (whose output is $y_j = f(net_j)$): $$\frac{\partial net_c}{\partial w_{ij}} = \frac{\partial}{\partial w_{ij}} \left( \sum_{r=1}^{n_H} w_{rc} \cdot f(net_r) \right) = \frac{\partial}{\partial w_{ij}} \left( w_{jc} \cdot f(net_j) \right)$$Then applying the chain rule: $$w_{jc} \cdot \frac{\partial f(net_j)}{\partial net_j} \cdot \frac{\partial net_j}{\partial w_{ij}} = w_{jc} \cdot f'(net_j) \cdot \frac{\partial}{\partial w_{ij}} \left( \sum_{q=1}^{d} w_{qj} \cdot x_q + w_{0j} \right)$$In the sum of the inputs to neuron $j$, only the term with $q = i$ depends on $w_{ij}$, so $\frac{\partial net_j}{\partial w_{ij}} = x_i$. We thus obtain:$$\frac{\partial net_c}{\partial w_{ij}} = w_{jc} \cdot f'(net_j) \cdot x_i$$
4) Let’s return to the expression from Step 2, note that $(t_c - z_c) \cdot f'(net_c) = \delta_c$ (from Eq. 2), and substitute $\frac{\partial net_c}{\partial w_{ij}}$:$$\frac{\partial J}{\partial w_{ij}} = -\sum_{c=1}^{s} \delta_c \cdot \left( w_{jc} \cdot f'(net_j) \cdot x_i \right)\frac{\partial J}{\partial w_{ij}} = -x_i \cdot f'(net_j) \sum_{c=1}^{s} w_{jc} \cdot \delta_c$$
5) We define the backpropagated error on hidden neuron $j$ as:$$\delta_j = f'(net_j) \cdot \sum_{c=1}^{s} w_{jc} \cdot \delta_c \quad \text{(Eq. 5)} $$The gradient thus becomes analogous to that of the output layer:$$\frac{\partial J}{\partial w_{ij}} = -\delta_j \cdot x_i \quad \text{(Eq. 6)} $$And the update rule for the input-hidden weights is:$$w_{ij} = w_{ij} + \eta \cdot \delta_j \cdot x_i \quad \text{(Eq. 7)}$$
### Online Backpropagation
- Where $size_b=1$ (batch size = 1)
```
Initialize n_H, w, η, num_epoch, epoch ← 0
do
    epoch ← epoch + 1
    totErr ← 0 // cumulative error over all patterns in the TS
    for each x in the Training Set
        forward step: x → z_k, k = 1...s (eq. 1)
        totErr ← totErr + J(w, x)
        backward step: δ_k, k = 1...s (Eq. 2), δ_j, j = 1...n_H (Eq. 5)
        update hidden-output weights: w_jk = w_jk + η · δ_k · y_j (Eq. 4)
        update input-hidden weights:  w_ij = w_ij + η · δ_j · x_i (eq. 7)
    loss ← totErr / n // average error on the TS
    Calculate accuracy on the Train Set and Validation Set
while (not convergence and epoch < num_epoch)
```
### Stochastic Gradient Descent
- The most used approach for implementing backpropagation
- The training set is randomly shuffled and divided into mini-batches of size `size_mb` (e.g., 32, 64, 128, etc.)
- The weights are updated after each mini-batch, rather than after each individual pattern or after the entire training set. This is done by accumulating the gradients of the loss function over all patterns in the mini-batch temporarily, and then updating the weights using the average gradient. This approach reduces the variance of the weight updates, leading to more stable convergence and faster training.
```
Initialize n_H, w, η, num_epoch, size_mb, epoch ← 0
do
    epoch ← epoch + 1
    randomly sort the training set (n patterns)
    for each mini-batch B of size_mb
        reset gradients
        for each x in B
            forward pass
            backward pass // cumulative gradients across all patterns in the mini-batch
        update weights w with learning rate η
    Calculate loss
    Calculate accuracy on the training set and validation set
while (not converged and epoch < num_epoch)
```
- Then:
	- Online Backpropagation is a specific case in which `size_mb = 1`
	- Full-Batch Backpropagation is a specific case in which `size_mb = n` (the entire training set)
- Some terminologies:
	- **epoch**: one complete pass through the entire training set
    - **iteration**: one update of the weights, which occurs after processing a mini-batch. The number of iterations per epoch is equal to the total number of training samples divided by the mini-batch size ($n/size_{mb}$).
    - **convergence**: the point at which the model's performance on the training set and validation set stabilizes, indicating that further training is unlikely to yield significant improvements. Convergence can be determined by monitoring the loss function or accuracy over epochs and stopping when changes fall below a predefined threshold.
    - **early stopping**: a regularization technique used to prevent overfitting by halting the training process when the model's performance on a validation set starts to degrade, even if the training loss continues to decrease. This helps ensure that the model generalizes well to unseen data.
- How to choose the proper size of mini-batches?
	- **Small mini-batches** (e.g., 32 or 64) can lead to more frequent updates and faster convergence, but they may introduce more noise in the gradient estimates, potentially causing the optimization process to be less stable. They can also take advantage of parallel processing on GPUs.
    - **Large mini-batches** (e.g., 128 or 256) provide more accurate gradient estimates, leading to more stable convergence, but they may require more memory and can be slower to train. They can also lead to poorer generalization performance if the model overfits to the training data.
    - Another thing involves for example in finding sharp minimum and flat minimum, or also called **Generalization Gap**:
	    - **Sharp minimum**: A sharp minimum is a point in the loss landscape where the loss function has a steep curvature, meaning that small changes in the model parameters can lead to significant increases in the loss. If the network encounters a narrow valley (sharp minimum), it slides into it and does not have enough “random momentum” to escape, so it remains trapped.
	    - **Flat minimum**: Since the gradient is estimated based on only a few patterns at a time, the descent direction is “noisier” and more erratic (random). This noise acts as a continuous search: if the network enters a sharp minimum, the fluctuations in the gradient easily cause it to “bounce” back out until it finds a valley that is wide and stable enough (a flat minimum) to settle into.
	- Another small factor is the **learning rate**: a small learning rate can lead to slow convergence and may get stuck in local minima, while a large learning rate can cause the model to overshoot the optimal solution and diverge. 
- The learning rate can be adjusted dynamically during training using techniques such as learning rate schedules or adaptive learning rate methods.
### Softmax
- The softmax function is a mathematical function that converts a vector of real numbers into a probability distribution. It is commonly used in the output layer of neural networks for multi-class classification problems, where the goal is to assign probabilities to each class.
- The softmax function is defined as follows: $$z_k=softmax(net_k)=\frac{e^{net_k}}{\sum_{c=1}^se^{net_c}}$$
- This is used as activation function of the last layer
- This allows the last layer of a multiclassifier to compute the net values as probabilities that defines the probability that a pattern belongs to a certain class.
### Cross-Entropy Loss
- **One-hot encoding**:  If there are $s$ classes, the true label $y$ is represented as a vector of length $s$, where the index corresponding to the true class is set to 1, and all other indices are set to 0. For example, if there are 4 classes and the true class is class 2, the one-hot encoded vector (called *one-hot target*) would be $y = [0, 1, 0, 0]$.
- The cross-entropy loss is a commonly used loss function for multi-class classification problems. It measures the difference between the predicted probability distribution (output of the softmax function) and the true probability distribution (one-hot encoded labels).
- The cross-entropy loss for a single sample is defined as: $$H(y, \hat{y}) = -\sum_{k=1}^{s} y_k \log(\hat{y}_k)$$ where $y$ is the true label (one-hot encoded)
- *Soft target*: In some cases, the true labels may not be strictly one-hot encoded, but rather a probability distribution over the classes. For example, if there are 3 classes and the true label is class 1 with a confidence of 0.8 and class 2 with a confidence of 0.2, the soft target would be $y = [0.8, 0.2, 0]$. The cross-entropy loss can still be computed using the same formula, but now $y$ represents a probability distribution rather than a one-hot encoded vector.
- For multi-label problems, in which a pattern can belong to multiple classes, the cross-entropy loss can be computed independently. For example, if there are 4 classes and a pattern belongs to classes 1 and 3, the true label would be $y = [1, 0, 1, 0]$. This result can be achieved by relaxing the boundary in which the sum of elements in the result is equal to 1.
- Another thing to be highlighted is that the last layer for classification models can be anything and *depends on the type of the problem* and how many classes there are. For example for binary classification we can use a cross-entropy with one neuron in this form: $$H(t,z) = -(t\log(z) + (1-t) \log(1-z))$$
### Weight Initialization
- Finding an optimal range in which we want to initialize weights is crucial for the convergence of the network. 
- The main technique used is **Xavier Initialization (Glorot)**: for keeping the output variance similar to the input variance we can initialize weights:
	- With a normal distribution with mean 0 and variance 1: $$w_{ij} \sim N(0, \frac{1}{n_{in}}) \rightarrow \sigma=\sqrt{\frac{1}{avg}}$$  where $n_{in}$ is the number of input neurons to the layer and $avg$ is the average of the number of input and output neurons in the layer.
	- With a uniform distribution in the range: $$w_{ij} \sim U(-\sqrt{\frac{3}{avg}}, \sqrt{\frac{3}{avg}})$$
### Regularization
- In general we want a model that uses small weights, thanks to the fact they produce a regular output. This is because if the weights are too large, the output will be very sensitive to small changes in the input, which can lead to overfitting and poor generalization performance.
- 
- For reducing overfitting, we can add a new term in evaluating a loss function that penalizes large weights. This is called **regularization** and it helps to prevent overfitting by discouraging the model from relying too heavily on any single feature or input. For example: $$J_{Tot}=J_{Cross\_Entropy}+J_{Reg}$$
- some regularization techniques:
    - **L1 regularization**: adds a penalty term to the loss function that is proportional to the absolute values of the weights. This encourages sparsity in the weight matrix, effectively driving some weights to zero and promoting feature selection.$$J_{Reg}=\frac{1}{2}\lambda\sum_i w_i^2$$
    - **L2 regularization**: adds a penalty term to the loss function that is proportional to the square of the weights. This discourages large weights and helps prevent overfitting by smoothing the model's predictions.$$J_{Reg}= \lambda\sum_i |w_i|$$
    - **Dropout**: randomly sets a fraction of the neurons in a layer to zero during training, effectively "dropping out" those neurons from the network. This prevents co-adaptation of neurons and encourages the network to learn more robust features.
- In both L1 and L2 regularization, the hyperparameter $\lambda$ controls the strength of the regularization. A larger value of $\lambda$ increases the penalty for large weights, leading to stronger regularization, while a smaller value of $\lambda$ reduces the penalty and allows for larger weights.
- Also L1 is used for feature selection, while L2 is used for **weight decay**. This means that L1 regularization tends to produce sparse weight matrices, where some weights are exactly zero, effectively selecting a subset of features. In contrast, L2 regularization encourages smaller weights overall but does not necessarily drive them to zero, leading to weight decay rather than feature selection.
- This produce a bounce effect in which weights are limited: $$w_k=w_k-\eta(\frac{dJ_{Cross-Entropy}}{dw_k}+\lambda \cdot w_k)$$
### Learning rate
- The learning rate is a hyperparameter that controls the step size at each iteration while moving toward a minimum of the loss function. It determines how quickly or slowly a neural network learns during training. Choosing an appropriate learning rate is crucial for the convergence and performance of the model.
- If the learning rate is too high, the model may overshoot the optimal solution and fail to converge, leading to divergence or oscillation in the loss function. Conversely, if the learning rate is too low, the model may converge very slowly, taking a long time to reach the optimal solution, or it may get stuck in local minima. 
![[Pasted image 20261115172451.png]]
- Some techniques for adjusting the learning rate during training include:
    - **Learning rate schedules**: Predefined strategies that change the learning rate over time, such as step decay, exponential decay, or cosine annealing.
    - **Adaptive learning rate methods**: Algorithms that adjust the learning rate based on the gradients observed during training, such as AdaGrad, RMSprop, and Adam. These methods can help improve convergence and stability by adapting the learning rate to the specific characteristics of the data and model.
- There are other techniques that work in parallel with learning rate, such as the **Momentum**:
	- It is used in SGD with mini-batch to accelerate convergence and reduce oscillations in the optimization process. 
	- The momentum term helps the optimizer maintain its direction and speed when moving through the loss landscape, allowing it to escape shallow local minima and converge faster to a better solution.
	- Instead of updating the weights based solely on the current gradient, momentum incorporates a fraction of the previous weight update into the current update.$$\Delta w_k^{(t)} = \mu \cdot \Delta w_k^{(t-1)} - \eta \frac{\partial J_{Tot}}{\partial w_k}$$ $$w_k^{(t)} = w_k^{(t-1)} + \Delta w_k^{(t)}$$ Where $\mu$ is the momentum coefficient(typically at 0.9)

![[Pasted image 20261115173250.png]]
- Also there are other techniques that are based on momentum.