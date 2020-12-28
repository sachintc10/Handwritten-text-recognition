# Handwritten Text Recognition with TensorFlow



## Run demo

Install the required libraries given in the requirements.txt by running  pip3 install -r requirements.txt on your command prompt or terminal.
This install all the required libraries required for the model.

Then run main.py in src folder
> python main.py
Validation character error rate of saved model: 10.624916%
Init with stored values from ../model/snapshot-38
Recognized: "little"
Probability: 0.96625507
```

Tested with:

* Python 3
* TF 1.3, 1.10 and 1.12 (commit <= 97c2512)
* TF 1.14, 1.15, 2.3.1 (commit >= ec00c1a)
* Ubuntu 20.04, 18.04 and Windows 10


## Command line arguments

* `--train`: train the Neural Network.
> python main.py --train
Init with new values
Epoch: 1
Train NN
Batch: 1 / 500 Loss: 130.354
Batch: 2 / 500 Loss: 66.6619
Batch: 3 / 500 Loss: 36.0154
Batch: 4 / 500 Loss: 24.5898
Batch: 5 / 500 Loss: 20.1845
Batch: 6 / 500 Loss: 19.2857
Batch: 7 / 500 Loss: 18.3493
...

* `--validate`: validate the NN.
Validate NN
Batch: 1 / 115
Ground truth -> Recognized
[OK] "," -> ","
[ERR:1] "Di" -> "D"
[OK] "," -> ","
[OK] """ -> """
[OK] "he" -> "he"
[OK] "told" -> "told"
[ERR:2] "her" -> "nor"
...
Character error rate: 13.956289%. Word accuracy: 67.721739%.
```

* `--beamsearch`: use vanilla beam search decoding (better, but slower) instead of best path decoding.
* `--wordbeamsearch`: use word beam search decoding (only outputs words contained in a dictionary) instead of best path decoding.

If neither `--train` nor `--validate` is specified, the NN infers the text from the test image (`data/test.png`).
Two examples: if you want to infer using beam search, execute `python main.py --beamsearch`, while you have to execute `python main.py --train --beamsearch` if you want to train the NN and do the validation using beam search.

## Train model 

### IAM dataset

The data-loader expects the IAM dataset \[5\] (or any other dataset that is compatible with it) in the `data/` directory.
Follow these instructions to get the dataset:

1.download the datasets from iam (http://www.fki.inf.unibe.ch/databases/iam-handwriting-database).
2. Download `words/words.tgz`.
3. Download `ascii/words.txt`.
4. Put `words.txt` into the `data/` directory.
5. Create the directory `data/words/`.
6. Put the content (directories `a01`, `a02`, ...) of `words.tgz` into `data/words/`.
7. Go to `data/` and run `python checkDirs.py` for a rough check if everything is ok.

If you want to train the model from scratch, delete the files contained in the `model/` directory.
Otherwise, the parameters are loaded from the last model-snapshot before training begins.
Then, go to the `src/` directory and execute `python main.py --train`.
After each epoch of training, validation is done on a validation set (the dataset is split into 95% of the samples used for training and 5% for validation as defined in the class `DataLoader`).
If you only want to do validation given a trained NN, execute `python main.py --validate`.

## FAQ

1. I get the error message "... TFWordBeamSearch.so: cannot open shared object file: No such file or directory": if you want to use word beam search decoding, you have to compile the custom TF operation from source.
2. I get the error message "... ModuleNotFoundError: No module named 'editdistance'": you have to install the mentioned module by executing `pip install editdistance`.
3. Where can I find the file `words.txt` of the IAM dataset: it is located in the subfolder `ascii` of the IAM website.


## References

\[1\] [Build a Handwritten Text Recognition System using TensorFlow](https://towardsdatascience.com/2326a3487cd5)

\[2\] [Scheidl - Handwritten Text Recognition in Historical Documents](https://repositum.tuwien.ac.at/obvutwhs/download/pdf/2874742)

\[3\] [Shi - An End-to-End Trainable Neural Network for Image-based Sequence Recognition and Its Application to Scene Text Recognition](https://arxiv.org/pdf/1507.05717.pdf)

\[4\] [Marti - The IAM-database: an English sentence database for offline handwriting recognition](http://www.fki.inf.unibe.ch/databases/iam-handwriting-database)
