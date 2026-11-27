- **Deep Neural Network**s are NN that have 2 or more hidden layers.
- They are born from the *universal approximation theorem* that claims that NN with one layer can approximate every function, but it's quite unmaintainable
- Hierarchy structure could improve performances by re-using and sharing information between layers, in which they can select features and discard useless details, like in our brain:
![[Pasted image 20261116142930.png]]
- Today DNN have many layers, between 7 and 100. Models that have more layers could improve accuracy, but efficiency decreases with more layers
- Between muscles actuators and retina there are "only" a dozen of levels
![[Pasted image 20261116143315.png]]
- But how many levels and how deep must be a DNN?
- This is a complexity factor in which we can involve the number of neurons, connections and weights
- More weights must be tuned, more complex will be the training phase
- More neurons implies a more complex forward and backpropagation.
![[Pasted image 20261117095241.png]]

# Deep Learning Architectures
- *Feedforward discriminative models* for supervised classification and regression. For example: FC DNN (Fully Connected DNN, MLP with 2 or more hidden levels) and CNN (Convolutional Neural Network or ConvNet)
- *Memory and Attention based recurrent models* for speech recognition, NLP and sequences prediction. For example: RNN (Recurrent Neural Network), LSTM (Long Short-Term Memory) and Transformers(we include also LLMs)
- *Unsupervised Learning models* for input reconstruction, denoising and anomaly detection. Mainly are used Autoencoders (stacked, denoising)
- *Generative Models* for data augmentation, style transfer and art applications. In this category are included GAN (Generative Adversarial Networks) and VAE (Variational Autoencoders)
- *Reinforcement Learning Models* for behaviour learning. For example: Deep Q-Learning and PPO, GRPO

## Main Ingredients and techniques
- What is necessary for training a DNN? (1998, LeCun and more importantly 2012, AlexNet)
	- **BigData**: labeled datasets with many datas
	- **GPU computing**: huge computational hardware capacities
	- **Techniques**: one huge problem at that time was the *vanishing(or exploding) gradient* if the network was bad initialized in terms of weights and the activation function was sigmoid. For this reason one well-placed alternative is the ReLu for the activation function and it was developed an initialization method for weights (like Xavier and Hu initialization)
		- The sigmoid derivative function is typically less than one and for this reason levels that are far from the output level have weights that tend to be 0. For more details: http://neuralnetworksanddeeplearning.com/chap5.html
		- For this reason in particular we say that a sigmoid is saturating
		- The ReLu (Rectified Linear) resolves this problem![[Pasted image 20261117101758.png]]
		- There are also other activation functions:![[Pasted image 20261117101845.png]]

### Regularization Techniques
- There are also regularization techniques that help control overfitting, like the **Dropout** technique.
	- It works with small datasets and large models
	- slows convergence but improves generalization
	- It simply works as a neuron inhibitor, in other words it turns off some neurons during the training phase
	- For CNN this technique is applied only in the last fully connected layers (but never in the output layer)
![[Pasted image 20261117102814.png]]


### Autodiff (Automatic Differentiation in reverse-mode)
- It's a composition technique that simplifies the gradient evaluation during *backpropagation phase*
- Specifically, it applies more efficiently the chain rule by isolating the contribution of each layer
- Each time a new layer is defined, it requires only to evaluate the local gradient of that layer:
![[Pasted image 20261117102932.png]]
- Under the hood, input,output and weights of each layer is organized in *tensors* and gradients are evaluated by computing their *Jacobian* matrix. Then gradients are obtained by multiplying their Jacobian matrix.
- GPUs work really well in this problem for the fact they can compute gemm (General Matrix-Matrix) multiplications
![[Pasted image 20261117102955.png]]
- But the hardware for this kind of operations is really expensive and complex to be realized: ![[Pasted image 20261117104717.png]]
- This is why there are services that provides GPU usage:
	- Workstation
	- GPU cloud
	- Supercomputer
- But how to use DNN models when they are used locally on smartphones and IoT devices? This is called *edge inference* and since CPUs aren't actually powerful enough to compute this kind of models, there are *custom chips* that can compute DNN models with real time performances
# Convolutional Neural Network (CNN)
- Hubel & Wiesel, 1962, discover that inside cat visual cortex there are two kind of neurons:
	- Simple cells: each of them works as local feature detector, and each of them observes a little portion of the field of view (selectivity)
	- Complex cells: merge (specifically it is called "pooling") each simple cell output to guarantee *invariance under translations*
