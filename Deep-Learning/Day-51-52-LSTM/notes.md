# LSTM

a plain RNN multiplies its hidden state by Wh at every step, so memory and gradient both die
exponentially. an LSTM adds a seperate cell state that is added to instead of multiplied, plus
three gates that decide what to keep, write and output.

## the equations

```
i = sigmoid(...)      how much to write
f = sigmoid(...)      how much of the old memory to keep
g = tanh(...)         what the new information is
o = sigmoid(...)      how much of the memory to expose

c = f * c_old + i * g
h = o * tanh(c)
```

`c` is the long memory, `h` is the output. gates are sigmoid because 0 to 1 reads as a
percentage, content is tanh because it can be negative.

all four are packed into one matrix of width 4H and split with `chunk`, one matmul instead of four.

## The thing I actually found out

textbooks say LSTM fixes long memory. my own runs say it depends completly on the forget gate bias.

memory task, remember the first value across T steps of noise:

| T | rnn | lstm (forget bias 0) | lstm (forget bias 1) |
|---|---|---|---|
| 10 | 100% | 100% | 100% |
| 30 | 100% | 100% | 100% |
| 60 | 100% | **49.7%** | 100% |

at T=60 the plain RNN solved it and the default LSTM failed. reason: pytorch starts the forget
bias at 0, so the gate starts at sigmoid(0) = 0.5 and the cell state gets halved every step.
0.5^60 is 8.7e-19, the memory is gone before training even starts.

## Measuring it directly

one backward pass, no training. how much does the input at step t affect the output at step 59:

| | gradient at t=0 compared to t=59 |
|---|---|
| rnn | 7.9e-15 |
| lstm, forget bias 0 | 1.2e-12 |
| lstm, forget bias 3 | 0.37 |

so the default LSTM is only about 150 times better than an RNN, both are dead. with the bias
opened the gradient survives the whole 60 steps almost unchanged.

after training on T=30 the forget gates settle around 0.91 average, which is the hold behaviour
the task needs.

## Takeaway

set the forget gate bias to 1 when using `nn.LSTM`, pytorch does not do it for you.

## What I did in this folder

1. intuition, gates, cell state, holding a value by hand for 50 steps (`01`)
2. LSTM and RNN cells written by hand in torch, memory task at T = 10, 30, 60 (`02`)
3. measuring the vanishing gradient directly, and the forget gates of a trained model (`03`)

needs torch, everything else is generated data so no downloads.
