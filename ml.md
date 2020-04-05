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

the example i'll be referring to is [MNIST](https://en.wikipedia.org/wiki/MNIST_database) the starter ml project that consists of training a model to recognize handwritten images of numbers.

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

## open problems
- models are too big and too slow
- training a model takes too long
- training a model or making predicitions with one can be privacy invasive
- deploying models to production is a hassle, has bad ux (more like developer experience in this case)

### scaling

solutions:

### privacy

solutions:

### ux/dx

solutions:
