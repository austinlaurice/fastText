# fastText

fastText is a library for efficient learning of word representations and sentence classification.

## Requirements

fastText uses C++11 features and therefore it requires a compiler with good C++11 support.
These include :

* (gcc-4.6.3 or newer) or (clang-3.3 or newer)

Compilation is carried out using a Makefile, so you will need to have a working **make**.
For the word-similarity evaluation script you will need:

* python 2.6 or newer

## Building fastText

In order to build `fastText`, use the following:

```
$ git clone git@github.com:facebookresearch/fastText.git
$ cd fastText
$ make
```

This will produce object files for all the classes as well as the main binary `fasttext`.
If you do not plan on using the default system-wide compiler, update the two macros defined at the beginning of the Makefile (CC and INCLUDES).

## Example use cases

This library has two main use cases: word representation learning and text classification.
These were described in the two papers [1] and [2].

### Word representation learning

In order to learn word vectors, as described in [1], do:

```
$ ./fasttext skipgram -input data.txt -output model
```

where `data.txt` is a training file containing `utf-8` encoded text.
By default the word vectors will take into account character n-grams from 3 to 6 characters.
At the end of optimization the program will save two files: `model.bin` and `model.vec`.
`model.vec` is a text file containing the word vectors, one per line.
`model.bin` is a binary file containing the parameters of the model along with the dictionary and all hyper parameters.
The binary file can be used later to compute word vectors or to restart the optimization.

### Obtaining word vectors for out-of-vocabulary words

The previously trained model can be used to compute word vectors for out-of-vocabulary words.
Provided you have a text file `queries.txt` containing words for which you want to compute vectors, use the following command:

```
$ ./fasttext print-vectors model.bin < queries.txt
```

This will output word vectors to the standard output, one vector per line.
This can also be used with pipes:

```
$ cat queries.txt | ./fasttext print-vectors model.bin
```

See the provided scripts for an example. For instance, running:

```
$ ./get-vectors.sh
```

will compile the code, download data, compute word vectors and evaluate them on the rare words similarity dataset RW [Thang et al. 2013].

### Text classification

This library can also be used to train supervised text classifiers, for instance for sentiment analysis.
In order to train a text classifier using the method described in [2], use:

```
$ ./fasttext supervised -input train.txt -output model
```

where `train.txt` is a text file containing a training sentence per line along with the labels.
By default, we assume that labels are words that are prefixed by the string `__label__`.
This will output two files: `model.bin` and `model.vec`.
Once the model was trained, you can evaluate it by computing the precision at 1 (P@1) on a test set using:

```
$ ./fasttext test model.bin test.txt
```

In order to obtain the most likely label for a piece of text, use:

```
$ ./fasttext predict model.bin test.txt
```

where `test.txt` contains a piece of text to classify per line.
Doing so will output to the standard output the most likely label per line.
See `classification.sh` for an example use case.
In order to reproduce results from the paper [2], run `classification-results.sh`, this will download all the datasets and reproduce the results from Table 1.

## Full documentation

```
The following arguments are mandatory:
  -input      training file path
  -output     output file path

The following arguments are optional:
  -lr         learning rate [0.05]
  -dim        size of word vectors [100]
  -ws         size of the context window [5]
  -epoch      number of epochs [5]
  -minCount   minimal number of word occurences [1]
  -neg        number of negatives sampled [5]
  -wordNgrams max length of word ngram [1]
  -loss       loss function {ns, hs, softmax} [ns]
  -bucket     number of buckets [2000000]
  -minn       min length of char ngram [3]
  -maxn       max length of char ngram [6]
  -thread     number of threads [12]
  -verbose    how often to print to stdout [1000]
  -t          sampling threshold [0.0001]
  -label      labels prefix [__label__]
```

## References

[1] Piotr Bojanowski, Edouard Grave, Armand Joulin, Tomas Mikolov, Enriching Word Vectors with Subword Information, arXiv 1607.04606, 2016

[2] Armand Joulin, Edouard Grave, Piotr Bojanowski, Tomas Mikolov, Bag of Tricks for Efficient Text Classification, arXiv 1607.01759, 2016

## Join the fastText community

* Facebook page: https://www.facebook.com/groups/1174547215919768
* Contact: [egrave@fb.com](mailto:egrave@fb.com) [bojanowski@fb.com](mailto:bojanowski@fb.com) [ajoulin@fb.com](mailto:ajoulin@fb.com) [tmikolov@fb.com](mailto:tmikolov@fb.com)

See the CONTRIBUTING file for information about how to help out.

## License

fastText is BSD-licensed. We also provide an additional patent grant.
