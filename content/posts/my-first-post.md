---
title: "Simple Neural Network in Pytorch"
date: 2022-06-07T15:26:15+02:00
tags:
    - Pytorch
category: tech
keywords:
    - Pytorch
    - Python
    - Neural Network
---

The only pre-requeste to understanding this simple article is just the basic understanding of python.

We are going to create a simple regressor -  a model which is used for predicting the continuous variable - in pytorch.

## Dataset

First, create a dataset containing our observations(examples) and the labels associated with each observation.

```python
import torch
import numpy
# column 1
mu1,sig1 = 0,0.1
x1 = np.random.normal(mu1,sig1,(100))
x1 = x1.reshape(100,1)

# column 2
mu2,sig2 = 1,1.2
x2 = np.random.normal(mu2,sig2,(100))
x2 = x2.reshape(100,1)

# concatenate both columns
x = np.concatenate((x1,x2),axis=1)

#labels
y = np.sin(x1)+np.power(x2,2)

```
In the first line, we imported torch. Consider it as a toolbox which contains all the necessary tools to build our neural network.

Next, our observations are represented by a matrix **x** which is of size (100,2) - 100 observations and 2 values per observation(also called atrribute/feature/column). Each individual vector x1 and x2 are generated from a normal distrubtion. The label variable is **y** which is of size (100,1). One label for each observation.

## Convert input data to tensors

The next step is to convert these numpy arrays to tensors. Tensors are just like arrays but they are for pytorch. 

```python
xt = torch.Tensor(x)
yt = torch.Tensor(y)
```

## Define Neural Network

After that, now we should define our neural network.

```python
class naiveRegressor(torch.nn.Module):
  def __init__(self):
    super(naiveRegressor,self).__init__()
    self.layer1 = torch.nn.Linear(2,20)
    self.act1 = torch.nn.ReLU()
    self.layer2 = torch.nn.Linear(20,20)
    self.act2 = torch.nn.ReLU()
    self.out = torch.nn.Linear(20,1)
    torch.nn.init.xavier_uniform_(self.layer1.weight)
    torch.nn.init.zeros_(self.layer1.bias)
    torch.nn.init.xavier_uniform_(self.layer2.weight)
    torch.nn.init.zeros_(self.layer2.bias)

  def forward(self,x):
    x = self.layer1(x)
    x = self.act1(x)
    x = self.layer2(x)
    x =  self.act2(x)
    x = self.out(x)
    return x

  
model = naiveRegressor()
mse_loss = torch.nn.MSELoss()
optim = torch.optim.SGD(model.parameters(),lr=0.001)

```

Our NN is actually a class and torch.nn.Module is a base class for all neural network modules. In init function, we define all of our NN layers, activation functions and kind of initialization for our weights and biases. Weights and biases are the parameters that our NN learn during the training phase. Lets go line by line. First we define layer1 of size (2,20). The first number should be the number of features which is 2 in our case as our x size is [100,2]. The number 20 could be anything it is the number of hidden neurons. In the next line, we applied activation function whose job is to introduce non-linearity for better generalization. Same goes for the layer 2 and activation 2. The last layer is output whose size is (20,1). 1 means that the final output is a single value.

Next we initialize weights and biases. If **i** is the number of input nodes/hidden nodes and **j** is the number of hidden nodes/output nodes, then the number of weights will be **ixj**.You can extend this to multiple layers and add them up for total number of weights. In our case it is, (2x20)+(20x20)+(20+1) = 320,000. One bias term is attached to each hidden and output node so biases will be (20+20+1) = 41. You don't need to explicitly initialize them as their is a default initialization. But, it is a good practice to self initialize as you would be hands on when you needed to initialize with different form of  initialization like uniform initialization etc.

In the forward function, we simply passing our input from one layer to the next and finally through the output layer(self.out).

In the next three lines after the class, we define our model object, loss function(function to calculate the deviation of our prediction from actual labels/target variable) and optimizer(method for optimizing our objective function).

Now everything is set-up. We should train our model now.

## Train our NN

```python
model.eval()
n_epochs = 500
for i in range(n_epochs):
  #forward pass
  l = model(xt)
  #loss 
  J = mse_loss(yt,l)
  # clear the gradients
  optim.zero_grad()
  #compute grad
  J.backward()
  # step
  optim.step()
  print("cuurent epoch {} and error value{}".format(i,J.item()))

```
Start our loop our n_epochs. It means we should pass our whole dataset through our model 500 times. In each iteration, we try to minimize the gap between the predicted value and actual value. The training part consists of 5 steps:

1. Forward Pass: Predict outputs(l) by passing our dataset into out NN model.
2. Calculate loss: calculate the loss which is the distance between our predicted value comes from the forward pass and the actual target variable value(y).
3. Clear the gradients.
4. Compute gradients(J.backward())
5. Update parameters(weights and biases): through optim.step() function.

We will see the output such as given below. In which we can see that our error is reducing with each epoch.

```python
cuurent epoch 0 and error value13.367425918579102
cuurent epoch 1 and error value13.296558380126953
cuurent epoch 2 and error value13.22631549835205
cuurent epoch 3 and error value13.156633377075195
cuurent epoch 4 and error value13.087475776672363
cuurent epoch 5 and error value13.018775939941406
cuurent epoch 6 and error value12.9505033493042
cuurent epoch 7 and error value12.882621765136719
cuurent epoch 8 and error value12.815115928649902
cuurent epoch 9 and error value12.747956275939941
cuurent epoch 10 and error value12.681158065795898

```

Now we can call our trained model to make predictions for us

```python
model(xt[0]).detach()

```

This is not going to be quite accurate as this was just a simple NN designed for the better explainability.