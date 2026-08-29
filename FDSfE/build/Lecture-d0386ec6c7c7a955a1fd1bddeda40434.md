# Learning Questions

-   What is a perceptron?

-   What is a multilayer perceptron?

-   What is a "feed-forward" "densely connected" neural network?

-   What is an RNN?

-   What is LSTM?

-   What is a CNN?

-   What is regularization?

# Introduction

The neural network is an algorithm --- a collection of algorithms ---
inspired by the human brain. A biological neuron receives signals
through its dendrites, processes them in the cell body and sends the
output through its axon. **Artificial neural networks** (ANNs) are
simplified mathematical models of the biological process. They are a
metaphor for the brain. The human (and not only human) brain has an
uncanny ability to learn patterns and make connections between events.
When ANNs were developing, the hope was that if we mimic *the way* the
brain learns with a computer, the computer can learn just as much, just
as well, just as fast.

This lesson introduces *inferential neural networks*, networks designed
to make predictions from data. We will start with the simplest neural
network, the perceptron, and then build up to modern architectures. We
will cover densely connected neural networks for tabular data, recurrent
neural networks (RNNs) for sequences, and convolutional neural networks
(CNNs) for images. A discussion of generative neural networks
(variational autoencoders, diffusion models, generative adversarial
networks, transformers and large language models, all of which are now
colloquially referred to as "Artificial Intelligence" (AI)) will come in
the next lesson.

# The Perceptron

The **perceptron** is the simplest neural network. It was invented in
1957, though it was theorized a decade earlier. When the New York Times
reported on it, they said that it would be able to read, write and even
become conscious of its existence. It would take many decades for neural
networks to learn to read and write, and their sentience is not even on
the horizon.

Anyway. A perceptron is a binary classifier. It takes inputs, multiplies
each input by a weight, sums the products, and then passes the result
through a step function. If the output is above a threshold, the
perceptron predicts one class; otherwise, it predicts the other.

Eventually, it was shown that such a simple algorithm could not only
model linear functions. Everyone already knows how to model linear
functions at this point, so the perceptron faded for a while. It was
then discovered that multiple perceptrons *would* be able to model
nonlinearity. This is the multilayer perceptron, the foundation of
modern deep learning.

# The Neuron Equation

A **neuron** is the fundamental unit of a neural network. Ultimately, a
neuron is just a function. It has inputs and outputs. We can represent
the inputs by a vector, $\mathbf{x}=[x_1,x_2,\ldots,x_n]$ and we can
represent the output by a scalar, $y$. This function has parameters too,
just like any other model we discussed. Normally, we referred to these
parameters as $\boldsymbol{\beta}$, but in the context of ANNs, we will
call them **weights,** which is also a vector,
$\mathbf{w}=[w_1,w_2,\ldots,w_n]$. Do you recall how, in a linear model,
there was always one more parameter than there were inputs? $p=n+1$.
It's similar with the neuron: there is another parameter called the
**bias** $b$. Thus we have $n$ inputs, $n$ weights, and $1$ bias. The
last component of the neuron is its **activation function,** $f(z)$. The
choice of activation function is a hyperparameter.

The neuron equation combines all of these elements into a simple
equation:

$$\begin{aligned}
    y &= f \left( \mathbf{w} \cdot \mathbf{x} + b \right) \\
    y &= f \left( \sum_{i=1}^n w_i x_i + b \right)
\end{aligned}$$

The importance of this equation, not just for the field of machine
learning and data science but for world history too, is nearly
impossible to overstate.

# Activation Functions

The activation function is what gives ANNs their nonlinearity and thus
their ability to learn complex relationships between inputs and outputs.
The most widely used activation function is likely the **Rectified
Linear Unit** (ReLU). It's defined simply as $f(z)= \max(0,z)$ where
$z=\mathbf{w} \cdot \mathbf{x} + b$. If $z$, that combination of inputs,
weights, and bias is less than zero, ReLU outputs a zero. If $z>0$, ReLU
simply outputs $z$. This function is very computationally efficient and
is generally the default, first choice activation function if you are
unsure what to use.

The second most widely used activation function is likely the
**sigmoid.** We've actually seen the sigmoid before --- it's the
logistic function, $\sigma(z)=\frac{1}{1+e^{-z}}$. The input of the
sigmoid can be any real number, but the output is always between zero
and one.

# The Multilayer Perceptron

A single neuron is limited, but multiple neurons can learn anything. A
**multilayer perceptron** (MLP) consists of a group of neurons called
the input layer, one or more groups of neurons called hidden layers, and
one group of neurons called the output layer. In a **densely connected**
(or fully connected) layer, every neuron in the layer receives input
from every neuron in the previous layer. Each connection has it's own
weight, and each neuron has its own bias.