- First model developed for this behaviour: Neocognitron (Fukushima, 1980)
- Convolutional Neural Networks where developed by LeCun et al. starting from 1998.
- Main features (also in respect of MLP):
	- **Local processing**: each neuron is connected locally to the neurons of the previous layer
	- **Shared weights**: weights are shared among each local group
	- *Feature extraction layers and Pooling layers*
- Pooling: dimensional reduction on the feature map
- Their first application was for images and it worked far better than fully connected models
- For images the CNN architecture is composed of a hierarchy of layers:
	- The input layer is connected to the corresponding pixel image
	- Intermediate layers use local connections and shared weights for processing with the same way different image regions, this is intended because different regions of the visual field contain the same type of information
	- Last layers are fully-connected layers, working as a MLP classifier
	- The **receptive field** (field of view) of each neuron grows by moving towards the high part of the hierarchy
![[Pasted image 20261117112128.png]]

## Convolution (from Computer Vision) and Neuron for Convolution
- It's an *image* *preprocessing* operation that is used for applying *filters*
![[Pasted image 20261117183752.png]]
![[Pasted image 20261117183808.png]]
- Neurons can be used for convolution by simply connecting with the $d\times d$ matrix in the input layer and using values inside the matrix as the weights:
![[Pasted image 20261117184442.png]]
- **Volume notation**: each layer can be organized in 3D volumes in which:
	- *width-height* descrive the neuron position
	- *depth* describe *feature map*s, in other words the filter applied to the image (or in better words the result of applying the filter)
![[Pasted image 20261117185340.png]]
- Suppose that we have a image in input $32 \times 32 \times 3$, where the depth corresponds to the RGB channels
- Each volume can have multiple purposes:
	- Convolution: modifying the depth extracts features (borders, textures, colors), keeping the image size
	- Pooling: reduce width and height, and the depth remains unchanged
## Convolution
![[Pasted image 20261117190607.png]]
- Number of connections: $Volume_{Filter} \times Volume_{feature-maps}$ . (In this case: $5\times 5\times 3 \times 28 \times 28 \times 6 = 352800$)
- Suppose that we want to apply a $5\times 5$ filter (called **kernel**), and this filter is not 2D but instead 3D dimensional in fact it has a depth of $3$ so it's $5 \times 5 \times 3$ dimensional, with $5 \times 5 \times 3 = 75$ weights
- Applying it to the input volume, summing up the result with a bias and computing it into the activation function (ReLU), we have as a result a single number
- Then we **stride** the filter on the whole area of the image and each position produce a single number and in general it corresponds into a matrix $28 \times 28 \times 1$, or also called *feature map*
- But this is not finished yet. For each volume we want to apply $K=6$ filters, corresponding into a $28 \times 28 \times 6$ volume
- The *stride* can be set at the start of the training and with little stride we can gain efficiency sacrificing accuracy
- Also we can set **padding** parameter to determine the thickness of the borders of input volume. This addition corresponds into adding zeros around the image.
- We define $W_{out}$ as the horizontal dimension of the output feature map, $W_{in}$ as the input dimension and $F$ as the horizontal dimension of the filter. Then we can state that:$$W_{out}=\frac{W_{in}-F+ 2\cdot Padding}{Stride}+1$$
- For more details check: https://cs231n.github.io/convolutional-networks/
## Pooling
- With this operation we only reduce the size of the image (width and height) and the depth remains unchanged during this operation
- Then the result consist of the same number of feature map, but each one is reduced in terms of size
- Pooling works independently from each feature map in input
- This operation is implemented with a sliding window with typically $Stride = 2$. For each area $Stride \times Stride$ applies a fixed mathematical function:
	- **Max pooling** (most used): extract the max value inside the grid and discards other values
	- **Average pooling**: compute the mean between values
