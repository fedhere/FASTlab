# Learning Questions

-   What is the difference between an inferential and generative neural
    network?

-   What is an autoencoder?

-   What is a variational autoencoder?

-   What is the latent space?

-   What is a generative adversarial network?

-   What is a diffusion model?

-   What is the Transformer?

-   What is the attention mechanism?

-   What is a large language model?

# Introduction

In the last lesson, we explored inferential neural networks, models that
make predictions from data. For example, a neural network can be trained
on a dataset of handwritten digits so that it can classify what digit an
image contains. The model's input is an image, and its output is a
label. But what if we want to create new data instead of classifying (or
regressing) it? What if we want to generate a new image of a handwritten
digit that doesn't exist in the training set? This is the domain of
**generative neural networks.**

Generative models learn the underlying distribution of the training
data. Once trained, they can sample from this distribution to create new
data that "resembles" (or is consistent with) the training set.
Generative models can produce images, text, music and video that appear
human-made. This is its greatest strength, but it also presents the
greatest dilemma, which we will discuss at the end of the lesson.

# Autoencoders

An **autoencoder** is a neural network that learns to reconstruct its
input. It consists of two components: an encoder and a decoder, which
can almost be considered two separate models. The encoder takes the
input data and compresses it into a lower-dimensional representation
called the **latent space.** The decoder takes this compressed
representation and reconstructs the original input. Just like any model,
this one needs a loss: the network is trained to minimize the difference
between the input and the reconstruction --- the reconstruction loss.

