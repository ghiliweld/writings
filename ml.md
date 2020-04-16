# machine learning

my personal notes on machine learning. might not be completely accurate, and i make heavy use of analogies.

---

machine learning (ml) is about:
- recognizing patterns
- making predictions

## a primer on ml
there are 3 parts here at play during ml development:
- the data: loads of data collected meant to train your model, split into training and testing sets
- the training algorithm: the steps your algorithm takes to best train your model for a certainn task
- the model: the is the "black box" that makes predictions on the inputs you feed it

not every problem is one that needs to be solved with ml. ml is particularly powerful at identifying patterns in data and differences in variables within that data that an algorithm could not account for easily. that last sentence might be poorly formulated so i'll explain what i mean with a simple examples.

say i had a camera app which, thanks to a model i trained, is able to identify objects that i point to with the camera. when people look at a banana, they can usually identify what it is no matter how it's placed or what angle they're viewing it from. while it may be trivial for a person to do this, it's surprisingly difficult to implement an algorithm that can identify a banana accounting for all the angles a banana might be placed or under which lighting conditions it's placed in. a model, through sufficient training and data, can learn to identify a banana (or anything) no matter the circumstances it's presented in.

the example problem i'll be mostly referring to throughout this doc is [MNIST](https://en.wikipedia.org/wiki/MNIST_database) the starter ml project that consists of training a model to recognize parse numbers from images of handwritten of numbers.

a model, of which they're are many diff types (but i'm talking about neural networks here), is an ensemble of weights and biases (called "parameters") fine-tuned to return the probabilites of certain outputs being the actual result. in layman terms, you can see a parameter as being a setting in your model that determine what outcomes become more likely based on input is fed in. with the MNIST problem, if you feed an image of a number to your model what you'll get is an array of probabilites that the input number is the number corresponding to the array index.
ex:
```py
# we consider a model to be a function which returns a probability distribution
results = model(5)  # returns [0, 0, 0, 0.3, 0.2, 0.9, 0.3, 0, 0.1, 0.2]
```
here, index 5 of our result is also the highest number in the array meaning our input is likely to be the number 5.

this is a key distinction of how ml works. a model is never entirely sure, only probably sure, of what the right result is. it might start off making very bad predictions but it can get better through training.

## training
training is an  iterative process of examining loss (aka cost but ion really like that term), fine-tuning/modifying parameters, and repeating until you get the results you like. we'll get to what that means in a bit.

let's explore what happens during training:
- we initialize our model with random parameters, these will get fine-tuned and more accurate as training progresses
- we pass in our first input from our training set and observe the outcomes the model returns (spoiler alert: it'll be attrocious at first)
- we compare the results to our desired outcome, this measure is know as loss. as in, the loss in accuracy from our result to desired outcome
- naturally, we want to decrease this loss to make our model more accurate
- using calculus (gradient descent), we can tell our model how to modify it's parameters to minimize loss next time around
- we propagate the modification back into the model (this is called backpropagation)
- we repeat this process until loss is sufficiently minimized meaning our parameters are finally fine-tuned and we have an accurate model

let's see how this works in the MNIST context:
- our model is initialized with random params
- we feed in a 5 to our model
- we get back an array representing our probability distribution
- because our model is trash at first, the highest probaility in our distribution is at index 2, meaning it thinks a 5 is a 2
- our resulting loss is thus very high, but we tweak the parameters to reduce this loss
- we keep doing this until the model can finally properly classify numbers

keep in mind, in this example we would like all other probabilities to be close to zero, while the actual answer should have a probability close to 1.

a key part of trainning a good model is having good data. without good data, a model won't be able to extract meaningful patterns from them and thus won't be able to make accurate predictions. it would be like asking someone to look at pictures of numbers and guess what numbers they are except the number is almost completely covered. it would be pretty hard for them to answer.

## types of models
i'll introduce a few basic neural network architectures and talk about a few in depth.

**perceptron**: a basic neural network architecture.

**cnn**: a perceptron with convolutional layers that makes it much better at feature extraction. handy for image recognition since it makes of use of kernels that pool neighboring pixels together. it makes much more sense to enterpret neighboring pixels as being related than every pixel being related to every other one.

**autoencoder**: an autoencoder is comprised of two models: an encoder and a decoder. the input goes through the encoder, in which the model encodes it in another (often compressed) format. the encoded input then goes through the decoder which reconstructs the original input. the goal is to train the autoencoder to encode and decode with minimal loss in data. it would be like starting with a completed puzzle, breaking it back into it's pieces and put em in a box, then asking the model to solve the puzzle.

sounds basic but it can be used in interesting ways. given a noisy image (like an image with watermarks), an autoencoder can reconstruct an image sans watermarks. paired with GANs (which we'll see below), autoencoders can even be used to emulate cryptographic techniques like encryption such that adversaries cannot decode messages between two parties.

**recurring/lstm**: models w/ memory. is able to retain past context for continued use down the line in the computation. popular architecture for natural language processing or time-series data analysis. reading doesn't solely happen character-by-character, words that appeared before are important to the meaning in a sentence. this is why being able to remember past information is important.

**gan**: a gan is also comprised of two models: a generator and a discriminator. we want to train the generator to generate new original data such as paintings.

i'll start explaning this by using a popular example. an art forger starts making bank by selling their forgeries to unsuspecting buyers (conveniently, none of them seem to be able to detect forgeries no matter how bad they are). soon enough a detective comes along that is able to, quite easily, distinguish between real art pieces and mediocre forgeries. the forger quickly has to improve in order to fool the detective again. likewise, the detective must improve in order to keep spotting fakes. this continues until neither party can get any better and the detective can only reach a theoretical certainty of 50% on every input, no better than a guess essentially. you've prolly already guessed it, but the forger is the generator and the detective is the discriminator in the gan context. our goal is to train our generator to make as accurate forgeries as possible but this approach can be used for just about anything.

an advanced example of this is the cryptography example i wrote about above in the autoencoder section. briefly, imagine we have two parties, alice and bob, that want to exchange private messages and a third party, eve, that wants to eavesdrop. in the gan model, our generator is alice and the discriminator is eve. eve's goal is to decrypt encrypted messages from alice to bob. alice's goal is generate encrypted messages that only bob can decrypt.

## open problems
this section is less of a notes section and more of a place to write my thoughts on open problems in ml, and some ideas that could solve em.
the area of research that interests me the most in ml is ai safety & privacy. there's a lot of subfields to it such as privacy perserving ml. [Toward Trustworthy AI Development: Mechanisms for Supporting Verifiable Claims](https://arxiv.org/pdf/2004.07213.pdf) does a great job of discussing the topic. section 3 of the paper, on software mechanisms and recommendations, is what this section will be about.