![[Pasted image 20261117194849.png]]
## CNN example - Cifar-10
- https://cs.stanford.edu/people/karpathy/convnetjs/demo/cifar10.html
![[Pasted image 20261117194937.png]]
- Another example: 
![[Pasted image 20261117195016.png]]
## AlexNet Analysis and Modern CNNs
- 96 filters on the level 1 of AlexNet from the ImageNet training 
![[Pasted image 20261117195123.png]]
- A brief analysis:
	- The first 48 filters works as Simple cells: process in gray scale borders, scale and orientations
	- The other 48 filters process color information
	- This specialization wasn't wanted and more over expected
- More techniques developed from that discover:
	- **Batch normalization**: After each layer, the outputs are normalized to ensure that the inputs to the next layer do not shift too much within the space (*covariate shift*). This operation is applied at minibatch level (and on single feature map in CNN models). Stable and faster training, a greater learning rate
	- **Skip connections**(ResNet): "Lateral" connections are added to the layers, which sum the original input with the layer's output. This change the mapping behaviour in a way that focus only computing the residual from the original input. Resolves vanishing gradient
	- **Depthwise ($1 \times 1$) convolutions**: Significant reduction in complexity without a major loss in performance if the convolution architecture has the same kernel for each convolution. Suppose that we have $M$ convolutions with kernel $3\times 3 \times C$ (cost: $M\times 3 \times 3 \times C$)
		- First for each $C$ input feature map we compute a 2D convolution ($3 \times 3$)
		- Then for each feature map we apply $M$ kernel filters $1 \times 1 \times C$, obtaining $M$ feature maps.
	- **Transfer Learning**: CNN requires large datasets and then they were created two methods for labeling very large datasets:
		- **Fine tuning**: using pre-trained nets on similar problem and:
			- the output layer becomes a softmax layer (fixed to the appropriate number of classes)
			- as initial weights we use the pre-trained net weights and randomly choose weights for the last and second-last layers
			- new iterations are executed for capturing peculiarities of the new dataset.
		- **Feature reuse**: in this case instead we a pre-trained net in which we extract feature at certain levels generated during the forward phase. Then we apply these features in a classifier (SVM) and then we classify new patterns.
		- For more details check: http://cs231n.github.io/transfer-learning/
# Recurrent Neural Network (RNN)
- Recurrent Neural Networks are networks with layers that can include backward connections or connection in the same layer
- Given a temporal instance $t$ in a recurrent layer, it receives the input from the previous layer $x_t$ and the output from the same layer but in the previous instance $y_{t-1}$. This allows to the network to take decisions from the past and the model can take decision also in terms of *relative positions*
- Main applications regards languages (sign language, translations,...)
## Sequence to Sequence
- This is a particular application in which the input length could be variable
	- **One to one**: the input and the output -> feedforward NN
    - **One to many**: the input is a single instance and the output is a sequence (i.e. image captioning)
    - **Many to one**: the input is a sequence and the output is a single instance (i.e. sentiment analysis)
    - **Many to many**: the input is a sequence and the output is a sequence (i.e. language translation)
![[Pasted image 20261118065043.png]]

## Unfolding in Time
- The recurrent layer can be unfolded in time, in which each instance of the layer is connected to the previous one and the output is computed at each time step.
- A neuron can preserve its state $h_t$ and this is called *memory*. Then the output of the neuron is computed as a function of the input $x_t$ and the previous state $h_{t-1}$.$$h_t = f(h_{t-1}, x_t)$$
- **Unfolding or unrolling in time**: given the number of temporal instances $T$, the recurrent layer can be unfolded in time and each instance is connected to the previous one. Then the output is computed at each time step.
	- Equivalence between a 20 step unfolded RNN and a 20 layer feedforward NN with shared weights -> vanishing gradient problem
	- Note that in an unfolded RNN, the input and output are generally connected to all instances of the cell, and the cell’s weights (hidden within $f$) are shared across all instances of the cell.
![[Pasted image 20261118065557.png]]
## Basic Cells
- In a **basic cell** the output is computed as a function of the input and the previous state, and the state is updated with a new value. The cell can be unfolded in time and each instance is connected to the previous one. Then the output is computed at each time step. $$h_t = \phi(x_t^T \cdot W_x + h_{t-1}^T \cdot W_h + b)$$ where $\phi$ is the activation function (typically ReLU), $W_x$ and $W_h$ are the weights matrices for the input and the previous state respectively, and $b$ is the bias vector.
- Basic cells have some limitations:
    - They are not able to capture long-term dependencies, because the gradient can vanish or explode during backpropagation through time (BPTT)
    - They are not able to capture complex patterns, because they have a simple structure and a limited capacity