Consider a dataset of handwritten digits. One such popular dataset
exists called the `MNIST` dataset. Each image in the dataset is
$28 \times 28\text{ pixels}$. An autoencoder for `MNIST` might have an
encoder that compresses the image represented by
$28\times28=784\text{ numbers}$ into just $16$ numbers. These $16$
numbers constitute the latent space. The decoder then takes those $16$
numbers and reconstructs a $28 \times 28\text{ pixel}$. The network
learns to preserve the most important features of the digits, its shape,
the width of each line, the orientation, etc.
[1.12](#fig:vae){reference-type="ref+label" reference="fig:vae"} shows
an example architecture diagram of an autoencoder.

![An example (variational) autoencoder architecture. Image credit:
EugenioTL (Wikipedia)](../figures_pedagogy/VAE_Basic.png)

We talked before about how convolutional neural networks (CNNs) are
particularly well-suited to dealing with image data. Thus, you may
imagine that an autoencoder could make use of convolutions too. The
convolution can certainly be used to create this latent space. However,
what operation do we use to increase the dimensions of the data from the
latent space? This is done using the transpose of the convolution
operation. Transposed convolution (sometimes called
"deconvolution"[^21]) increases the size of the image, rather than
decreases it as the convolution operation does. Transposed convolution
learns the best way to increase the resolution from the data.

The autoencoder can be extended to perform more advanced tasks. For
example, we can mask parts of the input --- either random pixels or
entire regions are hidden from the model during training --- and train
the network to reconstruct the full image from the partial input. This
forces the network to learn a richer representation of the data. Or we
can train it to improve the resolution or quality of an image (Instagram
and TikTok filters that do this could very well be based on this
technology).

# Variational Autoencoders

A standard autoencoder learns a deterministic mapping from input to
latent space. Given an input, the encoder always produces the same
latent representation. This works for reconstruction, but it makes
generation difficult. If we observe the distribution of values that the
latent space takes for each digit, then we can sample from those
distributions, feed them into the decoder, and then produce an entirely
new image of a digit. But if we do this, we may end up choosing latent
space parameters that do not correspond to any real data.

The **variational autoencoder** (VAE) solves this problem by learning a
probability distribution over the latent space. Instead of outputting a
single point, the encoder outputs a mean and a variance for each latent
dimension. The decoder then samples from this distribution to generate
new data.

The VAE has a special loss function with two components. The
reconstruction loss term measures how well the decoder reconstructs the
input, but the **KL divergence** term measures how close the learned
latent distribution is to a standard normal distribution. The
Kullback--Leibler (KL) divergence is a measure of how one probability
distribution differs from another. This term penalizes the model during
training if the latent space is not smooth and continuous. This allows
us to then sample from the latent space and generate new data that looks
plausible. If a VAE were trained on the `MNIST` dataset, we could sample
the latent space at different points to generate images of digits drawn
with different styles.

# Generative Adversarial Networks

A **generative adversarial network** (GAN) takes a different approach to
generation. Instead of learning a latent space, a GAN uses two networks
that compete against each other. The **generator** creates fake data
from random noise. The **discriminator** tries to distinguish real data
from fake data. The two networks are trained together in an adversarial
game. The generator creates images with the express purpose of trying to
fool the discriminator. Over time, the generator becomes better at
creating realistic data as the discriminator becomes better at detecting
fakes.

For `MNIST`, the generator takes a random noise vector as input and
outputs a $28 \times 28$ image of a digit. The discriminator receives
both real `MNIST` images and generated images, and tries to classify
them as real or fake. As training progresses, the generator learns to
produce images that look like real digits.

GANs can produce high-quality, realistic images, but the training can be
unstable. The generator may suffer from *mode collapse,* where it
produces only a few types of outputs.

# Diffusion models

**Diffusion models** work by gradually adding noise to data and then
learning to reverse the process. In the **forward process,** noise is
added to an image step-by-step. After enough steps, the image becomes
only random noise. In the **reverse process,** the model learns to
remove the noise step by step, recovering the original image. Once
trained, a diffusion model can generate new images by starting with pure
random noise and iteratively applying the learned denoising process.
After enough steps, the noise becomes a coherent image.

Diffusion models produce high-quality, diverse outputs and are more
stable to train than GANs. However, they are also slow to generate
images because they require many iterative steps.

![The strengths and weaknesses of each of the three main generative AI
frameworks: Variational Autoencoders and Normalizing Flows, Generative
Adversarial Networks, and Diffusion Models. From
<https://www.nvidia.com/en-us/glossary/generative-ai/>.](../figures_pedagogy/evaluateGAI.png)

# VAE, GAN or Diffusion?

The previous sections contained guidance on the pros and cons of three
generative AI approaches commonly used for image generation.
[1.13](#fig:genai)
conveys these pros and cons graphically to help you choose what to use
based on your requirements on accuracy, diversity of generated data, and
computational constraints.

# Transformers

Generative models are not limited to images. How can models be trained
to generate realistic sentences? Language is sequential and structured;
the meaning of a word depends on the words around it. As we discussed
earlier in this chapter, recurrent neural networks (RNNs) can work with
sequential data, but they struggle to deal with long-range dependencies
( the effect of words that are separated from each other by many words)
without long short-term memory (LSTM). The **Transformer** architecture
solved this problem in another way.

The key innovation of the Transformer is the **attention mechanism.**
Attention allows a model to focus on the relevant parts of the input.
For text, attention lets each word "look at" all other words in the
sequence. This captures relationships between distant words. For
example, in the sentence "The cat that chased the mouse was hungry,"
attention helps the model connect "was" with "cat" even though many
words separate them.

The original Transformer has an encoder-decoder structure. The encoder
processes the input sequence, and the decoder generates the output
sequence. Some models only use the decoder. These decoder-only models
(like GPT) are designed for autoregressive generation. They process the
input sentence and generate the output one word at a time.[^22] The
model predicts the next word in the sequence based on all previous
words.

Consider the sentence: "The quick brown fox jumps over the lazy." One
such language model could learn to predict the next word, "dog," based
on the text of the sentence and the context it learned throughout
training. Indeed, such a model could continue predicting words forever.

# Foundation Models and Pretraining

A **foundation model** is a large neural network[^23] trained on a
massive dataset on a reconstruction task ( regenerating the data, like
we have seen in the autoencoder case or reconstructing missing pixels in
images). This way, these models learn general patterns and relationships
among the data, and then the models are adapted to many different tasks.

**Pretraining** is the process of training a model on a large, general
dataset before fine-tuning it for a specific task. For example, a
language model can be trained on billions of words from the internet,
and after this pretraining, it can be **fine-tuned** on a smaller
dataset for a specific task. For example, you can pretrain on a British
English corpus and fine-tune on an American English corpus. Or you can
pretrain on a large corpus of artwork and fine-tune on Andy Warhol alone
to generate art in his style (we will address ethical implications in a
moment).

**Large language models** (LLMs) are foundation models for text. They
can generate human-like text, answer questions and even write code. They
are the result of scaling up the Transformer architecture to enormous
sizes. GPT, Claude, Grok, Gemini and DeepSeek are all examples of LLMs.

# Ethics of Generation

Generative models raise significant ethical concerns. They learn from
human data --- our text, our images, etc. --- which contains human
biases. These biases can be amplified by the model, leading to harmful
outputs when generative models are applied to tasks like hiring, law
enforcement, healthcare and translation. Gender and racial biases are
some of the most visible biases in these models, where they will make
obviously harmful assumptions about people based on their race and
gender. These stereotypes exist in the modern world, and these training
sets are so large that it's impossible to completely de-bias them.

Generative models are often misused to lie and mislead. They can create
a convincing fake news post, advertisement or video so quickly and
easily. The existence of these models threatens public trust in the
world around them.

Most generative models are trained on data scraped from the internet
without permission (from the website or from the authors). This includes
copyrighted works, personal data, and art. It's now possible for an
artist's entire portfolio to be uploaded to the dataset of a generative
image model, and then ask the model to produce a new image in that
artist's style. The data *is* the model. If the data is stolen, then any
output of the model represents theft.

[^21]: The term "deconvolution" does not actually refer to a transposed
    convolution, though the two terms are often used interchangeably.
    Avoid using "deconvolution."

[^22]: These models don't process words but "tokens," mathematical
    representations of words or word fragments.

[^23]: "Large" meaning many parameters.
