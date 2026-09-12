# Transformer

the attention folder already built the main part. a transformer is that block plus positional
encoding, a feed forward layer, and residual + layer norm around both.

```
tokens
  |
embedding + positional encoding
  |
[ attention -> add & norm -> feed forward -> add & norm ]  x N
  |
linear to vocabulary
```

no recurrence, every position computed at the same time.

## Attention cannot see order

this is the part i had not realised. attention is a weighted **sum**, and a sum does not care
about order. i shuffled the input and got the same outputs back, just shuffled with it.

so `dog bites man` and `man bites dog` are identical to raw attention. positional encoding is
not a nice extra, without it the model is broken.

sine and cosine of diffrent frequencies, one row per position. nearby positions get similar
vectors, cosine similarity of position 10 with 11 is 0.966 and with 40 is 0.533.

## The block

- **multi head** : one big matmul then `view` + `transpose` so heads become a batch dimension, no loop
- **mask** : `masked_fill(-inf)` **before** the softmax
- **feed forward** : `D -> 4D -> D`, twice as many parameters as attention
- **pre norm residual** : `x = x + sublayer(norm(x))`

attention moves information sideways between positions, feed forward thinks about each position
on its own.

## Tests instead of assuming

**does the mask work** : change the last token, check earlier outputs. with mask, change at
position 0 is 0.000000. without mask its 0.151, so information really does leak backwards.

**do residuals matter** : 8 blocks deep, gradient reaching the input is 2.4 with residuals and
0.145 without, 17x difference at only 8 layers. real models are 12 to 96.

## Mini language model

2 layers, 4 heads, width 64, 105k parameters, trained on the same paragraph as the char RNN in
Day 49 to 50 so the two are comparable. 300 epochs, 33 seconds.

| | char RNN | this transformer |
|---|---|---|
| parameters | 24k | 105k |
| signal per sample | 1 character | 32 characters |
| greedy generation | reproduces the text | reproduces the text |
| sampling at high temp | breaks into non words | slips to another real sentence |
| parallel | no | yes |

both memorise, 10 lines of data. the interesting bit was the failure mode. at temperature 1.5
the transformer jumped into a diffrent sentence of the paragraph and kept going in real english,
while the rnn fell apart into `ttuny toges ainetw`. likely because attention sees the last 32
characters directly, so after one bad character 31 good ones remain and it can recover. the rnn
had one hidden state and once it was corrupted there was no way back.

## Where I was wrong

i expected layer 0 to attend nearby and layer 1 to attend far back. measured it and got 3.88
characters vs 3.63, so layer 1 looks slightly **closer**. no division of labour appeared. with
2 layers and 700 characters there is no reason for a long range head to form, the layer
specialisation people report is from real models on real corpora.

## What I did in this folder

1. intuition, permutation invariance proved, positional encoding, residual and layer norm (`01`)
2. the block written out in torch, multi head by reshaping, mask test and residual test (`02`)
3. mini char language model, generation, attention maps, compared against my own rnn (`03`)

needs torch. no downloads, the training text is in the notebook.