## LSTM (Long Short-Term Memory)
![[Pasted image 20261118070326.png]]
- In LSTM cells the state $h_t$ is divided into two parts:
	- $h_t$ is the short term memory, which is used for the output and the next state
	- $c_t$ is the long term memory, which is used for the cell state and the next state
- The cell is trained to learn how to update the long term memory $c_t$, in particular what to forget (**forget gate**) from the previous state $c_{t-1}$ and what to take for the next state $c_t$ (**input gate**). 
- Then the output $h_t$ is computed as a function of the long term memory $c_t$ and the short term memory $h_{t-1}$ (**output gate**). 
## GRU (Gated Recurrent Unit)
![[Pasted image 20261118070746.png]]
- GRU is a simplified version of LSTM, in which the long term memory $c_t$ and the short term memory $h_t$ are merged into a single state $h_t$.
- The cell is trained to learn how to update the state $h_t$, in particular what to forget (**reset gate**) from the previous state $h_{t-1}$ and what to take for the next state $h_t$ (**update gate**).
- Update gate = forget gate and input gate in LSTM $$z_t = \sigma(W_z x_t + U_z h_{t-1} + b_z)$$
- Reset gate = forget gate in LSTM $$r_t = \sigma(W_r x_t + U_r h_{t-1} + b_r)$$ for $r\approx 0$ the previous state is ignored, for $r\approx 1$ the previous state is used
- http://colah.github.io/posts/2015-08-Understanding-LSTMs/
## Deep RNN
![[Pasted image 20261118073041.png]]
- Deep RNN are RNN with multiple layers, in which each layer can have its own parameters and its own state. The output of each layer is used as the input for the next layer.
- A Deep RNN can be expanded in time (horizontal, as normal RNN) and in space (vertically)
- Each cell $C_t^{(l)}$ (that could be a Basic Cell, LSTM or GRU) receives two stream of information:
    - the output of the previous layer $h_t^{(l-1)}$, that comes from the same layer, providing memory to the cell
    - the output of the previous time step $h_{t-1}^{(l)}$, that comes from the previous layer at the same time step, providing context to the cell
- For a generic layer $l$: $$h^{(l)}_t = f^{(l)}\Big(h^{(l)}_{t-1}, \; h^{(l-1)}_t\Big)$$
- Why create a hierarchy of layers? Because each layer can learn different levels of abstraction, and the higher layers can capture more complex patterns and dependencies.
	- Layer 1 could learn low-level features (like edges, corners, textures)
    - Layer 2 could learn mid-level features (like shapes, objects, parts)
    - Layer 3 could learn high-level features (like actions, events, relations)
