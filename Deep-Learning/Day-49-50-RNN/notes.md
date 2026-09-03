# Recurrent Neural Networks (RNN)

a dense net takes a fixed size input and has no idea about order. a sequence has neither of
those, the length changes and the order is the meaning. an RNN handles it with a loop and a
hidden state that carries whatever it has seen so far.

## the one equation

```
h_t = tanh(x_t Wx + h_(t-1) Wh + b)
```

- `h` is the memory, starts at zero, updated once per time step
- the **same** Wx, Wh, b are used at every step (weight sharing), so any length works
- the output is usually taken from the last hidden state

## how long is the memory

Wh decides it. feed one spike at t=0 and watch it decay:

- Wh = 0.3, gone in 2 steps
- Wh = 0.9, still there after 10 steps

too small and the signal dies before it reaches the loss (vanishing gradient), too big and it
explodes. this is the main weakness of a plain RNN.

## BPTT

unrolled, an RNN is a deep net where every layer shares the same weights. so backprop still
works, it just runs backwards through time. the one real difference from an MLP is that the
gradients of Wx, Wh, b are **summed** over all the time steps.

gradient clipping is needed, without it the loss went to nan a few times.

## Results I got

- **wave prediction (numpy)** : test mse 0.0021 against 0.0078 for the naive baseline that just
  repeats the last value. free running drifts after a while, expected, its own errors feed back
- **char level text (torch)** : 100% next character accuracy but the generated text is garbage

that second one was the useful lesson. 100% accuracy and a useless model, because it memorised
10 lines instead of learning english. greedy decoding gives the paragraph back exactly, sampling
breaks after one wrong character (exposure bias).

also the seed length must match the training window exactly, a 24 char seed on a 25 char model
produced garbage from the first character.

## What I did in this folder

1. intuition, why order matters, hidden state by hand, memory decay for diffrent Wh (`01`)
2. numpy RNN with BPTT written out, trained on a two frequency wave, free running generation (`02`)
3. torch char level RNN on a small paragraph, greedy vs sampling, temperature (`03`)

all offline, `03` needs torch. next folder is LSTM, wich fixes the memory problem.
