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

# open problems
this section is less of a notes section and more of a place to write my thoughts on open problems in ml, and some ideas that could solve em.
the area of research that interests me the most in ml is ai safety & privacy. there's a lot of subfields to it such as privacy perserving ml. [Toward Trustworthy AI Development: Mechanisms for Supporting Verifiable Claims](https://arxiv.org/pdf/2004.07213.pdf) does a great job of discussing the topic. section 3 of the paper, on software mechanisms and recommendations, is what this section will be about.

## [succinct secure aggregation](https://github.com/ghiliweld/writings/blob/master/mpc.md#succint-secure-aggregation-ssa)
sa solves the problem of federated learning where data providers want to train a model on their data w/o leaking their data to the model owner. in the sa protocol, gradients are split into shares blinded by pairwise-generated masks such that the masks cancel out once the data is aggregated. the problems w/ sa is that the number of messages needed for the protocol to run scales quadraticaly O(n^2) where n is the number of parties participating in the aggregation. succinct secure aggregation (ssa), seeks to improve on this complexity making it O(n).

### additive secret sharing
additive secret sharing is masking a secret by adding a mask (also a secret) to the secret we want to hide.
```
let x = a + b for some a,b in Zn i.e. the integers modulo n (or any finite abelian group)
given only x, a and b are impossible to derive since they could be anything
```
this is a powerful property for masking provided data for aggregation.
below i'll define a constant round O(n) aggregation scheme based on this property. i'll define it with scalars but it should work with vectors as well.
```
let P = {p_1, ..., p_n} be the set of providers in the aggregation, p_i is the i-th provider in the set

let g_i be the gradient belonging to the i_th provider

a provider p_i generates a random mask r_i, and sends x_i = g_i + r_i to the aggregator

the aggregator computes X = sum(from: i = 1, to: n, of: x_i) i.e. x_1 + ... + x_n

the aggregator then broadcasts X to all the providers

p_i then verifies that X != (n-1)*g_i + (n)*r_i, if it is then the protocol is aborted

else, p_i sends y_i = X - (n-1)*g_i - (n)*r_i

once the aggregator then computes the Y = nX + sum(from: i = 1, to: n, of: y_i)

Y = sum(from: i = 1, to: n, of: g_i) i.e. g_1 + ... + g_n which is the result we want
```

here's an example with 3 users to be aggregated:
```
let P = {p_1, p_2, p_3}
p_1 sends x_1 = g_1 + a, p_2 sends x_2 = g_2 + b, and p_3 sends x_3 = g_3 + c
aggregator returns X = g_1 + a + g_2 + b + g_3 + c to each provider
p_1 returns y_1 = X - 2g_1 - 3a, p_2 returns y_2 = X - 2g_2 - 3b, p_3 returns y_3 = X - 2g_3 - 3c
aggregator computes Y = 3(g_1 + a + g_2 + b + g_3 + c) - 2g_1 - 3a - 2g_2 - 3b - 2g_3 - 3c
                      = g_1 + g_2 + g_3
```
note: the security of the scheme is still trash. it looks aight under honest conditions but this is def fucked if a malicious user joins the round. commited values won't be unmasked but it's fairly easy to force a stalemate or send in innacurate data to mess up the results without being detected.

edit: that was cap, there's a major flaw that undoes the masking
if the aggregator computes the following, the scheme falls apart
```
(n *x_i - (X - y_i)) = n * x_i - (X - X + (n-1)g_i + n * r_i) = n(g_i + r_i) - (n-1)g_i - n*r_i = g_i
```

unfortunately, the scheme does not support any dropouts so if a user goes missing after the whole round needs to be aborted. an added improvement would be making this dropout resistant or not making it non-interactive. the problem is making it non-interactive means the unmasking value needs to be sent with the mask, making it trivial for a malicious server to derive the value we're trying to hide. adapting this scheme to work with shamir's techniques of polynomial based secret sharing might solve this problem, transforming it to a k-of-n threshold aggregation scheme.

### obliviousity
obliviousity is a sort of zero-knowledge property where parties don't glean any information from each other. integrating obliviousity in a ml context would imply providers not learning any info about the model from the model owner and the owner won't learn any sensitive data from the providers.
obliviousity can be obtained through:
- **one-to-many oblivious data (gradient vectors) aggregation**
  <br> providers p_1 through p_n send gradients g_1, ..., g_n for aggregation. these gradients are summed to g_total obliviously.
- **one-to-one onblivious backpropagation**
  <br> the owner send a blinded model to the provider then the provider runs backpropagation on their private data without learning anything about the model. the owner then receives the updated model and decrypts it.
  
 some papers on the topic:
 - [Oblivious Polynomial Evaluation and Oblivious Neural Learning](http://citeseer.ist.psu.edu/viewdoc/download;jsessionid=295C7C83757F0440263D873AE249ECFF?doi=10.1.1.132.4411&rep=rep1&type=pdf)
 - [Privacy-Preserving Aggregation of Time-Series Data with Public Verifiability from Simple Assumptions](https://eprint.iacr.org/2017/479.pdf)
 - [Privacy-Preserving Stream Aggregation with Fault Tolerance](https://eprint.iacr.org/2011/655.pdf)
 - [A New Framework for Privacy-Preserving Aggregation of Time-Series Data](https://hal.inria.fr/hal-01181321v3/document)

