# Word Embeddings

a network cannot take words. one hot works but every pair of words is equally far apart, so
`cat` and `dog` are as unrelated as `cat` and `democracy`. an embedding is a short dense vector
per word where similar words point in similar directions.

## where the numbers come from

**distributional hypothesis** : a word is defined by the words around it. if two words appear in
the same kind of context they probably mean something similar. so the context is the training
signal and no labels are needed.

two ways to use it:

1. **count** the contexts and compress the matrix with SVD (`02`)
2. **predict** the context with a network and keep the weights (`03`)

## Corpus

nltk brown, categories news + fiction + romance + adventure. about 18k sentences, 400k words,
vocab cut to the top 3000. small, and that turned out to be the main limit on everything.

## Count based (SVD)

co-occurrence matrix 3000x3000, window 4, then `log(1+count)` because the raw counts are all
`the` and `of`, then SVD down to 100 dimensions.

## Skip gram with negative sampling

predict context from centre word. instead of a 3000 way softmax, push the real pair's dot
product up and 5 random pairs down. negatives are drawn from frequency^0.75.

1 million pairs, 8 epochs, about 10 seconds on cpu.

## What I actually found

neither method wins:

| word | count based | skip gram |
|---|---|---|
| three | four, five, two | nine, four, two, five |
| red | black, gray, blue | hung, white, colored, lips |
| water | still, dark, turned | glass, blood, mist, soil |
| doctor | alone, call, stand | watching, sox, elder |

and the quality is **not about frequency**. `red` appears 58 times and gives clean colour
neighbours, `doctor` appears 56 times and gives junk. the difference is how consistent the
contexts are, a colour always sits in the same slot while a doctor turns up anywhere.

analogies: `he : she` works, `one:two :: three:?` gives five, and `man:king :: woman:?` put
queen at rank 2, which is half a success considering king appears 14 times in the whole corpus.

## What I did in this folder

1. one hot vs dense, cosine similarity, tiny co-occurrence matrix by hand (`01`)
2. co-occurrence + SVD on brown, nearest neighbours, 2d plot (`02`)
3. skip gram with negative sampling in torch, analogies, comparison with `02` (`03`)

`02` and `03` need the nltk brown corpus, `nltk.download('brown')` is in the first cell. it
downloads once and then works offline.
