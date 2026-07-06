# Neural Networks

going from a single neuron all the way to a net that does any task. all of it is just the math (forward, loss, backprop, gradient descent) turned into numpy.

## Perceptron (single neuron)
- output = step(w·x + b), fires 1 or 0
- learning rule: w = w + lr*(y - pred)*x
- works on OR and AND (linearly separable), draws ONE straight line
- **fails on XOR** because no single line can split it

## Why XOR needs more
- a perceptron is one straight line, only good for linearly separable data
- the fix is a hidden layer, which lets the net bend the boundary into nonlinear shapes

## Neural network from scratch (1 hidden layer)
- **forward**: a1 = tanh(X·W1 + b1), a2 = sigmoid(a1·W2 + b2)
- **loss**: binary cross entropy
- **backprop**: dz2 = a2 - y, then push the error back through W2 and the tanh to get every gradient
- **update**: gradient descent, W = W - lr*dW
- solves XOR (100%) and two moons (~98%)

## Activation functions
- without one, stacked linear layers collapse into a single linear layer (proved it)
- **sigmoid** (0,1) for binary output, **tanh** (-1,1), **relu** max(0,z), **leaky relu**
- **vanishing gradient**: sigmoid slope caps at 0.25, tanh near 1, so deep nets multiply tiny slopes and the gradient dies. relu slope is 1, doesnt vanish, so its the default for hidden layers
- **softmax** at the output for multiclass (probabilities that sum to 1)

## On real data
- breast cancer (binary) with **our own net**: ~96% test acc, matches sklearn
- digits (multiclass, 10 classes) with sklearn MLP: ~97%
- diabetes (regression) with sklearn MLPRegressor: predicts a continuous number (R2 ~0.4, hard dataset)
- same network, just swap the output + loss: sigmoid (binary), softmax (multiclass), linear (regression)

## Notebooks
1. perceptron, OR/AND work, XOR fails (`01`)
2. neural network from scratch, solves XOR + moons (`02`)
3. activation functions and the vanishing gradient (`03`)
4. trained on breast cancer, digits, and diabetes (`04`)