- I.e. Time Series: https://medium.com/data-science/how-not-to-use-machine-learning-for-time-series-forecasting-avoiding-the-pitfalls-19f9d7adf424
- For classification and regression on Time Series we connect a simple MLP to the output $Y_{(t)}$ on the last time step of the last layer
![[Pasted image 20261118154451.png]]
- If you have both data that does not change over time (e.g., a patient's age/sex) and a time series (e.g., blood glucose levels), an **encoder** is used, processing the static data at step $0$, to initialize the hidden states $h^{(l)}_0$ of all layers in the recurrent **decoder**, which then processes the dynamic sequence.
![[Pasted image 20261118154647.png]]
- Main challenges regards parallelization on GPU (*Wavefront Parallelization*):
	- To compute the cell at Layer 2, time $t=2$, the GPU must first complete both the cell at Layer 2, time $t=1$ (horizontal dependency) and the cell at Layer 1, time $t=2$ (vertical dependency). 
	- This creates a diagonal "wavefront" execution pattern (visible in the sequence $1 \rightarrow 2 \rightarrow 3 \rightarrow 4 \rightarrow 5 \rightarrow 6$ on slide 24), which prevents the parallel processing of all elements in a sequence and significantly slows down training compared to Transformers.
	- Furthermore, stacking too many layers (rarely more than 3–4 in RNNs, as opposed to over 100 in Transformers) results in a very long gradient path during backpropagation, both vertically and horizontally. This exacerbates the risk of vanishing or exploding gradients (a problem typically mitigated by using LSTM/GRU cells and residual connections).

# Natural Language Processing and LLM
- Main contributions: Text Classification, Entity Extraction, Summarization, Generation, Machine Translation, Coding, Question Answering, Digital Assistant/Chatbots, Automatic Speech Recognition(and Synthesis), Multimodal Extensions
## Tokenization
- Tokenization is the process of breaking down text into smaller units called tokens, which can be words, subwords, or characters. It is a crucial step in NLP as it converts raw text into a format that can be processed by machine learning models. Different tokenization methods include:
	- **Word-level tokenization**: Splits text into individual words based on spaces and punctuation.
	- **Subword-level tokenization**: Breaks down words into smaller meaningful units (e.g., Byte Pair Encoding, WordPiece).
	- **Character-level tokenization**: Treats each character as a separate token, useful for languages with complex morphology.
- There are also techniques that keep the vocabulary size fixed, without amplifying the , like *Byte Pair Encoding (BPE)* and *WordPiece*, which allow the model to handle rare or unseen words by breaking them into subword units.
- For example BPE computes the most frequent pairs of characters in a corpus and merges them iteratively to form subword units, while WordPiece builds a vocabulary of subwords based on their likelihood of occurrence in the training data.
## Word Embedding
- Word embedding is a technique used to represent words in a continuous vector space, where semantically similar words are mapped to nearby points. This allows models to capture the meaning and relationships between words, enabling better performance in NLP tasks.
- This is mainly done due to the fact giving a word a unique ID is not enough to capture the semantic meaning of words, and it does not allow the model to generalize to unseen words or contexts.
- Word2Vec is one of the most popular word embedding techniques, which uses a shallow neural network (unsupervised with 2 layers) to learn word representations based on their context in a large corpus.
![[Pasted image 20261118160500.png]]
- Embedding techniques can be trained using different objectives, such as predicting a word given its context (skip-gram) or predicting the context given a word (CBOW). The resulting embeddings capture semantic relationships, such as synonyms and analogies, allowing for better performance in downstream NLP tasks.
- Also there are multimedia embeddings, in which the embedding space is shared between different modalities (e.g., text and images), allowing for cross-modal retrieval and understanding.
## Neural Machine Translation
- **Autoregressive Generation**: The model (Generative and sequence-to-sequence) generates the target sequence one token at a time, using the previously generated tokens as context. This allows for more accurate translations, as the model can consider the entire context of the sentence.
![[Pasted image 20261118160911.png]]
- **Neural Machine Translation** (NMT) is an approach to machine translation based on deep learning neural networks (using a many-to-many, variable-length model), in which a single architecture is trained end-to-end to map a sequence of words in a source language directly to a sequence in a target language.
- The main architecture of NMT is based on encoder-decoder models with attention mechanisms
	- **Encoder**: Processes the input sequence (source language) and encodes it into a fixed-length context vector, which captures the semantic meaning of the input.
	- **Decoder**: Takes the context vector and generates the output sequence (target language in autoregressive manner), one token at a time, using the previously generated tokens as context.
## Transformers
- Google Brain, 2017, "Attention is All You Need" paper, introduced the Transformer architecture, which revolutionized NLP by enabling parallel processing of sequences and capturing long-range dependencies more effectively than RNNs. 
- There are layers for **self-attention** that allow the model to weigh the importance of different tokens in the input sequence when generating the output, and **feedforward layers** that process the attended information.
- The first transform from Google Brain was called BERT (Bidirectional Encoder Representations from Transformers), which is a pre-trained language model that can be fine-tuned for various NLP tasks, such as text classification, question answering, and named entity recognition.
- Modern transformers (including LLMs) are *Decoder-only*:
	- Columnar structure and weight sharing: Each column corresponds to a position in the sequence (a token). As in RNNs, the network's weights are shared across all columns (meaning the number of parameters does not depend on the length of the prompt), and generation during inference remains autoregressive (one token at a time).
	- **Residual Stream**: Each column possesses a high-dimensional vertical "channel" (e.g., $d_{model} = 12,288$ in GPT-3) that carries information from the initial encoding (token embedding + positional encoding) all the way to the final decoding. The various layers communicate with one another via message passing, reading from and writing to specific subspaces of this residual stream without necessarily overwriting the information already present.
	- For each layer there are:
		- an MLP that reads and writes on its own residual stream, working as an associative memory.  
		- an Attention mechanism that reads from the residual stream of all previous tokens and writes on its own residual stream. This allows the model to capture long-range dependencies and relationships between tokens, as well as to attend to relevant information from the entire sequence when generating the output. **$1/3$** of the weights are used for attention mechanisms
![[Pasted image 20261118165155.png]]
![[Pasted image 20261118165233.png]]
- For example: an LLM with $90B$ parameters and $100$ layers it has $900$ millions of parameters for each layer, which is divided into $300$ millions for the attention mechanism and $600$ millions for the MLP. But as regards residual stream the weights are shared among them
	- If the prompt is composed of 10 token, there are 10 residual streams but the weights are shared among them and the model still composed of $90B$ weights.
![[Pasted image 20261118165814.png]]
- Also computation can be parallelized
    - Each layer can be computed in parallel, and each token can be computed in parallel within a layer. This allows for efficient training and inference on modern hardware (GPUs/TPUs).
	- The (global) attention mechanism does not dilute information during propagation.
## Large Language Models
- LLMs are autoregressive transformer-based models that are pre-trained on large corpora of text data and can generate coherent and contextually relevant text. 
- They have hundreds of billions of parameters
- The main architecture is based on Decoder-only transformers with a colonnar structure:
	- **Tokenization and Token Embedding**: The input text is tokenized into subword units (with BPE), and each token is mapped to a high-dimensional vector representation (token embedding) that captures its semantic meaning.
	- **Residual stream**: each token is processed by a residual stream that carries information from the initial encoding (token embedding + positional encoding) all the way to the final decoding.
	- **Attention + MLP Mechanism**: Each layer has an attention mechanism that allows the model to weigh the importance of different tokens in the input sequence when generating the output, capturing long-range dependencies and relationships between tokens.
- This architecture works in two phases:
	- **Next-Token Prediction**: self-supervised phase in which a window is slid across the texts in the corpus, and the model is trained to predict the token that immediately follows a given sequence.
	- **Autoregressive generation**(Inference): the model generates text one token at a time, using the previously generated tokens as context. The generation process continues until a stopping criterion is met (e.g., reaching a maximum length or generating a special end-of-sequence token EOS).
- Writing a prompt then becomes a way to *condition* the model to generate text that is relevant to a specific task or context. The prompt can be a question, a statement, or a few examples of the desired output, and the model will generate text that is coherent and contextually appropriate based on the given prompt.
## Prompt Engineering
- *Prompt engineering* is an art that involves crafting prompts that effectively guide the model's output, and it can significantly impact the quality and relevance of the generated text. There are some techniques that helps out providing good answers:
	- **Chain of Thoughts**: give examples of reasoning steps that lead to the final answer, which helps the model to generate more accurate and logical responses.
	- **In-Context Learning**: provide examples of the desired input-output pairs in the prompt, which helps the model to learn the task and generate more relevant responses without modifying its parameters.
		- Zero-shot learning: provide only the task in the prompt
		- One-shot learning: provide one example of the task in the prompt
        - Few-shot learning: provide a few examples of the task in the prompt
## RAG systems and Document Embedding
- **Document embedding**: Instead of assigning a vector to each token (token embedding), we can assign a vector to each document. 
- This allows us to represent the semantic meaning of entire documents in a high-dimensional vector space, enabling efficient retrieval and comparison of documents based on their content, showing similarities and differences between them.
	- Documents that are semantically similar will have embeddings that are close together in the vector space, also in the case they are not similar in terms of words used.
	- Given a space of documents, we can see how similar a new document is to the existing ones by computing the cosine similarity (or euclidean distance) between their embeddings.
- Besides, a 1000 pages of a document inserted in a prompt is not feasible, so we can use a **RAG system** (*Retrieval-Augmented Generation*) that allows the model to retrieve relevant information from a large corpus of documents and use it to generate more accurate and contextually relevant responses.
- RAG systems work in two steps:
	- **Indexing** (done offline): the documents are processed and embedded into a vector space, and an index is created to allow for efficient retrieval of relevant documents based on their embeddings.
		- **Chunking**: each document is divided in small fragments(*chunk*) and partially overlapped to avoid losing information.
		- **Embedding and Archiving**: a embedding model (generally a small size LLM) is used to compute the embedding of each chunk and store it in a vector database (like Pinecone, Weaviate, Milvus, ChromaDB, etc.)
	- **Retrieval and Generation** (online given the user task)
		- **Query Embedding**: given a prompt in natural language, the query is embedded into the same vector space as the documents using the same embedding model.
		- **Document Retrieval**: the most relevant documents are retrieved from the index based on their similarity to the query embedding (using cosine similarity or other distance metrics).
		- **Generation**: an enriched prompt is sent to a more strong generative LLM (in cloud), and that prompt contains the original query and the chunks of the retrieved documents. The model generates a response based on the combined information, providing a more accurate and contextually relevant answer.
- **Fine tuning**: is an alternative process from RAG systems in which a pre-trained model is further trained on a specific task or domain, allowing it to adapt to the new data and improve its performance on the target task. This can be done by updating the model's parameters using a smaller learning rate and a smaller dataset, while keeping the pre-trained weights as a starting point.
	- It requires a labeled dataset with `Prompt: Output`
	- **Catastrophic Forgetting**: weight modification during fine-tuning can lead to the model forgetting previously learned knowledge, which can negatively impact its performance on other tasks or domains. To mitigate this, techniques such as regularization, knowledge distillation, and multi-task learning can be employed.
	- There are strategies that could avoid catastrophic forgetting, like *LoRA* (Low-Rank Adaptation), which is a technique that allows for efficient fine-tuning of large language models by updating only a small subset of the model's parameters, while keeping the majority of the pre-trained weights frozen by using "lateral adapters", in other words low rank matrices which are trained in parallel with the frozen layers to gradually steer the responses toward the new domain.
	- There are also techniques that change only the last layers.
# Reinforcement Learning 
- The goal is to learn optimal behavior through direct interaction and past experiences, without having a pre-labeled dataset
![[Pasted image 20261118175717.png]]
- Terminologies:
	- **Agent**: It is the decision-maker or the model that must learn how to behave (e.g., software that controls a robotic arm or plays a video game). It observes the environment and chooses which actions to take.
	- **State**($s$): It is the representation of the current state of the environment as perceived by the agent at a given moment (e.g., the coordinates of a robotic arm in relation to an object, or the pixels on a video game screen).
	- **Action**($a$): It is the decision or move that the agent can make in response to the current state (e.g., moving the robotic arm up or down, or pressing a button in a video game).
	- **Reward (and penalties)**: It is a scalar numerical signal that the environment returns to the agent after an action, in order to evaluate how good that action was (e.g., $+100$ if it captures a ball, $-1$ if it moves away).
	- **Game** (or episode): An entire finite sequence of interactions from the beginning to the end of a task. For example: $s_0, a_0, r_1, s_1, a_1, r_2, \dots, s_n$
- "An agent performs actions $a$ that modify the environment, causing transitions from one state $s$ to another. When the agent achieves positive results, it receives a reward $r$—which may, however, be temporally delayed relative to the action, or sequence of actions, that brought it about".
- *Optimal action* for the *future reward* is then the main objective of the agent model: given a state $s_{t-1}$ the agent must not simply choose the action that *yields* the highest immediate gain, but *rather the one that maximizes the sum of all future* *rewards* until the end of the episode. $$R_t = r_t + r_{t+1} + \dots + r_n$$
- This implies that the agent must learn a long term strategy that helps it to maximize the total reward over time, rather than just focusing on short-term gains (*credit assignment*). This is particularly important in environments where actions may have delayed consequences or where the optimal strategy requires planning and foresight.
- *Discounted future reward*: most applications are *stochastic*, then each action won't certainly produce the same result. For this reason, a **discount factor** $\gamma$ is introduced to weigh the importance of future rewards, with $0 \leq \gamma < 1$. The discounted future reward is then defined as: $$R_t = r_t + \gamma r_{t+1} + \gamma^2 r_{t+2} + \dots + \gamma^{n-t} r_n \space \text{ or } R_t = r_t + \gamma \cdot R_{t+1}$$Each time the agent takes an action, it receives a reward that is discounted by a factor of $\gamma$ raised to the power of the number of time steps into the future that the reward is received. This encourages the agent to prioritize immediate rewards while still considering the potential benefits of future rewards.
- The recursive equation is the **Bellman Equation**, used for *Q-Learning* algorithm.
## Q-Learning
- Q-Learning is a model-free reinforcement learning algorithm that learns the optimal action-value function (Q-function) for a given environment. 
- The **Q-function** $Q(s,a)$ (or *optimality*) estimates the expected cumulative reward for taking a specific action $a$ in a specific state $s$:$$Q(s_t, a_t)= max R_{t+1}$$
- Given the Q-function, we can demonstrate that the optimal policy $a^*$ is to choose the action that maximizes the Q-value for the current state:$$a^*(s_t) = argmax_a Q(s_t, a)$$
- Given the transition $<s_t, a_t, r_{t+1}, s_{t+1}>$ we can state that: $$Q_(s_t, a_t) = max R_{t+1} = max (r_{t+1} + \gamma \cdot R_{t+2}) = \\ r_{t+1} + \gamma \cdot max R_{t+2} = r_{t+1} + \gamma \cdot Q(s_{t+1}, a_{t+1}) $$
- In particular this is the *Bellman equation:* $$Q(s_t, a_t) = r_{t+1} + \gamma \cdot max_a Q(s_{t+1}, a)$$
- Pseudocode:
```
Initialize Q(s, a) arbitrarily for all states s and actions a
For each episode:
    Initialize state s
    For each step of the episode:
        Choose action a from state s using policy derived from Q (a = argmax Q(s,a))
        Take action a, observe next reward r' and next state s'
        Update Q(s, a) using the Bellman equation:
            Q(s, a) = Q(s, a) + alpha * (r' + gamma * max_a' Q(s', a') - Q(s, a))
        Set state s = s'
```
- Where `alpha` is the **learning rate** and it's progressively reduced during the learning phase. The learning rate controls how much the Q-values are updated based on new experiences, and a smaller learning rate allows the agent to converge more slowly but steadily to the optimal policy.
- Typical initialization: $\gamma = 0.9$, $\alpha = 0.5$
- Examples:
	- OpenAI Gym + Box2D: https://gym.openai.com/ 
		- ![[Pasted image 20261118183605.png]]
		- State: $\varDelta x, \varDelta y$ + arm gripper state (open/closed)
		- Action: rotation for three joints and reverse arm gripper state (close if open and viceversa)
		- Reward: $+1$ if close to the ball, $-1$ if far from the ball, $+100$ if it catches the ball, $-0.2$ if movement with closed gripper
	- Also Q table memorization version done by prof. Maltoni: 
		- ![[Pasted image 20261118184130.png]]
		- Pre-training:  http://bias.csr.unibo.it/maltoni/ml/Demo/Qarm_pre.wmv
		- Post-training: http://bias.csr.unibo.it/maltoni/ml/Demo/Qarm_post.wmv
	- Balancing with 3D simulation and Bullet Physics Engine:![[Pasted image 20261118184348.png]]

## Deep Reinforcement Learning
- Deep Mind, 2013, "Playing Atari with Deep Reinforcement Learning" paper, introduced Deep Q-Networks (DQN), which combine Q-learning with deep neural networks to handle high-dimensional state spaces (like images) and learn complex policies.
- In particular an agent is trained to play Atari videogames at a super-human level, using only the raw pixel inputs and the game score as feedback. The DQN architecture uses a convolutional neural network to process the input images and output Q-values for each possible action.
- The CNN works as a Q-table that approximates the Q-function, generalizing on never-seen images. This allows to use the same model on different games only by seeing the pixels and the score, without any knowledge of the game rules or dynamics.
- In 2016 was developed the model AlphaGo, that beats the world champion in the game of Go, using a combination of deep reinforcement learning and Monte Carlo tree search. The model was trained on a large dataset of expert games and then improved through self-play, achieving superhuman performance.
- Nowadays there are more efficient approaches like *Proximal Policy Optimization (PPO)*, which is a policy gradient method that optimizes the policy directly, rather than learning a value function, and *Group Relative Policy Optimization (GRPO)* that is a distributed version of PPO that allows for more efficient training on large-scale environments.