![ A simple Artificial Neural Network diagram of a densely connected
network. Source: Wikipedia, Colin M.L.
Burnett.](../figures_pedagogy/Colored_neural_network.svg.png)

Take a look at [1.11](#fig:pedagogy_ann), which shows an example of a densely
connected neural network with one hidden layer. The input layer has
three neurons, the hidden layer has four neurons, and the output layer
has two neurons. How many parameters does this model have?

Let's construct a simple example. We have a dataset with three features
and two targets.[^17] Each of the three features is given as input to
one of the three input neurons. These input neurons don't really do
anything. They don't have any weights or biases, and no activation
function is applied. They just serve as starting points for each of the
features in our dataset. You'll always have as many input neurons as you
have features.

Because the network is densely connected, each input neuron is passed to
each hidden layer; each hidden layer has three inputs. This hidden layer
consists of full neurons now, so we can begin counting the parameters.
Each neuron in the hidden layer has one weight for each input ($3$), and
there are four neurons in this layer, so there are $3\cdot4=12$ weights.
There is also one bias per neuron, so that's another four parameters. We
then repeat this with the output layer: each neuron in the output layer
has four inputs, so there are $4\cdot2=8$ weights, and there are two
neurons in the layer, so there are two biases.

In total, that's $26$ parameters: 20 weights and 6 biases. That's a lot
more than the two parameters of the simple linear model! In general, the
number of parameters in the $i$-th *layer* of a densely connected neural
network, $p_i$, is

$$p_i=(n_{i-1} \cdot n_i) + n_i$$

where the $n_i$ is the number of neurons in the $i$-th layer, and
$n_{i-1}$ is the number of neurons in the previous layer.

When someone talks about a "simple" neural network, or a "dense
network," they're most likely referring to an MLP.

# The Universal Approximation Theorem

In 1989, it was proven that a neural network with a single hidden layer
(with a finite number of neurons in that layer) can approximate *any*
continuous function. This is the **universal approximation
theorem**.[^18] Of course, there is no guarantee that you will be able
to create that model; the theorem does not provide a way to calculate
how many neurons you need. If you've ever heard that neural networks can
learn "anything," that is not hyperbole. However, I would advise you to
consider what that person is trying to convince you of, because neural
networks aren't magic. They rely on good data and on good architectural
choices, like how many neurons, which activation functions, which loss
function, etc., just like any other model.

# Training Neural Networks

The goal of training a neural network is to adjust the weights and
biases so that the output of the network matches the target output. Does
this sound familiar? This is exactly like every other supervised machine
learning model we've discussed. And just like those other models, the
neural network needs an objective function. In the context of ANNs,
you'll mostly hear them called "loss functions."

For regression tasks, you may use any of the objective functions we've
discussed before. Mean Squared Error (MSE) is still the most common. For
binary classification tasks, you'll want to use a version of the log
loss utilized in logistic regression. This time it's called **binary
cross-entropy**, but really it's the same thing. For multi-class
classification[^19] the loss function is similar, but it's called
**categorical cross-entropy**.[^20]

## Backpropagation

It was a challenge figuring out the OLS method for finding the best
parameters for the linear model, and the tiny network shown in
[1.11](#fig:pedagogy_ann) had 26 parameters! How do we find the best
values for all those weights and biases? The algorithm is called
**backpropagation**. First, the network passes the data through the
network, calculating the neuron equation for every neuron in sequence.
The model produces an output, and the objective function is used to
calculate the loss on that output compared to the desired output. Then,
the errors are propagated *backwards* through the network. The details
of backpropagation lie in complex calculus; essentially, the algorithm
determines which parameter is responsible for increasing the loss on
each output, and by how much (leveraging differentiation and the chain
rule). The parameter is then updated to lower the loss, and all of that
represents one **batch** of learning.

A batch can include one observation passing through the network; it can
include the entire training set passing through the network, or it can
(and in most cases does) include some amount in between. The
`batch_size` hyperparameter governs this for most implementations of
ANNs.

# Recurrent Neural Networks

Feed-forward neural networks treat each input independently. The neuron
doesn't store any information about what it has done in the past. This
works great for tabular data, but many tasks involve sequential data:
time series, text, audio and video. A **recurrent neural network** (RNN)
processes each element of the sequence in order, rather than all at
once. The RNN introduces the concept of a hidden state. At each element,
the RNN saves some information in that hidden state variable. At the
next element, the hidden state information from the previous element is
provided. The model produces an output and a new hidden state, and the
cycle repeats. This hidden state is like the memory of the network.

Consider a time series of daily temperatures. You want to predict
tomorrow's temperature based on the past week. An RNN processes the
sequence day by day. At each step, it updates its hidden state based on
the temperature of that day and the previous hidden state. At the final
step, the hidden state contains information about the entire sequence,
and the network produces a prediction for tomorrow.

The RNN as described suffers from an issue called the **vanishing
gradient problem.** When the sequence is long, the gradients (the part
of the backpropagation algorithm that propagates errors through the
model) can become so small that they "vanish." This means that RNNs
struggle with long sequences.

The solution is the **long short-term memory** (LSTM) network. The LSTM
introduces a **cell state** that runs through the entire sequence,
**gates** that control the flow of information. The details are
interesting but beyond the scope of this lesson. Suffice it to say that
LSTMs are the de facto standard ANN for sequential data, though this has
changed since 2017 with the advent of the Transformer, which we will
discuss in the next lesson.

# Convolutional Neural Networks

None of the networks discussed so far were designed with images in mind.
Images are tricky. They are at least 2D (height, width), but they can be
three or four dimensional (height, width, color, transparency/depth). On
top of that, images have spatial structure; neighboring pixels are
related. The MLP would ignore this structure, treating each pixel as an
independent feature. A **convolutional neural network** (CNN) is
designed for 2D data and includes the spatial structure in its
calculations.

The CNN uses the convolution operation to extract features from images.
The convolution starts with a filter (also called a kernel) that slides
over the image. For example, consider an image that is five pixels by
five pixels and a kernel that is three pixels by three pixels. We can
represent them with matrices.

$$\begin{aligned}
    \text{Image}=
    \begin{bmatrix}
        a_{11} & a_{12} & a_{13} & a_{14} & a_{15} \\
        a_{21} & a_{22} & a_{23} & a_{24} & a_{25} \\
        a_{31} & a_{32} & a_{33} & a_{34} & a_{35} \\
        a_{41} & a_{42} & a_{43} & a_{44} & a_{45} \\
        a_{51} & a_{52} & a_{53} & a_{54} & a_{55}
    \end{bmatrix}
    \quad
    \text{Kernel}=
    \begin{bmatrix}
        k_{11} & k_{12} & k_{13} \\
        k_{21} & k_{22} & k_{23} \\
        k_{31} & k_{32} & k_{33}
    \end{bmatrix}
\end{aligned}$$

where $a_{ij}$ is the image pixel value at row $i$ and column $j$, and
$k_{ij}$ are the kernel pixel values.

When we say that the kernel "slides" over the image, imagine that the
kernel is overlaying on top of the image. The kernel starts in the top
left of the image, represented here by the $3 \times 3$ sub-matrix.

$$\begin{bmatrix}
        a_{11} & a_{12} & a_{13}\\
        a_{21} & a_{22} & a_{23}\\
        a_{31} & a_{32} & a_{33}
    \end{bmatrix}$$

Then the sub-matrix and the kernel are multiplied together,
element-wise, and all of the elements are added together. This results
in one element, $c_{11}$, of the result of the convolution. The kernel
slides over the image, combining with the sub-matrices to fill out the
resulting convolution.

$$\begin{aligned}
    \text{Convolution}=
    \begin{bmatrix}
        c_{11} & c_{12} & c_{13}\\
        c_{21} & c_{22} & c_{23}\\
        c_{31} & c_{32} & c_{33}
    \end{bmatrix}
\end{aligned}$$

That was pretty abstract, so let's try that again with actual numbers.
Given the following image and kernel, what is the resulting convolution?

$$\begin{aligned}
    \text{Image}=
    \begin{bmatrix}
        1 & 0 & 0 & 0 & 1 \\
        0 & 1 & 0 & 1 & 0 \\
        0 & 0 & 1 & 0 & 0 \\
        0 & 1 & 0 & 1 & 0 \\
        1 & 0 & 0 & 0 & 1
    \end{bmatrix}
    \quad
    \text{Kernel}=
    \begin{bmatrix}
        1 & 0 & 0 \\
        0 & 1 & 0 \\
        0 & 0 & 1
    \end{bmatrix}
\end{aligned}$$

The first three sub-matrices that the kernel would combine with are:

$$\begin{aligned}
    \begin{bmatrix}
        1 & 0 & 0 \\
        0 & 1 & 0 \\
        0 & 0 & 1 
    \end{bmatrix}\quad
    \begin{bmatrix}
        0 & 0 & 0\\
        1 & 0 & 1\\
        0 & 1 & 0
    \end{bmatrix}\quad
    \begin{bmatrix}
        0 & 0 & 1 \\
        0 & 1 & 0 \\
        1 & 0 & 0
    \end{bmatrix}.
\end{aligned}$$

When we perform the element-wise multiplication with the kernel and then
sum all the elements, we get:

$$\begin{aligned}
    \begin{bmatrix}
        1 & 0 & 0 \\
        0 & 1 & 0 \\
        0 & 0 & 1 
    \end{bmatrix}\rightarrow3 \quad
    \begin{bmatrix}
        0 & 0 & 0\\
        0 & 0 & 0\\
        0 & 0 & 0
    \end{bmatrix}\rightarrow0 \quad
    \begin{bmatrix}
        0 & 0 & 0 \\
        0 & 1 & 0 \\
        0 & 0 & 0
    \end{bmatrix}\rightarrow1.
\end{aligned}$$

Repeating this for all nine sub-matrices, the result of the convolution
is

$$\begin{aligned}
    \text{Convolution}=
    \begin{bmatrix}
        3 & 0 & 1 \\
        0 & 3 & 0 \\
        1 & 0 & 3 
    \end{bmatrix}.
\end{aligned}$$

The original image was an x-shape, the kernel was a diagonal line going
down to the right, and the final convolution was another x-shape with a
diagonal line emphasized. This example demonstrates what convolutions
do. **The convolution finds shapes in the image that are similar to the
shape in the kernel**.

The CNN incorporates several convolutional layers, each with several
different kernels working in parallel, to extract parts of the input
that look like the kernel. We call the result of these convolutions
"feature maps." The trick with the CNN is that the parameters in a
convolutional layer are the pixels of the kernel. As the CNN learns, the
shape of the kernels changes, and the features extracted from the inputs
are adjusted based on what would produce the best output for the model.

# Regularization

Because neural networks are so flexible, they're prone to overfitting.
The universal approximation theorem already tells us that a network with
only one dense layer can learn any function. If you have a network with
many dense layers, with many neurons each, its very easy for a network
to learn the training data so well that it fails to generalize to
anything else. **Regularization** is the term for the set of techniques
that aim to improve generalizability in machine learning models. For
ANNs, regularization is often necessary.

**Dropout** is a regularization technique that forces a fraction of the
neurons in a network to set their output to zero. This forces the
network to learn multiple representations of the data, which helps the
model generalize. The dropout rate is a hyperparameter. Another form of
regularization is **early stopping.** During training, we are always
monitoring how the loss function performs on the validation set. If the
loss on the validation set stops improving or starts worsening, early
stopping will halt training. When the validation set performance stops
improving, that's a good indicator that the model's generalizability
isn't improving either.

# The Black Box

Neural networks are referred to as "black boxes." They often have so
many parameters that it's impossible to interpret why a particular
prediction was made. Contrast this with a decision tree classifier, and
it's immediately obvious what decisions were made that led to a
prediction. This critique is valid, but it's not always a problem.
Neural networks achieve state-of-the-art performance on many tasks ---
that's why they're so popular; they're demonstrably powerful. But if
interpretability is important, which it is in some domains like medicine
or criminal justice, then ANNs may not be appropriate.


[^17]: We've only ever had datasets with one target feature before, but
    when you have more than one, it's called "multi-label
    classification."

[^18]: We could be a bit more precise here. The theorem states that for
    any given function and any desired accuracy ($\epsilon > 0$), there
    exists a network with a sufficiently large, but finite, number of
    neurons in its hidden layer to achieve that accuracy on a compact
    (closed and bounded) subset of $\mathbb{R}^n$ ( \[0,1\]). Also, the
    activation function has some requirements, namely, it cannot be
    linear (recall that the perceptron could only learn linear
    functions, partially because it lacked a nonlinear activation
    function). Finally, it is an *existence* theorem, not a learnability
    theorem: it proves that such a network exists, but it does not prove
    that the training will actually find the correct weights, nor does
    it guarantee the network will generalize well to unseen data.

[^19]: Multi-class classification is where you are predicting one target
    with multiple classes, predicting whether an image is of an apple,
    banana, or peach. Binary classification is when you are predicting
    one target with only two classes. Multi-label classification is when
    you are predicting multiple *targets*, predicting whether an image
    contains fruit *and* whether the image contains an animal.

[^20]: As it turns out, one can express a lot of creativity in the
    formulation of the loss function, and a lot of what makes a model
    successful is a good, perhaps custom-designed loss function. The
    ones listed above are the canonical choices which are all you will
    need to work with in this class, and perhaps in your whole data
    science career.