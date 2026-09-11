# Attention

an lstm squeezes the whole sequence into one final hidden state and everything downstream works
from that. attention removes the bottleneck, it keeps all the steps and takes a weighted average
where the weights are computed from the data.

## the formula

```
Attention(Q, K, V) = softmax( Q K^T / sqrt(d) ) V
```

- **query** : what am i looking for
- **key** : what does each position advertise
- **value** : what each position returns if selected

score = query dot key, softmax over the keys, then weighted sum of the values. thats it, 4 lines
of numpy.

`sqrt(d)` matters. dot products grow with dimension, big scores make softmax nearly a hard max
and the gradient dies. at d=512 the raw scores ran from -50 to +50, after scaling they sat
between -2 and +2.

## masking

for generation a position must not see the future. set the blocked scores to -1e9 **before** the
softmax, not zero the weights after.

## multi head

not more layers. the same width split into pieces, one attention per piece, outputs concatenated.
so one head can track the subject while another tracks the action.

## Cost

every position scores every other position, so the matrix is T x T. sequence 1000 means a
million entries. double the sequence and attention gets four times bigger. that is why context
windows were small for years.

## The experiment that proves it works

20 step sequences, a random number at each step and a marker at exactly one position. the model
must output the number at the marked position, so there is one correct place to look and i can
check whether the attention weights land there.

| | test mse |
|---|---|
| attention | 0.00011 |
| mean pooling (control) | 0.0154 |
| predicting the mean | 0.863 |

attention put its peak on the marked step in **100%** of test sequences, with about 55% of the
weight there.

the control was the interesting part. i expected mean pooling to fail completly and land near
0.863, but it got 0.0154. averaging does not destroy the marker, it dilutes it by 20. so
attention is 140x better, not infinitely better.

## rnn vs attention

| | rnn / lstm | attention |
|---|---|---|
| distance between two steps | t hops | always 1 hop |
| computation | one step at a time | all positions at once |
| cost | O(T) | O(T^2) |

the parallel row is why transformers took over.

## What I did in this folder

1. intuition, weighted average, query key value by hand, why sqrt(d) (`01`)
2. scaled dot product attention in numpy, self attention on a 6 word sentence, causal mask,
   multi head (`02`)
3. attention trained on the marker task, weights visualised and checked, mean pooling control (`03`)

`03` needs torch, everything else is numpy. no downloads.
